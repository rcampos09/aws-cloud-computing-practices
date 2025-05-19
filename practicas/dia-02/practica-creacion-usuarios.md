# Práctica: Creación y Gestión de Usuarios

## Introducción

Esta práctica está diseñada para la asignatura "Introducción a Cloud Computing". El objetivo es aprender a crear y gestionar usuarios en una instancia EC2 ejecutando Amazon Linux 2 en AWS Academy.

La gestión adecuada de usuarios es una tarea fundamental en la administración de sistemas en entornos cloud. Crear usuarios específicos con permisos adecuados, en lugar de utilizar siempre el usuario root, es una buena práctica que mejora la seguridad y permite implementar el principio de privilegio mínimo.

Para esta práctica, utilizarás una instancia EC2 con Amazon Linux 2 con la configuración por defecto. El usuario predeterminado sigue siendo **ec2-user**.

Cada desafío incluye una explicación simple, la tarea a realizar y la solución para consultar en caso de necesidad.

## Requisitos previos

- Acceso a AWS Academy
- Una única instancia EC2 con Amazon Linux 2 creada y en funcionamiento
- Acceso a la terminal a través de la consola web de AWS
- Recuerda que en Amazon Linux 2, el usuario predeterminado es **ec2-user**
- Conocimientos básicos de comandos Linux (sudo, cat, ls)

## Objetivo de la Práctica

Aprender a crear un nuevo usuario con tu nombre y apellido (formato: nombre_apellido) en una instancia EC2 con Amazon Linux 2, establecer su contraseña, configurar sus permisos y verificar su correcta configuración.

## Tareas

1. Conectarse a la instancia EC2 y obtener privilegios de administrador
2. Crear un nuevo usuario con tu nombre_apellido y establecer su contraseña
3. Verificar la creación del usuario en archivos del sistema
4. Configurar permisos de sudo para el usuario
5. Probar el acceso y los permisos del nuevo usuario

## Desafío 1: Conectarse a la instancia y obtener privilegios de administrador

**Objetivo:** Conectarse a una instancia EC2 y obtener privilegios de administrador para realizar tareas de gestión de usuarios. En entornos cloud, el acceso seguro a los servidores y la elevación de privilegios son fundamentales para la administración del sistema.

### Tareas:

1. Conectarse a la instancia EC2 a través de la consola web de AWS.
2. Obtener privilegios de superusuario.
3. Verificar que has obtenido privilegios de administrador.

### Solución:

```bash
# Conectarse a la instancia EC2 usando EC2 Instance Connect
# (Esto se hace a través de la interfaz web de AWS)

# Una vez conectado como ec2-user, obtener privilegios de superusuario
sudo su -

# Verás que el símbolo del prompt cambia de "$" a "#" indicando que eres root

# Verificar que tienes privilegios de superusuario
whoami

# Verás la salida: "root"
```

## Desafío 2: Crear un nuevo usuario

**Objetivo:** Aprender a crear un nuevo usuario en el sistema. En entornos multiusuario como servidores cloud, es fundamental crear cuentas específicas para cada persona o servicio en lugar de compartir credenciales, siguiendo el principio de seguridad de privilegio mínimo.

### Tareas:

1. Crear un nuevo usuario con tu nombre y apellido (formato: nombre_apellido).
2. Establecer una contraseña para el usuario.
3. Verificar el mensaje de confirmación.

### Solución:

```bash
# Asegúrate de tener privilegios de superusuario

# Crear el nuevo usuario (reemplaza "nombre_apellido" con tu nombre y apellido)
# Por ejemplo: maria_rodriguez, juan_perez, etc.
useradd nombre_apellido

# Establecer contraseña para el usuario
passwd nombre_apellido

# El sistema te pedirá que introduzcas la contraseña:
# Nueva contraseña: (introduce una contraseña segura)
# Vuelva a escribir la nueva contraseña: (repite la misma contraseña)

# Verás un mensaje similar a:
# "passwd: todos los tokens de autenticación se actualizaron exitosamente."
# Esto confirma que la contraseña se ha establecido correctamente
```

## Desafío 3: Verificar la creación del usuario en /etc/passwd

**Objetivo:** Aprender a verificar que un usuario ha sido creado correctamente examinando el archivo /etc/passwd. Entender los archivos de configuración del sistema es fundamental para administradores de servidores, ya que permite verificar y diagnosticar problemas de configuración.

### Tareas:

1. Visualizar la entrada del nuevo usuario en el archivo /etc/passwd.
2. Comprender el formato y los campos de este archivo.
3. Identificar el directorio home y el shell asignados al usuario.

### Solución:

```bash
# Buscar la entrada del usuario en /etc/passwd (reemplaza "nombre_apellido" con tu usuario)
grep nombre_apellido /etc/passwd

# Verás una línea similar a:
# nombre_apellido:x:1001:1001::/home/nombre_apellido:/bin/bash

# Los campos están separados por ":" y son:
# 1. Nombre de usuario (nombre_apellido)
# 2. Contraseña (x indica que está en /etc/shadow)
# 3. UID (ID de usuario, probablemente 1001)
# 4. GID (ID de grupo, probablemente 1001)
# 5. Campo de comentario (vacío en este caso)
# 6. Directorio home (/home/nombre_apellido)
# 7. Shell predeterminado (/bin/bash)
```

