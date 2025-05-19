# Práctica: Automatización con Cron y Scripts

## Introducción

Esta práctica está diseñada para la asignatura "Introducción a Cloud Computing". El objetivo es aprender a crear un pequeño programa (script) que se ejecute automáticamente cada cierto tiempo en una instancia EC2 con Amazon Linux 2.

Imagina que puedes programar tu computadora para que realice tareas por ti mientras duermes o estás ocupado. Eso es exactamente lo que aprenderás en esta práctica: a programar tu servidor para que haga cosas automáticamente en momentos específicos.

Para esta práctica, utilizarás una instancia EC2 con Amazon Linux 2. El usuario predeterminado es **ec2-user**.

Cada desafío incluye una explicación sencilla, las tareas a realizar y la solución paso a paso.

## Requisitos previos

- Acceso a AWS Academy
- Una instancia EC2 con Amazon Linux 2 
- Acceso a la terminal a través de la consola web de AWS
- Haber completado las prácticas anteriores

## Objetivo de la Práctica

Aprender a crear un pequeño programa (script) y configurar el sistema para que lo ejecute automáticamente cada cierto tiempo.

## Tareas

1. Instalar el servicio que permite programar tareas
2. Crear un pequeño programa que escriba mensajes
3. Programar el sistema para que ejecute el programa automáticamente
4. Ver cómo el programa se ejecuta sin nuestra intervención
5. Entender cómo funciona la programación de tareas

## Desafío 1: Instalar el programador de tareas (cron)

**Objetivo:** Aprender a instalar y activar el servicio que nos permitirá programar tareas automáticas.

### Tareas:

1. Conectarse a la instancia EC2.
2. Obtener permisos de administrador.
3. Instalar y activar el servicio cron.

### Solución:

```bash
# Conectarse a la instancia (esto lo haces a través de la consola web de AWS)

# Obtener permisos de administrador
sudo su -

# Instalar el servicio cron (en Amazon Linux 2 se llama "cronie")
yum install cronie -y

# Iniciar el servicio
systemctl start crond

# Configurar para que se inicie automáticamente cuando arranque el servidor
systemctl enable crond

# Verificar que el servicio está funcionando
systemctl status crond

# Deberías ver un mensaje que indica que el servicio está activo (running)
```

## Desafío 2: Crear un script simple

**Objetivo:** Crear un pequeño programa (script) que escriba mensajes en un archivo.

### Tareas:

1. Crear una carpeta para guardar el script.
2. Crear un archivo con instrucciones básicas.
3. Hacer que el archivo sea ejecutable.
4. Probar que funciona.

### Solución:

```bash
# Crear una carpeta para nuestro script
mkdir -p /home/scripts

# Crear el archivo del script
cat > /home/scripts/mensaje.sh << 'EOF'
#!/bin/bash

# Obtener la fecha y hora actual
FECHA=$(date "+%Y-%m-%d %H:%M:%S")

# Escribir un mensaje con la fecha en un archivo
echo "[$FECHA] ¡Hola! Este mensaje fue generado automáticamente" >> /home/scripts/registro.log

# Contar cuántas veces se ha ejecutado el script
TOTAL=$(wc -l < /home/scripts/registro.log)
echo "[$FECHA] Este script se ha ejecutado $TOTAL veces" >> /home/scripts/registro.log
EOF

# Hacer que el script sea ejecutable
chmod +x /home/scripts/mensaje.sh

# Probar que el script funciona
/home/scripts/mensaje.sh

# Ver el resultado
cat /home/scripts/registro.log

# Deberías ver dos líneas con mensajes y la fecha actual
```

## Desafío 3: Programar la ejecución automática

**Objetivo:** Configurar el sistema para que ejecute nuestro script automáticamente cada 5 minutos.

### Tareas:

1. Abrir el editor de tareas programadas.
2. Agregar una configuración para ejecutar nuestro script.
3. Verificar que la tarea se ha programado correctamente.

### Solución:

