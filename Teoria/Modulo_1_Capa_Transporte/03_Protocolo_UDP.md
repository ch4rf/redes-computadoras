# Módulo 1: Capa de Transporte
## Tema 1.3: Protocolo UDP (User Datagram Protocol - RFC 768)

---

### 1. ¿Qué es UDP y cuáles son sus Características?
UDP es un protocolo de capa de transporte ligero, diseñado para aplicaciones que priorizan la **velocidad y la baja latencia** por encima de la confiabilidad absoluta.

* **No orientado a conexión (Connectionless):** No realiza ningún saludo previo (no hay Three-Way Handshake). Envía los datos directamente en cuanto la aplicación los genera.
* **No confiable / Mejor esfuerzo (Best-Effort Delivery):** No garantiza que los datagramas lleguen a su destino, ni que lleguen en el orden en que se enviaron, ni retransmite paquetes perdidos.
* **Mínima sobrecarga (Low Overhead):** Al tener un encabezado diminuto y no mantener estados de conexión en memoria, consume muy poca CPU y ancho de banda.
* **Sin Control de Flujo ni de Congestión:** UDP envía datos a la velocidad que la aplicación le pida, sin importar si la red o el receptor están saturados.
* **Soporte de Difusión Múltiple:** Soporta Unicast (uno a uno), Multicast (uno a un grupo) y Broadcast (uno a todos). *Nota: TCP solo soporta Unicast.*

---

### 2. Estructura del Encabezado UDP (UDP Header)
El encabezado UDP es extraordinariamente simple: tiene un tamaño fijo e inmutable de solo **8 bytes** (64 bits), dividido en 4 campos de 16 bits cada uno.

```text
 0                   1                   2                   3
 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|          Source Port          |       Destination Port        |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|            Length             |           Checksum            |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                             data                              |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
```

#### Descripción de los 4 Campos:
1. **Source Port (16 bits):** Número de puerto del proceso emisor (puede ser 0 si la aplicación no requiere respuesta).
2. **Destination Port (16 bits):** Número de puerto del servicio destinatario.
3. **Length (16 bits):** Longitud total del datagrama UDP en bytes (encabezado + datos). El valor mínimo posible es **8** (un datagrama con 0 bytes de datos).
4. **Checksum (16 bits):** Suma de verificación para detectar corrupción de bits en el encabezado o los datos. En IPv4 este campo es opcional (aunque ampliamente usado), mientras que en IPv6 es **obligatorio**.

---

### 3. ¿Por qué usar UDP si no es confiable?
Existe una confusión común en exámenes: *"Si UDP pierde datos, ¿por qué alguien querría usarlo?"*

La respuesta radica en tres factores críticos:
1. **Velocidad y Tiempo Real:** En una videollamada (Zoom/Teams) o juego en línea, un paquete de voz o de posición que llega con 300 ms de retraso tras una retransmisión TCP ya no sirve de nada. Es preferible descartar ese microsegundo de audio y continuar en vivo.
2. **Peticiones Rápidas de Solicitud/Respuesta:** Servicios como DNS o DHCP consisten en un solo mensaje de pregunta y un mensaje de respuesta. Iniciar una conexión TCP (3 paquetes de saludo) para enviar 1 sola consulta generaría un desperdicio del 300% en tiempo y paquetes.
3. **Control en Capa de Aplicación:** Si una aplicación basada en UDP requiere confiabilidad (como TFTP), la propia capa de aplicación puede implementar sus propios acuses de recibo ligeros sin la sobrecarga pesada de TCP.

---

### 4. Protocolos y Servicios Principales que utilizan UDP

| Protocolo | Puerto | Descripción y Justificación de uso de UDP |
| :--- | :--- | :--- |
| **DNS** *(Domain Name System)* | **53** | Consultas rápidas de resolución de nombres de dominio a IP. |
| **DHCP** *(Dynamic Host Configuration)* | **67 (Servidor) / 68 (Cliente)** | Configuración de red inicial cuando el cliente aún no tiene IP asignada ni conexión previa. |
| **TFTP** *(Trivial File Transfer Protocol)* | **69** | Transferencia simple de archivos (ej. cargar firmware IOS en routers/switches Cisco). |
| **SNMP** *(Simple Network Management Protocol)* | **161 / 162** | Monitoreo periódico del estado de switches y routers. |
| **NTP** *(Network Time Protocol)* | **123** | Sincronización precisa de reloj del sistema en servidores y routers. |
| **VoIP / RTP** *(Real-Time Transport Protocol)* | **5060 (SIP) / Dinámicos (RTP)** | Transmisión de voz y video en tiempo real. |

---

### 5. Preguntas Clave de Examen (Tipo UMG)

**P1: ¿Cuál es el tamaño fijo en bytes del encabezado UDP?**
> **R:** Es de **8 bytes** (fijo, sin opciones).

**P2: ¿Por qué el protocolo DNS utiliza UDP en el puerto 53 para consultas estándar?**
> **R:** Porque una consulta DNS consta de un solo paquete de pregunta y uno de respuesta; usar UDP evita la sobrecarga y latencia del saludo de tres vías de TCP.

**P3: ¿Qué sucede en la capa de transporte si un datagrama UDP se pierde en el camino?**
> **R:** Absolutamente nada a nivel de capa de transporte: UDP no detecta la pérdida, no envía ACKs ni retransmite. La responsabilidad de reintentar recae exclusivamente en la aplicación si esta lo requiere.

**P4: ¿Puede UDP enviar transmisiones a múltiples destinos a la vez?**
> **R:** Sí, UDP soporta **Broadcast** y **Multicast**, a diferencia de TCP que únicamente puede comunicarse en modo **Unicast** (punto a punto).
