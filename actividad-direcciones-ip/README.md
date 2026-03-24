# 🌐 Actividad: Direcciones IP para Developers

## 🧩 1. ¿Qué es una dirección IP?

| Concepto | Definición simple | Nivel técnico | Ejemplo |
|----------|------------------|---------------|---------|
| IP | Es el "número de casa" de un dispositivo en una red. Sin ella, los datos no saben a dónde ir. | Identificador numérico único asignado a cada dispositivo conectado a una red. | `192.168.1.1` |
| IPv4 | Formato clásico de IP, cuatro números separados por puntos. | 32 bits, permite ~4.300 millones de direcciones. Ya casi agotadas. | `192.168.1.1` |
| IPv6 | Formato nuevo, mucho más largo. Creado para reemplazar IPv4. | 128 bits, permite 340 sextillones de direcciones. | `2001:db8::8a2e:370:7334` |

---

## ⚙️ 2. Tipos de IP

| Tipo | ¿Qué es? | ¿Cuándo se usa? | Ejemplo real |
|------|----------|-----------------|--------------|
| Pública | La IP que ve internet | Cuando un servidor necesita ser accesible desde fuera | `200.45.132.10` |
| Privada | Solo existe dentro de tu red local | Dispositivos dentro de tu casa u oficina | `192.168.0.5` |
| Estática | No cambia nunca | Servidores de producción, bases de datos | `54.23.180.9` (fija) |
| Dinámica | Cambia cada vez que te conectas | Laptops, celulares, hogares | La asigna el router automáticamente |

---

## 💻 3. Conexión directa con desarrollo

### ¿Qué IP usa tu backend?

**En local:**
```bash
npm run dev
# El servidor escucha en http://localhost:3000 o http://127.0.0.1:3000
```
`127.0.0.1` es una IP especial que significa "este mismo computador". Nunca sale de tu máquina.

**En producción:**
El backend corre en un servidor con IP pública real, por ejemplo `54.23.180.9`, accesible desde cualquier parte de internet.

### ¿Por qué no puedes acceder al `localhost` de otro computador?
Porque `127.0.0.1` está reservada para el propio dispositivo. Es como una dirección que solo existe dentro de tu casa — nadie de afuera puede usarla.

### ¿Qué pasa cuando haces `npm run dev`?
Tu servidor se levanta y escucha en `127.0.0.1` en un puerto (ej: 3000). Solo tú, desde ese computador, puedes acceder a `http://localhost:3000`.

### ¿Qué IP usa una base de datos en la nube?
Tiene una IP privada dentro de la red del proveedor (AWS, GCP, etc.). Te conectas por un hostname como `db.us-east-1.rds.amazonaws.com` que internamente resuelve a esa IP privada.

---

## 🌍 4. Caso práctico: el frontend no conecta al backend

**Escenario:** Tu frontend funciona pero no logra conectarse al backend.

### ¿Podría ser problema de IP? Sí. Razones posibles:
- La URL del backend tiene la IP o el puerto mal escrito
- El servidor escucha en `127.0.0.1` en vez de `0.0.0.0` (no acepta conexiones externas)
- El firewall está bloqueando el puerto
- El backend no está corriendo

### ¿Qué revisarías primero?
1. ¿La URL en el fetch tiene la IP y puerto correctos?
2. ¿El servidor está configurado para aceptar conexiones externas?
3. ¿El puerto está abierto en el firewall?
4. ¿El backend realmente está corriendo?

### Errores típicos
- `Connection refused` → el servidor no está corriendo o el puerto está mal
- `CORS error` → el backend no permite solicitudes desde ese origen
- `Network Error` → IP incorrecta o sin conexión

---

## 🧪 5. DHCP y DNS

### ¿Qué hace DHCP?
Asigna IPs automáticamente cuando un dispositivo se conecta a una red. Sin DHCP tendrías que configurar la IP a mano en cada dispositivo.

### ¿Qué hace DNS?
Traduce nombres legibles (`google.com`) a IPs (`142.250.80.46`). Sin DNS tendrías que memorizar IPs para entrar a cada sitio.

### ¿Qué pasa cuando escribes `google.com` en el navegador?
```
1. El navegador pregunta al DNS local: "¿cuál es la IP de google.com?"
2. Si no la sabe, consulta a un servidor DNS externo
3. El DNS responde: "la IP es 142.250.80.46"
4. El navegador se conecta directamente a esa IP
5. Google responde y ves la página
```

---

## 🔄 6. Analogías

| Concepto | Analogía |
|----------|----------|
| **IP** | El número de tu casa — identifica exactamente dónde está algo en la red |
| **DNS** | Una agenda de contactos — convierte un nombre en un número |
| **DHCP** | El conserje del edificio — te asigna un departamento automáticamente cuando llegas |

---

## 🧠 Bonus: Nivel Pro

### ¿Qué es NAT?
NAT (Network Address Translation) permite que muchos dispositivos con IPs privadas compartan una sola IP pública. Tu router lo hace constantemente — todos tus dispositivos salen a internet con la misma IP pública, pero internamente cada uno tiene su IP privada.

### ¿Cómo funciona una IP en Docker?
Cada contenedor tiene su propia IP privada dentro de una red virtual creada por Docker. Los contenedores se comunican entre sí por esas IPs internas. Desde afuera accedes mapeando puertos (`-p 3000:3000`).

### ¿Qué pasa con las IPs en AWS EC2?
Cada instancia recibe una IP privada dentro de la red de AWS y, opcionalmente, una IP pública para acceso desde internet. Si la instancia se reinicia, la IP pública puede cambiar — para evitar esto se usa una **Elastic IP** (IP estática en AWS).

---

## 💡 Conclusión

> *"Saber programar sin entender IPs es como saber escribir cartas sin saber direcciones… puedes hacer el mensaje perfecto, pero nunca va a llegar."*