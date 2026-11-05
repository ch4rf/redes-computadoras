# 📚 Manual Maestro Enciclopédico de Redes de Computadoras (UMG)

Guía completa y exhaustiva de estudio teórico, metodologías de cálculo de ingeniería y diseño para el examen de Redes de Computadoras en la Universidad Mariano Gálvez de Guatemala.

---

## 🌟 [Capítulo Maestro de Integración Entre Capas](Teoria/00_Integracion_Entre_Capas_Modelo_TCP_IP.md)
* **Encapsulamiento y Desencapsulamiento:** El viaje completo de un dato desde el bit en el cobre/fibra hasta la aplicación.
* **Restricciones métricas y fórmulas:** Relación física directa entre Cableado, MTU (1500 B), Encabezado IP (20 B), Encabezado TCP (20 B) y MSS (1460 B).
* **Anatomía de una transacción real:** El viaje segundo a segundo de un clic hacia `https://portal.umg.edu.gt` atravesando Capa Física, DHCP, DNS, TCP Handshake, TLS 1.3 y HTTP.
* **Matriz de diagnósticos y fallas cruzadas.**

---

## 📑 Módulos de Aprendizaje Exhaustivo

### 🌐 [Módulo 1: Capa de Transporte (Capa 4 OSI)](Teoria/Modulo_1_Capa_Transporte/)
* 📘 [1.1 Fundamentos y Modelo de Sockets](Teoria/Modulo_1_Capa_Transporte/01_Fundamentos_Capa_Transporte.md)
  * Entrega Process-to-Process vs Host-to-Host. Búferes del Kernel (Send/Receive Buffer).
  * La tupla del Socket y la 4-tupla de sesión. Agotamiento de puertos efímeros.
  * Rangos IANA completos y tabla maestra de puertos bien conocidos.
  * Relación matemática entre MTU y MSS.
* 📘 [1.2 Protocolo TCP Exhaustivo](Teoria/Modulo_1_Capa_Transporte/02_Protocolo_TCP.md)
  * Anatomía profunda del encabezado (20 a 60 bytes) y significado de las 6 banderas principales.
  * **La Máquina de Estados Finitos de TCP (Los 11 Estados Oficiales):** Diagrama y tabla completa (`SYN-SENT`, `ESTABLISHED`, `CLOSE-WAIT`, `TIME-WAIT`, etc.).
  * **¿Por qué TIME-WAIT dura 2MSL?** Justificación técnica de confiabilidad.
  * **Algoritmos de Control de Congestión:** *Slow Start* (crecimiento exponencial), *Congestion Avoidance* (AIMD lineal), *Fast Retransmit* (3 ACKs duplicados), *Fast Recovery*, $ssthresh$ y $cwnd$.
  * **Producto Retardo-Ancho de Banda (BDP):** Fórmula $BDP = \text{BW} \times RTT$, cálculo numérico y la opción *Window Scale* (RFC 1323) para enlaces Gigabit.
  * Algoritmo de Nagle vs. Delayed ACKs (`TCP_NODELAY`).
* 📘 [1.3 Protocolo UDP Exhaustivo](Teoria/Modulo_1_Capa_Transporte/03_Protocolo_UDP.md)
  * Cabecera de 8 bytes y cálculo matemático del **Checksum con Pseudo-Encabezado IP**.
  * El peligro de la **Fragmentación IP** cuando los datagramas UDP superan los 1,472 bytes.
  * Protocolos modernos de tiempo real: RTP/RTCP para VoIP/video y QUIC (base de HTTP/3).
* 📘 [1.4 Comparativa Avanzada y Banco de Problemas Resueltos](Teoria/Modulo_1_Capa_Transporte/04_Comparativa_TCP_vs_UDP.md)
  * Gran matriz de ingeniería (memoria TCB, tablas conntrack de firewall, preservación de fronteras).
  * Cálculo matemático de eficiencia de red y sobrecarga de protocolo ($\eta$).
  * **Problemas numéricos de examen resueltos paso a paso:**
    * Problema 1: Cálculo exacto de números `Seq` y `Ack` en transferencias secuenciales con ISN.
    * Problema 2: Cálculo de BDP y throughput real limitado por ventana de recepción.

---

### 🖥️ [Módulo 2: Capa de Aplicación (Capa 7 OSI / Capa de Aplicación TCP/IP)](Teoria/Modulo_2_Capa_Aplicacion/)
* 📗 [2.1 Modelos de Red y Servicios](Teoria/Modulo_2_Capa_Aplicacion/01_Modelos_y_Servicios_Red.md)
  * Arquitectura Cliente-Servidor vs Peer-to-Peer (P2P).
* 📗 [2.2 DNS Exhaustivo](Teoria/Modulo_2_Capa_Aplicacion/02_DNS_Domain_Name_System.md)
  * Formato binario del mensaje DNS (Transaction ID, Flags QR/AA/TC/RD/RA/RCODE, secciones).
  * **Sintaxis real de un archivo de zona BIND:** Registros `$TTL`, `@`, `SOA`, `NS`, `A`, `AAAA`, `CNAME`, `MX`, `TXT` y `PTR`.
  * Vulnerabilidad de envenenamiento de caché (Ataque Kaminsky) y arquitectura de seguridad **DNSSEC** (`RRSIG`, `DNSKEY`, `DS`).
