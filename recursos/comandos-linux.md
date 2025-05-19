# Comandos Básicos de Linux

Esta hoja de referencia contiene los comandos Linux más comunes que utilizarás durante las prácticas de cloud computing.

## Navegación y Gestión de Archivos

| Comando | Descripción | Ejemplo |
|---------|-------------|---------|
| `pwd` | Muestra el directorio actual | `pwd` |
| `ls` | Lista archivos y directorios | `ls -la` (muestra todos los archivos, incluso ocultos) |
| `cd` | Cambia de directorio | `cd /home/ec2-user` |
| `mkdir` | Crea un directorio | `mkdir nuevo_directorio` |
| `touch` | Crea un archivo vacío | `touch archivo.txt` |
| `cp` | Copia archivos o directorios | `cp archivo.txt copia.txt` |
| `mv` | Mueve o renombra archivos | `mv archivo.txt nuevo_nombre.txt` |
| `rm` | Elimina archivos | `rm archivo.txt` |
| `rmdir` | Elimina directorios vacíos | `rmdir directorio_vacio` |
| `cat` | Muestra el contenido de un archivo | `cat archivo.txt` |
| `more` / `less` | Muestra contenido página a página | `less archivo_grande.txt` |
| `head` | Muestra las primeras líneas | `head -10 archivo.txt` (10 primeras líneas) |
| `tail` | Muestra las últimas líneas | `tail -5 archivo.txt` (5 últimas líneas) |
| `find` | Busca archivos | `find /home -name "*.txt"` |
| `grep` | Busca texto dentro de archivos | `grep "palabra" archivo.txt` |

## Usuarios y Permisos

| Comando | Descripción | Ejemplo |
|---------|-------------|---------|
| `sudo` | Ejecuta comandos como superusuario | `sudo yum update` |
| `su` | Cambia de usuario | `su - ec2-user` |
| `useradd` | Crea un nuevo usuario | `sudo useradd nuevo_usuario` |
| `passwd` | Cambia contraseña | `sudo passwd usuario` |
| `usermod` | Modifica usuarios | `sudo usermod -aG wheel usuario` (añade a grupo wheel) |
| `groups` | Muestra grupos de un usuario | `groups ec2-user` |
| `chmod` | Cambia permisos de archivos | `chmod 755 archivo.sh` |
| `chown` | Cambia el propietario | `sudo chown usuario:grupo archivo.txt` |

## Sistema

| Comando | Descripción | Ejemplo |
|---------|-------------|---------|
| `top` | Muestra procesos en ejecución | `top` |
| `ps` | Lista procesos | `ps aux` |
| `kill` | Termina procesos | `kill 1234` (donde 1234 es el PID) |
| `df` | Muestra espacio en disco | `df -h` (en formato legible) |
| `free` | Muestra uso de memoria | `free -m` (en megabytes) |
| `date` | Muestra fecha y hora | `date` |
| `uptime` | Tiempo de funcionamiento | `uptime` |
| `whoami` | Muestra el usuario actual | `whoami` |
| `uname` | Información del sistema | `uname -a` (toda la información) |
| `ip` | Muestra configuración de red | `ip a` |

## Redes

| Comando | Descripción | Ejemplo |
|---------|-------------|---------|
| `ping` | Comprueba conexión | `ping google.com` |
| `curl` | Transfiere datos desde/hacia servidores | `curl https://example.com` |
| `wget` | Descarga archivos | `wget https://example.com/archivo.zip` |
| `netstat` | Muestra conexiones de red | `netstat -tulpn` |
| `ss` | Muestra sockets | `ss -tulpn` |

## Archivos y Directorios

| Comando | Descripción | Ejemplo |
|---------|-------------|---------|
| `tar` | Comprime/descomprime archivos | `tar -czvf archivo.tar.gz directorio/` |
| `unzip` | Descomprime archivos .zip | `unzip archivo.zip` |
| `ln` | Crea enlaces | `ln -s archivo enlace` (enlace simbólico) |
| `wc` | Cuenta líneas, palabras, bytes | `wc -l archivo.txt` (cuenta líneas) |
| `sort` | Ordena líneas de texto | `sort archivo.txt` |
| `diff` | Muestra diferencias entre archivos | `diff archivo1.txt archivo2.txt` |

## Gestión de Paquetes (Amazon Linux 2)

| Comando | Descripción | Ejemplo |
|---------|-------------|---------|
| `yum` | Gestor de paquetes | `sudo yum install nombre_paquete` |
| `yum search` | Busca paquetes | `yum search keyword` |
| `yum update` | Actualiza paquetes | `sudo yum update` |
| `yum remove` | Elimina paquetes | `sudo yum remove nombre_paquete` |

## Servicios

| Comando | Descripción | Ejemplo |
|---------|-------------|---------|
| `systemctl` | Controla servicios | `sudo systemctl start servicio` |
| `systemctl status` | Estado de un servicio | `systemctl status sshd` |
| `journalctl` | Muestra logs del sistema | `journalctl -u servicio` |

## Programación de Tareas

| Comando | Descripción | Ejemplo |
|---------|-------------|---------|
| `crontab -e` | Edita tareas programadas | `crontab -e` |
| `crontab -l` | Lista tareas programadas | `crontab -l` |

## Ayuda

| Comando | Descripción | Ejemplo |
|---------|-------------|---------|
| `man` | Muestra el manual de un comando | `man ls` |
| `--help` | Muestra ayuda rápida | `ls --help` |
| `info` | Muestra información detallada | `info ls` |
