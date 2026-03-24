# 🌐 Fundamentos de Redes e IPs para Desarrolladores

**Bootcamp:** Generation
**Objetivo:** Comprender la infraestructura de red, enrutamiento y gestión de IPs para diagnosticar errores en entornos de desarrollo y producción.

> "Saber programar sin entender de redes es como escribir cartas perfectas sin saber direcciones; el mensaje nunca llegará."

---

## 🏗️ 1. Dispositivos de Red: La Infraestructura

Los desarrolladores no programamos en el vacío. Nuestras APIs y bases de datos se comunican a través de hardware físico.

| Concepto | Definición Simple | Nivel Técnico | Ejemplo Real |
| :--- | :--- | :--- | :--- |
| **Router** | Conecta diferentes redes entre sí y busca la mejor ruta para enviar la información. | Opera en capa 3 (Red). Usa direcciones IP para enrutar paquetes. | El módem/router de tu casa conectándote a Internet. |
| **Switch** | Conecta dispositivos dentro de una misma red y envía los datos solo a quien los pidió. | Opera en capa 2 (Enlace). Usa direcciones MAC para dirigir el tráfico. | Conectar varios PCs en la misma oficina por cable. |
| **Hub** | Conecta dispositivos, pero repite todo lo que recibe a todos los conectados. | Repetidor multipuerto (Capa 1). Obsoleto por generar colisiones. | Un "ladrón" de enchufes, pero para cables de red. |

### 🧠 Analogías
* **Router:** La oficina de correos central que envía cartas a otras ciudades.
* **Switch:** El cartero del edificio que entrega la carta exactamente en tu departamento.
* **Hub:** Una persona con un megáfono gritando el mensaje a todo el barrio esperando que el destinatario lo escuche.

### 💻 Conexión con el Desarrollo
* Cuando haces un `fetch("https://api.miapp.com")`, el **router** es quien saca tu petición de tu red local y la guía por internet hasta el servidor.
* El **switch** es crítico en los Data Centers para que los servidores del backend se comuniquen rápidamente con los servidores de base de datos sin saturar la red.

---

## 📍 2. Direccionamiento IP y Resolución de Nombres

Las direcciones son la base de la comunicación. Si el backend no tiene la IP correcta del frontend, hay error de CORS o Timeout.

| Concepto | Definición Simple | Nivel Técnico | Ejemplo |
| :--- | :--- | :--- | :--- |
| **IPv4** | Dirección numérica tradicional de 4 bloques. | 32 bits, separada por puntos. Se están agotando. | `192.168.1.15` |
| **IPv6** | La nueva versión de direcciones, mucho más larga y con letras. | 128 bits, alfanumérica, separada por dos puntos. | `2001:0db8::ff00:42:8329` |

### 🔄 Tipos de IP

| Tipo | ¿Qué es? | ¿Cuándo se usa? | Ejemplo Real |
| :--- | :--- | :--- | :--- |
| **Pública** | Visible desde cualquier parte de internet. Es única mundialmente. | En servidores web, APIs en producción o tu router hacia afuera. | `200.89.54.12` |
| **Privada** | Solo funciona dentro de tu red local (casa/oficina). | Para tu PC, tu celular, o contenedores internos. | `192.168.0.10` |
| **Estática** | Una IP que nunca cambia, configurada manualmente. | Servidores de bases de datos, APIs de producción. | La IP fija de un servidor AWS. |
| **Dinámica** | Cambia periódicamente, asignada automáticamente. | Dispositivos de usuarios comunes (teléfonos, laptops). | Tu celular conectándose al WiFi. |

### 🛠️ Herramientas Clave: DNS y DHCP
* **DHCP:** El servicio que te asigna una IP dinámica automáticamente cuando te conectas a una red. *(Analogía: El acomodador del cine que te da un asiento libre).*
* **DNS:** Traduce nombres fáciles de leer (google.com) en direcciones IP numéricas. *(Analogía: La agenda de contactos de tu teléfono).*
* **El flujo de una petición:** Escribes `google.com` ➔ DNS busca su IP ➔ El Router envía la petición ➔ El Switch del servidor de Google lo recibe ➔ Te devuelve la página.

### 💻 Conexión con el Desarrollo (Local vs Producción)
* **Localhost (`127.0.0.1`):** Es una IP de "bucle local". Tu PC habla consigo mismo. Por eso otro computador no puede ver tu `npm run dev` si usas localhost.
* **Producción:** Tu backend usará una IP Pública Estática para que todo el mundo pueda consumirla, y tu Base de Datos usará una IP Privada para mayor seguridad (solo el backend puede verla).

---

## 🐛 3. Casos Prácticos (Debugging)

**Escenario 1: La app en producción está caída.**
* **¿Es el Router?** Sí, si el proveedor de internet del Data Center o la configuración de salida a internet (gateway) se cayó. El servidor no puede devolver el request.
* **¿Es código o red?** Si haces ping a la IP del servidor y responde, la red funciona. Si entras a la app y da error 500, el problema es tu código (Java/Node).

**Escenario 2: Frontend no conecta al Backend.**
* **Revisión:** ¿Está apuntando el fetch a `localhost` en vez de la IP pública del servidor? ¿El firewall bloquea el puerto (ej: 8080)? ¿Hay problemas de CORS?

---

## 🚀 4. BONUS: Nivel Bootcamp Pro

* **Load Balancer:** Un policía de tránsito. Recibe todo el tráfico masivo y lo reparte entre varios servidores (Switches/Routers internos) para que ninguno colapse.
* **NAT:** Traduce tu IP privada a pública para salir a internet. Es la razón por la que todos los dispositivos de tu casa comparten la misma IP pública hacia el exterior.
* **Docker/AWS:** En AWS EC2, configuras IPs elásticas (públicas estáticas). En Docker, cada contenedor tiene su propia IP privada interna gestionada por un bridge (un switch virtual).