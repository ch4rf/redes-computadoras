# Módulo 3: Cableado Estructurado
## Tema 3.3: Códigos de Color y Terminación de Pines (T568A vs. T568B)

---

### 1. Los 4 Pares del Cable de Par Trenzado (UTP)
Un cable de red estándar de 4 pares (8 hilos de cobre) utiliza un código de colores universal con un hilo base de color sólido y un hilo blanco con franja de ese color:

* **Par 1:** Azul / Blanco-Azul
* **Par 2:** Naranja / Blanco-Naranja
* **Par 3:** Verde / Blanco-Verde
* **Par 4:** Marrón / Blanco-Marrón

---

### 2. Normas de Terminación: T568A vs. T568B (Pinout RJ-45)

La norma ANSI/TIA/EIA-568 define dos secuencias de conexión para el conector modular de 8 posiciones (RJ-45 macho) y los Jacks hembra (Keystone):

```text
       VISTA FRONTAL DEL CONECTOR RJ-45 (Pines 1 al 8 con el clip/lengüeta hacia abajo)
       [ 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 ]
```

| Pin | Norma T568A | Norma T568B (La más utilizada comercialmente) | Par Asociado |
| :---: | :--- | :--- | :---: |
| **1** | **Blanco / Verde** | **Blanco / Naranja** | Par 3 (en A) / Par 2 (en B) |
| **2** | **Verde** | **Naranja** | Par 3 (en A) / Par 2 (en B) |
| **3** | **Blanco / Naranja** | **Blanco / Verde** | Par 2 (en A) / Par 3 (en B) |
| **4** | **Azul** | **Azul** | Par 1 |
| **5** | **Blanco / Azul** | **Blanco / Azul** | Par 1 |
| **6** | **Naranja** | **Verde** | Par 2 (en A) / Par 3 (en B) |
| **7** | **Blanco / Marrón** | **Blanco / Marrón** | Par 4 |
| **8** | **Marrón** | **Marrón** | Par 4 |

#### Mnemotecnia de Examen:
Observa que **los pares 1 (Azul) y 4 (Marrón) nunca cambian de posición en los pines 4, 5, 7 y 8**.  
La **ÚNICA diferencia** entre T568A y T568B es que **se intercambian entre sí los pares Verde y Naranja (Pines 1 y 2 con Pines 3 y 6)**.

---

### 3. Tipos de Cables y Aplicación

#### A. Cable Directo (Straight-Through)
* **Construcción:** Ambos extremos tienen la **misma norma** (T568B en ambos lados, o T568A en ambos lados).
* **Función:** Conectar dispositivos que operan en **diferentes capas del modelo OSI**:
  * PC o Servidor (Capa 3/7) $\longleftrightarrow$ Switch (Capa 2).
  * Switch (Capa 2) $\longleftrightarrow$ Router (Capa 3).
  * Patch Panel $\longleftrightarrow$ Switch.

```text
Extremo 1 (T568B) -------------------- Extremo 2 (T568B)  ==> Cable Directo
```

#### B. Cable Cruzado (Cross-Over)
* **Construcción:** Un extremo está ponchado con la norma **T568A** y el otro extremo con la norma **T568B**.
* **Función:** Conectar directamente dispositivos que operan en la **misma capa del modelo OSI** (o dispositivos similares):
  * Switch (Capa 2) $\longleftrightarrow$ Switch (Capa 2) *(Cableado vertical entre pisos)*.
  * Router (Capa 3) $\longleftrightarrow$ Router (Capa 3).
  * PC (Capa 3/7) $\longleftrightarrow$ PC (Capa 3/7) *(Conexión directa entre computadoras)*.
  * PC (Capa 3/7) $\longleftrightarrow$ Router (Capa 3) *(Ambos transmiten por pines 1 y 2)*.

```text
Extremo 1 (T568A) -------------------- Extremo 2 (T568B)  ==> Cable Cruzado
```

#### C. Auto-MDIX (Automatic Medium-Dependent Interface Crossover)
En switches y tarjetas de red modernas de Cisco, los chips Ethernet detectan automáticamente el tipo de conexión y cruzan internamente los pares de transmisión (Tx) y recepción (Rx) si se usa un cable incorrecto.  
*Sin embargo, en exámenes teóricos y laboratorios tradicionales de Cisco Packet Tracer, debes respetar rigurosamente la regla del cable directo y cruzado.*

---

### 4. Preguntas Clave de Examen (Tipo UMG)

**P1: ¿Cuáles pares de colores se intercambian entre las normas T568A y T568B?**
> **R:** Se intercambian el **Par 2 (Naranja)** y el **Par 3 (Verde)**. Los pares Azul (Pines 4 y 5) y Marrón (Pines 7 y 8) se mantienen idénticos en ambas normas.

**P2: ¿Qué tipo de cable físico se requiere para conectar dos switches 2960 entre sí en Packet Tracer?**
> **R:** Un **Cable Cruzado (Copper Cross-Over)**.

**P3: ¿Qué tipo de cable se utiliza para conectar una PC a un puerto FastEthernet del Switch?**
> **R:** Un **Cable Directo (Copper Straight-Through)**.

**P4: Si conectas una PC directamente al puerto Gigabit0/0 de un Router sin pasar por ningún switch, ¿qué cable se debe emplear?**
> **R:** Un **Cable Cruzado (Copper Cross-Over)**, porque tanto la tarjeta de red de la PC como la interfaz del Router transmiten por los mismos pines (1 y 2).