## Desafío 4: Verificar la contraseña en /etc/shadow

**Objetivo:** Examinar el archivo /etc/shadow donde se almacenan las contraseñas cifradas de los usuarios. Comprender cómo Linux almacena las credenciales de forma segura es importante para la administración de seguridad del sistema.

### Tareas:

1. Visualizar la entrada del nuevo usuario en el archivo /etc/shadow.
2. Identificar la parte que corresponde a la contraseña cifrada.
3. Observar la diferencia entre un usuario con contraseña y uno sin contraseña.

### Solución:

```bash
# Buscar la entrada del usuario en /etc/shadow (reemplaza "nombre_apellido" con tu usuario)
grep nombre_apellido /etc/shadow

# Verás una línea con este formato:
# nombre_apellido:$6$abc...xyz:18901:0:99999:7:::

# La segunda parte (después del primer ":") es la contraseña cifrada
# Observa que comienza con "$6$" lo que indica que usa algoritmo SHA-512

# Para comparar, puedes ver un usuario sin contraseña o con contraseña bloqueada
grep "nologin" /etc/passwd | head -1 | cut -d: -f1 | xargs grep -o "^[^:]*:[^:]*" /etc/shadow

# Verás que en usuarios bloqueados, la contraseña suele empezar con "!" o "*"
```

## Desafío 5: Verificar el directorio home del usuario

**Objetivo:** Comprobar que el sistema ha creado correctamente el directorio home del usuario y configurado sus permisos. El directorio home es el espacio personal de cada usuario en el sistema, y su correcta configuración es esencial para la privacidad y la seguridad.

### Tareas:

1. Verificar que existe el directorio home del usuario.
2. Examinar los permisos del directorio.
3. Listar el contenido inicial del directorio.

### Solución:

```bash
# Verificar la existencia y permisos del directorio home
ls -la /home

# Verás una lista que incluye el directorio /home/nombre_apellido
# Los permisos deberían ser "drwx------" o similar (permisos 700)

# Examinar el contenido del directorio home (reemplaza "nombre_apellido" con tu usuario)
ls -la /home/nombre_apellido

# Verás los archivos de inicialización básicos como .bash_profile, .bashrc, etc.
# Estos son archivos ocultos (empiezan con .) que configuran el entorno del usuario
```

## Desafío 6: Configurar permisos de sudo

**Objetivo:** Aprender a otorgar privilegios de sudo a un usuario para que pueda realizar tareas administrativas cuando sea necesario. La configuración de sudo permite implementar el principio de privilegio mínimo, donde los usuarios solo tienen acceso de administrador cuando lo necesitan específicamente.

### Tareas:

1. Añadir el usuario al grupo wheel para otorgarle permisos de sudo.
2. Verificar que el usuario ha sido añadido al grupo correctamente.
3. Comprobar que el grupo wheel tiene permisos sudo en el sistema.

### Solución:

```bash
# Añadir el usuario al grupo wheel (grupo con permisos sudo en Amazon Linux 2)
# Reemplaza "nombre_apellido" con tu usuario
usermod -aG wheel nombre_apellido

# Verificar los grupos a los que pertenece el usuario
groups nombre_apellido

# Verás una lista que incluye "wheel" junto con el grupo principal del usuario

# Verificar que el grupo wheel tiene permisos sudo
grep wheel /etc/sudoers

# Verás una línea similar a:
# %wheel  ALL=(ALL)       ALL
# o
# %wheel  ALL=(ALL)       NOPASSWD: ALL
# Esto confirma que los miembros del grupo wheel pueden usar sudo
```

## Desafío 7: Probar el acceso del nuevo usuario

**Objetivo:** Verificar que el nuevo usuario puede acceder al sistema y utilizar sus privilegios correctamente. Probar la configuración realizada es un paso fundamental en la administración de sistemas para asegurar que todo funciona según lo previsto.

### Tareas:

1. Cambiar al usuario recién creado.
2. Verificar la identidad y los permisos del usuario.
3. Probar la ejecución de un comando con privilegios sudo.

### Solución:

```bash
# Cambiar al usuario recién creado (reemplaza "nombre_apellido" con tu usuario)
su - nombre_apellido

# El prompt cambiará y ahora estarás en el contexto del nuevo usuario

# Verificar la identidad
whoami

# Verás el nombre de tu usuario (nombre_apellido)

# Verificar los detalles del usuario y sus grupos
id

# Verás información sobre UID, GID y grupos a los que pertenece el usuario

# Probar un comando con sudo
sudo ls -la /root

# Se te pedirá la contraseña del usuario
# Después de introducirla, verás el contenido del directorio /root
# Esto confirma que el usuario tiene permisos sudo funcionando correctamente
```

