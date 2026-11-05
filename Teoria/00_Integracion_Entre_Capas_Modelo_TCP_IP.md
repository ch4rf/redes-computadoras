# Capítulo Maestro: Integración y Relación Entre Capas (Modelo OSI y TCP/IP)

---

### 1. El Mapa Completo de Encapsulamiento y Desencapsulamiento
Una de las fallas más comunes en exámenes de redes es estudiar las capas como islas desconectadas. En la realidad de la ingeniería, **ninguna capa puede operar sin los servicios y límites de las capas inferiores**.

A continuación se muestra cómo se empaqueta la información desde el clic del usuario hasta los electrones o fotones en el cable:

```text
+----------------------------------------------------------------------------------------------------+
| CAPA 7: APLICACIÓN (HTTP, DNS, DHCP, SSH)                                                          |
| Datos Puros de la Aplicación (ej. Mensaje HTTP GET /index.html)                                     |
+----------------------------------------------------------------------------------------------------+
                                      |
                                      v Encapsulación en Capa de Transporte (agrega Puerto Origen y Destino)
+-------------------------------------+--------------------------------------------------------------+
| Encabezado TCP (20 bytes)           | DATOS DE APLICACIÓN (Carga Útil / Payload)                  |
+-------------------------------------+--------------------------------------------------------------+
| <----------------------------- SEGMENTO TCP (Unidad de Capa 4) ----------------------------------> |
                                      |
                                      v Encapsulación en Capa de Red (agrega IP Origen y Destino)
+--------------------------+----------+--------------------------------------------------------------+
| Encabezado IP (20 bytes) | TCP Hdr  | DATOS DE APLICACIÓN                                          |
+--------------------------+----------+--------------------------------------------------------------+
| <------------------------------- PAQUETE IP (Unidad de Capa 3) ----------------------------------> |
                                      |
                                      v Encapsulación en Capa de Enlace (agrega MACs y Checksum CRC)
+-------------------+----------+----------+----------------------------------------------+-----------+
| Cabecera Ethernet | IP Hdr   | TCP Hdr  | DATOS DE APLICACIÓN                          | Cola FCS  |
| (14 bytes)        |          |          |                                              | (4 bytes) |
+-------------------+----------+----------+----------------------------------------------+-----------+
| <-------------------------------- TRAMA ETHERNET (Capa 2) ---------------------------------------> |
                                      |
                                      v Codificación en Capa Física (Voltajes o Pulsos de Luz)
+----------------------------------------------------------------------------------------------------+
| 01101001 01110100 01100101 01101101 (Bits / Señales en Cobre UTP o Fibra Óptica - Capa 1)         |
+----------------------------------------------------------------------------------------------------+
```

---

### 2. La Cadena de Restricciones Métricas: Del Cable a la Aplicación
¿Por qué un archivo descargado por Internet viaja típicamente en bloques de **1,460 bytes**? La respuesta demuestra la interconexión directa entre Cableado, Enlace, Red y Transporte:

```mermaid
graph TD
    L1["Capa 1: Cable UTP Cat 6 / Fibra<br/>Impone estándar Ethernet IEEE 802.3"] --> L2["Capa 2: MTU Máxima de Ethernet = 1500 Bytes<br/>Tamaño máximo de carga útil que la tarjeta de red puede transmitir"]
    L2 --> L3["Capa 3: Encabezado IPv4 = 20 Bytes fijos<br/>Espacio restante para datos: 1500 - 20 = 1480 Bytes"]
    L3 --> L4["Capa 4: Encabezado TCP = 20 Bytes fijos<br/>MSS (Maximum Segment Size) = 1480 - 20 = 1460 Bytes"]
    L4 --> L7["Capa 7: La aplicación web o transferencia de archivo solo puede<br/>inyectar hasta 1460 Bytes netos por segmento sin fragmentar"]
```

#### Fórmulas de Relación Inter-Capa:
$$\text{MTU (Maximum Transmission Unit de Capa 2)} = 1500 \text{ bytes}$$
$$\text{Tamaño del Paquete IP} = \text{Encabezado IP (20 B)} + \text{Carga Útil IP}$$
$$\text{MSS (Maximum Segment Size de TCP)} = \text{MTU} - \text{Encabezado IP (20 B)} - \text{Encabezado TCP (20 B)}$$
$$\mathbf{MSS = 1500 - 20 - 20 = 1460 \text{ bytes}}$$

