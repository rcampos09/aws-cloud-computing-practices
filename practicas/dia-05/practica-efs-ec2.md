# Práctica: Amazon EFS con Instancias EC2

## Introducción

Este desafío está diseñado para estudiantes de primer año de la asignatura "Introducción a Cloud Computing". El objetivo es aprender a configurar un sistema de archivos compartido (EFS) entre dos instancias EC2 en AWS Academy utilizando Amazon Linux 2.

Amazon EFS (Elastic File System) es un servicio de almacenamiento de archivos totalmente administrado que permite compartir archivos entre múltiples instancias EC2 de manera simultánea. En este desafío, crearás dos instancias EC2 que compartirán archivos através de un sistema EFS, simulando un entorno de trabajo colaborativo en la nube.

El usuario predeterminado en las instancias será **ec2-user**, y utilizarás comandos básicos de Linux para crear y verificar archivos compartidos.

Este desafío incluye instrucciones detalladas paso a paso para completar la configuración y verificar que el sistema funciona correctamente.

## Requisitos previos

- Acceso a AWS Academy
- Conocimiento básico de la consola de AWS
- Acceso a la terminal a través de la consola web de AWS
- Conocimiento básico de comandos Linux (mkdir, cat, echo)
- Recuerda que en Amazon Linux 2, el usuario predeterminado es **ec2-user**

## Objetivo del Desafío

Configurar un sistema de archivos compartido (EFS) que permita a dos instancias EC2 con Amazon Linux 2 acceder a los mismos archivos de manera simultánea, demostrando cómo múltiples servidores pueden compartir almacenamiento en AWS.

## Tareas

1. Crear dos instancias EC2 con Amazon Linux 2
2. Configurar la política de seguridad para permitir NFS
3. Crear y configurar un sistema de archivos EFS
4. Conectarse a ambas instancias y montar el EFS
5. Crear y verificar archivos compartidos entre instancias

## Tareas

1. Crear dos instancias EC2 con Amazon Linux 2
2. Configurar la política de seguridad para permitir NFS
3. Crear un sistema de archivos EFS
4. Configurar la política de seguridad del EFS
5. Conectarse a cada instancia y montar el EFS
6. Crear un archivo en una instancia
7. Verificar el archivo desde la segunda instancia

## Solución Paso a Paso

### Paso 1: Crear las instancias EC2

```
1. En AWS Academy, accede a la consola EC2
2. Haz clic en "Launch Instance"
3. Selecciona Amazon Linux 2 AMI (asegúrate de elegir la versión correcta)
4. Elige el tipo de instancia t2.micro (elegible para capa gratuita)
5. Configura:
   - Number of instances: 2
   - Network: Default VPC
   - Subnet: Acepta la sugerida
6. Añade etiquetas:
   - Name: EFS-Instance-1 (para la primera)
   - Name: EFS-Instance-2 (para la segunda)
7. Crea o selecciona un key pair
8. Lanza las instancias
   
Verás: Dos instancias EC2 ejecutándose en tu panel de instancias
```

### Paso 2: Configurar el grupo de seguridad para NFS

```
1. En la consola EC2, ve a "Security Groups"
2. Localiza el grupo de seguridad de tus instancias
3. Haz clic en "Edit inbound rules"
4. Añade una nueva regla:
   - Type: NFS
   - Protocol: TCP
   - Port: 2049
   - Source: Custom
   - En el campo de fuente, selecciona el mismo grupo de seguridad
5. Guarda los cambios
```

### Paso 3: Crear el sistema de archivos EFS

```
1. En AWS, busca "EFS" en el buscador de servicios
2. Haz clic en "Create file system"
3. Selecciona la VPC donde creaste las instancias
4. Elige "Customize" para configurar manualmente
5. Configura:
   - Name: MiEFS
   - Storage class: Standard
   - Throughput mode: Bursting
   - Performance mode: General Purpose
6. Haz clic en "Next"
7. En Network access, asegúrate de que el grupo de seguridad sea el mismo que tus instancias
8. Haz clic en "Next" y luego "Create"
```

