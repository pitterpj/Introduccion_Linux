### PERMISOS EN LINUX / 777 / 755 / 700

# Esto es algo que los usuarios de Window no pueden. En Linux hay una serie de comandos para ocultar o restringir información a otros usuarios. Como usuario de Window tomamos directamente el control con todos los permisos para hacer cualquier acción con los datos. Esto refleja un fallo de seguridad del sistema operativo. Eso es porque window está diseñado para multiusuario. Linux al ser diseñado para el trabajo en red, la seguridad de la información que almacenemos es fundamental.

# Existe tres niveles de permisos:
    - Permisos de propietarios
    - Permisos del grupo
    - Permisos del resto de usuarios ("otros")

- En los sistemas de red siempre existe la figura del administrador o root. Encargado de crear, dar de baja usuarios así como establecer privilegios.

- Permisos de propietarios: Aquel que genera el archivo/carpeta. Él y solo él tiene acceso a la información en los archivos/directorios

- Permisos de grupo: Cada usuario pertenece a un grupo de trabajo. Los permisos se asignan a grupos en general y todo aquel del grupo tiene los mismos permisos.

- Permisos de "otros": Privilegios que otros usuarios pueden tener sobre un archivo/directorio. Son los que no son dueños ni pertenecen al grupo de trabajo.

## LECTURA E INTERPRETACIÓN

# ¿Cómo ver los permisos? "ls -l"
- Apareceran 10 caracteres que se denominan máscara. El primero (de izq-der) hace referencia al tipo. Los otros 9 en bloques de 3, hacen referencia a los permisos que se les conceden respectivamente propietario,grupo y otros.

- El primer caracter de los archivos puede ser el siguiente
# Permiso          Identifica
    -               Archivo
    d               Directorio
    b               Archivo de bloques especiales
    c               Archivo de caracteres especiales
    l               Archivo de vinculo o enlace
    p               Archivo especial de cauce
    s               Socket

- Los siguientes 9 caracteres son los permisos otrogador a los usuarios.
# Permiso          Identifica
    -               Sin permiso
    r               Lectura
    w               Escritura
    x               Ejecución
    s	            setuid, setgid o sticky

# ¿Qué es cada permiso?
- Lectura: permite visualizar el contenido del archivo.
- Escritura: permite modificar el contenido del archivo.
- Ejecución: permite ejecutar el archivo como si de un programa ejecutable se tratase.

# Nota: Si no se dispone del permiso de ejecución, no podremos acceder a dicho directorio (aunque utilicemos el comando “cd”), ya que esta acción será denegada.

# Ejemplo: - rw- rw- r--
    - es un archivo (-)
    - el propietario puede leerlo y modificarlo pero no ejecutarlo. Observa que este archivo no es ejecutable, el permiso de ejecución aparece deshabilitado (rw-)
    - el grupo tiene permisos idénticos al propietario (rw-)
    - los demás usuarios sólo tienen permiso para leer el archivo, pero no lo pueden modificar ni ejecutar (r–).


## ASIGNACIÓN DE PERMISOS

# El comando "chmod" (change mode)
- Permite modificar la máscara para realizar más o menos operaciones sobre archivos/directorios. Es decir puedes dar/quitar permisos a cada tipo de usuario.

# chown [new_propietario] [archivo] 
- Cambiará el propiertario de un archivo/directorio

# chown :grupo [archivo] 
- Cambiará el grupo de un archivo/directorio

# chown propietario:grupo [nombre del archivo] 
- Cambiará el propietario y el grupo

# El comando "chgrp" (change group)
- Cambiará el grupo de un archivo/directorio

# Parámetro     Nivel           Descripción
    u           dueño           propietario del archivo/directorio
    g           grupo           grupo al que pertenece el archivo/directorio
    o           otros           Todos los demás usuarios
    a           todos           Todos

# Operador  Descripción
    +       agregar permiso        
    -       quitar permiso
    =       configuración del permiso

- Dar permisos a propietario: chmod +rwx [archivo/directorio]
- Dar permisos a grupo: chmod g+rwx [archivo/directorio]
- Dar permisos a otros: chmod o+rwx [archivo/directorio]

