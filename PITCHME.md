---
¿Cómo crear una VPC y no morir en el intento?

Gilberto Vargas
---
Sobre el speaker:
- Entusiasta de la programación competitiva.
- También de la programación funcional.
- Un poco de Dev, un poco de Ops, pero más SRE.
- 3 años pegándole en kueski.
---
Motivación de la charla

- Pocas personas saben qué es una VPC y aun un menor número las han levantado de cero.
- Todas las personas que trabajan con AWS utilizan una VPC, este número es mucho más alto que el anterior.
- No busco que sean expertos, pero al menos que sepan dónde buscar.

---
Conceptos clave:

- VPC: La forma en que AWS logra "abstraer" un centro de datos. Son cerrados por sí solos.
- Subred: Segmentación de una red.
- Tabla de Ruteo: Reglas de direccionamiento del tráfico.
- Internet Gateway: Punto de salida al internet externo.
- Nat Gateway: Servidor que ayuda a enviar el tráfico des una red privada al internet externo.

---
Primer trampa: VPC CIDR

- CIDR: Classless Inter-Domain Routing.
- Una dirección IPv4 consta de 32 bits.
- Los 32 bits se reparten para segmentar las direcciones por subredes.
- Se divide en 4 octetos para hacer más sencilla la lectura.
- Muchos Devs tienen problemas entendiendo esto, pero es más sencillo de lo que parece.
---
La notación CIDR fuera de las redes.

- Normalmente, los hoteles utilizan números de 3 dígitos para numeras las habitaciones.
- Utilizan el dígito de las centenas para el piso.
- Utilizan las decenas y las unidades para el número de las habitaciones.
- Por lo regular, los hoteles no tienen 100 habitaciones por piso.

---
Ejemplos:
- La habitación 445 está en el cuarto piso.
- La habitación 101 probablemente sea la primera.

---
CIDR
- Es muy similar, solo que la cantidad de bits que utiliza para identificar el prefijo de una subred es variable.
- Se pone el sufijo `/n` para denotar cuántos bits se utilizan en el prefijo de la red.
- Ejemplos:
	- 10.12.0.0/16 utiliza 16 bits (2 octetos).
	- 10.0.0.0/8 utiliza 8 bits (1 octeto).
	- 172.16.0.0/12 utiliza 12 bits.
	- etc.
---
KISS: Keep it simple, stupid.
- Utiliza un prefijo 10.X.0.0/16 al crear tu VPC.
- Para segmentar las subredes utiliza un prefijo 10.X.Y.0.
- Tus direcciones tendran la forma 10.X.Y.Z, donde:
	- X es la VPC.
	- Y es la subred.
	- Z es la IP de la maquina en la subred.
- Limitantes:
	- Solo 251 direcciones por subred.
---
Segunda trampa: Manejando inventarios.

- No hay un estándar definido de cómo nombrar los recursos.
- Si el equipo no se pone de acuerdo, los inventarios pueden acabar muy heterogéneos.
- Nombres redundantes como "MiCompañiaServer", "GatitosAPI", "PerritosServerAPI"
---
¿Cómo manejar el inventario entonces?

- No importa cómo lo hagas, siempre y cuando sea ordenado, uniforme y se respete.
- Evita utilizar prefijos/sufijos como "server", "api", "service", "miempresa".
- Utiliza etiquetas que permitan distinguir entre ambientes productivos y de pruebas.
- De preferencia, utiliza una cuenta distinta para mantener producción separada de desarrollo.
---
Un hechizo simple pero inquebrantable:
- `%env-%resource`
- Utiliza solo minúsculas y guión bajo para los nombres de los recursos.
- Comunica la notación con el equipo y refuerza la utilización.

---
Hands on:
- Levantar un blog en wordpress.
- Levantar mysql en un servidor en una red privada sin acceso directo a internet.
---
Inventario:
	- Una VPC.
	- Una red "private"
	- Una red "public"
	- Un "Internet Gateway"
	- Un "Nat Gateway"
	- Un servidor para correr wordpress
	- Un servidor para correr mysql
