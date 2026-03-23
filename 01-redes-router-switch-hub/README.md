# 🌐 Redes para Desarrolladores — Router, Switch & Hub

> "Como developers, no trabajamos en el vacío. Nuestro código vive en una red. Si no entendemos cómo viaja la información, estamos programando a ciegas."

---

## 🧩 1. ¿Qué es cada dispositivo?

| Concepto | Definición simple | Nivel técnico | Ejemplo real |
|----------|------------------|---------------|--------------|
| **Router** | Conecta redes diferentes entre sí | Opera en capa 3 (IP). Enruta paquetes usando tablas de enrutamiento y aplica NAT | Módem WiFi de tu casa / AWS Internet Gateway |
| **Switch** | Conecta dispositivos dentro de la misma red local (LAN) | Opera en capa 2 (MAC). Envía datos solo al dispositivo correcto usando tabla CAM | Switch en un data center |
| **Hub** | Conecta dispositivos sin inteligencia, manda todo a todos | Opera en capa 1 (física). Broadcast total, colisiones frecuentes | Obsoleto, reemplazado por el switch |

---

## ⚙️ 2. ¿Cómo funcionan realmente?

**¿Qué hace el router con los paquetes?**
Lee la dirección IP de destino y consulta su tabla de enrutamiento para decidir el camino. Aplica NAT para traducir IPs privadas a públicas.

**¿Cómo decide el switch a dónde enviar?**
Mantiene una tabla CAM que mapea direcciones MAC con puertos físicos. Entrega el dato solo al puerto correcto, sin molestar al resto.

**¿Por qué el hub es obsoleto?**
Repite la señal a TODOS los puertos. Provoca colisiones, desperdicia ancho de banda y tiene cero privacidad.

---

## 💻 3. Conexión con desarrollo
```javascript
fetch("https://api.miapp.com/users")
```

- **Router**: lleva tu request desde tu red hasta internet aplicando NAT
- **Switch**: conecta los servidores dentro del data center con latencia sub-milisegundo
- **Problema clave**: packet loss, timeouts, DNS failures pueden romper tu app aunque tu código esté perfecto

---

## 🌍 4. Caso práctico — App caída en producción

**Escenario**: "Tu aplicación está en producción pero los usuarios no pueden acceder."

| Dispositivo | ¿Puede ser la causa? | Cómo verificarlo |
|-------------|---------------------|-----------------|
| **Router** | Sí, si el gateway cayó ningún tráfico externo entra | `ping api.miapp.com` / `traceroute api.miapp.com` |
| **Switch** | Sí, el servidor vive pero no llega a la base de datos | `curl -v https://api.miapp.com/health` |
| **¿Red o código?** | Si ping falla → red. Si responde con error → código | `nslookup api.miapp.com` / `curl -I https://api.miapp.com` |

---

## 🧪 5. Analogías

- **Router** ✈️ = Aeropuerto internacional: conecta redes distintas y decide por qué ruta enviar cada paquete
- **Switch** 📮 = Cartero inteligente: sabe exactamente a qué puerta entregar, no molesta a todos
- **Hub** 📢 = Micrófono en un patio: todos escuchan todo, caos total

---

## 🧠 Bonus

**Load Balancer**: reparte el tráfico entre varios servidores para que ninguno se sature. Ejemplos: Nginx, HAProxy, AWS ALB.

**En Cloud (AWS)**:
| Físico | AWS |
|--------|-----|
| Router | Internet Gateway |
| Switch | VPC + Subnets |
| Firewall | Security Groups |
| Load Balancer | ALB / NLB |

---

*Grupo de Redes — Desarrollo de Software*