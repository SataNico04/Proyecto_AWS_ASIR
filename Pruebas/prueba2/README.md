# Creación de una interconexión de VPC

Una VPC es una red privada dentro de nuestro sistema AWS, y estás, se pueden dividir en subredes que pueden ser públicas o privadas. Es decir, imaginemos que tenemos un servidor con un apache instalado y otro servidor con una base de datos. Estos tienen sus IPs públicas y se pueden conectar sin problemas, pero alguien con malas intenciones también puede, por lo que debemos crear redes virtuales para evitar que esto pase. Lo normal sería crear una VPC en un rango concreto, ej. 172.1.0.0/24, y dos subredes, una pública que permita las conexiones entrantes del exterior para visualizar la página web y una subred privada que conecte el servidor web con la base de datos. De esta forma, lo único accesible por los usuarios sería la página web.

--