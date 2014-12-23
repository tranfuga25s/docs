Configuración
#############

Configurar una applicacion CakePHP es pan comido. Luego de que has
instalado CakePHP, crear unaa application web basica requiere solo
que configures la base de datos.

Sin embargo, hay otros pasos de configurar que se pueden utilizar 
para utilizar la arquitectura flexible de CakePHP. Puedes agregar
facilmente funcionalidades nuevas a las heredadas desde el nucleo
de CakePHP, agregar o modificar diferentes mapeos de URL (routes),
y definir reglas de infleccion adicionales o diferentes.

.. index:: database.php, database.php.default
.. _database-configuration:

Configuración de la Base de Datos
=================================

CakePHP espera que los detalles de configuración para la base de datos
se encuentren ubicados en un archivo en ``app/Config/database.php``. 
Un ejemplo de este archivo de configuracion puede ser encontrado en 
``app/Config/database.php.default``. La configuracion final debería de 
ser algo similar a esto::

    class DATABASE_CONFIG {
        public $default = array(
            'datasource'  => 'Database/Mysql',
            'persistent'  => false,
            'host'        => 'localhost',
            'login'       => 'cakephpuser',
            'password'    => 'c4k3roxx!',
            'database'    => 'my_cakephp_project',
            'prefix'      => ''
        );
    }

El array de conexión $default es utilizado excepto que se especifique 
la propiedad ``$useDbConfig`` en un modelo. Por ejemplo, si mi aplicación
tiene una base de datos antigua además de la predeterminada, podría utilizarla
en mis modelos creando un nuevo array de conexión similar a $default, y 
colocar ``public $useDbConfig = 'legacy';`` en los modelos que corresponda.

Completa los pares clave/valor en la configuración según tus necesidades.

datasource
    El nombre de la fuente de datos que utiliza esta array de conexión.
    Ejemploss: Database/Mysql, Database/Sqlserver, Database/Postgres, Database/Sqlite.
    Puedes usar la :term:`plugin syntax` para indicar una fuente de datos de un plugin.
persistent
    Utilizar o no una conexión persistente a la base de datos.
host
    La dirección IP o el nombre de host del servidor de base de datos.
login
    Nombre de usuario de la cuenta.
password
    Contraseña para la cuenta.
database
    Nombre de la base de datos que utilizará la conexión.
prefix (*opcional*)
    Prefijo que se agrega al nombre de todas las tablas en la base de datos.
    Si tus tablas no poseen prefijos, dejar vacío.
port (*optional*)
    El puerto TCP o  socket Unix usado para connectar al servidor.
encoding
    Indica el juego de caracteres a utilizar cuando se envian comandos SQL a
    el servidor. Se utilizará la codificación predeterminada de la base de datos
    para todas las bases de datos excepto DB2. Si deseas utilizar codificación UTF-8 
    con mysql/mysqli debes usar 'utf8' sin los guiones.
schema
    Usado en bases de datos PostgreSQL para especificar que esquema utilizar.
unix_socket
    Usado por drivers que suportan coneccion mediante archivos de socket unix. Si estas
    utilizando postgres y quieres oncextarte mediante sockets, deja en blanco el nombre
    de host.
ssl_key
    La ruta al archivo de clave SSL. ( Solo soportado por MySQL, requiere PHP
    5.3.7+).
ssl_cert
    La ruta al archivo de certificado SSL. (Solo soportado por MySQL,
    requiere PHP 5.3.7+).
ssl_ca
    La ruta al archivo de certificado de autoridad SSL. (Solo soportado por MySQL,
    requiere PHP 5.3.7+).
settings
    Un arreglo de clave/valor que seran enviados al servidor mediante comandos 
    ``SET`` cuando la conexión es creada. Esta opción solo está soportada por 
    MySQL, Postgres, y SQLserver en este momento.

.. versionchanged:: 2.4
    Las claves ``settings``, ``ssl_key``, ``ssl_cert`` y ``ssl_ca`` fueron
    agregadas en la versión 2.4.

.. note::

    El prefixo es para las tablas, **no** modelos. Por ejemplo, si 
    creas una tabla de union entre Apple y Flavor, el nombre de la tabla
    será prefix\_apples\_flavors (**no** prefix\_apples\_prefix\_flavors), 
    y setea el prefijo a 'prefix\_'.

