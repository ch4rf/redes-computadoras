# Módulo 3: Cableado Estructurado
## Tema 3.4: Medios Físicos de Red (Cobre UTP vs. Fibra Óptica Explicado Fácil)

---

### 📖 Mini Diccionario para Empezar

* **Atenuación:** La pérdida natural de fuerza que sufre una señal conforme viaja lejos por un cable (como cuando gritas: quien está a 5 metros te escucha fuerte, pero quien está a 150 metros no escucha nada).
* **Diafonía (Crosstalk):** El ruido o interferencia que un cable eléctrico le mete al cable que tiene pegado al lado.
* **Fibra Óptica:** Hilos de vidrio puro tan delgados como un cabello que transmiten pulsos de luz en lugar de electricidad.
* **Módulo SFP (Transceiver):** El cartucho metálico pequeño que se mete en una ranura especial del switch Cisco para poder enchufar el cable de fibra óptica.

---

### 1. Las Categorías de Cable de Cobre (UTP)

En las tiendas te venderán diferentes tipos de cable UTP. Para tu examen debes conocer estas tres:

| Categoría | Frecuencia | Velocidad Máxima a 100 Metros | ¿Para qué se usa en la vida real? |
| :---: | :---: | :---: | :--- |
| **Cat 5e** | 100 MHz | **1 Gbps** (1,000 Mbps) | Redes residenciales básicas o instalaciones viejas. |
| **Cat 6** | 250 MHz | **1 Gbps** a 100 metros<br/>*(10 Gbps hasta 37-55m)* | **El estándar actual de oficinas.** Trae una cruceta plástica en el centro para separar los pares y reducir el ruido. |
| **Cat 6A** | 500 MHz | **10 Gbps a los 100 metros completos** | **Cableado vertical entre pisos** y cuartos de servidores (es más grueso y pesado). |

---

### 2. Fibra Óptica: Monomodo vs. Multimodo (La Gran Analogía)

La fibra óptica transmite fotones de luz. Es inmune al ruido electromagnético, no le afectan los rayos y no conduce electricidad. Existen dos tipos:

```mermaid
flowchart TD
    subgraph SMF ["Fibra Monomodo (SMF) - Kilómetros de Distancia"]
        F1["Tubo de vidrio ultra delgado (9 µm)<br/>Fuente: LÁSER Infrarrojo Recto<br/>La luz viaja derecha por el centro sin rebotar<br/><b>Distancias: 10 km hasta más de 50 km</b><br/>Chaqueta estándar: <b>AMARILLA</b>"]
    end
    
    subgraph MMF ["Fibra Multimodo (MMF) - Para Adentro del Edificio"]
        F2["Tubo de vidrio más ancho (50 µm)<br/>Fuente: LED / VCSEL económico<br/>La luz va rebotando en zigzag en las paredes de vidrio<br/><b>Distancias: Hasta 300 - 550 metros</b><br/>Chaqueta estándar: <b>AQUA o NARANJA</b>"]
    end

    style SMF fill:#1e293b,stroke:#eab308,stroke-width:2px,color:#fff
    style MMF fill:#1e293b,stroke:#06b6d4,stroke-width:2px,color:#fff
```

#### Comparativa Sencilla:
* **Monomodo (Single-Mode - SMF):**
  * Imagina un puntero láser disparado a través de una pajilla ultra fina.
  * La luz viaja recta como una flecha por el centro, sin rebotar en las paredes.
  * **Alcanza distancias gigantescas:** Desde 10 km hasta más de 50 km (se usa para cruzar ciudades o conectar edificios muy lejanos en un campus universitario).
  * El cable casi siempre tiene la cubierta exterior de color **Amarillo**.
* **Multimodo (Multi-Mode - MMF):**
  * Imagina un tubo más ancho donde enciendes un foco LED.
  * La luz entra en varios ángulos y viaja **rebotando en zigzag** contra las paredes de vidrio.
  * Debido a los rebotes continuos, la luz se dispersa y pierde fuerza más rápido.
  * **Solo alcanza distancias cortas:** Hasta 300 o 550 metros. Es perfecta para conectar los pisos de un mismo edificio o cuartos técnicos cercanos.
  * El cable casi siempre tiene la cubierta exterior de color **Aqua (Turquesa)** o **Naranja**.

---

### 3. El Código de 12 Colores de Fibra Óptica (Norma TIA-598-C)

En un cable de fibra con 12 hilos de vidrio por dentro, como los hilos son transparentes y microscópicos, vienen pintados con 12 colores en un orden estandarizado:

```mermaid
flowchart LR
    C1["1. Azul"] --> C2["2. Naranja"] --> C3["3. Verde"] --> C4["4. Marrón"]
    C4 --> C5["5. Gris"] --> C6["6. Blanco"] --> C7["7. Rojo"] --> C8["8. Negro"]
    C8 --> C9["9. Amarillo"] --> C10["10. Violeta"] --> C11["11. Rosa"] --> C12["12. Aqua"]

    style C1 fill:#1d4ed8,color:#fff
    style C2 fill:#ea580c,color:#fff
    style C3 fill:#15803d,color:#fff
    style C4 fill:#78350f,color:#fff
    style C5 fill:#64748b,color:#fff
    style C6 fill:#f8fafc,color:#000
    style C7 fill:#b91c1c,color:#fff
    style C8 fill:#020617,color:#fff
    style C9 fill:#ca8a04,color:#000
    style C10 fill:#7e22ce,color:#fff
    style C11 fill:#db2777,color:#fff
    style C12 fill:#0d9488,color:#fff
```

* **Pregunta Típica de Examen:**
  * *¿De qué color es la fibra número 4?* $\rightarrow$ **Marrón**.
  * *¿De qué color es la fibra número 2?* $\rightarrow$ **Naranja**.
  * *¿De qué color es la fibra número 12?* $\rightarrow$ **Aqua**.

---

### 4. Preguntas Típicas de Examen

**P1: ¿Por qué la fibra óptica monomodo alcanza distancias mucho mayores que la multimodo?**
> **R:** Porque tiene un núcleo microscópico (9 µm) por el cual la luz láser viaja en línea recta en un solo haz, eliminando completamente la dispersión modal (los rebotes contra las paredes que limitan a la multimodo).

**P2: ¿Qué categoría de cable de cobre garantiza una velocidad de 10 Gbps a los 100 metros completos?**
> **R:** La **Categoría 6A (Cat 6A)** a 500 MHz.

**P3: ¿Qué color de chaqueta identifica comúnmente a un cable de fibra óptica monomodo?**
> **R:** El color **Amarillo**.
