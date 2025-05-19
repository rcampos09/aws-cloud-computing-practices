# Práctica: Gestión de Permisos de Archivos

## Introducción

Esta práctica está diseñada para la asignatura "Introducción a Cloud Computing". El objetivo es aprender los conceptos básicos de permisos de archivos en una instancia EC2 ejecutando Amazon Linux 2 en AWS Academy.

Linux utiliza un sistema de permisos para controlar quién puede ver, modificar o ejecutar archivos. Esto es como tener llaves diferentes para distintas puertas en una casa: algunas llaves son solo para ti, otras las pueden usar tus familiares, y algunas pueden usarlas los visitantes.

Para esta práctica, utilizarás una instancia EC2 con Amazon Linux 2. El usuario predeterminado es **ec2-user**.

Cada desafío incluye una explicación sencilla, las tareas a realizar y la solución paso a paso.

## Requisitos previos

- Acceso a AWS Academy
- Una instancia EC2 con Amazon Linux 2 
- Acceso a la terminal a través de la consola web de AWS
- Haber completado la práctica anterior de creación de usuarios

## Objetivo de la Práctica

Aprender cómo funcionan los permisos de archivos en Linux y cómo cambiarlos para controlar quién puede acceder a tus archivos.

## Tareas

1. Ver los permisos de un archivo
2. Crear un archivo privado
3. Crear un archivo que todos puedan leer
4. Probar los permisos con otro usuario
5. Crear un archivo ejecutable

## Desafío 1: Ver los permisos de un archivo

**Objetivo:** Aprender a ver qué permisos tiene un archivo en Linux.

### Tareas:

1. Crear un archivo de texto.
2. Ver los permisos que tiene ese archivo.
3. Entender qué significan esos permisos.

### Solución:

```bash
# Crear un archivo de texto
touch mi_archivo.txt

# Escribir algo en el archivo
echo "Hola, este es mi primer archivo" > mi_archivo.txt

# Ver los permisos del archivo
ls -l mi_archivo.txt

# Verás algo como esto:
# -rw-rw-r-- 1 ec2-user ec2-user 29 May 17 10:15 mi_archivo.txt

# Lo importante son los primeros caracteres: -rw-rw-r--
# Esto se divide en 4 partes:
# - El primer carácter (-) indica que es un archivo normal
# - Los siguientes 3 (rw-) son tus permisos como dueño
# - Los siguientes 3 (rw-) son los permisos de tu grupo
# - Los últimos 3 (r--) son los permisos para los demás usuarios

# r = puedes leer el archivo
# w = puedes modificar o borrar el archivo
# x = puedes ejecutar el archivo (si es un programa)
# - = no tienes ese permiso
```

## Desafío 2: Crear archivo con tu nombre y establecer permisos específicos

**Objetivo:** Aprender a crear un archivo y establecer permisos específicos utilizando el comando chmod.

### Tareas:

1. Crear un archivo con tu nombre y apellido seguido de "_Linux.txt".
2. Establecer los permisos para que solo tú puedas modificarlo, pero todos puedan leerlo.
3. Verificar que los permisos se hayan aplicado correctamente.

### Solución:

```bash
# Crear un archivo con tu nombre y apellido
# Reemplaza "nombre_apellido" con tu nombre y apellido reales
touch nombre_apellido_Linux.txt

# Agregar contenido al archivo
echo "Este es mi archivo de prueba en Linux" > nombre_apellido_Linux.txt

# Establecer permisos: solo tú puedes modificarlo, todos pueden leerlo
chmod 644 nombre_apellido_Linux.txt

# Verificar los permisos
ls -l nombre_apellido_Linux.txt

# Verás algo similar a:
# -rw-r--r-- 1 ec2-user ec2-user 35 May 17 10:20 nombre_apellido_Linux.txt

# Esto significa:
# - Tú (propietario) puedes leer y escribir (rw-)
# - Tu grupo puede solo leer (r--)
# - Otros usuarios pueden solo leer (r--)
```