En este punto sería recomentable de que le peges una mirada a
:doc:`/getting-started/cakephp-conventions`. Nombrar las tablas correctamente
( y la adicion de algunas columnas ) puede agregar funcionalidad
gratuita y evitarte configuraciones extras. Por ejemplo, si tu tabla
 tiene el nombre caja\_grande, tu modelo CajaGrande, tu controlador
 CajaGrandeController, todo trabaja automaticamente junto. por convencion
 usa guión bajo, minusculas, y plural en los nombres de las tablas - 
 Por ejemplo:
bakers, pastry\_stores, and savory\_cakes.

.. todo::

    Add information about specific options for different database
    vendors, such as SQLServer, Postgres and MySQL.

Rutas de Clases Adicionales
===========================

Ocacionalmente es util la habilidad de compartir clases MVC entre
aplicaciones del mismo sistema.. Si quieres tener el mismo controlador
en dos aplicación, puedes utilizar el archivo bootstrap.php de CakePHP
para traer esas clases a tu aplicacion.

Utilizando :php:meth:`App::build()` en bootstrap.php podemos definir 
rutas adiciones en donde CakePHP buscará por clases::

    App::build(array(
        'Model' => array(
            '/path/to/models',
            '/next/path/to/models'
        ),
        'Model/Behavior' => array(
            '/path/to/behaviors',
            '/next/path/to/behaviors'
        ),
        'Model/Datasource' => array(
            '/path/to/datasources',
            '/next/path/to/datasources'
        ),
        'Model/Datasource/Database' => array(
            '/path/to/databases',
            '/next/path/to/database'
        ),
        'Model/Datasource/Session' => array(
            '/path/to/sessions',
            '/next/path/to/sessions'
        ),
        'Controller' => array(
            '/path/to/controllers',
            '/next/path/to/controllers'
        ),
        'Controller/Component' => array(
            '/path/to/components',
            '/next/path/to/components'
        ),
        'Controller/Component/Auth' => array(
            '/path/to/auths',
            '/next/path/to/auths'
        ),
        'Controller/Component/Acl' => array(
            '/path/to/acls',
            '/next/path/to/acls'
        ),
        'View' => array(
            '/path/to/views',
            '/next/path/to/views'
        ),
        'View/Helper' => array(
            '/path/to/helpers',
            '/next/path/to/helpers'
        ),
        'Console' => array(
            '/path/to/consoles',
            '/next/path/to/consoles'
        ),
        'Console/Command' => array(
            '/path/to/commands',
            '/next/path/to/commands'
        ),
        'Console/Command/Task' => array(
            '/path/to/tasks',
            '/next/path/to/tasks'
        ),
        'Lib' => array(
            '/path/to/libs',
            '/next/path/to/libs'
        ),
        'Locale' => array(
            '/path/to/locales',
            '/next/path/to/locales'
        ),
        'Vendor' => array(
            '/path/to/vendors',
            '/next/path/to/vendors'
        ),
        'Plugin' => array(
            '/path/to/plugins',
            '/next/path/to/plugins'
        ),
    ));

.. note::

    Toda ruta adicional configurada debería de ser hecha al principio de el archivo
    bootstrap.php de tu aplicacion. Esto asegura que las rutas agregadas estan disponibles
    para el resto de la aplicación.


.. index:: core.php, configuration

Configuración del núcleo
========================

Cada aplicación en CakePHP contiene un archivo de configuración para
determinar el comportamiento interno.
``app/Config/core.php``. Este archivo es una colección de clases de 
configuración y definiciones de variables y constantes que determinan
como tu aplicación se comportará. Antes de ingrear en variables en 
particular, es necesario que familiarices con :php:class:`Configure`, 
la clase de registro de configuraciones de CakePHP's.

Configuration del nucleo CakePHP
--------------------------------

La clase :php:class:`Configure` es usada para administar un conjunto de
variables de configuración del núcleo de CakePHP. Esas variables pueden
ser encontradas en ``app/Config/core.php``. A continuación una descripcion
de cada variable y como afecta tu aplicación CakePHP.

