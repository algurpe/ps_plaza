

#Prestashop en c9.io



## 1) Registrarse en c9.io


[Entrar en c9.io](http://www.c9.io)

## 2) Iniciar un nuevo proyecto PHP

En Dashboard de c9 -> Create new workspace -> New Workspace -> PHP

## 3) Descargar prestashop

En la terminal escribimos

    wget https://www.prestashop.com/download/old/prestashop_1.6.0.14.zip

## 4) Descomprimir zip
Renombramos el archivo `prestashop_1.6.0.14.zip` por `prestashop.zip` y escribimos

    unzip prestashop.zip
    

## 5) Recomendable, dar permisos 777:

Entramos en `/workspace/prestashop/` recién descomprimida con:

    cd prestashop

Para dar permisos 777 a todo pegamos esto:

    find -exec chmod 777 {} \;

Movemos todo su contenido a la carpeta principal que tendrá el 
nombre del proyecto de c9 arrastrando con el ratón. Para seleccionar puedes
seleccionar el primero pulsar MAYUSCULULAS hacer click en el último.

- Borramos la carpeta Prestashop vacia

Vamos atras de `/workspace/prestashop` debemos volver a `/workspace` con:

    cd ..

## 6) Creamos la base de datos con mysql-ctl:

    mysql-ctl start

## 7) Arrancando la instalación:
En clou9 pulsamos sobre `Run Project` para arrancar apache & MySQL 
y en `Preview` -> `Preview Running application`.


## 8) Información sobre su tienda
En esta página ponemos nombre a la tienda y pais, el resto de datos
será para crear el usuario administrador de la tienda, especificamos:
nombre, apellido, email y contraseña


## 9) Configura tu base de datos 

rellenando los siguientes campos los datos de base de datos creada por `mysql-ctl start` son:


Solicita                                      | Valor a escribir
--------------------------------------------  | --------
Dirección del servidor de la base de dados    | 0.0.0.0
Nombre de la base de datos                    | c9
usuario de la base de datos                   | masclic
Contraseña de la base de datos                | Dejar en blanco

Una vez puestos estos datos damos al botón verde que dice
¡Comprueba la conexión de tu base de datos ahora!

Debería dar correcto, pulsamos siguiente y comenzará la instalación.

- Una vez terminada la instalación elminamos la carpeta `install`.

Entramos en la ruta de administración y veremos que cambia el directorio admin por unos 8 caracteres extra 
estilo admin23821ld3s3d por seguridad la renombramos a admin321 la carpeta para mayor facilidad de recordarla.

## 10) Hábilitar SSL forzado en todas las páginas.

c9.io requiere SSL, de no estar activo el frontoffice 
puede mostrarnos pantalla en blanco, en el backoffice de prestashop, 
`Preferencias`-> `Configuración` 
Configurala `Habilitar SSL` en `SI` y `Guardar`. 

Luego verás que se creó una nueva opción de `Forzar SSL` debajo 
, ponla en `SI` y `Guardar`

Captura de configuración con la que se solucióna 
la pantalla en blanco del frontoffice.

![](http://i.gyazo.com/a6eccd89d957335c12afc35d57defd63.png)


## Para evitar problemas con git, la carpeta modules contiene directorios .git 

Una opción es eliminarlos, entra en la carpeta modules desde la terminal.

    cd modules

Borra todos los directorios .git con find así [Asegurate de estar en workspace/modules $]:

    find -type d -name ".git" -exec rm -rf {} \;

Vuelve a la ruta principal con :

    cd ..

La otra opción es ignorarlos con .gitignore añadiendo. (TENGO dudas probarlo...)

    modules/**/.git
    
## Permisos para mayor seguridad

Estableciendo los permisos de carpetas a 755 adecuados pero ignorando a los ocultos:

    find \( ! -regex '.*/\..*' \) -type d -exec chmod 755 {} \;
    
Estableciendo los permisos de archivos a 744 adecuados pero ignorando a los ocultos:

    find \( ! -regex '.*/\..*' \) -type f -exec chmod 744 {} \;

Ahora puede ser conveniente asegurarse de que apache se apaga.

    apachectl stop

Vuelve a ejecutar en c9 `Run Project`

Git prestashop
--------------------------------------------------------------------------------
    
    
Comprueba la configuración de tu instalación de git con:

    git config --list

Si tenemos ya configurado git localmente con nuestro email y nombre nos
muestra esto:

    user.name=David Zapatería
    user.email=masclic.com@gmail.com
    core.editor=nano
    core.whitespace=off
    core.excludesfile=~/.gitignore
    advice.statusuoption=false
    color.ui=true
    push.default=simple
    core.repositoryformatversion=0
    core.filemode=true
    core.bare=false
    core.logallrefupdates=true


### Configurar GIT por primera vez.
(Solo si no salen nuestros datos user.name y user.email con `config --list`)

    $ git config --global user.name "Javier Martinez"
    $ git config --global user.email javiermartinez@gmail.com

### Iniciamos un repo local 
desde la terminal para comenzar a realizar seguimiento.

    git init

### Crear un repo en github / bitbucket y añadiendolo como remoto.
Crea un repo y copia/pega la URL de el así:

    git remote add origin https://github.com/zapcode/prestashop2-c9.git


### El archivo .gitignore

crea un archivo llamado .gitignore y para evitar problemas para ver el 
archivo en c9.io activa ver ocultos en icono de ajustes del la ventana 
del explorador de archivos:

![](http://i.gyazo.com/afccd5e1d75958606ab1f48e8ced1168.png)

Ejemplo de .gitignore ligero. (Ignora todo menos lo que indiquemos)

    /*
    !modules/
    !themes/
    !override/
    !README.md
    !.gitignore

Ejemplo de .gitignore c9 pesado (5826 archivos)

    # Directorios de Cache
    cache/class_index.php
    cache/smarty/cache/*
    !cache/smarty/cache/index.php
    cache/smarty/compile/*
    !cache/smarty/compile/index.php
    cache/tcpdf/*
    !cache/tcpdf/index.php
    # conexión de base de datos
    config/settings.inc.php
    # ignorar logs
    log/*.log
    # ignorar imagenes
    img/*
    !img/index.php
    !img/*/index.php
    # ignorar tools cache pero no los index
    tools/smarty*/cache/*.php
    !tools/smarty*/cache/index.php
    tools/smarty*/compile/*.php
    !tools/smarty*/compile/index.php
    # Ignorar cache de los themes
    themes/default/cache/*.js
    themes/default/cache/*.css
    # Ignorar archivos del editor c9.io
    .c9/*


Obserba que se ignora el archivo `config/settings.inc.php` ya que
contiene los datos de conexión de la base de datos. 

Contenido del archivo `settings.inc.php`

    <?php
    define('_DB_SERVER_', '0.0.0.0');
    define('_DB_NAME_', 'c9');
    define('_DB_USER_', 'masclic');
    define('_DB_PASSWD_', '');
    define('_DB_PREFIX_', 'ps_');
    define('_MYSQL_ENGINE_', 'InnoDB');
    define('_PS_CACHING_SYSTEM_', 'CacheMemcache');
    define('_PS_CACHE_ENABLED_', '0');
    define('_COOKIE_KEY_', 'bVLYDJMAOjmoLTIY1GgmjHy5LmI3juhNQzSTgBfMGULx1UO7JVNJP23G');
    define('_COOKIE_IV_', 'BHgc9IUm');
    define('_PS_CREATION_DATE_', '2015-04-21');
    if (!defined('_PS_VERSION_'))
    	define('_PS_VERSION_', '1.6.0.14');
    define('_RIJNDAEL_KEY_', 'ZaN3iv5gcyho3Hvrt5nLgHTKKfz0LKZs');
    define('_RIJNDAEL_IV_', 'TkgilQ0DO/N6GLCAgQyk5Q==');

Este archivo es creado durante la instalación y rellenado según el 
transcurso de ella. En este caso al ser de c9 proyecto de pruebas y la base
de datos es la que crea para pruebas c9 no es importante pero este archivo 
debe no ser visible para tus proyectos en producción.

### Añadiendo archivos para siguiente commit

    git add .

### Nuestro primer commit

    git commit -m "primer commit proyecto funcional"

### Nuestro primer envio a remoto

Puede que nos solicite el user/password de github/bitbucket

    git push origin master
    
### Comprueba el estado de tu repo local con:

    git status

### Actualiza tu proyecto desde el repo remoto.

    git pull

### Crea un archivo README.md (recomendable).

Escribe sobre tu proyecto notas e instrucciones de uso así como 
una pequeña descripción
Este archivo será interpretado usando lenguaje markdown.


## MIGRACIÓN DE BASE DE DATOS CON phpmyadmin

Exportamos la base de datos, esto es posible hacerlo desde:

- Terminal / SSH
- PHPMyAdmin
- Backoffice de prestashop

Para hacerlo desde phpmyadmin primero accedemos a phpmyadmin
en cloud9 que la ruta es:





