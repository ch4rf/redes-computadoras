# 📚 Redes de Computadoras - Material de Estudio Completo para Examen Parcial (UMG)

Guía teórica y práctica de estudio desarrollada para el examen de Redes de Computadoras en la Universidad Mariano Gálvez de Guatemala.

---

## 📑 Índice General de Contenidos

### 🌐 [Módulo 1: Capa de Transporte (Capa 4 OSI)](Teoria/Modulo_1_Capa_Transporte/)
* 📘 [1.1 Fundamentos de la Capa de Transporte](Teoria/Modulo_1_Capa_Transporte/01_Fundamentos_Capa_Transporte.md)
  * Entrega Process-to-Process vs Host-to-Host.
  * Concepto de Socket y Socket Pair.
  * Clasificación de puertos IANA: Bien conocidos (0-1023), Registrados (1024-49151) y Dinámicos/Efímeros (49152-65535).
  * Mecanismo de multiplexación y demultiplexación.
* 📘 [1.2 Protocolo TCP (RFC 793)](Teoria/Modulo_1_Capa_Transporte/02_Protocolo_TCP.md)
  * Características fundamentales y anatomía del encabezado (20 a 60 bytes).
  * Three-Way Handshake (SYN, SYN-ACK, ACK) y Four-Way Teardown (FIN, ACK, FIN, ACK).
  * Banderas de control (`URG`, `ACK`, `PSH`, `RST`, `SYN`, `FIN`).
  * Mecanismos de confiabilidad (Seq/Ack) y Ventana Deslizante (*Window Size* para control de flujo).
* 📘 [1.3 Protocolo UDP (RFC 768)](Teoria/Modulo_1_Capa_Transporte/03_Protocolo_UDP.md)
  * Características, entrega de mejor esfuerzo (*Best-effort*) y cabecera inmutable de 8 bytes.
  * Ventajas de latencia y velocidad sobre confiabilidad.
  * Protocolos dependientes de UDP (DNS, DHCP, TFTP, NTP, VoIP/RTP).
* 📘 [1.4 Comparativa TCP vs. UDP y Banco de Preguntas](Teoria/Modulo_1_Capa_Transporte/04_Comparativa_TCP_vs_UDP.md)
  * Gran matriz comparativa multidimensional para examen.
  * Mapeo de protocolos de Capa de Aplicación según su transporte.
  * Banco de preguntas tipo examen UMG resueltas.

---

### 🖥️ [Módulo 2: Capa de Aplicación (Capa 7 OSI / Capa de Aplicación TCP/IP)](Teoria/Modulo_2_Capa_Aplicacion/)
* 📗 [2.1 Modelos de Red y Servicios](Teoria/Modulo_2_Capa_Aplicacion/01_Modelos_y_Servicios_Red.md)
  * Diferencia entre aplicación de usuario y protocolo de capa 7.
  * Modelo Cliente-Servidor (*Client-Server*): características, ventajas y desventajas.
  * Modelo Punto a Punto (*Peer-to-Peer* / P2P): redes puras vs aplicaciones P2P.
* 📗 [2.2 DNS (Domain Name System - RFC 1034 / 1035)](Teoria/Modulo_2_Capa_Aplicacion/02_DNS_Domain_Name_System.md)
  * Puerto 53 (UDP para consultas < 512 bytes, TCP para transferencias de zona).
  * Estructura jerárquica: Raíz (.), TLDs (.gt, .com), SLDs y FQDNs.
  * Servidores Resolver, Raíz, TLD y Autoritativos.
  * Consultas Recursivas vs Iterativas.
  * Registros obligatorios de examen: `A`, `AAAA`, `CNAME`, `MX`, `NS`, `PTR`, `SOA`, `TXT`.
  * Memoria caché local y TTL (*Time To Live*).
* 📗 [2.3 DHCP (Dynamic Host Configuration Protocol - RFC 2131)](Teoria/Modulo_2_Capa_Aplicacion/03_DHCP_Dynamic_Host_Configuration.md)
  * Puertos UDP: 67 (Servidor) y 68 (Cliente).
  * El Proceso **DORA** a fondo: **D**iscover, **O**ffer, **R**equest, **A**cknowledge.
  * Temporizadores de renovación: T1 (50% Unicast) y T2 (87.5% Broadcast).
  * Parámetros DHCP (Opciones 1, 3, 6, 15, 51).
  * DHCP Relay Agent (`ip helper-address`) en routers para cruzar subredes.
  * Rango de autoasignación APIPA (`169.254.x.x /16`).
* 📗 [2.4 Servicios Web (HTTP y HTTPS)](Teoria/Modulo_2_Capa_Aplicacion/04_Servicios_Web_HTTP_HTTPS.md)
  * HTTP (Puerto 80 TCP) vs HTTPS (Puerto 443 TCP con cifrado TLS/SSL).
  * Estructura de Request y Response.
  * Métodos HTTP: `GET`, `POST`, `PUT`, `DELETE`, `HEAD`.
  * Códigos de estado HTTP: 200, 201, 301, 304, 400, 401, 403, 404, 500, 502, 503.
  * Cifrado Asimétrico (Handshake) vs Simétrico (Sesión).
