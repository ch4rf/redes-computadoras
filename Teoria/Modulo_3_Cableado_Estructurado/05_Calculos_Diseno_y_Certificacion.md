# Módulo 3: Cableado Estructurado
## Tema 3.5: Metodología de Cálculos de Diseño y Certificación de Enlaces

---

### 1. Metodología de Cálculo Métrico de Materiales (Fórmulas para Examen Práctico UMG)

En el examen práctico de la UMG, deberás calcular con precisión matemática la cantidad exacta de cable UTP, bobinas y accesorios requeridos para el plano del edificio. A continuación se presentan las fórmulas estandarizadas:

#### A. Cálculo del Cableado Horizontal (Por Piso o Área)
1. **Distancia más cercana ($D_{\text{mín}}$):** Distancia del puesto de trabajo más próximo al rack (se asume típicamente entre **5 m y 6 m**).
2. **Distancia más lejana ($D_{\text{máx}}$):**
   * *Si el rack está en una esquina del plano:*
     $$D_{\text{máx}} = \text{Largo del plano} + \text{Ancho del plano} + \text{Altura de bajada (4 m)}$$
   * *Si el rack está ubicado al centro del plano:*
     $$D_{\text{máx}} = \frac{\text{Largo}}{2} + \frac{\text{Ancho}}{2} + \text{Altura de bajada (4 m)}$$
3. **Distancia Promedio ($D_{\text{prom}}$):**
   $$D_{\text{prom}} = \frac{D_{\text{mín}} + D_{\text{máx}}}{2}$$
4. **Distancia Promedio con Holgura del 10% ($D_{\text{holgura}}$):**
   *(El 10% compensa curvas, bajadas en canaleta, esquinas y peinado dentro del rack).*
   $$D_{\text{holgura}} = D_{\text{prom}} \times 1.10$$
5. **Total de Cable Horizontal por Piso:**
   $$\text{Total Metros} = D_{\text{holgura}} \times \text{Número de Tomas (Wall Mounts)}$$
6. **Cantidad de Bobinas UTP Cat 6 (Cajas de 305 metros):**
   $$\text{Bobinas} = \left\lceil \frac{\text{Total Metros}}{305} \right\rceil \quad \text{(Siempre se redondea al entero superior más cercano)}$$

#### B. Cálculo del Cableado Vertical (Backbone entre Pisos)
* Por cada enlace entre un IDF y el MDF:
  $$\text{Metros Verticales} = (\text{Número de pisos de separación} \times 4\text{ m de entrepiso}) + 6\text{ m de holgura en racks (3 m en cada rack)}$$

#### C. Cálculo del Cableado Inter-Edificios (Exterior)
* Enlace de fibra óptica o cobre subterráneo entre dos edificios:
  $$\text{Metros Exteriores} = \text{Distancia medida en croquis} + 20\text{ m de holgura (10 m de reserva en el rack de cada edificio)}$$

---

### 2. Parámetros Eléctricos de Prueba y Certificación (Fluke Tester)

Para certificar que una instalación cumple con la norma ANSI/TIA/EIA-568 y opera a 1 Gbps o 10 Gbps, se utiliza un certificador de campo (como un Fluke DSX CableAnalyzer). Los parámetros obligatorios de evaluación son:

#### A. Mapa de Cableado (Wire Map)
Verifica la continuidad física pin a pin de los 8 hilos del cable. Detecta 5 fallas típicas de ponchado:
1. **Hilo Abierto (*Open*):** Un hilo roto o mal ponchado que no hace contacto.
2. **Cortocircuito (*Short*):** Dos o más hilos que hacen contacto eléctrico entre sí.
3. **Pares Invertidos (*Reversed Pair*):** Los dos hilos de un mismo par están al revés (ej. pin 1 en pin 2 y pin 2 en pin 1).
4. **Pares Transpuestos / Cruzados (*Transposed / Crossed Pair*):** Un par completo se ponchó en los pines de otro par (ej. Par verde en lugar del Par naranja).
5. **Pares Divididos (*Split Pairs*):** ¡El error más engañoso de examen! Hay continuidad eléctrica en todos los pines (el tester barato enciende luces 1-2-3-4-5-6-7-8), pero los hilos se mezclaron entre pares distintos rompiendo el trenzado, lo que destruye la cancelación de ruido electromagnético.

#### B. Pérdida de Inserción / Atenuación (Insertion Loss)
* La pérdida de potencia de la señal eléctrica conforme viaja a lo largo del cable. Se mide en decibelios (**dB**). A mayor distancia o mayor temperatura ambiente, mayor atenuación. Si el cable supera los 90 metros, la atenuación reprueba la certificación.

#### C. Diafonía (Crosstalk)
El acoplamiento electromagnético no deseado de una señal de un par sobre los otros pares adyacentes:
* **NEXT (Near-End Crosstalk - Diafonía en el extremo cercano):** Se mide en el mismo extremo donde el transmisor emite la señal. A mayor valor en dB, mejor (indica mayor aislamiento).
* **FEXT (Far-End Crosstalk - Diafonía en el extremo lejano):** Se mide en el extremo receptor opuesto del cable.
* **PS-NEXT (Power Sum NEXT):** Cálculo matemático que suma el ruido generado por tres pares sobre el cuarto par simultáneamente (vital para Gigabit Ethernet 1000BASE-T, que transmite por los 4 pares a la vez).

#### D. Pérdida de Retorno (Return Loss)
* Medida de la cantidad de energía de la señal que se refleja de vuelta hacia el transmisor debido a variaciones de impedancia (desajuste de 100 ohmios causado por dobleces bruscos, aplastamiento de cables o destrenzado excesivo al ponchar el conector RJ-45).
* **Regla estricta de ponchado:** Al terminar un conector o Jack RJ-45, **no se debe destrenzar el cable más de 1.3 cm (0.5 pulgadas)** para evitar diafonía y pérdida de retorno.

---

### 3. Preguntas Clave de Examen (Tipo UMG)

**P1: ¿Cuál es el destrenzado máximo permitido en un cable UTP Cat 6 al momento de ponchar un Jack o conector RJ-45?**
> **R:** Máximo **1.3 cm (0.5 pulgadas)** de destrenzado.

**P2: ¿Qué falla de cableado permite que un multímetro reporte continuidad correcta en los 8 pines pero hace que la red falle o tenga velocidad pésima?**
> **R:** La falla de **Pares Divididos (Split Pairs)**.

**P3: ¿Por qué en el cálculo de cable horizontal se añade un 10% de holgura a la distancia promedio?**
> **R:** Para compensar curvas en esquinas, subidas y bajadas por canaletas de pared, desvíos estructurales y la reserva necesaria para peinado dentro del rack del cuarto de telecomunicaciones.
