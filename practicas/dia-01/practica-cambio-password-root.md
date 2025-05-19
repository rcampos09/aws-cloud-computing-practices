# Práctica: Cambio de Contraseña de Usuario Root

## Introducción

Esta práctica está diseñada para la asignatura "Introducción a Cloud Computing". El objetivo es aprender a modificar la contraseña del superusuario (root) en una instancia EC2 ejecutando Amazon Linux 2 en AWS Academy.

La gestión segura de credenciales y usuarios es un aspecto fundamental en entornos cloud. En esta práctica, aprenderás a cambiar la contraseña del usuario root y a verificar este cambio, una habilidad básica pero esencial para la administración de servidores en la nube.

Para esta práctica, utilizarás una instancia EC2 con Amazon Linux 2 con la configuración por defecto. El usuario predeterminado sigue siendo **ec2-user**.

Cada desafío incluye una explicación simple, la tarea a realizar y la solución para consultar en caso de necesidad.

## Requisitos previos

- Acceso a AWS Academy
- Una única instancia EC2 con Amazon Linux 2 creada y en funcionamiento
- Acceso a la terminal a través de la consola web de AWS
- Recuerda que en Amazon Linux 2, el usuario predeterminado es **ec2-user**

## Objetivo de la Práctica

Aprender a modificar la contraseña del usuario root en una instancia EC2 con Amazon Linux 2 y verificar el cambio mediante la visualización de archivos del sistema relacionados con la seguridad.

## Tareas

1. Conectarse a la instancia EC2 con Amazon Linux 2
2. Obtener privilegios de superusuario
3. Cambiar la contraseña de root
4. Verificar el cambio en archivos del sistema
5. Comprobar el funcionamiento de la nueva contraseña

## Desafío 1: Conectarse a la instancia y obtener privilegios de superusuario

**Objetivo:** Aprender a conectarse a una instancia EC2 mediante la consola web y obtener privilegios de administrador. En entornos cloud, la conexión segura a servidores y la elevación de privilegios son operaciones fundamentales para la administración del sistema.

### Tareas:

1. Conectarse a la instancia EC2 a través de la consola web de AWS.
2. Obtener acceso como superusuario (root).
3. Verificar que has obtenido privilegios de superusuario.

### Solución:

```bash
# Conectarse a la instancia EC2 usando EC2 Instance Connect
# (Esto se hace a través de la interfaz web de AWS)

# Una vez conectado como ec2-user, obtener privilegios de superusuario
sudo su -

# Verificar que estás como usuario root
whoami

# Verás la salida: "root", confirmando que tienes privilegios de superusuario
# El prompt también cambiará de "$" a "#", indicando que eres root
```

## Desafío 2: Cambiar la contraseña del usuario root

**Objetivo:** Aprender a cambiar la contraseña del usuario root. Cambiar las contraseñas predeterminadas es una práctica fundamental de seguridad en cualquier sistema, especialmente en entornos cloud donde los servidores están expuestos a Internet.

### Tareas:

1. Utilizar el comando adecuado para cambiar la contraseña de root.
2. Crear una contraseña segura (combinación de letras mayúsculas, minúsculas, números y símbolos).
3. Verificar el mensaje de confirmación.

### Solución:

```bash
# Asegúrate de tener privilegios de superusuario (si no los tienes, ejecuta: sudo su -)

# Cambiar la contraseña de root
passwd

# El sistema te pedirá que introduzcas la nueva contraseña:
# Nueva contraseña: (introduce tu nueva contraseña)
# Vuelva a escribir la nueva contraseña: (vuelve a introducir la misma contraseña)

# Verás un mensaje similar a:
# "passwd: todos los tokens de autenticación se actualizaron exitosamente."
# Este mensaje confirma que la contraseña se ha cambiado correctamente
```

## Desafío 3: Explorar el archivo shadow

**Objetivo:** Familiarizarse con el archivo /etc/shadow donde se almacenan las contraseñas cifradas. Entender cómo Linux almacena las contraseñas de forma segura es importante para comprender la seguridad del sistema y proteger la información sensible.

