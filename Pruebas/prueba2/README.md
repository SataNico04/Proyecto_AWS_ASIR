# Creación de una interconexión de VPC

Una VPC (Virtual Private Cloud) en AWS es una red virtual aislada dentro de la nube de Amazon donde puedes lanzar y gestionar recursos como instancias EC2, bases de datos o balanceadores de carga. Funciona como si fuera una red tradicional en un centro de datos, permitiéndote definir rangos de IP, subredes públicas o privadas, tablas de rutas, gateways y reglas de firewall (security groups y NACLs), todo con un control total sobre la conectividad interna y externa.

--

Empecemos creando la VPC principal, entramos en la opción ``VPC`` y le damos a ``Crear VPC``:

![alt text](img/image.png)

Aquí dentro podremos elegir entre la configuración simple y la compleja. Yo elegiré la completa para ver todas las posibles configuraciones.

Lo primero y más simple de entender es el nombre, le podemos poner el que qeramos, yo le voy a poner ``Principal``.

**Bloque de CIDR IPv4** hace referencia al rango de IPs que va a tener nuestra red. Por defecto viene la ``10.0.0.0/16``con un total de ``65.536 IPs``. Dependiendo de nuestras necesidades, podemos cambiar el rango aumentando la máscara de subred, no puede ser más bajo. En esta ocasión, dejaré el rango en ``10.0.0.0/24`` con ``256 IPs``. Por otro lado, la opción de ``IPv6`` la dejaré desactivada.

Respecto a las zonas de disponibilidad, lo optimo sería poner al menos 2, para que, si una falla, la otra siga funcionando, además, las ``instancias de base de datos`` requieren más de una zona de disponibilidad.

La cantidad de subredes va en función de lo que necesitemos, para esta prueba que voy a hacer solo necesito ``1 pública`` y ``2 privadas``.

![alt text](img/image-6.png)

Los ``Gateways NAT`` sirven para que las ``VPC`` tengan _internet_. Esto puede ser útil dependiendo de la situación. Podemos elegir que una tenga Gateway una sola zona, o todas las de la red.

La vista previa quedaría tal que así:

![alt text](img/image-1.png)

Vemos varias cosas que no hemos configurado como tal, pero de momento vamos a crear la ``VPC``.

![alt text](img/image-2.png)

Genial, nuestra ``VPC`` se ha creado con exito, turno de revisar los recursos que se han creado automáticamente.

![alt text](img/image-3.png)

En la imagen vemos 3 tablas, 2 de ellas son las tablas privadas rutas solo llevan a la red que hemos creado:

![alt text](img/image-4.png)

A diferencia de la pública, que tiene una ruta al gateway:

![alt text](img/image-5.png)

> Tened en cuenta que una subred pública sin acceso a internet... no tiene nigún sentido, se volvería una red aisalada o privada.

Por último, tenemos la ``puerta de enlace de internert``, cuya única función es permitir el accesao a internet de las subredes públicas. Además de estas, existen varias configuraciones respecto a las ``VPCs``, pero verlas todas aquí sería un infierno interminable. Voy a crear directamente las intancias que vamos a utilizar, la EC2 y las RDS. Prefiero hacer una nueva entrada para estas 2, por lo que no explicaré aquí como crearlas y pasaré a la configuración directamente.

Si quereís ver las pruebas antes de continuar aquí tenéis las pruebas de las intancias [EC2](../prueba3/README.md) y la [RDS](../prueba4/README.md).

Continuando con la práctica, tenemos que modificar cierto parámetros de la ``configuración de red`` de la instancia, más concretamente la ``VPC`` y la ``Subred`` (podemos elegir la subred pública que queramos, no hay diferencia, pero debe ser pública), el grupo de seguridad que podríamos dejarlo por defecto.

![alt text](img/image-7.png)

Y también debemos asignarle una IP elástica a la instancia.

Respecto a la ``RDS`` tenemos que decirle que se conecte directamente a una instancia ``EC2``:

![alt text](img/image-8.png)

Esto pone la base de datos en la ``VPC`` automáticamente. Y configurar los grupos de subredes, donde pondremos la ``VPC`` y las ``subredes``.

![alt text](img/image-9.png)

![alt text](img/image-10.png)
> Las 2 son iguales.

![alt text](img/image-11.png)

Ya tendriamos todo lo necesario para hacer las pruebas. Empecemos por entrar a la instancia EC2 mediante SSH:

![alt text](img/image-12.png)

Podemos acceder a ella y vemos que tenemos conexión a internet (obviamente). En la parte de conectividad y seguridad de la ``Base de Datos``, veremos que no hay ninguna ``IP``, lo único que hay es un punto de enlace, eso es lo que usaremos en su lugar:

![alt text](img/image-13.png)

Voy a crear una base de datos _randome_ para probar que son 2 ``RDS`` diferentes:

![alt text](img/image-14.png)

![alt text](img/image-15.png)

Y, como última comprobación, vamos a verificar que álguien fuera de la red no puede acceder a las bases de datos:

![alt text](img/image-16.png)
> La conexión da error.