## Desafío 8: Configurar opciones avanzadas de la cuenta

**Objetivo:** Aprender a configurar opciones avanzadas para la gestión de cuentas de usuario, como fechas de caducidad y políticas de contraseñas. Estas configuraciones son importantes para mantener la seguridad del sistema a lo largo del tiempo.

### Tareas:

1. Configurar el usuario para que deba cambiar su contraseña en el próximo inicio de sesión.
2. Establecer una fecha de caducidad para la cuenta.
3. Verificar la configuración establecida.

### Solución:

```bash
# Regresa a usuario root si aún estás como tu usuario
exit

# Forzar cambio de contraseña en el próximo inicio de sesión
# Reemplaza "nombre_apellido" con tu usuario
chage -d 0 nombre_apellido

# Establecer una fecha de caducidad para la cuenta (ejemplo: 31 de diciembre de 2025)
usermod -e 2025-12-31 nombre_apellido

# Verificar la configuración de envejecimiento de contraseña
chage -l nombre_apellido

# Verás información sobre:
# - Último cambio de contraseña
# - Caducidad de la contraseña
# - Fecha de caducidad de la cuenta
# - Y otros parámetros de la política de contraseñas
```

## Desafío 9: Crear un script para gestionar usuarios

**Objetivo:** Aprender a automatizar tareas de gestión de usuarios mediante un script básico. La automatización de tareas administrativas es fundamental en entornos cloud, donde la escalabilidad y la consistencia son prioritarias.

### Tareas:

1. Crear un script sencillo para añadir un nuevo usuario.
2. Incluir la creación del usuario, establecimiento de contraseña y asignación de permisos sudo.
3. Ejecutar el script para crear un segundo usuario.

### Solución:

```bash
# Crear un script para añadir usuarios
cat > crear_usuario.sh << 'EOF'
#!/bin/bash
# Script simple para crear un nuevo usuario con permisos sudo

# Verificar si se proporcionó un nombre de usuario
if [ $# -ne 1 ]; then
    echo "Uso: $0 nombre_usuario"
    exit 1
fi

USUARIO=$1
PASSWD="Temporal123"  # En producción, usar algo más seguro o generarlo

# Crear el usuario
useradd $USUARIO

# Establecer contraseña
echo "$USUARIO:$PASSWD" | chpasswd

# Añadir al grupo wheel para permisos sudo
usermod -aG wheel $USUARIO

# Forzar cambio de contraseña en el próximo inicio
chage -d 0 $USUARIO

echo "Usuario $USUARIO creado con éxito"
echo "Contraseña inicial: $PASSWD"
echo "El usuario deberá cambiar la contraseña en el próximo inicio de sesión"
EOF

# Hacer el script ejecutable
chmod +x crear_usuario.sh

# Ejecutar el script para crear un segundo usuario
# Puedes usar el nombre de un compañero o otro nombre representativo
sudo ./crear_usuario.sh segundo_usuario

# Verificar que el usuario se ha creado
grep segundo_usuario /etc/passwd
```

## Conclusión

¡Felicitaciones por completar esta práctica sobre creación y gestión de usuarios en instancias EC2 con Amazon Linux 2! Has aprendido a crear tu propio usuario personalizado, establecer contraseñas, asignar permisos sudo y verificar la configuración de las cuentas.

La gestión adecuada de usuarios es fundamental para mantener la seguridad de tus sistemas en la nube. Recuerda siempre seguir el principio de privilegio mínimo: cada usuario y servicio debe tener solo los permisos necesarios para realizar sus tareas, ni más ni menos.

En entornos de producción, considera implementar prácticas adicionales como la autenticación por claves SSH en lugar de contraseñas, la rotación regular de credenciales y el monitoreo de actividades de los usuarios para detectar comportamientos sospechosos.

## Recursos adicionales

- [Documentación oficial de Amazon Linux 2](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/amazon-linux-2-virtual-machine.html)
- [Guía de AWS sobre prácticas recomendadas de seguridad para EC2](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-security.html)
- [Guía de seguridad de usuarios en Linux](https://www.tecmint.com/usermod-command-examples/) 2! Has aprendido a crear usuarios, establecer contraseñas, asignar permisos sudo y verificar la configuración de las cuentas.

La gestión adecuada de usuarios es fundamental para mantener la seguridad de tus sistemas en la nube. Recuerda siempre seguir el principio de privilegio mínimo: cada usuario y servicio debe tener solo los permisos necesarios para realizar sus tareas, ni más ni menos.

En entornos de producción, considera implementar prácticas adicionales como la autenticación por claves SSH en lugar de contraseñas, la rotación regular de credenciales y el monitoreo de actividades de los usuarios para detectar comportamientos sospechosos.

## Recursos adicionales

- [Documentación oficial de Amazon Linux 2](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/amazon-linux-2-virtual-machine.html)
- [Guía de AWS sobre prácticas recomendadas de seguridad para EC2](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-security.html)
- [Guía de seguridad de usuarios en Linux](https://www.tecmint.com/usermod-command-examples/)