---
Crear una VPC que se llame j4g, con un CIDR `10.4.0.0/16`
![-](assets/image/create_vpc.png)
---
Crear una subred "public" con un CIDR `10.4.1.0/24`
![-](assets/image/create_public.png)
---
Crear una subred "private" con un CIDR `10.4.2.0/24`
![-](assets/image/create_priv.png)
---
Crear un Internet Gateway "j4g"	y adjuntarlo a la VPC
![-](assets/image/create_ig.png)
![-](assets/image/attach_ig.png)
---
Crear un nat Gateway "j4g" asignandole una IP elastica nueva y ponerlo en la red public.
![-](assets/image/create_nat.png)
---
- Crear una tabla de ruteo privada:
- Asignarla a la VPC.
![-](assets/image/create_rt_priv.png)
---
![-](assets/image/edit_rt.png)
---
- Agregar la regla `0.0.0.0/0` para que salga por el NAT
![-](assets/image/edit_rt_priv.png)
---
- Crear una tabla de ruteo publica:
- Asignarla a la VPC.
- Agregar la regla `0.0.0.0/0` para que salga por el IG
![-](assets/image/edit_rt_pub.png)
---
Asignar las tablas de ruteo a las respectivas redes.
![-](assets/image/attach_rt.png)
---
![-](assets/image/attach_rt_priv.png)
---
- Crear un servidor con una ip pública en la red "public" para correr wordpress
![-](assets/image/ami_choose.png)
---
![-](assets/image/choose_instance.png)
---
![-](assets/image/blog_disk.png)
---
![-](assets/image/blog_server_config.png)
---
![-](assets/image/blog_sg.png)
---
![-](assets/image/blog_tags.png)
---
![-](assets/image/blog_lauch.png)
---
![-](assets/image/blog_starting.png)
---
![-](assets/image/blog_test.png)
---
Crear un servidor sin una ip pública en la red "private" para correr mysql
![-](assets/image/database_config.png)
---
![-](assets/image/db_tags.png)
---
![-](assets/image/db_launch.png)
---
![-](assets/image/db_test.png)
---
Configurar wordpress
![-](assets/image/running_blog.png)
---
Tercer trampa: Utilizar la consola (UI) de amazon para crear recursos productivos.

- Crear recursos "a pata" los hace poco auditables y cero reproducibles.
- Si nadie documenta la creación de los recursos, el conocimiento no se distribuye en el equipo.
- Mucho trabajo manual y repetitivo, con mayor probabilidad de error.

---
Ansible

- Utiliza la alternativa que mejor se adapte al equipo.
- Ansible:
	- Herramienta para automatización de configuraciones y construcción de servidores.
	- Es imperativa.
---
Terraform

- Herramienta de infraestructura como código enfocada a la orquestación.
- Es declarativa.
---
AWS-SDK

- Permite hacer invocaciones a AWS dentro de nuetro lenguaje de programación favorito.
- Sería reinventar ansible o terraform, pero puede adaptarse mejor a las necesidades del equipo.
---
Cloud Formation

AWS provee cloud formation para esto, pero simplemente "no lo haga compa":
	- Si actualizas la platilla de CF, destruye todo lo que cambia para reconstruirlo.
	- Si un recurso ya existe y no lo esperaba ahí falla.
	- Si se modifica de forma manual algún recurso puede romperse todo.
	- Es un archivo YAML que se hace muy dificil de gestionar.
---
Ejercicios
---
¿Qué pasa si configuro una instancia en una red privada y le adjunto una red pública?
---
¿Cómo se puede configurar una NAT de forma manual?
---
¿Qué IP pública tienen los servidores de las redes privadas?
---
¿Cómo harías una integración con un proveedor que te ofrece el servicio por VPN?
---
¿Qué es mejor? ¿VPN o Nat Gateway?
---
Conclusiones

Tras este viaje, deberías de saber:
- Cómo es eso de los CIDR y mas o menos cómo googlear para diseñar una red.
- En dónde poner atención al momento de configurar una VPC.
- Entender todos los componentes necesarios para montar una VPC.
- Segmentación de la red en una VPC.


---
Contacto

Gilberto Vargas. Site Reliability Engineer @ Kueski
- github.com/tachomex
- https://www.linkedin.com/in/tachomex
- gilberto@kueski.com
- https://tacho.codes
