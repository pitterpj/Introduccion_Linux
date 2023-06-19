### COMANDO FIND EN LINUX

## sintaxis básica: find [desde_donde_buscar] [opciones] [termino_de_búsqueda]

# Búsqueda por nombre
- find / -name [archivo]
- find / -iname [archivo] (en caso de no saber si esta escrito en mayus o minus)

# Búsqueda por nombre y formato
- find / -name "*.txt"

# Búsqueda por tipo: 
-     Filtros     Descripción

        d           directorio o carpeta
        f           archivo
        l           enlace simbólico
        c           dispositivos de caracteres
        b           dispositivos de bloque  

- Ejemplo: find / type d

# Búsqueda por nombre y tipo
- find / -type f -name [archivo]

# Búsqueda por prioridad
- find / -user [name_user]
- find / -group [name_group]

# Búsqueda por permisos
- find / -perm 664 (solo tienen rw)
- find / -perm -664 (que contienen rw)
- find / -perm 4000 (que tenga permiso SUID)

# Búsqueda por nombre incompleto
- find / -name [algo]\* (asi añadimos el principio)
- find / -name [algo]\*.sh (asi añadimos el principio y el tipo (.sh))
- find / -name \*[algo]\* (asi añadimos la mitad)
- find / -name \*[algo] (asi añadimos el final)






