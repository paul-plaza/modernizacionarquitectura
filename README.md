# modernizacionarquitectura
Archivos para retos curso modernizacion de arquitectura


#Propuesta de Arquitectura


#1. Modularidad y Arquitectura Hexagonal
•	La Arquitectura Hexagonal es ideal para facilitar la extensibilidad y el mantenimiento del sistema. Dividiremos nuestra aplicación en tres capas principales:
o	Dominio: Contendrá la lógica de negocio central (entidades como Usuario, Producto, Orden).
o	Aplicación: Representa la lógica de los casos de uso, es decir, cómo los usuarios interactúan con la lógica de negocio.
o	Adaptadores: Puertos de entrada y salida. En esta capa ubicaremos los controladores REST (entrada) y servicios de infraestructura como bases de datos, conectores de mensajería con Kafka, entre otros (salida).
#2. Contenedorización y Orquestación
•	Contenedorización con Docker:
o	Cada servicio (Usuarios, Ordenes, Productos) estará contenedorizado con Docker.
o	Se generan imágenes de cada microservicio que luego se despliegan y gestionan mediante Kubernetes.
•	Orquestación con Kubernetes:
o	Se utilizará Kubernetes para orquestar y escalar los microservicios.
o	Cada microservicio se despliega en un Pod, y estos se agrupan en Deployments.
o	Se utiliza un Load Balancer que distribuye las solicitudes hacia los diferentes Pods.
#3. Componentes de Integración y Comunicación
•	Apache Kafka: Se utilizará como bus de mensajería entre los microservicios para eventos que requieran sincronización, por ejemplo:
o	Cuando se actualiza un producto, se emite un evento de "actualización de inventario" para asegurar que todos los servicios tengan la misma información.
o	Kafka facilita la comunicación asíncrona, asegurando una comunicación eficiente y desacoplada.
•	MongoDB: Como base de datos principal, se utilizará MongoDB por su flexibilidad en manejar documentos de productos y ordenes con atributos variados.
o	Utilizaremos MongoDB Atlas como solución gestionada en la nube para mayor disponibilidad y facilidad de escalado.

![image](https://github.com/user-attachments/assets/6102b2c1-d7a0-4b30-8a66-b4707b5f78db)