# Ejemplo: Vamos a suponer que desees agregar permiso de escritura en el archivo prueba.txt para un usuario
-   chmod u+w prueba.txt
# Ejemplo2: En caso de que quieras dar permisos de lectura y escritura a tu grupo
-   chmod g+rw prueba.txt
# Ejemplo3: vamos a suponer que el archivo prueba.txt deberá estar con todos los permisos disponibles para el grupo
-   chmod g=rwx prueba.tx

## NOTACIÓN OCTAL DE PERMISOS

- Usar chmod con valores numéricos es una tarea bastante práctica. En vez de utilizar letras como símbolos para cada permiso, se usan números.

#   Permiso   Valor Octal     	Descripción
    – – –   	0           	no se tiene ningún permiso
    – – x   	1           	solo permiso de ejecución
    – w –   	2           	solo permiso de escritura
    – w x   	3           	permisos de escritura y ejecución
    r – –   	4           	solo permiso de lectura
    r – x   	5           	permisos de lectura y ejecución
    r w –   	6           	permisos de lectura y escritura
    r w x   	7           	todos los permisos 

# Ejemplo: Permiso rwx para todos: "chmod 777 [archivo/directorio]"

# Ejemplo2: Asigna permisos de lectura y escritura (6) para el propietario, solo lectura para los usuarios del mismo grupo (4) y ningún permiso para otros usuarios (0).
-   chmod 640 prueba.txt 

## CONTROL DE ATRIBUTOS DE FICHEROS EN LINUX - CHATTR y lsLSATTR

# lsattr -> permite listar los atributos asignados a un archivo o directorio.

# chattr -> permite asignar atributos especiales.

#     atributo        descripción
        +A              Cuando accedemos al fichero no se modifique el registro time (No quedará registrada la fecha del último acceso)
        +a              Se pueden agregar datos al archivo pero no eliminarlo.
        +c              Activamos que el fichero se comprima automáticamente en el disco por el kernel.
        +i              El fichero no se puede modifcar o borrar (por nadie)
        +u              Permite recuperar un fichero aunque se borre
        +e              Cuando se borre algo sobreescribir sus bloques con 0 para hacerlo irrecuperable

## PERMISOS ESPECIALES STICKY BIT

- Su objetivo es que solo el usuario creador (o un root) pueda eliminar o renombrar un archivo en sistemas donde todos los usuarios tienen permisos de lectura y escritura. Además, su activación está representada con la letra T mayúscula en el listado de permisos de un directorio.

- Para asignarlo se hace con "chmod 1777 [archivo/directorio]" o "chmod +t [archivo/directorio]". Como vemos en octal sería añadir un 1 o un 0 en función de si queremos activarlo o no.

## PERMISOS ESPECIALES - SUID Y SGID

# SUID -> para usuarios. "chmod u+s" o "chmod 4*** <archivo>". Todo usuario que ejecute un archivo lo hará como si fuese el propietario.

# SGID -> para gurpos. "chmod g+s" o "chmod 2*** <archivo>". Todo usuario que ejecute el archivo lo hace como si perteneciera al grupo del archivo.

# En ambos aplica que la letra estará en mayúscula si tiene permiso de ejecución y en minúscula si no los tiene.


## EXTRAS

#  Código   Opción          Descripción
-   -R      recursive       El cambio  se aplica a todos los archivos y subdirectorios dentro de una carpeta.
-   -v      verbose         Después del comando se emite un diagnóstico de todos los archivos procesados.
-   -c      changes         Después del comando se muestra un diagnóstico para todos los archivos que se han modificado.
-   -f      silent          Se silencian los mensajes de error.

# Cambio recursivo de permisos: "find directorio -type d -exec chmod 755 {} +"
- De esta forma ejecutarás un cambio de permisos al directorio directorio y a todos sus subdirectorios, a 755.

### CAPABILITIES

- Las capabilities en Linux son permisos asignados a procesos sin necesidad de privilegios de superusuario. Permiten acciones privilegiadas controladas, como administración de red o tareas del sistema, sin otorgar acceso completo de root. Proporcionan mayor seguridad y control en la gestión de permisos en el sistema operativo Linux.

