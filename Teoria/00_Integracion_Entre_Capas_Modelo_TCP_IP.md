# Capítulo Maestro: Integración y Relación Entre Capas (Modelo OSI y TCP/IP)

---

### 📖 Mini Diccionario para Empezar

* **Encapsulamiento:** Es como meter una carta dentro de un sobre, luego ese sobre dentro de una caja de cartón, y luego la caja dentro de un camión de encomiendas. Conforme los datos bajan de capa, cada capa le pega su propia etiqueta con información necesaria para el viaje.
* **Desencapsulamiento:** El proceso inverso en la máquina que recibe. Va rompiendo la caja, abriendo el sobre y extrayendo la carta limpia para mostrársela a la persona.
* **Capa 1 (Física):** Los cables de cobre, la fibra óptica y los voltajes eléctricos.
* **Capa 2 (Enlace de Datos):** La tarjeta de red (NIC) y las direcciones físicas **MAC** que vienen grabadas de fábrica en el chip de tu computadora.
* **Capa 3 (Red):** Las direcciones lógicas **IP** que cambian según la red donde te conectes.
* **Capa 4 (Transporte):** Los números de **Puerto** (TCP o UDP) que identifican a tus programas.
* **Capa 7 (Aplicación):** Tu navegador web, WhatsApp o cliente de correo.

---

### 1. La Gran Analogía de las Muñecas Rusas (Matrioshkas)

Imagina que quieres mandarle un regalo por encomienda a tu amigo que vive en otro departamento:
1. **Tu regalo (Capa de Aplicación):** Es el objeto que quieres mandar (ejemplo: una taza personalizada).
2. **La caja pequeña con número de cuarto (Capa de Transporte):** Metes la taza en una cajita y le pegas una etiqueta que dice: *"Para entregar en la habitación 443"*. (Agrega el **Puerto de Destino**).
3. **La caja grande con la dirección de la casa (Capa de Red):** Metes la cajita dentro de una caja más grande y le pegas la dirección de la casa: *"Avenida Las Américas, Casa 15"*. (Agrega la **Dirección IP**).
4. **El camión de mensajería (Capa de Enlace):** El repartidor sube la caja a un camión que tiene placa y número de motor. (Agrega la **Dirección MAC** y el seguro contra golpes CRC).
5. **La carretera de asfalto (Capa Física):** Las ruedas del camión giran sobre el pavimento. (Son los voltajes eléctricos viajando por el cable UTP).

```mermaid
flowchart TD
    subgraph D_L7 ["1. Tu Regalo (Capa 7 - Aplicación)"]
        Data["Datos del Usuario<br/><b>Página Web HTTP</b>"]
    end

    subgraph D_L4 ["2. Cajita con Número de Habitación (Capa 4 - Segmento)"]
        direction LR
        TCP_H["Etiqueta de Puerto (20 B)"] --- TCP_P["Datos del Usuario"]
    end

    subgraph D_L3 ["3. Caja con Dirección de la Casa (Capa 3 - Paquete)"]
        direction LR
        IP_H["Etiqueta de IP (20 B)"] --- IP_P["Caja de Transporte Completa"]
    end

    subgraph D_L2 ["4. Camión de Mensajería (Capa 2 - Trama)"]
        direction LR
        ETH_H["Etiqueta de Placa MAC (14 B)"] --- ETH_P["Paquete IP"] --- ETH_F["Sello de Garantía FCS (4 B)"]
    end

    subgraph D_L1 ["5. Carretera (Capa 1 - Medio Físico)"]
        Bits["Señales Eléctricas en Cobre UTP Cat 6 o Pulsos de Luz en Fibra"]
    end

    D_L7 -->|"Se mete adentro de"| D_L4
    D_L4 -->|"Se mete adentro de"| D_L3
    D_L3 -->|"Se sube al camión"| D_L2
    D_L2 -->|"Rueda por el cable"| D_L1

    style D_L7 fill:#1e293b,stroke:#38bdf8,stroke-width:2px,color:#fff
    style D_L4 fill:#1e293b,stroke:#818cf8,stroke-width:2px,color:#fff
    style D_L3 fill:#1e293b,stroke:#34d399,stroke-width:2px,color:#fff
    style D_L2 fill:#1e293b,stroke:#f59e0b,stroke-width:2px,color:#fff
    style D_L1 fill:#1e293b,stroke:#ec4899,stroke-width:2px,color:#fff
```

---

### 2. La Cadena de Restricciones Métricas (Del Cable a la Computadora)

¿Por qué los archivos descargados viajan típicamente en bloques de **1,460 bytes**?
Porque cada capa le descuenta un pedazo al espacio disponible para pagar por sus etiquetas:

```mermaid
flowchart TD
    L1["Capa 1: Cable UTP de Cobre<br/>Soporta el estándar Ethernet"] --> L2["Capa 2: MTU Máxima de Ethernet = 1,500 Bytes<br/>El tamaño máximo de carga que cabe en el camión"]
    L2 --> L3["Capa 3: Encabezado IP = 20 Bytes<br/>Espacio restante: 1,500 - 20 = 1,480 Bytes"]
    L3 --> L4["Capa 4: Encabezado TCP = 20 Bytes<br/>Espacio libre para tus datos (MSS): 1,480 - 20 = 1,460 Bytes"]
    L4 --> L7["Capa 7: La aplicación web solo puede meter<br/>hasta 1,460 Bytes reales en cada viaje"]

    style L1 fill:#1e293b,stroke:#ec4899,stroke-width:2px,color:#fff
    style L2 fill:#1e293b,stroke:#f59e0b,stroke-width:2px,color:#fff
    style L3 fill:#1e293b,stroke:#34d399,stroke-width:2px,color:#fff
    style L4 fill:#1e293b,stroke:#818cf8,stroke-width:2px,color:#fff
    style L7 fill:#1e293b,stroke:#38bdf8,stroke-width:2px,color:#fff
```

$$\text{MTU (Tamaño máximo de la caja Ethernet)} = 1,500 \text{ bytes}$$
$$\text{Etiqueta de Capa 3 (IP)} = 20 \text{ bytes}$$
$$\text{Etiqueta de Capa 4 (TCP)} = 20 \text{ bytes}$$
$$\mathbf{MSS \text{ (Tus datos puros)}} = 1500 - 20 - 20 = \mathbf{1460 \text{ bytes}}$$

---

### 3. El Viaje de un Clic: ¿Qué pasa cuando entras al portal de la UMG?

Imagina que estás en una computadora de los laboratorios de la UMG, abres el navegador y tecleas:
$$\text{https://portal.umg.edu.gt}$$

A continuación se muestra la película completa de lo que ocurre en fracciones de segundo:

```mermaid
sequenceDiagram
    autonumber
    actor Tu as Tú en la PC
    participant Cable as Cable UTP / Switch (Capa 1 y 2)
    participant DHCP as Servidor DHCP (Capa 7)
    participant DNS as Servidor DNS (Capa 7 / Puerto 53)
    participant Web as Servidor Web Seguro (Puerto 443)

    Note over Tu,Cable: FASE 1: ARRANQUE Y CABLEADO (Módulo 3)
    Tu->>Cable: Señales eléctricas viajan por los pines 1 y 2 del cable UTP T568B
    Tu->>DHCP: DORA: Tu PC pide IP prestada al servidor DHCP
    DHCP->>Tu: DHCP ACK: "Tu IP es 192.168.10.50 y tu puerta de enlace es 192.168.10.1"

    Note over Tu,DNS: FASE 2: BUSCAR LA DIRECCIÓN (Módulo 2)
    Tu->>DNS: "¿Cuál es la IP numérica de portal.umg.edu.gt?" (Puerto 53 UDP)
    DNS->>Tu: "Su IP es 200.10.15.42"

    Note over Tu,Web: FASE 3: SALUDO FORMAL TCP (Módulo 1)
    Tu->>Web: 1. TCP SYN (Puerto 443)
    Web->>Tu: 2. TCP SYN-ACK
    Tu->>Web: 3. TCP ACK -> Estado ESTABLISHED

    Note over Tu,Web: FASE 4: SEGURIDAD Y PÁGINA WEB (Módulo 2)
    Tu->>Web: Negociación de cifrado seguro TLS 1.3
    Tu->>Web: Petición HTTP GET: "Entrégame la portada del portal"
    Web->>Tu: Respuesta HTTP 200 OK: Código HTML y fotos de la página
```

---

### 4. Guía Rápida de Diagnóstico de Fallas (Para el Examen)

| Si pasa este problema en la computadora... | ¿En qué capa está la culpa? | Explicación en palabras sencillas |
| :--- | :---: | :--- |
| **La luz de la tarjeta de red está apagada** | **Capa 1 (Física)** | El cable está roto, mal ponchado o desconectado. |
| **La PC tiene una IP que empieza por `169.254...`** | **Capa 7 (DHCP)** | El cable sirve, pero el servidor DHCP está apagado o el router no lo deja pasar (APIPA). |
| **El ping a `8.8.8.8` funciona, pero `google.com` no abre** | **Capa 7 (DNS)** | Tienes Internet perfecto, pero tu computadora no puede traducir nombres porque el DNS falló. |
| **El navegador dice: *"Tiempo de espera agotado"*** | **Capa 4 (TCP)** | La IP existe, pero un firewall tiene bloqueado el puerto 80 o 443 y no responde al saludo SYN. |