### Tareas:

1. Visualizar el contenido del archivo shadow.
2. Identificar la línea correspondiente al usuario root.
3. Comprender la estructura de este archivo.

### Solución:

```bash
# Asegúrate de tener privilegios de superusuario

# Visualizar el contenido del archivo shadow
cat /etc/shadow

# Verás varias líneas, cada una para un usuario diferente
# Busca la línea que comienza con "root:"
# El formato es: usuario:contraseña_cifrada:último_cambio:min:max:aviso:inactividad:expiración:reservado

# Para ver solo la entrada de root
grep "^root:" /etc/shadow

# Observa que la contraseña está cifrada, no en texto plano
# El segundo campo (después del primer ":") contiene el hash de la contraseña
```

## Desafío 4: Comprender el formato de /etc/shadow

**Objetivo:** Analizar en detalle el formato del archivo /etc/shadow y comprender su importancia para la seguridad del sistema. Este conocimiento es fundamental para administradores de sistemas, ya que este archivo contiene información crítica de autenticación.

### Tareas:

1. Examinar los diferentes campos del archivo shadow.
2. Identificar el algoritmo de cifrado utilizado.
3. Comprender la función de los campos de caducidad de contraseñas.

### Solución:

```bash
# Vamos a examinar en detalle un solo usuario
grep "^root:" /etc/shadow | awk -F':' '{print "Usuario: " $1 "\nContraseña cifrada: " $2 "\nÚltimo cambio (días desde 1/1/1970): " $3 "\nDías mínimos entre cambios: " $4 "\nDías máximos entre cambios: " $5 "\nDías de aviso antes de caducidad: " $6 "\nDías de inactividad antes de deshabilitar: " $7 "\nFecha de expiración: " $8}'

# La contraseña cifrada comienza con $6$ o similar
# - $1$ = MD5
# - $5$ = SHA-256
# - $6$ = SHA-512 (el más común en sistemas modernos)

# Para ver qué algoritmo de cifrado se usa en el sistema
authconfig --test | grep hashing
```

## Desafío 5: Comparar /etc/shadow con /etc/passwd

**Objetivo:** Entender la diferencia entre los archivos /etc/shadow y /etc/passwd y por qué esta separación mejora la seguridad. Conocer la historia de cómo Linux ha evolucionado sus mecanismos de seguridad te ayuda a apreciar las mejores prácticas actuales.

### Tareas:

1. Visualizar el contenido del archivo passwd.
2. Comparar la información disponible en passwd y shadow.
3. Entender por qué se separan estos archivos.

### Solución:

```bash
# Ver el contenido del archivo passwd
cat /etc/passwd

# Filtrar solo la entrada de root
grep "^root:" /etc/passwd

# Comparar con shadow
echo "En /etc/passwd:"
grep "^root:" /etc/passwd
echo "En /etc/shadow:"
grep "^root:" /etc/shadow

# Notar que:
# - /etc/passwd es legible por todos los usuarios (ls -l /etc/passwd)
# - /etc/shadow solo es legible por root (ls -l /etc/shadow)
# - Las contraseñas cifradas están en /etc/shadow, no en /etc/passwd

# Ver los permisos de ambos archivos
ls -l /etc/passwd /etc/shadow
```

## Desafío 6: Verificar el cambio de contraseña

**Objetivo:** Comprobar que la nueva contraseña de root funciona correctamente. Verificar los cambios realizados es un paso fundamental en cualquier tarea de administración para asegurar que los cambios de configuración tuvieron el efecto deseado.

### Tareas:

1. Cerrar la sesión actual de root.
2. Volver a iniciar sesión como root utilizando la nueva contraseña.
3. Verificar que el acceso funciona correctamente.

### Solución:

```bash
# Salir de la sesión de root
exit

# Ahora deberías estar como ec2-user
whoami
# Verás: "ec2-user"

# Cambiar al usuario root con la nueva contraseña
su -

# Introducir la nueva contraseña cuando se solicite
# Si la contraseña es correcta, tendrás acceso como root
whoami
# Verás: "root"
```

## Desafío 7: Explorar políticas de contraseñas

**Objetivo:** Conocer las políticas de contraseñas del sistema y cómo se pueden configurar. Las políticas de contraseñas son cruciales para garantizar que los usuarios utilicen credenciales seguras, especialmente en entornos cloud donde los ataques son constantes.

### Tareas:

1. Verificar la configuración actual de políticas de contraseñas.
2. Identificar dónde se configuran estas políticas.
3. Comprender cómo afectan a la seguridad del sistema.

### Solución:

```bash
# Ver módulos PAM para las políticas de contraseñas
cat /etc/pam.d/system-auth | grep password

# Ver la configuración específica de complejidad
cat /etc/security/pwquality.conf

# Consultar la duración máxima de las contraseñas
chage -l root

# Este comando muestra:
# - Fecha del último cambio de contraseña
# - Fecha de caducidad de la contraseña
# - Información sobre la política de contraseñas para el usuario root

# Ver la configuración específica de este usuario
grep "^root:" /etc/shadow | cut -d: -f4-8
```

## Desafío 8: Documentar las buenas prácticas de seguridad

**Objetivo:** Reflexionar sobre buenas prácticas de seguridad relacionadas con la gestión de contraseñas en servidores cloud. La documentación de buenas prácticas y políticas de seguridad es tan importante como la implementación técnica para garantizar un entorno seguro.

### Tareas:

1. Crear un archivo de texto llamado "buenas_practicas.txt".
2. Documentar al menos 3 buenas prácticas de seguridad para contraseñas.
3. Incluir recomendaciones específicas para entornos AWS.

### Solución:

```bash
# Crear el archivo de buenas prácticas
cat > buenas_practicas.txt << 'EOF'
BUENAS PRÁCTICAS DE SEGURIDAD PARA CONTRASEÑAS EN AWS EC2

1. Utilizar contraseñas fuertes:
   - Mínimo 12 caracteres
   - Combinación de mayúsculas, minúsculas, números y símbolos
   - Evitar palabras del diccionario o información personal

2. Implementar autenticación por claves SSH en lugar de contraseñas:
   - Más seguro que las contraseñas tradicionales
   - Desactivar la autenticación por contraseña en sshd_config
   - Proteger las claves privadas con passphrase

3. Rotación regular de contraseñas:
   - Cambiar contraseñas cada 90 días
   - No reutilizar contraseñas anteriores
   - Utilizar sistemas de gestión de secretos para contraseñas de servicio

4. Recomendaciones específicas para AWS:
   - Utilizar IAM roles en lugar de credenciales hard-coded
   - Implementar el principio de mínimo privilegio
   - Activar la autenticación multifactor (MFA) para cuentas AWS
   - Monitorizar intentos de acceso con CloudTrail
EOF

# Ver el contenido del archivo
cat buenas_practicas.txt
```

## Conclusión

¡Felicitaciones por completar esta práctica sobre gestión de contraseñas en instancias EC2 con Amazon Linux 2! Has aprendido a cambiar la contraseña del usuario root, explorar el archivo shadow, entender las políticas de contraseñas y documentar buenas prácticas de seguridad.

Estas habilidades son fundamentales para la administración segura de servidores en entornos cloud. Recuerda que en entornos de producción, se recomienda utilizar claves SSH en lugar de contraseñas para el acceso remoto, pero entender la gestión de contraseñas sigue siendo un conocimiento esencial para cualquier administrador de sistemas.

## Recursos adicionales

- [Documentación oficial de Amazon Linux 2](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/amazon-linux-2-virtual-machine.html)
- [Mejores prácticas de seguridad para EC2](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-security.html)
- [Guía de endurecimiento de seguridad de Linux](https://linuxsecurity.expert/security-tools/linux-security-hardening-guide/)
