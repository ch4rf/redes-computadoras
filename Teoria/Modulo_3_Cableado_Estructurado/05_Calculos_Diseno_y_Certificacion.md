# Módulo 3: Cableado Estructurado
## Tema 3.5: Caso Práctico Resuelto de Cálculo Métrico, Presupuesto y Certificación

---

### 1. Parámetros Eléctricos Avanzados de Certificación (Fluke Networks)

Para certificar que una instalación de cableado de cobre cumple la norma **ANSI/TIA-568.2-D** para enlaces de 1 Gbps o 10 Gbps, se evalúan parámetros físicos de alta frecuencia:

```text
       TRANSMISOR                                                          RECEPTOR
       [Par 1 (Tx)] === Señal Inyectada ===>                      ========> [Par 1 (Rx)]
            |                                                         |
     (NEXT medido aquí)                                        (FEXT medido aquí)
            v                                                         v
       [Par 2]      <--- Ruido Inducido ---                      <--------- [Par 2]
```

1. **NEXT (Near-End Crosstalk - Diafonía en el Extremo Cercano):** Ruido electromagnético que se filtra del par transmisor al par receptor en el mismo extremo donde se inyecta la señal. Se mide en decibelios (dB). **A mayor valor en dB, mejor es el aislamiento**.
2. **FEXT (Far-End Crosstalk - Diafonía en el Extremo Lejano):** Ruido inducido medido en el extremo opuesto del canal.
3. **Pérdida de Inserción / Atenuación (Insertion Loss):** Disminución de la amplitud de la señal conforme se propaga por el cable. A mayor frecuencia o temperatura, mayor pérdida. **A menor valor en dB, menor pérdida**.
4. **Pérdida de Retorno (Return Loss):** Medida de la energía reflejada de vuelta hacia la fuente debido a desajustes de impedancia característica ($100\ \Omega \pm 15\%$). Un destrenzado de más de 1.3 cm al montar el Jack arruina este parámetro.
5. **Retardo de Propagación (Propagation Delay):** Tiempo que tarda la señal en viajar de un extremo al otro (máximo permitido: **555 nanosegundos** a 100 metros).
6. **Sesgo de Retardo (Delay Skew):** La diferencia de tiempo de llegada entre el par más rápido y el más lento. Dado que cada par tiene un paso de trenzado distinto, los cables tienen longitudes físicas ligeramente distintas. **Límite máximo permitido: 45 nanosegundos**. Si el *Delay Skew* supera los 45 ns, los bits que viajan en paralelo por los 4 pares se desincronizan en la tarjeta Gigabit Ethernet.
7. **PS-ANEXT (Power Sum Alien Near-End Crosstalk):** Ruido que entra al cable proveniente de cables adyacentes empaquetados en la misma canaleta (fundamental en Cat 6A a 10 Gbps).

---

### 2. Caso Práctico Resuelto de Ingeniería (Modelo de Examen UMG)

#### Enunciado del Problema:
Se solicita diseñar el sistema de cableado estructurado para el nuevo edificio administrativo de 3 niveles de una sede universitaria.
* **Dimensiones de cada piso:** 30 metros de largo por 20 metros de ancho.
* **Altura de entrepiso:** 4.0 metros libres por piso.
* **Distribución de tomas:** 
  * Piso 1: 24 tomas dobles de red (48 puertos).
  * Piso 2 (MDF Central con Servidores y Router): 16 tomas dobles (32 puertos).
  * Piso 3: 20 tomas dobles (40 puertos).
  * **Total de tomas de pared (Faceplates):** 60 tomas. **Total de cables/puntos:** 120 puntos de red.
* **Ubicación de racks:** Racks centrados en el plano de cada piso.

---

#### Resolución Paso a Paso:

#### Paso 1: Cálculos Geométricos de Cable Horizontal por Piso
* **Distancia mínima asumida ($D_{\text{mín}}$):** $5.0\text{ m}$ (puestos contiguos al cuarto técnico).
* **Distancia máxima con rack céntrico ($D_{\text{máx}}$):**
  $$D_{\text{máx}} = \frac{\text{Largo}}{2} + \frac{\text{Ancho}}{2} + \text{Altura de bajada} = \frac{30}{2} + \frac{20}{2} + 4.0 = 15 + 10 + 4 = \mathbf{29.0 \text{ metros}}$$
  *(Cumple con la norma TIA-568, ya que $29\text{ m} \le 90\text{ m}$ de enlace permanente).*
* **Distancia promedio ($D_{\text{prom}}$):**
  $$D_{\text{prom}} = \frac{D_{\text{mín}} + D_{\text{máx}}}{2} = \frac{5.0 + 29.0}{2} = \mathbf{17.0 \text{ metros}}$$
* **Distancia promedio con 10% de holgura ($D_{\text{holgura}}$):**
  $$D_{\text{holgura}} = 17.0 \times 1.10 = \mathbf{18.70 \text{ metros por punto}}$$

