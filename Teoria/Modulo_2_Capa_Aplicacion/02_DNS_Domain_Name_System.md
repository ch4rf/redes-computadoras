# Módulo 2: Capa de Aplicación
## Tema 2.2: DNS Exhaustivo (Formato de Mensaje, Archivos de Zona BIND y Seguridad DNSSEC)

---

### 1. Formato Binario del Mensaje DNS (RFC 1035)

Tanto las preguntas como las respuestas DNS utilizan una estructura idéntica encapsulada dentro de un datagrama UDP (o segmento TCP si supera los 512 bytes):

```text
 0                   1                   2                   3
 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|        Transaction ID         |QR| Opcode |AA|TC|RD|RA| Z |RCODE|
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|          QDCOUNT              |            ANCOUNT            |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|          NSCOUNT              |            ARCOUNT            |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                    Question Section                           |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                    Answer Section                             |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                    Authority Section                          |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                    Additional Section                         |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
```

#### Análisis de Banderas y Campos Críticos (Wireshark / Examen):
* **Transaction ID (16 bits):** Número identificador generado por el cliente. La respuesta del servidor debe incluir exactamente el mismo ID para evitar falsificaciones.
* **QR (Query/Response - 1 bit):** `0` = Pregunta (*Query*); `1` = Respuesta (*Response*).
* **AA (Authoritative Answer - 1 bit):** Si vale `1`, la respuesta proviene del servidor que es dueño legal de la zona, no de una copia en caché.
* **TC (Truncated - 1 bit):** Si vale `1`, la respuesta superó los 512 bytes de UDP y fue recortada. Esto le ordena al cliente **reintentar la consulta inmediatamente usando TCP en el puerto 53**.
* **RD (Recursion Desired - 1 bit):** Petición del cliente solicitando resolución recursiva.
* **RA (Recursion Available - 1 bit):** Notificación del servidor indicando si soporta recursión.
* **RCODE (Response Code - 4 bits):**
  * `0`: *No Error* (Éxito).
  * `2`: *Server Failure* (Error interno del servidor).
  * `3`: *NXDOMAIN* (El dominio consultado no existe en Internet).
  * `5`: *Refused* (El servidor se niega a responder por políticas o lista blanca).

---

### 2. Sintaxis Real de un Archivo de Zona DNS (Estándar BIND)

Para entender cómo se configuran servidores como Linux BIND o Windows Server DNS, analiza este archivo de zona real para el dominio ficticio `redesumg.edu.gt`:

```text
$TTL 86400          ; Tiempo de vida por defecto en caché: 1 día (en segundos)
$ORIGIN redesumg.edu.gt.

; Registro SOA: Autoridad Inicial de la Zona
@   IN  SOA  ns1.redesumg.edu.gt. admin.redesumg.edu.gt. (
        2026110501  ; Número de serie (Formato: YYYYMMDDnn)
        10800       ; Refresh: Los servidores secundarios consultan cada 3 horas
        3600        ; Retry: Si falla la conexión, reintentan cada 1 hora
        604800      ; Expire: Si el primario no responde en 1 semana, descartan la zona
        86400 )     ; Negative Cache TTL: Tiempo para recordar dominios inexistentes

; Registros de Servidores de Nombres (NS)
@       IN  NS      ns1.redesumg.edu.gt.
@       IN  NS      ns2.redesumg.edu.gt.

; Registros A (Mapeo Nombre -> IPv4)
ns1     IN  A       200.10.15.2
ns2     IN  A       200.10.15.3
www     IN  A       200.10.15.10
portal  IN  A       200.10.15.20

; Registros AAAA (Mapeo Nombre -> IPv6)
www     IN  AAAA    2001:db8:acad::10

; Registros MX (Servidor de Correo con Prioridad)
@       IN  MX  10  mail.redesumg.edu.gt.
mail    IN  A       200.10.15.25

; Registros CNAME (Alias)
ftp     IN  CNAME   www.redesumg.edu.gt.
webmail IN  CNAME   mail.redesumg.edu.gt.

; Registro TXT (Validación de Correo SPF para evitar suplantación)
@       IN  TXT     "v=spf1 mx ip4:200.10.15.25 ~all"
```

* **Nota de examen:** El símbolo `@` es un atajo que representa el dominio actual del archivo (`redesumg.edu.gt.`). El punto al final de los nombres (`.`) es obligatorio para evitar que el software añada automáticamente el dominio base al final.

---

### 3. Vulnerabilidades Clásicas y la Solución DNSSEC

* **Envenenamiento de Caché DNS (DNS Cache Poisoning / Ataque de Kaminsky):**  
  Como UDP no tiene estado ni verificación de conexión previa, un atacante inunda a un servidor DNS recursivo con miles de respuestas falsas adivinando el `Transaction ID` de 16 bits y el puerto de origen. Si acierta antes de que llegue la respuesta legítima, el servidor DNS guardará la IP del atacante en su memoria caché, redirigiendo a todos los usuarios bancarios o institucionales a páginas fraudulentas.
* **DNSSEC (Domain Name System Security Extensions - RFC 4033):**  
  Añade autenticación criptográfica de origen e integridad de datos a las respuestas DNS mediante firmas digitales basadas en criptografía de clave pública:
  * **RRSIG (Resource Record Signature):** Cada registro DNS va firmado con una firma criptográfica.
  * **DNSKEY:** Clave pública del servidor que los clientes usan para validar la firma `RRSIG`.
  * **DS (Delegation Signer):** Huella digital (hash) de la clave del servidor hija almacenada en el servidor padre (crea una **Cadena de Confianza** ininterrumpida desde la raíz de Internet `.` hacia abajo).
