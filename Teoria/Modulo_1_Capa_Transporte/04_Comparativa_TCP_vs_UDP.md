# Módulo 1: Capa de Transporte
## Tema 1.4: Comparativa Exhaustiva TCP vs. UDP y Banco de Preguntas

---

### 1. Gran Matriz Comparativa (Directa para Examen)

| Característica / Parámetro | TCP (RFC 793) | UDP (RFC 768) |
| :--- | :--- | :--- |
| **Tipo de Conexión** | Orientado a conexión *(Connection-oriented)* | No orientado a conexión *(Connectionless)* |
| **Establecimiento de Sesión** | Sí, mediante **Three-Way Handshake** (SYN, SYN-ACK, ACK) | No hay negociación previa |
| **Confiabilidad** | **Confiable:** Garantiza la entrega mediante ACKs y retransmisión | **No confiable:** Mejor esfuerzo (*Best-effort*), sin retransmisión |
| **Orden de Entrega** | **Garantizado:** Usa números de secuencia para reordenar | **No garantizado:** Llegan en el orden en que la red los entregue |
| **Tamaño de Encabezado** | **20 a 60 bytes** (según opciones) | **8 bytes** (fijo) |
| **Sobrecarga (Overhead)** | Alta (cabecera grande y seguimiento de estados) | Mínima (muy ligero) |
| **Control de Flujo** | Sí, mediante **Ventana Deslizante (Window Size)** | No implementa |
| **Control de Congestión** | Sí (algoritmos AIMD, Slow Start, Congestion Avoidance) | No implementa |
| **Tipos de Comunicación** | Únicamente **Unicast** (punto a punto) | **Unicast, Multicast y Broadcast** |
| **Velocidad / Latencia** | Menor velocidad, mayor latencia por control | Máxima velocidad, mínima latencia |
| **PDU (Protocol Data Unit)** | **Segmento** | **Datagrama** |

---

### 2. Mapeo de Protocolos de Aplicación por Tipo de Transporte

| Protocolo de Aplicación | Puerto Estándar | Transporte Utilizado | ¿Por qué usa este protocolo de transporte? |
| :--- | :--- | :--- | :--- |
| **HTTP / HTTPS** | 80 / 443 | **TCP** | Una página web o imagen no tolera pérdida de código HTML o scripts. |
| **SSH / Telnet** | 22 / 23 | **TCP** | Las sesiones de comandos requieren que cada tecla digitada llegue íntegra. |
| **FTP** | 20 (Datos) / 21 (Control) | **TCP** | La descarga de un archivo binario/software se corrompería si faltan bytes. |
| **SMTP / POP3 / IMAP** | 25 / 110 / 143 | **TCP** | El correo electrónico no debe perder texto ni adjuntos. |
| **DNS (Consultas estándar)** | 53 | **UDP** | Rápido, un solo paquete de pregunta y uno de respuesta (< 512 bytes). |
| **DNS (Transferencia de zona)**| 53 | **TCP** | La sincronización completa entre servidores DNS supera 512 bytes y debe ser exacta. |
| **DHCP** | 67 (Servidor) / 68 (Cliente) | **UDP** | El cliente aún no tiene IP configurada para negociar un handshake TCP. |
| **TFTP** | 69 | **UDP** | Transferencia ligera en redes locales de switches/routers con memoria reducida. |
| **NTP** | 123 | **UDP** | La sincronización de hora requiere mínima latencia sin retrasos de ACKs. |
| **VoIP / Streaming (RTP)** | Dinámico / 5060 (SIP) | **UDP** | Prioriza la continuidad en tiempo real sobre la retransmisión de pérdidas. |

---

### 3. Banco de Preguntas de Práctica Teórica (Estilo Parcial UMG)

**1. ¿Qué campo del encabezado TCP evita que el emisor sature la memoria intermedia del receptor?**
- A) Sequence Number
- B) Checksum
- C) Window Size
- D) Urgent Pointer
> **Respuesta correcta:** **C) Window Size**. El receptor informa en este campo cuántos bytes está dispuesto a recibir antes de que el emisor deba detenerse a esperar un ACK.

---

**2. ¿Cuál es el tamaño de la cabecera UDP comparado con la cabecera base de TCP?**
- A) UDP tiene 20 bytes y TCP tiene 8 bytes.
- B) UDP tiene 8 bytes y TCP tiene 20 bytes.
- C) Ambos miden 16 bytes.
- D) UDP mide 4 bytes y TCP mide 32 bytes.
> **Respuesta correcta:** **B) UDP tiene 8 bytes y TCP tiene 20 bytes**.

---

**3. Durante el saludo de tres vías (Three-Way Handshake), ¿qué mensaje envía el servidor en respuesta al primer SYN del cliente?**
- A) ACK únicamente.
- B) FIN-ACK.
- C) SYN-ACK.
- D) RST.
> **Respuesta correcta:** **C) SYN-ACK**. El servidor responde con su propio SYN para sincronizar su número de secuencia y con un ACK confirmando el SYN del cliente.

---

**4. ¿Por qué una transmisión de voz sobre IP (VoIP) utiliza UDP en lugar de TCP?**
- A) Porque TCP no permite cifrar el audio.
- B) Porque UDP retransmite los paquetes más rápido.
- C) Porque el retardo generado por las retransmisiones de TCP hace inútil el audio en tiempo real.
- D) Porque UDP solo funciona en redes de fibra óptica.
> **Respuesta correcta:** **C**. En aplicaciones de tiempo real, si un paquete de audio se pierde, no tiene sentido retransmitirlo 300 ms después, ya que generaría ecos, saltos o desfase en la conversación.

---

**5. ¿Qué combinación forma un socket en una arquitectura de red?**
- A) Dirección MAC + Dirección IP.
- B) Dirección IP + Número de Puerto.
- C) Número de Puerto + Subred.
- D) Nombre de Dominio + Dirección MAC.
> **Respuesta correcta:** **B) Dirección IP + Número de Puerto** (ejemplo: `192.168.10.1:443`).