## Desafío 3: Crear un archivo que todos puedan leer

**Objetivo:** Crear un archivo que todos los usuarios puedan leer, pero solo tú puedes modificar.

### Tareas:

1. Crear un archivo compartido.
2. Configurar los permisos para que todos puedan leerlo.
3. Verificar los permisos.

### Solución:

```bash
# Crear un archivo compartido
echo "Este archivo lo pueden leer todos" > archivo_compartido.txt

# Cambiar los permisos para que todos puedan leerlo, pero solo tú modificarlo
chmod 644 archivo_compartido.txt

# Ver los permisos
ls -l archivo_compartido.txt

# Verás algo como:
# -rw-r--r-- 1 ec2-user ec2-user 32 May 17 10:40 archivo_compartido.txt

# Esto significa:
# - Tú puedes leer y escribir (rw-)
# - Los demás solo pueden leer (r--)
```

## Desafío 4: Probar si otro usuario puede leer tus archivos

**Objetivo:** Comprobar si los permisos que configuraste funcionan correctamente.

### Tareas:

1. Cambiar al usuario que creaste en la práctica anterior.
2. Intentar leer el archivo privado y el archivo compartido.
3. Observar las diferencias según los permisos.

### Solución:

```bash
# Cambiar al usuario que creaste antes
# (reemplaza "tu_usuario" con el nombre de usuario que creaste en la práctica anterior)
su - tu_usuario

# Intentar leer el archivo privado
cat /home/ec2-user/archivo_tu_nombre.txt

# Deberías ver un mensaje de error como:
# cat: /home/ec2-user/archivo_tu_nombre.txt: Permiso denegado

# Ahora intentar leer el archivo compartido
cat /home/ec2-user/archivo_compartido.txt

# Deberías poder ver el contenido:
# "Este archivo lo pueden leer todos"

# Volver al usuario ec2-user
exit
```

## Desafío 5: Hacer un archivo ejecutable

**Objetivo:** Aprender a crear un archivo que se pueda ejecutar como un programa.

### Tareas:

1. Crear un pequeño script (programa) de ejemplo.
2. Hacerlo ejecutable.
3. Ejecutar el script.

### Solución:

```bash
# Crear un script simple
echo '#!/bin/bash' > mi_script.sh
echo 'echo "Hola, soy un script!"' >> mi_script.sh
echo 'echo "Hoy es $(date)"' >> mi_script.sh

# Ver los permisos actuales
ls -l mi_script.sh

# El script no es ejecutable todavía, así que hacerlo ejecutable
chmod +x mi_script.sh

# Ver los nuevos permisos
ls -l mi_script.sh

# Ahora verás algo como:
# -rwxrwxr-x 1 ec2-user ec2-user 57 May 17 10:50 mi_script.sh
# (Nota la 'x' que indica que es ejecutable)

# Ejecutar el script
./mi_script.sh

# Deberías ver:
# Hola, soy un script!
# Hoy es Sat May 17 10:50:00 UTC 2025
```

## Desafío 6: Crear un directorio y configurar sus permisos

**Objetivo:** Aprender a crear un directorio y establecer permisos para él.

### Tareas:

1. Crear un directorio con tu nombre.
2. Configurar los permisos para que solo tú puedas acceder a él.
3. Crear un archivo dentro del directorio.

### Solución:

```bash
# Crear un directorio con tu nombre
# Reemplaza "nombre_apellido" con tu nombre y apellido reales
mkdir directorio_nombre_apellido

# Configurar permisos para que solo tú puedas acceder
chmod 700 directorio_nombre_apellido

# Verificar los permisos
ls -ld directorio_nombre_apellido

# Verás algo similar a:
# drwx------ 2 ec2-user ec2-user 4096 May 17 11:00 directorio_nombre_apellido

# Crear un archivo dentro del directorio
touch directorio_nombre_apellido/archivo_secreto.txt
echo "Este es un archivo secreto" > directorio_nombre_apellido/archivo_secreto.txt

# Verificar que el archivo se creó
ls -l directorio_nombre_apellido/
```

