.. index::
   single: Bundles; Best Practices

Bundles
=======
Un bundle es un directorio con una estructura definida que contiene desde 
clases hasta controladores y recursos de red. Aunque los bundles son 
muy flexibles, se recomienda siempre usar métodos (no se a que practicas se refieren) 
para distribuirlos de una forma mas efectiva.

.. index::
   pair: Bundles; Naming Conventions

.. _bundles-naming-conventions:

Bundle Name
-----------
Un bundle es también un PHP namespace. El namespace debe tener todos 
los requerimientos de compatibilidad de los namespaces y el nombre 
de las clases, con respecto a la versión PHP 5.3. Esto significa 
que el bundle debe empezar con un segmento del vendor, seguido de un 0 
o de segmentos de otras categorías, y debe terminar con el shortname 
del namespace, que debe terminar con la palabra Bundle.


Un namespace se vuelve un bundle tan pronto se añade una clase al bundle. 
La clase que se crea debe seguir las siguientes reglas:

* Use únicamente caracteres alfanuméricos y underscores;
* La primera letra de cada palabra debe empezar con mayúscula;
* Usar un nombre que tenga máximo dos palabras y sirva para su descripción;
* El nombre debe empezar con el nombre del vendor y puede tener seguido al vendor las categorías de los namespaces.
* El nombre debe terminar con la palabra ``Bundle``.

Un ejemplo de algunos nombres validos de namesapces y de las clases:

+-----------------------------------+--------------------------+
| Namespace                         | Bundle Class Name        |
+===================================+==========================+
| ``Acme\Bundle\BlogBundle``        | ``AcmeBlogBundle``       |
+-----------------------------------+--------------------------+
| ``Acme\Bundle\Social\BlogBundle`` | ``AcmeSocialBlogBundle`` |
+-----------------------------------+--------------------------+
| ``Acme\BlogBundle``               | ``AcmeBlogBundle``       |
+-----------------------------------+--------------------------+

Por convención, el método ``getName()`` de la clase del bundle debe retornar el 
nombre de la clase.

.. note::

    Si usted comparte su bundle, usted debe usar el mismo nombre 
    del repositorio con el nombre de la clase de su bundle. 
    (por ejemplo ``AcmeBlogBundle`` y no ``BlogBundle``).

.. note::

    Los bundles de Symfony2 por defecto no añaden el prefijo ``Symfony`` 
    al nombre de la clase y siempre agregan un ``Bundle`` subnamesapce: por ejemplo: 
    :class:`Symfony\\Bundle\\FrameworkBundle\\FrameworkBundle`.

Directory Structure
-------------------

La estructura básica de un ``HelloBundle`` debe ser de la siguiente forma:

.. code-block:: text

    XXX/...
        HelloBundle/
            HelloBundle.php
            Controller/
            Resources/
                meta/
                    LICENSE
                config/
                doc/
                    index.rst
                translations/
                views/
                public/
            Tests/

El directorio ``XXX`` hace referencia al nombre del namespace del bundle.

Estos archivos se deben crear obligatoriamente:

* ``HelloBundle.php``;
* ``Resources/meta/LICENSE``: Toda la licencia de nuestro código;
* ``Resources/doc/index.rst``: Este archivo contiene la documentación de nuestro Bundle. 


.. note::

    Esto nos permite que diferentes herramientas puedan identificar 
    esta estructura para trabajar con ella.

La máxima cantidad de sub-directorios para almacenar nuestras clases y 
archivos que mas usamos debe ser de 2 niveles, y si se usan mas 
niveles se recomienda guardar allí los archivos menos usados y 
nuestras clases que no sean fundamentales para la aplicación.

El directorio del bundle es de solo escritura. Si necesita escribir 
archivos temporales, archívelas en la ``cache/`` o el ``log/`` del 
directorio principal de la aplicación. Hay herramientas que crean 
archivos en el directorio del bundle, pero solo si dichos archivos 
van a hacer parte del repositorio.

Las siguientes clases y archivos se deben ubicar en los siguientes directorios:

+---------------------------+-----------------------------+
| Type                      | Directory                   |
+===========================+=============================+
| Controllers               | ``Controller/``             |
+---------------------------+-----------------------------+
| Translation files         | ``Resources/translations/`` |
+---------------------------+-----------------------------+
| Templates                 | ``Resources/views/``        |
+---------------------------+-----------------------------+
| Unit and Functional Tests | ``Tests/``                  |
+---------------------------+-----------------------------+
| Web Resources             | ``Resources/public/``       |
+---------------------------+-----------------------------+
| Configuration             | ``Resources/config/``       |
+---------------------------+-----------------------------+
| Commands                  | ``Command/``                |
+---------------------------+-----------------------------+

Classes
-------