* 📗 [2.3 DHCP Exhaustivo](Teoria/Modulo_2_Capa_Aplicacion/03_DHCP_Dynamic_Host_Configuration.md)
  * Anatomía de la trama BOOTP/DHCP: campos `xid`, `ciaddr`, `yiaddr`, `siaddr`, `giaddr`, `chaddr`.
  * **Matriz de valores de campos durante los 4 pasos de DORA**.
  * Temporizadores T1 (50% Unicast) y T2 (87.5% Broadcast).
  * Agente de retransmisión DHCP Relay (`ip helper-address`).
  * **Seguridad en Capa 2 contra ataques DHCP:** Ataques de inanición (*Starvation*), Servidores DHCP maliciosos (*Rogue DHCP*) y mitigación con **DHCP Snooping** (puertos de confianza vs no confiables).
* 📗 [2.4 Servicios Web: De HTTP/1.1 a HTTP/3 y TLS 1.3](Teoria/Modulo_2_Capa_Aplicacion/04_Servicios_Web_HTTP_HTTPS.md)
  * Evolución de la web: HTTP/1.0 $\rightarrow$ HTTP/1.1 $\rightarrow$ HTTP/2 $\rightarrow$ HTTP/3.
  * **El problema del Bloqueo Head-of-Line (HoL):** Diferencias entre bloqueo en capa de aplicación y bloqueo en capa de transporte TCP.
  * Solución de HTTP/3 mediante QUIC sobre datagramas UDP.
  * Cifrado web: Comparativa del Handshake TLS 1.2 (2 RTT) vs TLS 1.3 (1 RTT y 0-RTT Early Data).
* 📗 [2.5 Servicios Críticos de Infraestructura](Teoria/Modulo_2_Capa_Aplicacion/05_Transferencia_Archivos_Acceso_Remoto_Correo.md)
  * FTP Modo Activo vs Modo Pasivo (PASV) y el impacto del bloqueo por firewalls y NAT.
  * Criptografía en SSH (Diffie-Hellman, autenticación `known_hosts`, cifrado simétrico AES-GCM).
  * El trío de seguridad contra el fraude de correo: **SPF**, **DKIM** y **DMARC**.

---

### 🔌 [Módulo 3: Cableado Estructurado y Normativa ANSI/TIA/EIA](Teoria/Modulo_3_Cableado_Estructurado/)
* 📙 [3.1 Normativa y Estándares Internacionales](Teoria/Modulo_3_Cableado_Estructurado/01_Normativa_y_Estandares.md)
  * Estándares TIA/EIA 568, 569 (regla del 40% de llenado de canaletas), 606 (etiquetado) y 607 (tierras TMGB/TGB).
* 📙 [3.2 Los 6 Subsistemas del Cableado Estructurado](Teoria/Modulo_3_Cableado_Estructurado/02_Los_6_Subsistemas_Cableado.md)
  * EF, ER/MDF, Backbone vertical, TR/IDF, Cableado Horizontal (límite estricto: 90 m enlace permanente + 10 m patch cords = 100 m canal total) y Área de Trabajo.
* 📙 [3.3 Códigos de Color y Terminación T568A vs T568B](Teoria/Modulo_3_Cableado_Estructurado/03_Codigos_Color_Terminacion_T568A_T568B.md)
  * Código de colores de los 4 pares, pinout RJ-45, cable Directo vs Cruzado y Auto-MDIX.
* 📙 [3.4 Medios Físicos, Código de 12 Colores de Fibra y Balance Óptico](Teoria/Modulo_3_Cableado_Estructurado/04_Medios_Fisicos_Categorias_Fibra.md)
  * **Código de los 12 colores de la fibra óptica (Norma TIA-598-C):** Azul, Naranja, Verde, Marrón, Gris, Blanco, Rojo, Negro, Amarillo, Violeta, Rosa y Aqua. Identificación en tubos holgados.
  * Clasificación OM1 a OM5 en fibra multimodo y distancias máximas a 10 Gbps.
  * Fórmulas de balance de pérdidas ópticas (*Optical Power Budget*).
  * Fundamento electromagnético del paso de torsión (*Twist Rate*) en cobre UTP.
* 📙 [3.5 Caso Práctico Resuelto de Cálculo Métrico, Presupuesto y Certificación](Teoria/Modulo_3_Cableado_Estructurado/05_Calculos_Diseno_y_Certificacion.md)
  * Parámetros de certificación Fluke: NEXT, FEXT, Pérdida de Inserción, Pérdida de Retorno, Retardo de Propagación y **Sesgo de Retardo (*Delay Skew* $\le 45\text{ ns}$)**.
  * **Caso práctico resuelto de principio a fin:** Edificio de 3 niveles con 120 puntos de red:
    * Cálculo de distancia mínima, máxima y promedio con 10% de holgura.
    * Cálculo de bobinas de 305 m con redondeo superior.
    * Cálculo de cable vertical Cat 6A.
    * Dimensionamiento de unidades de rack (U) en gabinetes IDF y rack MDF.
    * **Presupuesto formal en Quetzales (GTQ)** con los precios de referencia del curso.

---

## 📱 Guía para Estudiar 100% Offline en Android

1. Entra a [github.com/ch4rf/redes-computadoras](https://github.com/ch4rf/redes-computadoras).
2. Pulsa en **Code** $\rightarrow$ **Download ZIP**.
3. Descomprime en tu teléfono y abre la carpeta con **Markor** (modo lectura limpio) u **Obsidian** (búsqueda instantánea de cualquier término).
