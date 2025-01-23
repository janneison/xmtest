# <a href="https://github.com/janneison">JANNEISON GERMAN GALINDO RIVERA</a>

# Caso  práctico Arquitecto de Solución
En esta documentacion se resuelve la prueba tecnica para el cargo de arquitecto de solución.

## Arquitectura AWS

El problema plantea una arquitectura orientada a microservicios con el objectivo de satisfacer la demanda a medida que aumente su uso, por ejemplo en el registro se escalaran los microservicios de registro, y al momento de crear los eventos posiblemente el de eventos. El siguiente diagrama ilustra la solución.


![](img/feria-del-libro.png)

Descripción de los servicios a usar.

- FrontEnd: Se sugiere desplegarla con s3, tambien se podria utilizar amplify para desplegarlo, en este caso sugerimos un bucket para sitios web estaticos, con el objetivo de usar un servicio serverless y de igual manera en el punto anterior se presento una propuesta de despliegue continuo por tal motivo obviamos amplify, adicionalmente se usara cloudfront para entregar el contenido de la app web.
- Apigateway: Se utilizara para entregar las api de los servicios expuestos ya sea por EKS, lambda, un servicio que aun no se migre del monolito, de igual manera se utilizara la funcion de autorizador para utilizar la autenticacion existente.
- Microservicios: Se orquestaran en kubernetes y adicionalmente los exporadicos se sugiere convertirlos en aws lambdas.
- SNS: El diagrama muestra un SNS el cual se puede utilizar para notificar que fue registrado en su correo.
- Cloudwacth: aqui manejaremos los logs.
- Almacenamiento: aqui observamos los 4 tipos de almacenamiento que seran usados dependiendo del caso de uso, por ejemplo la parametrizacion en bases de datos relacionales, los eventos en mongo, y el registro en una base de datos clave valor como dynamoDB.
- Seguridad: Se usuara kms para guardar certificados de seguridad, secret manager para los secretos y waf para la seguridad perimetral, la autenticacion con cognito.

## Criterios de aceptación
Escenario 1: Creacion de salones

- Dado que la feria requiere una ubicacion para cada evento
- Cuando el usuario quiera crear una ubicacion
- Entonces el usuario debería registrar los siguientes detalles:
    - nombre,
    - nomenclatura,
    - referencia,
    - Codigo.
- El sistema debera indicar que fue creado con exito y devolver el codigo

Escenario 2: Creacion de eventos

- Dado que la feria requiere eventos
- Cuando el usuario quiera crear un evento
- Entonces el usuario debería registrar los siguientes detalles:
    - nombre,
    - salon,
    - fecha y hora,
    - capacidad,
    - orador
    - tema
- El sistema debera indicar que fue creado con exito y devolver el id del evento

Escenario 3: Registro de eventos

- Dado que los asistentes requieren registrarse a un evento
- Cuando el usuario quiera registrarse
- Entonces el usuario debería registrar los siguientes detalles:
    - evento,
    - nombres,
    - identificacion,
    - email
- El sistema debera indicar que fue creado con exito y debe enviar un mensaje de correo al usuario con un codigo

Escenario 4: Ingreso de eventos

- Dado que los asistentes requieren asistir a un evento
- Cuando el usuario quiera ingresar
- Entonces el usuario debería registrar los siguientes detalles:
    - codigo,
    - email
- El sistema debera indicar en el registro que el usuario asistio, indicar que puede seguir y mandar un correo de confirmación.


## Definir métodos de integración y de exposición de datos

Para exponer los datos utilizaremos api de tipo rest, y las respuestas estaran en json. Si es necesario enviar la información a un sistema externo se sugiere el uso de Apis y/o Colas. 

Los datos tambien se expondran en una web la cual solo se podra acceder por https y debidamente autenticados.


## Atributos de calidad

![](img/atributos.png)

## Patrones, estilos y tacticas a usar.

- FrontEnd: Utilizaremos microfrontEnd que utilicen arquitectura hexagonal, para minimizar el impacto del uso del framework web que se escoja.

- Microservicios: Desde la arquitectura de AWS, se observa que tenemos una arquitectura orientada a microservicios, que planea utilizar para la observabilidad a cloudwatch, tambien como se usa EKS se hace necesario usar healtcheck, tambien utilizaremos el patron apigateway para descubir los servicios, de igual manera la arquitectura queda preparada para el uso de eventos.

- Tambien utilizaremos database per services, y es un sistema poliglota a nivel de persistencia, y por ultimo cabe resaltar que se utilizara arquitectura hexagonal para crear los microservicios.

- Se sugiere hacer un desarrollo orientado a pruebas para asegurar la calidad del mismo y seguir el marco de OWASP para el desarrollo de la web y los servicios.

## Conclusiones

Podemos observar un sistema que puede ser reutilizable para la creacion de cualquier tipo de eventos, seguramente la arquitectura  de solución y de software deba profundizarse y evolucionarse a medida que  se creen las historias de usuario y se definan todos los escenarios.


