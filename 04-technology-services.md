# Dominio 3: Tecnología y Servicios en la Nube (34%)

[← Anterior: Seguridad y Cumplimiento](03-security-compliance.md) | [Volver al Inicio](README.md) | [Siguiente: Facturación, Precios y Soporte →](05-billing-pricing-support.md)

---

Este dominio es el corazón del examen y cubre los servicios principales que conforman la plataforma de AWS. Debes entender no solo qué hace cada servicio, sino cuándo elegir uno sobre otro.

## Tabla de Contenidos

- [Infraestructura Global de AWS](#infraestructura-global-de-aws)
- [Servicios de Cómputo](#servicios-de-computo)
  - [Amazon EC2](#amazon-ec2)
  - [AWS Lambda](#aws-lambda)
  - [Contenedores (ECS, EKS, Fargate)](#contenedores-ecs-eks-fargate)
- [Servicios de Almacenamiento](#servicios-de-almacenamiento)
- [Servicios de Bases de Datos](#servicios-de-bases-of-datos)
- [Redes y Entrega de Contenido](#redes-y-entrega-de-contenido)
- [Gestión y Gobernanza](#gestion-y-gobernanza)
- [Servicios de Integración de Aplicaciones](#servicios-de-integracion-de-aplicaciones)
- [Preguntas de Repaso](#preguntas-de-repaso)

---

## Infraestructura Global de AWS (AWS Global Infrastructure)

AWS opera la infraestructura de nube más extensa del mundo. Entender cómo está organizada es clave para diseñar aplicaciones de alta disponibilidad.

### Regiones (Regions)
Una **Region** es una ubicación física en el mundo donde AWS tiene múltiples Zonas de Disponibilidad.

- **Aislamiento**: Cada región es independiente y está aislada de las demás.
- **Soberanía de Datos**: Los datos no salen de la región a menos que tú lo configures explícitamente.
- **Selección de Región**:
  - **Cumplimiento**: Requisitos legales de dónde deben residir los datos.
  - **Proximidad (Latencia)**: Elegir la región más cercana a tus usuarios finales.
  - **Disponibilidad de Servicios**: No todos los servicios están disponibles en todas las regiones.
  - **Precios**: El coste de los servicios varía según la región debido a impuestos y costes locales.

### Zonas de Disponibilidad (Availability Zones - AZs)
Una **AZ** consiste en uno o más centros de datos discretos con alimentación, redes y conectividad redundantes.

- **Separación Física**: Las **AZs** están separadas por kilómetros para protegerse contra desastres naturales.
- **Conectividad**: Están conectadas mediante redes de fibra óptica propias de AWS con latencia de milisegundos de un solo dígito.
- **Alta Disponibilidad**: Al desplegar en múltiples **AZs**, proteges tu aplicación contra el fallo de un centro de datos completo.

### Ubicaciones de Borde (Edge Locations)
Puntos de presencia situados en ciudades importantes globalmente.

- Utilizadas por **Amazon CloudFront** (CDN) para entregar contenido con baja latencia.
- También usadas por **Route 53** y **AWS Shield**.
- Existen cientos de ubicaciones de borde, muchas más que regiones.

---

## Servicios de Cómputo (Compute Services)

### Amazon EC2 (Elastic Compute Cloud)

Proporciona capacidad de cómputo segura y redimensionable. Es una oferta de **IaaS** (Infraestructura como Servicio).

#### Opciones de Compra de EC2 (Muy frecuente en el examen)

| Opción | Características | Caso de Uso Ideal |
|--------|-----------------|-------------------|
| **On-Demand** | Pago por segundo, sin compromiso. | Cargas de trabajo impredecibles o de corto plazo. |
| **Reserved Instances** | Compromiso de 1 o 3 años (hasta 75% descuento). | Cargas de trabajo estables y predecibles. |
| **Savings Plans** | Compromiso de gasto ($/hora) por 1 o 3 años. | Flexibilidad entre diferentes tipos de instancias y regiones. |
| **Spot Instances** | Usa capacidad sobrante (hasta 90% descuento). | Tareas que pueden interrumpirse (Batch processing). |
| **Dedicated Hosts** | Servidor físico dedicado para tu uso exclusivo. | Requisitos de cumplimiento o licencias de software complejas. |

#### Tipos de Instancias (Categorías)
- **General Purpose**: Equilibrio de cómputo, memoria y red (ej. familia `t3`, `m5`).
- **Compute Optimized**: Ideal para servidores web de alto rendimiento, procesamiento por lotes (ej. familia `c5`).
- **Memory Optimized**: Para bases de datos en memoria o análisis de datos en tiempo real (ej. familia `r5`).
- **Storage Optimized**: Para cargas que requieren acceso rápido a grandes conjuntos de datos en disco local (ej. familia `i3`).

### AWS Lambda (Serverless Compute)

Permite ejecutar código sin aprovisionar ni gestionar servidores.

- **Sin Servidor**: No hay sistema operativo que parchear ni servidores que escalar.
- **Escalado Automático**: AWS escala la ejecución de tu código en respuesta a las solicitudes.
- **Pago por Uso**: Solo pagas por el tiempo que tu código se está ejecutando (milisegundos).
- **Límites**: Tiempo máximo de ejecución de 15 minutos por solicitud.

### Contenedores en AWS

- **Amazon ECS (Elastic Container Service)**: Servicio de gestión de contenedores altamente escalable que soporta Docker.
- **Amazon EKS (Elastic Kubernetes Service)**: Servicio gestionado que facilita la ejecución de Kubernetes en AWS.
- **AWS Fargate**: Motor de cómputo sin servidor para contenedores. Funciona con ECS y EKS. No tienes que gestionar las instancias de EC2 subyacentes.

---

## Servicios de Almacenamiento (Storage Services)

AWS ofrece una variedad de servicios de almacenamiento diseñados para diferentes necesidades de rendimiento, durabilidad y costo.

### Amazon S3 (Simple Storage Service)

Servicio de almacenamiento de objetos diseñado para almacenar cualquier cantidad de datos con una durabilidad del 99.999999999% (11 nueves).

#### Conceptos Clave de S3
- **Buckets**: Contenedores para objetos. Los nombres de los buckets deben ser únicos globalmente.
- **Objetos**: Los archivos almacenados. Pueden tener un tamaño de hasta 5 TB.
- **Metadatos**: Información sobre el objeto (par clave-valor).
- **Control de Versiones**: Permite mantener múltiples versiones de un objeto en el mismo bucket.

#### Clases de Almacenamiento de S3 (Vital para el examen)

| Clase | Uso | Durabilidad | Disponibilidad |
|-------|-----|-------------|----------------|
| **Standard** | Datos de acceso frecuente. | 11 nueves | 99.99% |
| **Intelligent-Tiering** | Datos con patrones de acceso desconocidos. | 11 nueves | 99.9% |
| **Standard-IA** | Datos de acceso infrecuente pero rápido. | 11 nueves | 99.9% |
| **One Zone-IA** | Datos de acceso infrecuente, almacenados en una sola AZ. | 11 nueves | 99.5% |
| **Glacier Instant** | Archivos (recuperación en milisegundos). | 11 nueves | 99.9% |
| **Glacier Flexible** | Archivos (recuperación de minutos a horas). | 11 nueves | 99.99% |
| **Glacier Deep Archive** | Archivos a largo plazo (recuperación 12-48h). | 11 nueves | 99.99% |

### Amazon EBS (Elastic Block Store)

Almacenamiento de bloques persistente para su uso con instancias **EC2**. Equivale a un disco duro virtual.

- **Volúmenes**: Se pueden conectar a una instancia a la vez (excepto los volúmenes Multi-Attach).
- **Zonas de Disponibilidad**: Los volúmenes de EBS se crean en una AZ específica y solo pueden conectarse a instancias en esa misma AZ.
- **Snapshots**: Copias de seguridad incrementales almacenadas en S3.

#### Tipos de Volúmenes EBS

| Tipo | Categoría | Caso de Uso | Rendimiento |
|------|-----------|-------------|-------------|
| **gp3 / gp2** | SSD Propósito General | Cargas equilibradas (Sistemas operativos). | Hasta 16,000 IOPS |
| **io2 / io1** | SSD IOPS Provisionado | Bases de datos críticas de alto rendimiento. | Hasta 64,000 IOPS |
| **st1** | HDD Optimizado para Rendimiento | Grandes volúmenes de datos (Big Data). | 500 MB/s |
| **sc1** | HDD Frío | Datos accedidos rara vez. | 250 MB/s |

### Amazon EFS (Elastic File System)

Sistema de archivos de red (**NFS**) gestionado que puede ser compartido por múltiples instancias de EC2 simultáneamente.

- **Escalado Automático**: Crece y disminuye automáticamente a medida que añades o eliminas archivos.
- **Alta Disponibilidad**: Almacena datos en múltiples AZs dentro de una región.

### AWS Storage Gateway

Servicio de almacenamiento híbrido que conecta los entornos locales con el almacenamiento en la nube de AWS (S3, EBS, VTL).

## Servicios de Bases de Datos (Database Services)

AWS ofrece una amplia gama de bases de datos especializadas para diferentes tipos de datos y casos de uso.

### Amazon RDS (Relational Database Service)

Servicio gestionado que facilita la configuración y el escalado de bases de datos relacionales en la nube.

- **Motores Soportados**: MySQL, PostgreSQL, MariaDB, Oracle, SQL Server y **Amazon Aurora**.
- **Gestión de AWS**: Parcheado del SO, copias de seguridad, detección de fallos y recuperación.
- **Multi-AZ**: Proporciona alta disponibilidad y recuperación ante desastres mediante una réplica sincrónica en otra zona de disponibilidad.
- **Read Replicas**: Permiten escalar las cargas de trabajo de lectura (asíncronas).

### Amazon Aurora

Base de datos relacional compatible con MySQL y PostgreSQL diseñada específicamente para la nube.

- **Rendimiento**: Hasta 5 veces más rápida que MySQL estándar y 3 veces más rápida que PostgreSQL.
- **Almacenamiento**: Se escala automáticamente hasta 128 TB.
- **Aurora Serverless**: Opción que escala la capacidad de cómputo automáticamente según la demanda.

### Amazon DynamoDB (NoSQL)

Base de datos de clave-valor y documentos totalmente gestionada que ofrece un rendimiento de milisegundos de un solo dígito a cualquier escala.

- **Escalado**: Soporta picos de tráfico masivos sin intervención manual.
- **Tablas Globales**: Permiten replicar datos en múltiples regiones con latencia de lectura/escritura local.
- **DAX (DynamoDB Accelerator)**: Caché en memoria que reduce los tiempos de respuesta de milisegundos a microsegundos.

### Amazon ElastiCache

Servicio de almacenamiento en caché en memoria gestionado (Redis o Memcached). Mejora el rendimiento de las aplicaciones recuperando datos de cachés rápidas en lugar de bases de datos en disco.

---

## Redes y Entrega de Contenido (Networking & Content Delivery)

### Amazon VPC (Virtual Private Cloud)

Permite definir una red virtual aislada lógicamente donde puedes lanzar tus recursos de AWS.

- **Subredes (Subnets)**: Segmentos de IP. Pueden ser públicas (tienen acceso a internet) o privadas.
- **Internet Gateway (IGW)**: Permite que los recursos de la VPC se comuniquen con internet.
- **NAT Gateway**: Permite que las instancias en subredes privadas accedan a internet (para actualizaciones) pero impide conexiones entrantes desde internet.
- **Network ACLs**: Firewall a nivel de subred (Stateless).
- **Security Groups**: Firewall a nivel de instancia (Stateful).

### Amazon Route 53

Servicio de DNS web altamente disponible y escalable.

- **Registro de Dominios**: Puedes comprar y gestionar dominios.
- **Enrutamiento**: Dirige el tráfico a recursos de AWS o externos.
- **Políticas de Enrutamiento**:
  - **Simple**: Un solo recurso.
  - **Weighted**: Divide el tráfico por porcentajes.
  - **Latency**: Basado en la menor latencia para el usuario.
  - **Failover**: Para recuperación ante desastres (Activo-Pasivo).
  - **Geolocation**: Basado en la ubicación del usuario.

### Amazon CloudFront

Red de entrega de contenido (**CDN**) global que acelera la entrega de sitios web, APIs y videos.

- **Caché**: Almacena copias del contenido en las **Edge Locations**.
- **Seguridad**: Integrado con AWS Shield y WAF.
- **Origen**: S3 bucket, ALB o servidores externos.

### Elastic Load Balancing (ELB)

Distribuye automáticamente el tráfico entrante entre múltiples destinos (instancias EC2, contenedores, funciones Lambda).

- **ALB (Application Load Balancer)**: Capa 7 (HTTP/HTTPS). Soporta enrutamiento basado en rutas.
- **NLB (Network Load Balancer)**: Capa 4 (TCP/UDP). Ideal para alto rendimiento y tráfico volátil.
- **GWLB (Gateway Load Balancer)**: Para appliances de seguridad de terceros.

## Gestión y Gobernanza (Management & Governance)

### AWS CloudWatch

Servicio de monitoreo y observabilidad para tus recursos y aplicaciones de AWS.

- **Métricas (Metrics)**: Recopila datos numéricos sobre el rendimiento (CPU, red, disco).
- **Alarmas (Alarms)**: Realiza acciones automáticas según umbrales (ej. apagar una instancia si el uso de CPU es bajo).
- **Logs**: Recopila, almacena y analiza archivos de registro de tus recursos.
- **Dashboards**: Crea vistas visuales personalizadas de tus métricas.

### AWS CloudTrail

Registra todas las acciones realizadas en tu cuenta de AWS.

- **Auditoría**: Permite ver quién hizo qué, cuándo y desde dónde.
- **Historial de Eventos**: Por defecto guarda los últimos 90 días.
- **Seguridad**: Fundamental para investigar accesos no autorizados o cambios de configuración.

### AWS Config

Mantiene un historial de los cambios en la configuración de tus recursos.

- **Auditoría de Configuración**: Permite ver cómo estaba configurado un recurso en el pasado.
- **Reglas de Configuración**: Verifica automáticamente si tus recursos cumplen con tus políticas internas (ej. "¿Todos mis buckets de S3 están cifrados?").

### AWS CloudFormation (Infrastructure as Code)

Permite modelar y configurar tus recursos de AWS mediante archivos de texto (**JSON** o **YAML**).

- **Automatización**: Despliega infraestructuras completas de forma repetible.
- **Stacks**: Colección de recursos de AWS gestionados como una sola unidad.

### AWS Trusted Advisor

Proporciona recomendaciones en tiempo real para optimizar tu entorno de AWS en 5 categorías:
1. **Optimización de Costos**.
2. **Rendimiento**.
3. **Seguridad**.
4. **Tolerancia a Fallos**.
5. **Límites de Servicio**.

---

## Servicios de Integración de Aplicaciones

### Amazon SNS (Simple Notification Service)

Servicio de mensajería **Pub/Sub** (Publicación/Suscripción).

- **Tópicos**: Los mensajes se envían a un tópico.
- **Suscriptores**: Pueden ser emails, SMS, funciones Lambda o colas SQS.
- **Desacoplamiento**: Permite enviar un mensaje a múltiples destinos a la vez.

### Amazon SQS (Simple Queue Service)

Servicio de colas de mensajes totalmente gestionado.

- **Desacoplamiento**: Permite que los componentes de una aplicación se comuniquen de forma asíncrona.
- **Escalado**: Puede manejar cualquier volumen de mensajes.
- **Retención**: Los mensajes se guardan en la cola hasta que son procesados y eliminados (máximo 14 días).

---

## Servicios de Análisis y Machine Learning

- **Amazon Kinesis**: Para procesar datos en streaming en tiempo real.
- **Amazon Redshift**: Almacén de datos (**Data Warehouse**) para análisis masivo.
- **Amazon SageMaker**: Plataforma completa para crear, entrenar y desplegar modelos de Machine Learning.
- **Amazon Lex**: Para crear chatbots con voz y texto (tecnología de Alexa).

---

## Migración y Transferencia

- **AWS DMS (Database Migration Service)**: Migra bases de datos a AWS con tiempo de inactividad mínimo.
- **AWS Snow Family**: Dispositivos físicos (**Snowcone**, **Snowball**, **Snowmobile**) para migrar petabytes de datos de forma segura.

---

## 48 Errores Comunes de Configuración

A continuación, se presentan los errores más críticos detectados en infraestructuras de AWS y cómo evitarlos:

### Cómputo (EC2/Lambda)
1. **Instancias Subutilizadas**: No monitorizar el uso de CPU y RAM, pagando por recursos que no se usan. **Solución**: AWS Compute Optimizer.
2. **Sin Auto Scaling**: Confiar en una sola instancia. **Solución**: Usar Auto Scaling Groups en múltiples AZs.
3. **Uso de On-Demand para todo**: No aprovechar RIs o Spot Instances.
4. **Lambda sin límites de concurrencia**: Una función puede agotar la cuota de la cuenta.
5. **Instancias en Subredes Públicas**: Bases de datos expuestas. **Solución**: Mover a subredes privadas.

### Almacenamiento (S3/EBS)
6. **Buckets de S3 Abiertos**: Configuración accidental que expone datos. **Solución**: S3 Block Public Access.
7. **Sin Control de Versiones**: Imposibilidad de recuperar datos borrados accidentalmente.
8. **Sin Políticas de Ciclo de Vida**: Almacenar datos antiguos en S3 Standard indefinidamente.
9. **Volúmenes EBS sin Snapshots**: Falta de copias de seguridad.
10. **Tipos de EBS incorrectos**: Usar HDD para bases de datos de alto rendimiento.

### Bases de Datos (RDS/DynamoDB)
11. **RDS Single-AZ**: Sin alta disponibilidad. **Solución**: Habilitar Multi-AZ.
12. **Sin Réplicas de Lectura**: Sobrecarga de la base de datos principal con informes.
13. **DynamoDB con Provisioned Capacity fijo**: No usar Auto Scaling para manejar picos de tráfico.
14. **Sin Cifrado en Reposo**: Incumplimiento de normativas de seguridad.

### Redes (VPC)
15. **Security Groups demasiado abiertos**: Permitir el puerto 22 (SSH) desde 0.0.0.0/0.
16. **Sin VPC Endpoints**: Todo el tráfico interno sale por internet.
17. **Tablas de rutas incorrectas**: Instancias sin salida a internet por falta de NAT Gateway.
18. **Nombres de DNS no habilitados**: Dificultad para conectar servicios internos.

---

## Preguntas de Repaso

**1. ¿Qué servicio usarías para monitorizar el uso de CPU de tus instancias EC2 y recibir alertas?**
- A. **AWS CloudTrail**.
- B. **AWS Config**.
- C. **Amazon CloudWatch**.
- D. **AWS Trusted Advisor**.
*(Respuesta: C)*

**2. Una empresa necesita una base de datos NoSQL altamente escalable con latencia de milisegundos. ¿Cuál es la mejor opción?**
- A. **Amazon RDS**.
- B. **Amazon Redshift**.
- C. **Amazon DynamoDB**.
- D. **Amazon Aurora**.
*(Respuesta: C)*

**3. ¿Qué servicio de cómputo es "Serverless" y escala automáticamente?**
- A. **Amazon EC2**.
- B. **AWS Lambda**.
- C. **Amazon ECS**.
- D. **AWS Elastic Beanstalk**.
*(Respuesta: B)*

**4. ¿Qué clase de almacenamiento de S3 es la más económica para archivos que rara vez se acceden y pueden tardar horas en recuperarse?**
- A. **S3 Standard**.
- B. **S3 Standard-IA**.
- C. **S3 Glacier Deep Archive**.
- D. **S3 Intelligent-Tiering**.
*(Respuesta: C)*

**5. ¿Qué componente de la VPC permite que los recursos de una subred privada accedan a internet para actualizaciones?**
- A. **Internet Gateway**.
- B. **NAT Gateway**.
- C. **Virtual Private Gateway**.
- D. **Direct Connect**.
*(Respuesta: B)*

---

[← Anterior: Seguridad y Cumplimiento](03-security-compliance.md) | [Volver al Inicio](README.md) | [Siguiente: Facturación, Precios y Soporte →](05-billing-pricing-support.md)
