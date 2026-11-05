# Módulo 1: Capa de Transporte
## Tema 1.4: Análisis Comparativo Avanzado y Banco Maestro de Examen

---

### 1. Gran Matriz de Ingeniería: TCP vs. UDP

| Dimensión Técnica | TCP (RFC 793) | UDP (RFC 768) |
| :--- | :--- | :--- |
| **Paradigma de Conexión** | Conexión con estado bidireccional (*Stateful*) | Sin estado (*Stateless* / Fire-and-forget) |
| **Estructura de Memoria en el Kernel** | Requiere un **TCB (Transmission Control Block)** por socket (~1 KB a 4 KB de RAM para búferes, temporizadores y variables de estado) | Sin TCB ni búferes de retransmisión. Huella en RAM prácticamente nula |
| **Impacto en Routers NAT / Firewalls** | Mantiene entradas en la tabla `conntrack` durante horas mientras la sesión esté `ESTABLISHED` | Las entradas NAT expiran agresivamente a los 30 a 120 segundos de inactividad |
| **Tamaño de Cabecera y Sobrecarga** | **20 a 60 bytes** (mínimo 160 bits) | **8 bytes** fijos (64 bits) |
| **Control de Flujo** | Dinámico por el receptor (*Window Size*) | Inexistente (la app debe tolerar desbordamiento) |
| **Control de Congestión de Red** | Algoritmos AIMD (*Slow Start*, Reno, Cubic, BBR) | Inexistente (inyecta al medio a la tasa del emisor) |
| **Primitiva de Entrega** | Flujo continuo de bytes (*Byte Stream*) sin fronteras de mensajes | Datagramas independientes con delimitación estricta de mensajes (*Preserves Message Boundaries*) |
| **Soporte de Topologías de Envío** | Únicamente **Unicast** (1 a 1) | **Unicast, Multicast y Broadcast** |

---

### 2. Análisis Matemático de Eficiencia y Sobrecarga de Protocolo (Overhead)

La eficiencia de transmisión ($\eta$) se define como el porcentaje de bytes de datos útiles de aplicación transportados respecto al tamaño total de la trama Ethernet:

$$\eta = \frac{\text{Bytes de Datos de Aplicación}}{\text{Bytes Totales en el Cable}} \times 100\%$$

#### Caso 1: Descarga Masiva de Archivo (Carga Máxima, MTU = 1500 B)
* Encabezado Ethernet (Capa 2) = 14 bytes + 4 bytes FCS = 18 bytes.
* Encabezado IPv4 (Capa 3) = 20 bytes.
* Carga de Capa 4:
  * **En TCP:** Encabezado = 20 bytes $\rightarrow$ Carga útil de aplicación = $1500 - 20 - 20 = \mathbf{1460\text{ bytes}}$.
    $$\eta_{\text{TCP}} = \frac{1460}{1460 + 20 + 20 + 18} = \frac{1460}{1518} = \mathbf{96.18\% \text{ de eficiencia}}$$
  * **En UDP:** Encabezado = 8 bytes $\rightarrow$ Carga útil de aplicación = $1500 - 20 - 8 = \mathbf{1472\text{ bytes}}$.
    $$\eta_{\text{UDP}} = \frac{1472}{1472 + 8 + 20 + 18} = \frac{1472}{1518} = \mathbf{96.97\% \text{ de eficiencia}}$$

#### Caso 2: Telefonía IP VoIP (Códec G.711, Paquetes Pequeños de 20 ms de Voz = 160 Bytes)
En aplicaciones interactivas, los paquetes son diminutos, lo que castiga brutalmente la eficiencia:
* Carga útil de voz = 160 bytes.
* **Sobrecarga de Cabeceras en UDP:**
  $$\text{Total} = 160\text{ (voz)} + 12\text{ (RTP)} + 8\text{ (UDP)} + 20\text{ (IP)} + 18\text{ (Ethernet)} = 218\text{ bytes}$$
  $$\eta_{\text{VoIP}} = \frac{160}{218} \times 100\% = \mathbf{73.39\%}$$