#### Paso 2: Metraje Total de Cable UTP Horizontal y Bobinas
* **Piso 1 (48 puntos):** $48 \times 18.70\text{ m} = 897.60\text{ m}$
* **Piso 2 (32 puntos):** $32 \times 18.70\text{ m} = 598.40\text{ m}$
* **Piso 3 (40 puntos):** $40 \times 18.70\text{ m} = 748.00\text{ m}$
* **Total Cable Horizontal Edificio:**
  $$\text{Total Metros} = 897.60 + 598.40 + 748.00 = \mathbf{2,244.00 \text{ metros}}$$
* **Cantidad de Bobinas UTP Cat 6 (305 metros por caja):**
  $$\text{Cajas} = \left\lceil \frac{2,244.00}{305} \right\rceil = \lceil 7.35 \rceil = \mathbf{8 \text{ Bobinas de 305 m}}$$

#### Paso 3: Cableado Vertical (Backbone entre Pisos con Cat 6A)
* El **MDF** está en el **Piso 2**.
  * Enlace IDF Piso 1 hacia MDF Piso 2: $(1\text{ piso} \times 4\text{ m}) + 6\text{ m de reserva} = 10\text{ metros}$.
  * Enlace IDF Piso 3 hacia MDF Piso 2: $(1\text{ piso} \times 4\text{ m}) + 6\text{ m de reserva} = 10\text{ metros}$.
  * *Tirada doble por redundancia:* $(10\text{ m} \times 2) + (10\text{ m} \times 2) = \mathbf{40 \text{ metros de cable UTP Cat 6A}}$.

#### Paso 4: Dimensionamiento de Gabinetes y Racks (Unidades de Rack - U)
* **IDF Piso 1 (48 puntos):**
  * 2 Patch Panels Cat 6 de 24 puertos (2U)
  * 2 Organizadores horizontales de cable (2U)
  * 2 Switches de 24 puertos (2U)
  * 1 PDU regleta de energía (1U)
  * **Total Requerido:** 7U $\rightarrow$ Se instala un **Gabinete de pared de 9U o 12U**.
* **MDF Piso 2 (Centro de Cómputo Principal):**
  * Se instala un **Rack abierto de piso de 19" y 42U** para alojar Switches Core, Router Empresarial, Servidores y Patch Panels.

---

### 3. Presupuesto Formal en Quetzales de Guatemala (GTQ)

A continuación se presenta la tabla con los precios de referencia estandarizados para evaluación:

| Subsistema | Descripción del Material / Equipo | Cant | Unidad | Precio Unitario (GTQ) | Subtotal (GTQ) |
| :--- | :--- | :---: | :---: | :---: | :---: |
| **Área de Trabajo** | Faceplate superficial 2 puertos con caja | 60 | Pieza | Q 18.00 | Q 1,080.00 |
| **Área de Trabajo** | Jack RJ-45 Keystone Categoría 6 | 120 | Pieza | Q 25.00 | Q 3,000.00 |
| **Área de Trabajo** | Patch Cord UTP Cat 6 flexible de 3 metros | 120 | Pieza | Q 35.00 | Q 4,200.00 |
| **Cableado Horizontal**| Bobina de Cable UTP Cat 6 (Caja 305m) | 8 | Caja | Q 1,150.00 | Q 9,200.00 |
| **Canalización** | Canaleta plástica adhesiva 20*12 mm (Tramo 2m) | 180 | Tramo | Q 18.00 | Q 3,240.00 |
| **Cableado Vertical** | Cable UTP Categoría 6A para Backbone | 40 | Metro | Q 12.00 | Q 480.00 |
| **Cuarto Telecom (IDF)**| Gabinete de pared de telecomunicaciones 9U | 2 | Pieza | Q 950.00 | Q 1,900.00 |
| **Cuarto Equipos (MDF)**| Rack abierto de piso de 19" (42U) | 1 | Pieza | Q 2,200.00 | Q 2,200.00 |
| **Cuartos Técnicos** | Patch Panel modular 24 puertos Cat 6 | 6 | Pieza | Q 450.00 | Q 2,700.00 |
| **Cuartos Técnicos** | Patch Cord UTP Cat 6 de 1 metro (Gabinete) | 120 | Pieza | Q 22.00 | Q 2,640.00 |
| **Cuartos Técnicos** | Regleta de alimentación para rack (PDU) | 3 | Pieza | Q 280.00 | Q 840.00 |
| **Equipamiento Activo** | Switch Administrable de 24 puertos Gigabit | 5 | Pieza | Q 2,800.00 | Q 14,000.00 |
| **Equipamiento Activo** | Router Empresarial Gigabit Multi-Interfaz | 1 | Pieza | Q 1,950.00 | Q 1,950.00 |
| **Administración** | Kit de etiquetas térmicas TIA-606 | 1 | Kit | Q 350.00 | Q 350.00 |
| **TOTAL GENERAL** | **Presupuesto Total de Infraestructura de Red** | | | | **Q 47,780.00** |