### Paso 4: Obtener el comando de montaje

```
1. En la lista de sistemas de archivos EFS, haz clic en el EFS que creaste
2. Haz clic en "Attach"
3. Selecciona "Mount via DNS"
4. Copia el comando que se muestra (será similar a):
   sudo mount -t nfs4 -o nfsvers=4.1,rsize=1048576,wsize=1048576,hard,timeo=600,retrans=2,noresvport fs-12345678.efs.us-west-2.amazonaws.com:/ ~/efs
```

### Paso 5: Conectarse a la primera instancia

```
1. En la consola EC2, selecciona EFS-Instance-1
2. Haz clic en "Connect"
3. Elige "EC2 Instance Connect" o "Session Manager"
4. Haz clic en "Connect"

Verás: Una terminal web conectada a tu primera instancia con el prompt: [ec2-user@ip-xxx-xxx-xxx-xxx ~]$
```

### Paso 6: Crear la carpeta efs y montar el sistema

```bash
# Crear la carpeta efs
mkdir -p ~/efs

# Montar el EFS (reemplaza con tu comando de montaje)
sudo mount -t nfs4 -o nfsvers=4.1,rsize=1048576,wsize=1048576,hard,timeo=600,retrans=2,noresvport fs-12345678.efs.us-west-2.amazonaws.com:/ ~/efs

# Verificar que está montado
df -h | grep efs
```

### Paso 7: Crear un archivo en el EFS

```bash
# Crear un archivo con un mensaje
echo "Este archivo está en el EFS compartido desde la Instancia 1" > ~/efs/mi_archivo.txt

# Verificar que el archivo existe
ls -la ~/efs/
cat ~/efs/mi_archivo.txt
```

### Paso 8: Conectarse a la segunda instancia

```
1. En la consola EC2, selecciona EFS-Instance-2
2. Haz clic en "Connect"
3. Elige "EC2 Instance Connect" o "Session Manager"
4. Haz clic en "Connect"

Verás: Una terminal web conectada a tu segunda instancia con el prompt: [ec2-user@ip-xxx-xxx-xxx-xxx ~]$
```

### Paso 9: Montar el EFS en la segunda instancia

```bash
# Crear la carpeta efs
mkdir -p ~/efs

# Montar el EFS (usa el mismo comando que en la primera instancia)
sudo mount -t nfs4 -o nfsvers=4.1,rsize=1048576,wsize=1048576,hard,timeo=600,retrans=2,noresvport fs-12345678.efs.us-west-2.amazonaws.com:/ ~/efs

# Verificar que está montado
df -h | grep efs
```

### Paso 10: Verificar el archivo compartido

```bash
# Ver el archivo creado desde la primera instancia
ls -la ~/efs/

# Leer el contenido del archivo
cat ~/efs/mi_archivo.txt

# Crear un archivo adicional desde la segunda instancia
echo "Archivo creado desde la Instancia 2" > ~/efs/archivo_instancia2.txt

# Verificar ambos archivos
ls -la ~/efs/
```

## Resultados Esperados

Al completar este desafío deberías ver:

1. Dos instancias EC2 ejecutándose
2. Un sistema de archivos EFS creado
3. Ambas instancias con el EFS montado correctamente
4. El archivo `mi_archivo.txt` visible en ambas instancias
5. El contenido del archivo mostrando el mensaje de la Instancia 1
6. Un segundo archivo `archivo_instancia2.txt` visible en ambas instancias

## Conclusión

¡Felicitaciones! Has configurado exitosamente un sistema de archivos compartido usando Amazon EFS. Esto demuestra cómo múltiples instancias EC2 pueden acceder a los mismos archivos de manera simultánea, lo que es muy útil para aplicaciones que requieren almacenamiento compartido.

## Recursos adicionales

- [Documentación oficial de Amazon EFS](https://docs.aws.amazon.com/efs/latest/ug/whatisefs.html)
- [Best practices para EFS](https://docs.aws.amazon.com/wellarchitected/latest/security-pillar/shared-responsibility-for-efs.html)