* **Impacto práctico:** Si un switch o router intermedio tiene una MTU reducida o túneles VPN (que añaden encabezados GRE o IPsec), y el paquete supera el límite con la bandera **DF (Don't Fragment)** activa, el router descarta el paquete y devuelve un mensaje ICMP tipo 3 código 4 (*Fragmentation Needed*). Este fenómeno se conoce como **Path MTU Discovery (PMTUD)**.

---

### 3. Anatomía de una Transacción Completa: El Viaje de un Clic

Imagina que estás en una PC de la UMG, conectada mediante un Jack RJ-45 de pared ponchado con norma T568B, abres el navegador y escribes:
$$\text{https://portal.umg.edu.gt}$$

A continuación se detalla la secuencia exacta segundo a segundo que involucra **absolutamente todos los temas de tus 3 módulos**:

```mermaid
sequenceDiagram
    autonumber
    actor User as Usuario / PC
    participant Cable as Capa Física (UTP / Patch Panel / Switch)
    participant DHCP as Servidor DHCP
    participant DNS as Servidor DNS
    participant Router as Gateway / Router
    participant Server as Servidor Web Seguro

    Note over User,Cable: FASE 0: FÍSICA Y ARRANQUE (Módulo 3 y 2)
    User->>Cable: Señal eléctrica por par trenzado (Pines 1 y 2 Tx en T568B)
    User->>DHCP: DORA: DHCP Discover (Broadcast UDP 67/68)
    DHCP->>User: DHCP ACK: IP 192.168.10.50, Mask /24, GW 192.168.10.1, DNS 8.8.8.8

    Note over User,DNS: FASE 1: RESOLUCIÓN DE NOMBRES (Módulo 2 y 1)
    User->>DNS: Petición DNS (Registro A para portal.umg.edu.gt) vía UDP Puerto 53
    DNS->>User: Respuesta DNS: IP = 200.10.15.42

    Note over User,Server: FASE 2: TRANSPORTE Y CONEXIÓN TCP (Módulo 1)
    User->>Server: 1. TCP SYN (Seq=1000) hacia Puerto 443 TCP
    Server->>User: 2. TCP SYN-ACK (Seq=5000, Ack=1001)
    User->>Server: 3. TCP ACK (Seq=1001, Ack=5001) -> Estado: ESTABLISHED

    Note over User,Server: FASE 3: SEGURIDAD TLS Y APLICACIÓN (Módulo 2)
    User->>Server: TLS ClientHello (Negociación de cifrado)
    Server->>User: TLS ServerHello + Certificado Digital X.509
    Note over User,Server: Negocian clave de sesión simétrica AES
    User->>Server: HTTP Request cifrado: GET / HTTP/1.1 (Host: portal.umg.edu.gt)
    Server->>User: HTTP Response cifrado: 200 OK + Código HTML del portal
```

---

### 4. Matriz de Dependencias Cruzadas entre Protocolos

| Si falla este componente... | ¿Qué síntoma exacto se observa en la red? | ¿En qué capa se originó el error? |
| :--- | :--- | :--- |
| **Jack RJ-45 con par abierto en pin 3** | La tarjeta de red no sincroniza enlace (*Link LED apagado*) o negocia solo a 10 Mbps en modo semidúplex. | **Capa 1 (Física / Cableado Estructurado)** |
| **Canal horizontal con longitud de 115 metros** | Paquetes descartados por CRC erróneo debido a atenuación excesiva y colisiones tardías (*Late Collisions*). | **Capa 1 / Capa 2** |
| **DHCP saturado o servidor caído** | La PC se autoasigna una IP APIPA `169.254.x.x`. No hay salida a Internet. | **Capa 7 (Aplicación DHCP)** |
| **Servidor DNS mal configurado o puerto 53 bloqueado** | Puedes hacer ping a IPs públicas (`ping 8.8.8.8` funciona), pero el navegador dice *"No se puede encontrar la dirección IP del servidor"*. | **Capa 7 (DNS) sobre Capa 4 (UDP 53)** |
| **Firewall bloqueando el puerto 443** | El ping a la IP del servidor funciona, el DNS resuelve la IP, pero la página web nunca carga y el navegador se queda en espera infinita (*Connection Timed Out* tras reintentos SYN). | **Capa 4 (Transporte TCP)** |