```bash
# Abrir el editor de tareas programadas (crontab)
crontab -e

# Si es la primera vez, puede preguntarte qué editor usar. Elige 1 para nano (es el más sencillo)

# En el editor, agrega esta línea:
*/5 * * * * /home/scripts/mensaje.sh

# Esta configuración significa:
# - */5: cada 5 minutos
# - * * * *: cualquier hora, cualquier día, cualquier mes, cualquier día de la semana

# Guarda y cierra el editor:
# Si usas nano: presiona Ctrl+O, luego Enter, y finalmente Ctrl+X

# Verificar que la tarea se ha programado
crontab -l

# Deberías ver la línea que acabas de agregar
```

## Desafío 4: Ver la ejecución automática

**Objetivo:** Observar cómo el script se ejecuta automáticamente sin nuestra intervención.

### Tareas:

1. Esperar unos minutos.
2. Verificar que el script se ha ejecutado automáticamente.
3. Ver el archivo de registro en tiempo real.

### Solución:

```bash
# Espera 5 minutos (puedes hacer otras cosas mientras tanto)

# Ver el archivo de registro
cat /home/scripts/registro.log

# Deberías ver más entradas que las dos iniciales, 
# lo que indica que el script se ha ejecutado automáticamente

# Para ver el archivo en tiempo real (actualización continua)
watch -n 10 cat /home/scripts/registro.log

# Este comando muestra el archivo y lo actualiza cada 10 segundos
# Presiona Ctrl+C cuando quieras salir
```

## Desafío 5: Crear un script personalizado

**Objetivo:** Crear tu propio script que haga algo interesante y programarlo para que se ejecute automáticamente.

### Tareas:

1. Crear un nuevo script con tu nombre.
2. Hacer que el script muestre información del sistema.
3. Programarlo para que se ejecute cada 10 minutos.

### Solución:

```bash
# Crear un nuevo script con tu nombre (reemplaza "tu_nombre" con tu nombre real)
cat > /home/scripts/script_tu_nombre.sh << 'EOF'
#!/bin/bash

# Obtener fecha y hora
FECHA=$(date "+%Y-%m-%d %H:%M:%S")

# Archivo de registro
ARCHIVO="/home/scripts/sistema_tu_nombre.log"

# Información del sistema
echo "[$FECHA] == INFORMACIÓN DEL SISTEMA ==" >> $ARCHIVO
echo "[$FECHA] Uso de memoria:" >> $ARCHIVO
free -h | grep "Mem:" >> $ARCHIVO
echo "[$FECHA] Espacio en disco:" >> $ARCHIVO
df -h / | grep -v "Filesystem" >> $ARCHIVO
echo "[$FECHA] Usuarios conectados: $(who | wc -l)" >> $ARCHIVO
echo "[$FECHA] Tiempo que lleva encendido el servidor:" >> $ARCHIVO
uptime >> $ARCHIVO
echo "[$FECHA] ===========================" >> $ARCHIVO
echo "" >> $ARCHIVO
EOF

# Hacer que el script sea ejecutable
chmod +x /home/scripts/script_tu_nombre.sh

# Probar que funciona
/home/scripts/script_tu_nombre.sh

# Ver el resultado
cat /home/scripts/sistema_tu_nombre.log

# Programar para que se ejecute cada 10 minutos
crontab -e

# Añadir esta línea (sin borrar la anterior)
*/10 * * * * /home/scripts/script_tu_nombre.sh

# Guardar y salir del editor
```

## Conclusión

¡Felicitaciones! Has aprendido a:

1. Instalar y configurar el servicio cron, que permite programar tareas automáticas
2. Crear scripts básicos que realizan acciones y guardan resultados
3. Programar la ejecución automática de estos scripts a intervalos específicos
4. Verificar que las tareas automáticas funcionan correctamente

Esta habilidad de automatización es muy valiosa en el mundo de la informática y especialmente en la nube. Te permite configurar tareas que se ejecutarán sin tu supervisión, como:
- Hacer copias de seguridad automáticas
- Monitorear el rendimiento de un servidor
- Limpiar archivos antiguos
- Enviar informes periódicos
- Y muchas otras tareas repetitivas

Ahora, cada vez que necesites que algo se ejecute regularmente, sabrás cómo configurarlo para que suceda de forma automática.

## Recursos adicionales

- [Tutorial oficial de crontab](https://www.adminschoice.com/crontab-layout-and-environment)
- [Generador de expresiones cron](https://crontab.guru/) - Una herramienta online para entender y crear expresiones de tiempo para cron