debug
    Cambia la salida de información de depuración de CakePHP.
    0 = Modo Produccion. Sin salida.
    1 = Muestra errores y alertas.
    2 = Muestra errores, alertas y comandos SQL. [Los comandos SQL son mostrados
    solamente cuando agregas ``$this->element('sql\_dump')`` en el layout.]

Error
    Configura el gestor de errores utilizado para administrar los errores de tu
    aplicación. De manera predeterminada se utiliza :php:meth:`ErrorHandler::handleError()`.
    Mostrará los errores utilizando :php:class:`Debugger`, cuando debug > 0
    y cuando debug = 0 registrará los errores en el log con :php:class:`CakeLog`.

    Sub-claves:

    * ``handler`` - retrollamada - La función que se llamará para manejar los errores.
    Puedes colocar cualquier tipo de retrollamada, incluyendo funciones anonimas.
    * ``level`` - int - El nivel de error que estas interesado en capturar.
    * ``trace`` - boolean - Incluir historial de funciones llamadas en los archivos de log.

Exception
    Configura el gestor de excepciones utilizado para las excepciones no retenidoas. De manera
    predeterminada ErrorHandler::handleException() es usado. Mostrará una pagina HTML para 
    la excepcion, y mientras debug > 0, los errores tales como Missing Controller serán mostrados.
    Cuando debug = 0, los errores del framework serán convertidos en errores genericos HTTP.
    Para más información sobre el manejo de excepciones, vea la sección :doc:`exceptions`.

.. _core-configuration-baseurl:

App.baseUrl
    Si no deseas o no puede habilitar el modulo mod\_rewrite (o otro módulo compatile)  en tu
    servidor, necesitarás utilizar el embellecedor de URLs integrado en CakePHP. 
    En ``/app/Config/core.php``, descomenta la linea que es similar a::

        Configure::write('App.baseUrl', env('SCRIPT_NAME'));

    Además elimina los archivos .htaccess en las siguientes ubicaciones::

        /.htaccess
        /app/.htaccess
        /app/webroot/.htaccess


    Esto hará que las URLs de tu aplicacion sean simiar a esto:
    www.example.com/index.php/controllername/actionname/param 
    en vez de 
    www.example.com/controllername/actionname/param.

    Si estas instalando CakePHP en un servidor web que no es Apache, puedes
    encontrar instrucciones de como habilitar la reescritura de URL para
    otros servidores en la seccion :doc:`/installation/url-rewriting`.
    
App.encoding
    Define que codificación de caracteres utiliza tu aplicación. Esta codificacion
    es usada para generar el juego de caracteres en el layout, y codificar entidades.
    Debería de ser la misma que está especificada en tu base de datos.
    
Routing.prefixes
    Descomenta esta definición si deseas obtener las ventajas de las rutas prefijadas
    de CakePHP como admin. Coloca esta variable del prefijo de nombres que deseas usar
    Mas información mas adelante.
Cache.disable
    Cuando está colocado en verdadero, el cache persistente está deshabilitado en todo 
    el sistema. Esto hará que todas las escrituras/lecturas a :php:class:`Cache` fallen.
Cache.check
    Si está seteado en verdader, habilita el cache de vistas. Habilitacion en el controlador
    es necesario, pero esta variable es la que habilita la detección de esas preferencias.