## Desafío 7: Modificar propietario y grupo de archivos

**Objetivo:** Aprender a cambiar el propietario y grupo de los archivos utilizando los comandos chown y chgrp. Estas operaciones son importantes en entornos multiusuario para gestionar correctamente el acceso a los recursos.

### Tareas:

1. Crear un nuevo archivo.
2. Cambiar su propietario a otro usuario.
3. Cambiar su grupo y verificar los cambios.

### Solución:

```bash
# Asegúrate de tener privilegios de superusuario
sudo su -

# Crear un nuevo archivo
touch /tmp/archivo_compartido.txt
echo "Este es un archivo compartido" > /tmp/archivo_compartido.txt

# Ver propietario y grupo actuales
ls -l /tmp/archivo_compartido.txt

# Cambiar el propietario del archivo
# Reemplaza "nombre_apellido" con el usuario creado anteriormente
chown nombre_apellido /tmp/archivo_compartido.txt

# Verificar el cambio
ls -l /tmp/archivo_compartido.txt

# Cambiar el grupo del archivo (si existe un grupo específico)
# Por ejemplo, cambiarlo al grupo wheel
chgrp wheel /tmp/archivo_compartido.txt

# Verificar el cambio
ls -l /tmp/archivo_compartido.txt

# También podemos cambiar propietario y grupo a la vez
chown ec2-user:ec2-user /tmp/archivo_compartido.txt

# Verificar nuevamente
ls -l /tmp/archivo_compartido.txt

# Volver al usuario ec2-user si es necesario
exit
```

## Desafío 8: Configurar permisos especiales

**Objetivo:** Explorar permisos especiales como SUID, SGID y Sticky Bit, que proporcionan funcionalidades adicionales al sistema básico de permisos. Estos permisos avanzados son importantes para ciertas operaciones de sistema y para entender aspectos de seguridad más complejos.

### Tareas:

1. Crear un directorio compartido con Sticky Bit.
2. Entender el propósito y funcionamiento de los permisos especiales.
3. Verificar cómo afectan el comportamiento de archivos y directorios.

### Solución:

```bash
# Crear un directorio compartido
mkdir /tmp/directorio_compartido

# Establecer permisos básicos (lectura, escritura y ejecución para todos)
chmod 777 /tmp/directorio_compartido

# Agregar Sticky Bit
chmod +t /tmp/directorio_compartido
# o usando notación octal
chmod 1777 /tmp/directorio_compartido

# Verificar los permisos
ls -ld /tmp/directorio_compartido
# Verás algo como:
# drwxrwxrwt 2 ec2-user ec2-user 4096 May 17 11:00 /tmp/directorio_compartido
# Nota la 't' al final, que indica el Sticky Bit

# Explicación de permisos especiales:
# SUID (4000): cuando se aplica a un archivo ejecutable, el archivo se ejecuta con los permisos del propietario
# SGID (2000): cuando se aplica a un directorio, los nuevos archivos heredan el grupo del directorio
# Sticky Bit (1000): cuando se aplica a un directorio, solo el propietario puede eliminar sus propios archivos

# Creemos algunos archivos en el directorio compartido
echo "Archivo de ec2-user" > /tmp/directorio_compartido/archivo_ec2user.txt

# Cambiar a otro usuario
su - nombre_apellido

# Crear un archivo como este usuario
echo "Archivo de otro usuario" > /tmp/directorio_compartido/archivo_usuario2.txt

# Intentar eliminar el archivo de ec2-user
rm /tmp/directorio_compartido/archivo_ec2user.txt
# Esto debería fallar debido al Sticky Bit

# Pero puedes eliminar tu propio archivo
rm /tmp/directorio_compartido/archivo_usuario2.txt
# Esto debería funcionar

# Volver al usuario principal
exit
```