La estructura del directorio del bundle es usada con la jerarquía del namespace. 
Por ejemplo el ``HelloController`` es almacenado en 
``Bunlde/HelloBundle/Controller/HelloController.php`` y el 
nombre completo de la clase es ``Bundle\HelloBundle\Controller\``.

Todas las clases y archivos deben se codificados con los :doc:`standards</contributing/code/standards>` 
de Symfony2.

Algunas clases deben ser fachadas (facades no se como traducir esto) y 
deben ser lo mas cortas posible, como Commands, Helpers, Listeners, y Controllers.

Las clases que se usan para conectar con el Event Dispatcher deben terminar con 
el sufijo ``Listener``.

Las clases que sirven para usar excepciones deben almacenarse en 
el sub-namespace ``Exception``.

Vendors
-------

Un bundle no debe contener librerías PHP de terceros. Deben redireccionar al standard 
de auto carga usado por Symfony2. 

Un bundle no debe contener librerías de terceros escritas en 
JavaScript, CSS, u otro lenguaje.

Tests
-----

Un bundle debe tener un paquete de prueba escrito con PHPUnit y 
se debe almacenar en el directorio ``Tests/``. 
Estos test deben seguir los siguientes principios:

* El paquete de pruebas debe poder ejecutarse desde cualquier aplicación 
con un comando simple de ``phpunit``;
*	Las pruebas funcionales deben ser para probar la respuesta de 
salida e información de perfiles si usted  posee;
*	La cobertura del código de prueba deber ser del 95% con respecto 
a nuestro código.


.. note::
   El paquete de pruebas no debe contener los scripts de 
   ``AllTests.php``, si no que debe redireccionar al archivo ``phpunit.xml.dist``.

Documentation
-------------

Todas las clases y funciones deben tener completamente su PHPDoc.

Si llega haber documentación mas extensiva debe 
estar en el formato de :doc:`reStructuredText
</contributing/documentation/format>`, y debe almacenarse en el 
directorio ``Resources/doc/``; ``Resources/doc/index.rst`` 
Este archivo es obligatorio.


Controllers
-----------

Como buena practica, un controller en un bundle es usado p
ara ser distribuido hacia otros controllers pero no debe extender la clase 
base de :class:`Symfony\\Bundle\\FrameworkBundle\\Controller\\Controller`.
Los controller pueden implementar :class:`Symfony\\Component\\DependencyInjection\\ContainerAwareInterface`
o extender de :class:`Symfony\\Component\\DependencyInjection\\ContainerAware`.

.. note::

	Los métodos de :class:`Symfony\\Bundle\\FrameworkBundle\\Controller\\Controller`
	son simplemente atajos para facilitar la curva de aprendizaje.

Templates
---------

Si un bundle usa templates, estos deben usar Twig. Un bundle no 
debe usar un layout principal, a menos que este proporcione una aplicación 
de trabajo completa.

Translation Files
-----------------

Si un bundle contiene mensajes con traducción, estos deben ser 
definidos con el formato XLIFF; El domino debe ser nombrado después 
del nombre del bundle. (``bundle.hello``).

Un bundle no debe sobrescribir mensajes existentes de otro bundle.

Configuration
-------------

Para permitir mas flexibilidad, un bundle debe proveer una 
configuración que se pueda configurar mediante los métodos incluidos en Symfony2.

Para una configuración simple, use los ``parameters`` por defecto que se usan en 
la configuración de Symfony2. Los parámetros de Symfony2 son simplemente pares 
de key/value; un value es un valor valido de PHP. El nombre del parámetro debe 
empezar con el nombre en minuscula del nombre del bundle usando 
underscores (``acme_hello`` para ``AcmeHelloBundle``, o ``acme_social_blog`` 
para ``Acme\Social\BlogBundle`` por ejemplo), aunque esta es una sugerencia 
para realizar una buena practica. El resto de los parámetros 
deben usar un punto (``.``) para separar sus diferentes 
partes (e.g. ``acme_hello.email.from``).

El usuario final debe proveer valores en cualquier archivo de configuración:

.. configuration-block::

    .. code-block:: yaml

        # app/config/config.yml
        parameters:
            acme_hello.email.from: fabien@example.com

    .. code-block:: xml

        <!-- app/config/config.xml -->
        <parameters>
            <parameter key="acme_hello.email.from">fabien@example.com</parameter>
        </parameters>

    .. code-block:: php

        // app/config/config.php
        $container->setParameter('acme_hello.email.from', 'fabien@example.com');

    .. code-block:: ini

        [parameters]
        acme_hello.email.from = fabien@example.com

Obtén los parámetros de la configuración desde el container con el código::

    $container->getParameter('acme_hello.email.from');

Inclusive si este mecanismo es bastante simple, se recomienda 
altamente usar la configuración semántica descrita en el cookbook.

Aprende mas desde el Cookbook
----------------------------

* :doc:`/cookbook/bundles/extension`

.. _standards: http://groups.google.com/group/php-standards/web/psr-0-final-proposal