Session
    Contiene un arreglo de las preferencias para usar como configuración de la sesión.
    La clave predeterminada es usada para definir la configuración predeterminada para usar
    en la sesiones, cualquier preferencia declarada aquí sera sobreescrita.

    Sub-claves

    * ``name`` - El nombre que se usa en la `cookie`. Normalmente es 'CAKEPHP'.
    * ``timeout`` - El numero de minutos que deseas que la sesión tenga vida.
      Este tipo es manejado por CakePHP.
    * ``cookieTimeout`` - El número de munitos que deseas que la `cookie` de sesión viva.
    * ``checkAgent`` - Deseas que la cadena de agente de usuario sea verificada cuando se
      inicia la sesión? Puede ser que desees colocar este valor en falso, cuando estas 
      lidiando con versiones viejas de IE, Chrome Frame o ciertos dispositivos que navegan
       y pedidos AJAX.
    * ``defaults`` - El valor predeterminado para usar como base para tu sesiones.
      Los cuatro valores incluidos son: php, cake, cache, database.
    * ``handler`` - Puede ser usado para habilitar administradores de sesión personalizados.
      Espera un arreglo de funciones llamables (callables), que puede ser utilizado con
      `session_save_handler`. Usando esta opción agregará automaticamente `session.save_handler`
      a las opciones de ini.
    * ``autoRegenerate`` - Habilitando esta preferencia, habilita la renovación automatica
      de sesiones, y sessionsids que cambian frequentemente.
      Vea :php:attr:`CakeSession::$requestCountdown`.
    * ``ini`` - Un array asociative de valores adiciones para utilizar con ini.

    Los valores predeterminados son:

    * 'php' - Utiliza las preferencias definidas en tu archivo php.ini.
    * 'cake' - Guarda los archivos de sesion en el directorio /tmp de CakePHP.
    * 'database' - Usa sesiones en base de datos de CakePHP.
    * 'cache' - Usa la clase de Cache para guardar las sesiones.

    Para definir un administrador de sesiones personalizado, agregado en 
    ``app/Model/Datasource/Session/<nombre>.php``. Asegurate de que la clase implementa
    :php:interface:`CakeSessionHandlerInterface` y coloca la clave Session.handler
    a <nombre>

    Para utilizar las sesiones en base de datos, ejecuta el esquema 
    ``app/Config/Schema/sessions.php`` usando el comando de consola: 
    ``cake schema create Sessions``

Security.salt
    Una cadena aleatora utilizada para hashing seguro.
Security.cipherSeed
    Una cadena aleator numerica (solo digitos) usado para encriptar y 
    desencrimptar cadenas.
Asset.timestamp
    Agrega una cadena temporal conteniendo la ultima modificación de los
    archivos de contenido (CSS, JavaScript, Image) en la url de descarga
    cuando se utiliza los ayudantes.
    Los valores validos son:
    (boolean) false - No hace nada (predeterminado)
    (boolean) true - Agrega la cadena de tiempo cuando debug > 0
    (string) 'force' - Agrega la cadena de tiempo cuando debug >= 0
Acl.classname, Acl.database
    Constantess usadas por el Control de Acceso por Lista (ACL) de CakePHP's.
    Vea el capítulo de Control de Acceso por Lista para más información.

.. note::
    La configuración de Cache también se encuentra en core.php — Cubriremos eso
    más adelante, sigue leyendo..

La clase :php:class:`Configure` puede se utilizada para leer y escribir preferencias
del núcleo al vuelo. Esto es especialmente útil si deseas cambiar el valor de debug
en una sección específica de tu código para verificar la lógica de tu aplicación.

Constantes de Configuración
---------------------------

Mientras la mayoría de las opciones de configuracion son manejadas por Configure, existen
algunas pocas constantes que CakePHP usas durante su ejecución.

.. php:const:: LOG_ERROR

    Constante de Error. Usada para diferenciar el registro de errores y depuración. Actualmente
    PHP soporta LOG\_DEBUG.

Core Cache Configuration
------------------------

CakePHP uses two cache configurations internally. ``_cake_model_`` and ``_cake_core_``.
``_cake_core_`` is used to store file paths, and object locations. ``_cake_model_`` is
used to store schema descriptions, and source listings for datasources. Using a fast
cache storage like APC or Memcached is recommended for these configurations, as
they are read on every request. By default both of these configurations expire every
10 seconds when debug is greater than 0.

As with all cached data stored in :php:class:`Cache` you can clear data using
:php:meth:`Cache::clear()`.


Configure Class
===============

.. php:class:: Configure

Despite few things needing to be configured in CakePHP, it's
sometimes useful to have your own configuration rules for your
application. In the past you may have defined custom configuration
values by defining variable or constants in some files. Doing so
forces you to include that configuration file every time you needed
to use those values.

CakePHP's Configure class can be used to store and retrieve
application or runtime specific values. Be careful, this class
allows you to store anything in it, then use it in any other part
of your code: a sure temptation to break the MVC pattern CakePHP
was designed for. The main goal of Configure class is to keep
centralized variables that can be shared between many objects.
Remember to try to live by "convention over configuration" and you
won't end up breaking the MVC structure we've set in place.

This class can be called from
anywhere within your application, in a static context::

    Configure::read('debug');

.. php:staticmethod:: write($key, $value)

    :param string $key: The key to write, can use be a :term:`dot notation` value.
    :param mixed $value: The value to store.

    Use ``write()`` to store data in the application's configuration::

        Configure::write('Company.name','Pizza, Inc.');
        Configure::write('Company.slogan','Pizza for your body and soul');

    .. note::

        The :term:`dot notation` used in the ``$key`` parameter can be used to
        organize your configuration settings into logical groups.

    The above example could also be written in a single call::

        Configure::write(
            'Company',
            array(
                'name' => 'Pizza, Inc.',
                'slogan' => 'Pizza for your body and soul'
            )
        );

    You can use ``Configure::write('debug', $int)`` to switch between
    debug and production modes on the fly. This is especially handy for
    AMF or SOAP interactions where debugging information can cause
    parsing problems.

.. php:staticmethod:: read($key = null)

    :param string $key: The key to read, can use be a :term:`dot notation` value

    Used to read configuration data from the application. Defaults to
    CakePHP's important debug value. If a key is supplied, the data is
    returned. Using our examples from write() above, we can read that
    data back::

        Configure::read('Company.name');    //yields: 'Pizza, Inc.'
        Configure::read('Company.slogan');  //yields: 'Pizza for your body
                                            //and soul'

        Configure::read('Company');

        //yields:
        array('name' => 'Pizza, Inc.', 'slogan' => 'Pizza for your body and soul');

    If $key is left null, all values in Configure will be returned.

.. php:staticmethod:: check($key)

    :param string $key: The key to check.

    Used to check if a key/path exists and has not-null value.

    .. versionadded:: 2.3
        ``Configure::check()`` was added in 2.3

.. php:staticmethod:: delete($key)

    :param string $key: The key to delete, can use be a :term:`dot notation` value

    Used to delete information from the application's configuration::

        Configure::delete('Company.name');

.. php:staticmethod:: version()

    Returns the CakePHP version for the current application.

.. php:staticmethod:: config($name, $reader)

    :param string $name: The name of the reader being attached.
    :param ConfigReaderInterface $reader: The reader instance being attached.

    Attach a configuration reader to Configure. Attached readers can
    then be used to load configuration files. See :ref:`loading-configuration-files`
    for more information on how to read configuration files.

.. php:staticmethod:: configured($name = null)

    :param string $name: The name of the reader to check, if null
        a list of all attached readers will be returned.

    Either check that a reader with a given name is attached, or get
    the list of attached readers.

.. php:staticmethod:: drop($name)

    Drops a connected reader object.


Reading and writing configuration files
=======================================

CakePHP comes with two built-in configuration file readers.
:php:class:`PhpReader` is able to read PHP config files, in the same
format that Configure has historically read. :php:class:`IniReader` is
able to read ini config files. See the `PHP documentation <http://php.net/parse_ini_file>`_
for more information on the specifics of ini files.
To use a core config reader, you'll need to attach it to Configure
using :php:meth:`Configure::config()`::

    App::uses('PhpReader', 'Configure');
    // Read config files from app/Config
    Configure::config('default', new PhpReader());

    // Read config files from another path.
    Configure::config('default', new PhpReader('/path/to/your/config/files/'));

You can have multiple readers attached to Configure, each reading
different kinds of configuration files, or reading from
different types of sources. You can interact with attached readers
using a few other methods on Configure. To see check which reader
aliases are attached you can use :php:meth:`Configure::configured()`::

    // Get the array of aliases for attached readers.
    Configure::configured();

    // Check if a specific reader is attached
    Configure::configured('default');

You can also remove attached readers. ``Configure::drop('default')``
would remove the default reader alias. Any future attempts to load configuration
files with that reader would fail.


.. _loading-configuration-files:

Loading configuration files
---------------------------

.. php:staticmethod:: load($key, $config = 'default', $merge = true)

    :param string $key: The identifier of the configuration file to load.
    :param string $config: The alias of the configured reader.
    :param boolean $merge: Whether or not the contents of the read file
        should be merged, or overwrite the existing values.

Once you've attached a config reader to Configure you can load configuration files::

    // Load my_file.php using the 'default' reader object.
    Configure::load('my_file', 'default');

Loaded configuration files merge their data with the existing runtime configuration
in Configure. This allows you to overwrite and add new values
into the existing runtime configuration. By setting ``$merge`` to true, values
will not ever overwrite the existing configuration.

Creating or modifying configuration files
-----------------------------------------

.. php:staticmethod:: dump($key, $config = 'default', $keys = array())

    :param string $key: The name of the file/stored configuration to be created.
    :param string $config: The name of the reader to store the data with.
    :param array $keys: The list of top-level keys to save. Defaults to all
        keys.

Dumps all or some of the data in Configure into a file or storage system
supported by a config reader. The serialization format
is decided by the config reader attached as $config. For example, if the
'default' adapter is a :php:class:`PhpReader`, the generated file will be a PHP
configuration file loadable by the :php:class:`PhpReader`

Given that the 'default' reader is an instance of PhpReader.
Save all data in Configure to the file `my_config.php`::

    Configure::dump('my_config.php', 'default');

Save only the error handling configuration::

    Configure::dump('error.php', 'default', array('Error', 'Exception'));

``Configure::dump()`` can be used to either modify or overwrite
configuration files that are readable with :php:meth:`Configure::load()`

.. versionadded:: 2.2
    ``Configure::dump()`` was added in 2.2.

Storing runtime configuration
-----------------------------

.. php:staticmethod:: store($name, $cacheConfig = 'default', $data = null)

    :param string $name: The storage key for the cache file.
    :param string $cacheConfig: The name of the cache configuration to store the
        configuration data with.
    :param mixed $data: Either the data to store, or leave null to store all data
        in Configure.

You can also store runtime configuration values for use in a future request.
Since configure only remembers values for the current request, you will
need to store any modified configuration information if you want to
use it in subsequent requests::

    // Store the current configuration in the 'user_1234' key in the 'default' cache.
    Configure::store('user_1234', 'default');

Stored configuration data is persisted in the :php:class:`Cache` class. This allows
you to store Configuration information in any storage engine that :php:class:`Cache` can talk to.

Restoring runtime configuration
-------------------------------

.. php:staticmethod:: restore($name, $cacheConfig = 'default')

    :param string $name: The storage key to load.
    :param string $cacheConfig: The cache configuration to load the data from.

Once you've stored runtime configuration, you'll probably need to restore it
so you can access it again. ``Configure::restore()`` does exactly that::

    // restore runtime configuration from the cache.
    Configure::restore('user_1234', 'default');

When restoring configuration information it's important to restore it with
the same key, and cache configuration as was used to store it. Restored
information is merged on top of the existing runtime configuration.

Creating your own Configuration readers
=======================================

Since configuration readers are an extensible part of CakePHP,
you can create configuration readers in your application and plugins.
Configuration readers need to implement the :php:interface:`ConfigReaderInterface`.
This interface defines a read method, as the only required method.
If you really like XML files, you could create a simple Xml config
reader for you application::

    // in app/Lib/Configure/MyXmlReader.php
    App::uses('Xml', 'Utility');
    class MyXmlReader implements ConfigReaderInterface {
        public function __construct($path = null) {
            if (!$path) {
                $path = APP . 'Config' . DS;
            }
            $this->_path = $path;
        }

        public function read($key) {
            $xml = Xml::build($this->_path . $key . '.xml');
            return Xml::toArray($xml);
        }

        // As of 2.3 a dump() method is also required
        public function dump($key, $data) {
            // code to dump data to file
        }
    }

In your ``app/Config/bootstrap.php`` you could attach this reader and use it::

    App::uses('MyXmlReader', 'Configure');
    Configure::config('xml', new MyXmlReader());
    ...

    Configure::load('my_xml');

.. warning::

        It is not a good idea to call your custom configure class ``XmlReader`` because that
        class name is an internal PHP one already:
        `XMLReader <http://php.net/manual/en/book.xmlreader.php>`_

The ``read()`` method of a config reader, must return an array of the configuration information
that the resource named ``$key`` contains.

.. php:interface:: ConfigReaderInterface

    Defines the interface used by classes that read configuration data and
    store it in :php:class:`Configure`

.. php:method:: read($key)

    :param string $key: The key name or identifier to load.

    This method should load/parse the configuration data identified by ``$key``
    and return an array of data in the file.

.. php:method:: dump($key)

    :param string $key: The identifier to write to.
    :param array $data: The data to dump.

    This method should dump/store the provided configuration data to a key identified by ``$key``.

.. versionadded:: 2.3
    ``ConfigReaderInterface::dump()`` was added in 2.3.

.. php:exception:: ConfigureException

    Thrown when errors occur when loading/storing/restoring configuration data.
    :php:interface:`ConfigReaderInterface` implementations should throw this
    error when they encounter an error.

Built-in Configuration readers
------------------------------

.. php:class:: PhpReader

    Allows you to read configuration files that are stored as plain PHP files.
    You can read either files from your ``app/Config`` or from plugin configs
    directories by using :term:`plugin syntax`. Files **must** contain a ``$config``
    variable. An example configuration file would look like::

        $config = array(
            'debug' => 0,
            'Security' => array(
                'salt' => 'its-secret'
            ),
            'Exception' => array(
                'handler' => 'ErrorHandler::handleException',
                'renderer' => 'ExceptionRenderer',
                'log' => true
            )
        );

    Files without ``$config`` will cause an :php:exc:`ConfigureException`

    Load your custom configuration file by inserting the following in app/Config/bootstrap.php:

        Configure::load('customConfig');

.. php:class:: IniReader

    Allows you to read configuration files that are stored as plain .ini files.
    The ini files must be compatible with php's ``parse_ini_file`` function, and
    benefit from the following improvements

    * dot separated values are expanded into arrays.
    * boolean-ish values like 'on' and 'off' are converted to booleans.

    An example ini file would look like::

        debug = 0

        Security.salt = its-secret

        [Exception]
        handler = ErrorHandler::handleException
        renderer = ExceptionRenderer
        log = true

    The above ini file, would result in the same end configuration data
    as the PHP example above. Array structures can be created either
    through dot separated values, or sections. Sections can contain
    dot separated keys for deeper nesting.

.. _inflection-configuration:

Inflection Configuration
========================

CakePHP's naming conventions can be really nice - you can name your
database table big\_boxes, your model BigBox, your controller
BigBoxesController, and everything just works together
automatically. The way CakePHP knows how to tie things together is
by *inflecting* the words between their singular and plural forms.

There are occasions (especially for our non-English speaking
friends) where you may run into situations where CakePHP's
:php:class:`Inflector` (the class that pluralizes, singularizes, camelCases, and
under\_scores) might not work as you'd like. If CakePHP won't
recognize your Foci or Fish, you can tell CakePHP about your
special cases.

Loading custom inflections
--------------------------

You can use :php:meth:`Inflector::rules()` in the file
``app/Config/bootstrap.php`` to load custom inflections::

    Inflector::rules('singular', array(
        'rules' => array(
            '/^(bil)er$/i' => '\1',
            '/^(inflec|contribu)tors$/i' => '\1ta'
        ),
        'uninflected' => array('singulars'),
        'irregular' => array('spins' => 'spinor')
    ));

or::

    Inflector::rules('plural', array('irregular' => array('phylum' => 'phyla')));

Will merge the supplied rules into the inflection sets defined in
lib/Cake/Utility/Inflector.php, with the added rules taking precedence
over the core rules.

Bootstrapping CakePHP
=====================

If you have any additional configuration needs, use CakePHP's
bootstrap file, found in app/Config/bootstrap.php. This file is
executed just after CakePHP's core bootstrapping.

This file is ideal for a number of common bootstrapping tasks:

- Defining convenience functions.
- Registering global constants.
- Defining additional model, view, and controller paths.
- Creating cache configurations.
- Configuring inflections.
- Loading configuration files.

Be careful to maintain the MVC software design pattern when you add
things to the bootstrap file: it might be tempting to place
formatting functions there in order to use them in your
controllers.

Resist the urge. You'll be glad you did later on down the line.

You might also consider placing things in the :php:class:`AppController` class.
This class is a parent class to all of the controllers in your
application. :php:class:`AppController` is a handy place to use controller
callbacks and define methods to be used by all of your
controllers.


.. meta::
    :title lang=en: Configuration
    :keywords lang=en: finished configuration,legacy database,database configuration,value pairs,default connection,optional configuration,example database,php class,configuration database,default database,configuration steps,index database,configuration details,class database,host localhost,inflections,key value,database connection,piece of cake,basic web