## Desafío 9: Implementar buenas prácticas de permisos

**Objetivo:** Aprender las mejores prácticas para configurar permisos en un entorno de producción, siguiendo el principio de privilegio mínimo. Estas prácticas son fundamentales para mantener la seguridad en sistemas cloud donde múltiples usuarios y servicios interactúan.

### Tareas:

1. Crear una estructura de directorios para un proyecto.
2. Configurar permisos siguiendo las mejores prácticas de seguridad.
3. Documentar las decisiones tomadas y su justificación.

### Solución:

```bash
# Crear una estructura de directorios para un proyecto web
mkdir -p ~/proyecto/{public_html,data,config,logs}

# Crear algunos archivos de ejemplo
touch ~/proyecto/public_html/index.html
touch ~/proyecto/config/settings.conf
touch ~/proyecto/data/database.db
touch ~/proyecto/logs/access.log

# Aplicar permisos siguiendo el principio de privilegio mínimo:

# 1. Directorios públicos: readable y executable por todos
chmod 755 ~/proyecto/public_html
chmod 644 ~/proyecto/public_html/index.html

# 2. Datos sensibles: solo accesibles por el propietario
chmod 700 ~/proyecto/data
chmod 600 ~/proyecto/data/database.db

# 3. Configuración: readable por el propietario, pero protegida
chmod 750 ~/proyecto/config
chmod 640 ~/proyecto/config/settings.conf

# 4. Logs: escribibles por el propietario y grupo, pero protegidos
chmod 770 ~/proyecto/logs
chmod 660 ~/proyecto/logs/access.log

# Verificar la estructura y permisos
ls -la ~/proyecto/

# Crear un documento para explicar las decisiones
cat > ~/permisos_explicacion.txt << 'EOF'
Justificación de permisos:

1. Directorio public_html (755) y archivos (644):
   - Permiten lectura y ejecución a todos los usuarios
   - Son necesarios para que un servidor web pueda acceder y servir estos archivos
   - No permiten escritura a grupo u otros para evitar modificaciones no autorizadas

2. Directorio data (700) y archivos (600):
   - Contiene información sensible como bases de datos
   - Solo el propietario puede leer, escribir y acceder a esta información
   - Protege contra acceso no autorizado a datos confidenciales

3. Directorio config (750) y archivos (640):
   - Contiene configuraciones que podrían incluir contraseñas o claves API
   - El propietario tiene control total
   - El grupo puede leer pero no modificar configuraciones
   - Otros usuarios no tienen acceso

4. Directorio logs (770) y archivos (660):
   - Propietario y grupo pueden escribir logs (aplicaciones que corren con diferentes usuarios)
   - No accesible para otros usuarios
   - Protege la información sensible que podría estar en los logs
EOF

# Ver la explicación
cat ~/permisos_explicacion.txt
```

## Conclusión

¡Felicitaciones! Has completado la práctica sobre permisos de archivos en Linux. Ahora sabes:

- Cómo ver los permisos de un archivo usando `ls -l`
- Qué significan los símbolos r (leer), w (escribir) y x (ejecutar)
- Cómo crear archivos privados que solo tú puedes ver
- Cómo crear archivos compartidos que todos pueden leer
- Cómo hacer que un archivo sea ejecutable (como un programa)
- Cómo comprobar que los permisos funcionan correctamente

Estos conocimientos son muy importantes cuando trabajas con Linux, ya que te permiten controlar quién puede acceder a tus archivos y qué pueden hacer con ellos. Es como tener diferentes tipos de llaves para tu casa, decidiendo quién puede entrar a cada habitación.

## Recursos adicionales

- [Guía sencilla de permisos en Linux](https://www.redhat.com/sysadmin/linux-file-permissions-explained)
- [Tutorial para principiantes sobre permisos en Linux](https://linuxhandbook.com/linux-file-permissions/)


