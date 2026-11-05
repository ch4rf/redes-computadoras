# Módulo 2: Capa de Aplicación
## Tema 2.4: Servicios Web (HTTP y HTTPS)

---

### 1. HTTP vs. HTTPS: Comparativa Básica

| Parámetro | HTTP (HyperText Transfer Protocol) | HTTPS (HTTP Secure) |
| :--- | :--- | :--- |
| **Definición** | Protocolo sin cifrado para transferir hipertexto (páginas web, imágenes, APIs). | HTTP encapsulado dentro de una capa de transporte segura (TLS/SSL). |
| **Puerto Estándar** | **Puerto 80 TCP** | **Puerto 443 TCP** |
| **Seguridad / Cifrado** | En texto claro (*plaintext*). Vulnerable a ataques *Man-in-the-Middle* (sniffing de contraseñas). | Totalmente cifrado de extremo a extremo con TLS. Confidencialidad e integridad garantizadas. |
| **Autenticación** | Ninguna identidad verificada. | Utiliza **Certificados Digitales X.509** emitidos por Autoridades Certificadoras (CA). |
| **Rendimiento** | Ligeramente más rápido (sin sobrecarga criptográfica). | Muy ligera sobrecarga inicial durante la negociación TLS. |

---

### 2. Estructura de los Mensajes HTTP (Request y Response)

HTTP opera mediante un modelo de Solicitud-Respuesta sin estado (*Stateless*). Cada transacción consta de:

#### A. Mensaje de Petición (HTTP Request)
1. **Línea de Petición (Request Line):** Contiene el método, el recurso URI y la versión del protocolo.  
   *Ejemplo:* `GET /index.html HTTP/1.1`
2. **Cabeceras de Petición (Headers):** Información adicional del cliente.  
   *Ejemplos:* `Host: www.umg.edu.gt`, `User-Agent: Mozilla/5.0...`, `Accept: text/html`.
3. **Línea en blanco (CRLF):** Separa las cabeceras del cuerpo.
4. **Cuerpo del Mensaje (Body):** Opcional (usado en peticiones POST o PUT para enviar formularios o archivos JSON).

#### B. Mensaje de Respuesta (HTTP Response)
1. **Línea de Estado (Status Line):** Versión del protocolo, código de estado numérico y texto explicativo.  
   *Ejemplo:* `HTTP/1.1 200 OK`
2. **Cabeceras de Respuesta (Headers):** Información del servidor.  
   *Ejemplos:* `Content-Type: text/html; charset=UTF-8`, `Content-Length: 1024`, `Server: Apache`.
3. **Línea en blanco (CRLF):**
4. **Cuerpo de Respuesta (Body):** El código HTML, CSS, JavaScript o imagen binaria que el navegador renderiza en pantalla.

---

### 3. Métodos HTTP Principales (Verbos de Petición)

| Método | Propósito Principal | ¿Envía datos en URL o en Cuerpo? | Idempotente* |
| :--- | :--- | :--- | :--- |
| **GET** | Solicita y recupera un recurso del servidor sin modificar nada. | Datos en la URL (Query String: `?id=5`). | Sí |
| **POST** | Envía datos al servidor para crear un recurso nuevo o procesar un formulario (ej. login). | Datos en el Cuerpo (*Body*) de la petición. | No |
| **PUT** | Reemplaza o actualiza completamente un recurso existente en el servidor. | Datos en el Cuerpo. | Sí |
| **DELETE** | Elimina un recurso específico identificado por la URL. | No suele llevar cuerpo. | Sí |
| **HEAD** | Idéntico a GET, pero el servidor devuelve **únicamente las cabeceras**, sin el cuerpo HTML (ideal para verificar si un archivo cambió). | Sin cuerpo. | Sí |

*\*Idempotente: Ejecutar la misma petición múltiples veces consecutivas produce exactamente el mismo resultado en el servidor.*

---

### 4. Códigos de Estado HTTP (Categorías Obligatorias de Examen)

Los códigos de estado son números de 3 dígitos divididos en 5 clases:

```text
1xx: Informativos (Poco comunes en examen)
2xx: Éxito (Success)
3xx: Redirección (Redirection)
4xx: Error del Cliente (Client Error)
5xx: Error del Servidor (Server Error)
```

#### Los Códigos más Evaluados:
* **`200 OK`:** La solicitud se completó con éxito y el servidor entrega los datos solicitados.
* **`201 Created`:** La solicitud tuvo éxito y se ha creado un nuevo recurso (común en APIs REST con POST).
* **`301 Moved Permanently`:** El recurso cambió de dirección de forma permanente (el navegador redirige automáticamente a la nueva URL).
* **`302 Found / Moved Temporarily`:** Redirección temporal.
* **`304 Not Modified`:** El recurso no ha cambiado desde la última visita; el navegador puede usar su copia en memoria caché (ahorro de ancho de banda).
* **`400 Bad Request`:** La sintaxis de la petición del cliente es inválida o incomprensible para el servidor.
* **`401 Unauthorized`:** Se requiere autenticación previa (usuario y contraseña) para acceder.
* **`403 Forbidden`:** El servidor entiende quién es el cliente, pero este **no tiene permisos** para ver el recurso.
* **`404 Not Found`:** El recurso o página solicitada no existe en la ruta indicada del servidor.
* **`500 Internal Server Error`:** El servidor web falló internamente (error de código PHP, caída de base de datos, etc.).
* **`502 Bad Gateway`:** Un servidor proxy inverso o puerta de enlace recibió una respuesta inválida del servidor interno.
* **`503 Service Unavailable`:** El servidor está sobrecargado temporalmente o en mantenimiento.

---

### 5. ¿Cómo funciona HTTPS y el Cifrado TLS?

HTTPS utiliza **TLS (Transport Layer Security)**, sucesor moderno de SSL:
1. **Cifrado Asimétrico (Clave Pública y Privada):** Utilizado durante la fase inicial (*TLS Handshake*) para autenticar al servidor mediante su certificado digital y acordar de forma segura una clave secreta compartida.
2. **Cifrado Simétrico (Clave de Sesión única):** Una vez acordada la clave secreta, toda la transmisión masiva de páginas y datos web utiliza cifrado simétrico (como AES), debido a que es exponencialmente más rápido que el asimétrico.
