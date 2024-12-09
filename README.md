Reto 3

1. Administración de un Catálogo de Productos con Alta Concurrencia

Propuesta de Arquitectura

 ![image](https://github.com/user-attachments/assets/d69ca63a-88e2-4270-95a1-bdede3d128dc)


•	Base de Datos: Se puede usar una base de datos relacional con replica para manejar las escrituras y lecturas.

•	Cache distribuido: Implementación de un cache distribuido como Redis para consultas de productos frecuentes.


•	Servicios API:
o	Microservicio de consulta: Este maneja las solicitudes de consulta de productos, verificando con el caché primero o si no existe cache con la base de datos secundaria.

o	Microservicio de administración: Responsable de actualizaciones, con un mecanismo de invalidación de caché.


•	Cola de Mensajes: Uso de una cola como RabbitMQ o Apache Kafka para procesar actualizaciones de productos de manera asíncrona y sincronizar los cambios entre la base de datos principal y las réplicas.

•	Escalabilidad:
o	Implementación de un balanceador de carga AWS ELB o NGINX.
o	Autoescalado para manejar picos de tráfico en servicios de consulta.
o	Kubernetes en conjunto con ISTIO.
Justificación

Si separamos en nuestros servicios la lectura/escritura posiblemente usando CQRS reduce la carga en la base de datos primaria.

El cache reduce significativamente la latencia y mejora la experiencia del usuario.

La arquitectura basada en microservicios permite escalar de manera independiente las operaciones de consulta y administración.
 
2. Transacciones Distribuidas en un Sistema de Microservicios

Propuesta de Arquitectura

 ![image](https://github.com/user-attachments/assets/39efa80e-2fc7-4909-9559-11e1df193e85)


•	Saga Pattern: Implementariamos un servicio con patron coreografía para coordinar transacciones distribuidas.
o	Orquestador central para coordinar los pasos de la transacción.
o	En caso de fallo, ejecutar mecanismos de compensación (Rollback).

•	Consistencia Eventual: Integramos una cola de mensajeria para manejar los eventos de dominio que se puedan tener y tener una comunicación asincrona.

•	Bases de Datos: Base de datos transaccional SQLSERVER o ORACLE

•	Seguridad:
o	Uso de TLS para comunicación entre microservicios.
o	Tokens JWT para autenticación y autorización.

•	Monitoreo: Implementación de herramientas de monitoreo y tracing distribuidos como Jaeger o Datadog, cloudwatch.
Justificación
•	El patrón Saga asegura que las operaciones distribuidas sean consistentes incluso si fallan parcial o totalmente.
•	Al tener log de trazabilidad podemos monitorear por posibles fallas para realizar una mejora continua.
•	La arquitectura basada en microservicios con mensajes asincrónicos minimiza el acoplamiento entre servicios.
 
3. Sincronización de Datos entre Servicios con Arquitectura Escalable

Propuesta de Arquitectura

 ![image](https://github.com/user-attachments/assets/1fabf862-1779-431f-a9d1-251c584a4171)


•	Eventos de Dominio: Uso de eventos de dominio para comunicar cambios de datos relevantes a otros servicios.
o	Ejemplo: Publicar eventos a través de un bus de eventos SQS

•	CQRS :
o	Separación de los modelos de lectura y escritura.
o	Cada servicio mantiene su propia copia de los datos necesarios sincronizada mediante eventos.

•	Base de Datos Distribuida: Usar bases de datos como DynamoDB que soporten replicación y escalabilidad horizontal.

•	Kafka Connect:
o	Para sincronización en tiempo real, además del manejo de datos en caché local.

•	Despliegue Escalable:
o	Implementación en contenedores con Kubernetes.
o	Autoescalado basado en uso de CPU/memoria o volumen de eventos.

Justificación

•	Los eventos de dominio permiten que cada servicio mantenga su propia copia de datos relevantes, asegurando independencia y consistencia eventual.

•	CQRS mejora el rendimiento al optimizar lecturas y escrituras.

•	El uso de bases de datos distribuidas y herramientas de autoescalado asegura que el sistema pueda manejar grandes volúmenes de datos sin comprometer la sincronización.