* *Si usáramos TCP para esa llamada:* La sobrecarga agregaría números de secuencia, acuses de recibo constantes y retransmisiones, duplicando el consumo de ancho de banda y arruinando la conversación con ecos de retardo.

---

### 3. Banco de Problemas Resueltos de Ingeniería (Nivel Examen UMG)

#### Problema 1: Cálculo Numérico de Secuencia y Reconocimiento
**Enunciado:**  
El Host A inicia una conexión TCP con el Host B. Durante el Three-Way Handshake, el Host A elige un ISN de `10,000` y el Host B elige un ISN de `20,000`. Tras establecerse la conexión, el Host A envía dos segmentos consecutivos con datos de usuario:
- El primer segmento contiene **400 bytes** de datos.
- El segundo segmento contiene **600 bytes** de datos.
Determina los valores exactos de `Seq` y `Ack` en:
1. El segmento SYN del Host A.
2. El segmento SYN-ACK del Host B.
3. El primer segmento de datos del Host A.
4. El acuse de recibo del Host B tras recibir el primer segmento.
5. El segundo segmento de datos del Host A.
6. El acuse de recibo final del Host B tras recibir ambos segmentos.

**Solución Paso a Paso:**
1. **SYN (Host A $\rightarrow$ Host B):**  
   `Seq = 10,000`, `Ack = 0` (el flag SYN consume 1 número de secuencia virtual).
2. **SYN-ACK (Host B $\rightarrow$ Host A):**  
   `Seq = 20,000`, `Ack = 10,001` (reconoce el SYN recibido: $10,000 + 1$).
3. **Primer Segmento de Datos (Host A $\rightarrow$ Host B):**  
   `Seq = 10,001` (inicia en el siguiente byte), transporta 400 bytes (bytes 10,001 al 10,400).
4. **ACK del Host B al primer segmento:**  
   `Ack = 10,401` (espera el byte $10,001 + 400$).
5. **Segundo Segmento de Datos (Host A $\rightarrow$ Host B):**  
   `Seq = 10,401`, transporta 600 bytes (bytes 10,401 al 11,000).
6. **ACK final del Host B:**  
   `Ack = 11,001` (espera el byte $10,401 + 600$).

---

#### Problema 2: Cálculo de BDP y Saturación de Enlace
**Enunciado:**  
Un enlace dedicado de fibra óptica entre el Campus Central y la sede de Portales de la UMG tiene un ancho de banda de **100 Mbps** y una latencia RTT de **40 ms**.  
a) Calcula el BDP en bits y en bytes.  
b) Si el emisor tiene una ventana TCP fija de 64 KB ($65,535\text{ bytes}$), ¿qué tasa máxima de transferencia real podrá alcanzar sobre ese enlace?

**Solución Paso a Paso:**
* **a) Cálculo del BDP:**
  $$BDP = 100,000,000\text{ bps} \times 0.040\text{ s} = 4,000,000\text{ bits}$$
  $$BDP = \frac{4,000,000}{8} = \mathbf{500,000 \text{ bytes}} \approx \mathbf{500 \text{ KB}}$$
* **b) Tasa de transferencia máxima limitada por la ventana:**
  $$\text{Throughput Máximo} \le \frac{\text{Window Size}}{RTT} = \frac{65,535 \times 8 \text{ bits}}{0.040\text{ s}} = \frac{524,280\text{ bits}}{0.040\text{ s}} = \mathbf{13.1\text{ Mbps}}$$
* *Conclusión:* A pesar de haber contratado un enlace de 100 Mbps, la conexión TCP estándar **solo alcanzará 13.1 Mbps** debido a que la ventana de 64 KB es insuficiente para llenar el "tubo" del BDP (500 KB). Para alcanzar los 100 Mbps completos, es obligatorio habilitar la opción de escala de ventana (*TCP Window Scale*).