* 📗 [2.5 Transferencia de Archivos, Acceso Remoto y Correo Electrónico](Teoria/Modulo_2_Capa_Aplicacion/05_Transferencia_Archivos_Acceso_Remoto_Correo.md)
  * FTP (Puertos 20 Datos / 21 Control TCP) - Modo activo vs pasivo.
  * TFTP (Puerto 69 UDP) y su uso en routers/switches Cisco.
  * Telnet (Puerto 23 TCP - texto plano) vs SSH (Puerto 22 TCP - terminal cifrada).
  * Correo Electrónico: Envío con SMTP (Puertos 25/587 TCP) y recepción con POP3 (110 TCP) vs IMAP (143 TCP).

---

### 🔌 [Módulo 3: Cableado Estructurado y Normativa ANSI/TIA/EIA](Teoria/Modulo_3_Cableado_Estructurado/)
* 📙 [3.1 Normativa y Estándares Internacionales](Teoria/Modulo_3_Cableado_Estructurado/01_Normativa_y_Estandares.md)
  * Definición y propósitos del cableado estructurado.
  * Organismos: ANSI, TIA, EIA, ISO/IEC, IEEE.
  * Las 4 normas de examen:
    * **ANSI/TIA/EIA-568:** Requisitos generales y topología en estrella.
    * **ANSI/TIA/EIA-569:** Rutas y espacios (regla del 40% de ocupación en canaletas/ductos).
    * **ANSI/TIA/EIA-606:** Administración y etiquetado de infraestructura.
    * **ANSI/TIA/EIA-607:** Aterrizaje y puesta a tierra (barras TMGB y TGB).
* 📙 [3.2 Los 6 Subsistemas del Cableado Estructurado](Teoria/Modulo_3_Cableado_Estructurado/02_Los_6_Subsistemas_Cableado.md)
  1. Instalaciones de Entrada (EF - Entrance Facility) y punto de demarcación.
  2. Cuarto de Equipos (ER / MDF - Centro de Datos / Servidores).
  3. Cableado Vertical / Backbone (Enlaces entre pisos o edificios).
  4. Cuarto de Telecomunicaciones (TR / IDF - Racks de piso).
  5. Cableado Horizontal: Regla estricta de 90 m (enlace permanente) + 10 m (patch cords) = 100 m (canal).
  6. Área de Trabajo (WA - Work Area): tomas de pared, faceplates, jacks RJ45 y patch cords de usuario.
* 📙 [3.3 Códigos de Color y Terminación de Pines (T568A vs T568B)](Teoria/Modulo_3_Cableado_Estructurado/03_Codigos_Color_Terminacion_T568A_T568B.md)
  * Código de colores de los 4 pares (Azul, Naranja, Verde, Marrón).
  * Secuencia de pines 1 al 8 para T568A y T568B.
  * Cable Directo (*Straight-Through*) vs Cable Cruzado (*Cross-Over*) y aplicaciones.
  * Auto-MDIX.
* 📙 [3.4 Medios Físicos de Transmisión (Par Trenzado y Fibra Óptica)](Teoria/Modulo_3_Cableado_Estructurado/04_Medios_Fisicos_Categorias_Fibra.md)
  * Tipos de apantallamiento: UTP, FTP, S/FTP.
  * Categorías: Cat 5e (100 MHz / 1 Gbps), Cat 6 (250 MHz / 1 Gbps), Cat 6A (500 MHz / 10 Gbps a 100 m).
  * Fibra Óptica Monomodo (SMF - 9 µm / Láser / Larga distancia / Amarillo).
  * Fibra Óptica Multimodo (MMF - 50 µm / LED-VCSEL / Distancias cortas / Aqua-Naranja).
  * Conectores LC, SC, ST y transceivers SFP.
* 📙 [3.5 Cálculos de Diseño y Certificación de Enlaces](Teoria/Modulo_3_Cableado_Estructurado/05_Calculos_Diseno_y_Certificacion.md)
  * Fórmulas geométricas para el cálculo métrico de cable horizontal, vertical y de campus.
  * Factor de holgura del 10% y cálculo de bobinas de 305 m.
  * Parámetros de certificación: Wire Map (Open, Short, Reversed, Transposed, Split Pairs), Atenuación, NEXT, FEXT, PS-NEXT, Return Loss.

---

## 📱 Cómo leer este material en tu celular Android sin conexión

1. **Opción A (Recomendada con App Markor - 100% Offline):**
   * Descarga la aplicación gratuita **Markor** desde Google Play Store.
   * En tu teléfono entra a este repositorio: `https://github.com/ch4rf/redes-computadoras`.
   * Pulsa en el botón verde **Code** $\rightarrow$ **Download ZIP**.
   * Descomprime el archivo ZIP en tu almacenamiento interno y abre la carpeta con Markor para disfrutar de lectura nocturna fluida con tablas formateadas y diagramas.
2. **Opción B (Obsidian para Android):**
   * Descarga **Obsidian** y abre la carpeta descargada como "Vault". Tendrás buscador instantáneo de conceptos para repasar minutos antes del examen.
