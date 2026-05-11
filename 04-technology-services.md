# Dominio 3: Tecnología y Servicios en la Nube (34%)

[← Anterior: Seguridad y Cumplimiento](03-security-compliance.md) | [Siguiente: Facturación, Precios y Soporte →](05-billing-pricing-support.md)

---

Este es el dominio más extenso del examen AWS Certified Cloud Practitioner, y cubre los servicios principales de AWS en cómputo, almacenamiento, redes, bases de datos y más.

## Tabla de Contenidos

- [Infraestructura Global de AWS](#infraestructura-global-de-aws)
- [Servicios de Cómputo](#servicios-de-computo)
- [Servicios de Almacenamiento](#servicios-de-almacenamiento)
- [Servicios de Bases de Datos](#servicios-de-bases-de-datos)
- [Redes y Entrega de Contenido](#redes-y-entrega-de-contenido)
- [Gestión y Gobernanza](#gestion-y-gobernanza)
- [Servicios Adicionales](#servicios-adicionales)
- [Preguntas de Repaso](#preguntas-de-repaso)

---

## Infraestructura Global de AWS (AWS Global Infrastructure)

### Regiones (Regions)

**Áreas geográficas con múltiples Availability Zones**

- **Recuento Actual**: Más de 30 **Regions** en todo el mundo (y creciendo)
- **Isolation**: Cada **Region** está completamente aislada de las demás.
- **Criterios de Selección**:
  - **Compliance requirements**: Soberanía de datos y requisitos regulatorios.
  - **Proximity to users**: Menor latencia para los usuarios finales.
  - **Available services**: No todos los servicios están disponibles en todas las **Regions**.
  - **Pricing**: Los costes varían según la **Region**.

> **Consejo para el Examen:** Elige la **Region** adecuada basándote en el cumplimiento, la latencia, la disponibilidad del servicio y las consideraciones de coste.

### Zonas de Disponibilidad (Availability Zones - AZs)

**Centros de datos discretos dentro de una Region**

- **Composición**: Uno o más centros de datos discretos por **AZ**.
- **Redundancy**: Cada **AZ** tiene alimentación, redes y conectividad redundantes.
- **Physical Separation**: Las **AZs** están separadas físicamente dentro de una **Region**.
- **Connectivity**: Conectadas con redes de gran ancho de banda y baja latencia.
- **Recuento Mínimo**: Al menos 3 **AZs** por **Region** (la mayoría tiene más).
- **High Availability**: Despliega recursos en múltiples **AZs** para lograr tolerancia a fallos.

**Beneficios Clave**:
- Aislamiento de fallos.
- Alta disponibilidad mediante redundancia.
- **Disaster recovery** dentro de una **Region**.

**Ejemplo del Mundo Real**: Netflix despliega su infraestructura de entrega de contenido en múltiples **AZs** dentro de cada **Region**. Si una **AZ** experimenta problemas, su aplicación redirige automáticamente el tráfico a las **AZs** en buen estado, garantizando un streaming ininterrumpido para millones de usuarios.

**Errores Comunes de Configuración**:
1. Desplegar todos los recursos en una sola **AZ** (sin tolerancia a fallos).
2. No considerar los costes de transferencia de datos entre **AZs**.
3. Asumir que los nombres de las **AZ** (us-east-1a) son los mismos en todas las cuentas de AWS (están aleatorizados).
4. No probar el **failover** entre **AZs** antes del despliegue en producción.

**Límites de Servicio por Infraestructura**:
- Máximo de **VPCs** por **Region**: 5 (límite flexible, se puede aumentar).
- Máximo de subredes por **VPC**: 200.
- **Elastic IPs** por **Region**: 5 (límite flexible).
- Conexiones de **VPC Peering** por **VPC**: 125.

### Ubicaciones de Borde (Edge Locations)

**Endpoints de entrega de contenido en todo el mundo**

- **Recuento**: Más de 400 **Edge Locations** a nivel mundial.
- **Uso Principal**: Caché de contenido de **CloudFront**.
- **Performance**: Menor latencia para los usuarios finales.
- **Servicios**: También utilizados por **Route 53**, **AWS Shield** y **AWS WAF**.
- **Cobertura**: Hay más **Edge Locations** que **Regions**.

### AWS Local Zones

**Extensiones de Region para latencia ultra baja**

- Extensión de una **Region** ubicada más cerca de áreas geográficas específicas.
- Latencia de milisegundos de un solo dígito para los usuarios finales.
- Ideal para aplicaciones sensibles a la latencia (gaming, video en vivo, AR/VR).
- No disponible en todas las ubicaciones.

### AWS Wavelength

**Computación en el borde con 5G (5G edge computing)**

- Incrusta servicios de cómputo y almacenamiento de AWS dentro de las redes 5G.
- Aplicaciones de latencia ultra baja.
- Casos de uso de computación en el borde móvil (**Mobile edge computing**).
- Reduce el enrutamiento de datos hacia los servidores de aplicaciones.

### AWS Outposts

**Infraestructura de AWS on-premises**

- Servicio totalmente gestionado que extiende la infraestructura de AWS a instalaciones **on-premises**.
- Mismas **APIs**, herramientas y hardware de AWS disponibles localmente.
- Permite despliegues de nube híbrida reales.
- AWS gestiona y mantiene la infraestructura.
- Ejecuta servicios de AWS localmente con una experiencia consistente.

> **Punto Clave**: Recuerda la jerarquía: las **Regions** contienen **Availability Zones**. Las **Edge Locations** son independientes y se usan principalmente para la entrega de contenido.

### Tabla Comparativa de Infraestructura Global

| Componente | Recuento | Uso Principal | Nivel de Redundancia | Latencia |
|-----------|-------|-------------|------------------|---------|
| **Regions** | 33+ | Despliegue completo de servicios AWS | Aisladas entre sí | Variable (según la distancia) |
| **Availability Zones** | 100+ (3+ por Region) | Despliegues tolerantes a fallos | Dentro de la Region | Baja (ms de un solo dígito) |
| **Edge Locations** | 400+ | Caché de contenido | N/A | Mínima para usuarios finales |
| **Local Zones** | 16+ | Cómputo de latencia ultra baja | Extensión de la Region matriz | <10ms |
| **Wavelength Zones** | 20+ | 5G edge computing | Dentro de redes de telecom | <1ms |

---

## Servicios de Cómputo (Compute Services)

### Amazon EC2 (Elastic Compute Cloud)

**Servidores virtuales en la nube**

Amazon **EC2** proporciona capacidad de cómputo redimensionable en la nube, permitiéndote lanzar servidores virtuales (**instances**) bajo demanda.

#### Tipos de Instancia (Instance Types)

| Categoría | Caso de Uso | Ejemplos |
|----------|----------|----------|
| **General Purpose** | Equilibrio entre cómputo, memoria y redes | T3, M5 |
| **Compute Optimized** | Procesadores de alto rendimiento | C5, C6 |
| **Memory Optimized** | Grandes conjuntos de datos en memoria | R5, X1 |
| **Storage Optimized** | Acceso a lectura/escritura secuencial alta | I3, D2 |
| **Accelerated Computing** | Cargas de trabajo de GPU, FPGA | P3, G4 |

#### Modelos de Precios de EC2 (EC2 Pricing Models)

##### 1. On-Demand Instances

- **Billing**: Pago por hora o por segundo (mínimo 60 segundos).
- **Commitment**: Sin compromiso inicial ni contrato a largo plazo.
- **Cost**: Coste por hora más alto.
- **Ideal para**:
  - Cargas de trabajo impredecibles.
  - Cargas de trabajo a corto plazo con picos.
  - Pruebas y desarrollo.

##### 2. Reserved Instances (RI)

- **Commitment**: Contratos de 1 o 3 años.
- **Discount**: Hasta un 75% comparado con los precios **On-Demand**.
- **Tipos**:
  - **Standard RI**: Máximo descuento, no se puede cambiar el tipo de instancia.
  - **Convertible RI**: Menor descuento, permite cambiar el tipo/familia de instancia.
- **Ideal para**: Cargas de trabajo en estado estable con uso predecible.

##### 3. Savings Plans

- **Commitment**: Compromiso de uso de cómputo consistente medido en $/hora.
- **Term**: Compromiso de 1 o 3 años.
- **Discount**: Hasta un 72% comparado con **On-Demand**.
- **Flexibility**: Más flexible que las **Reserved Instances**.
- **Cobertura**: Se aplica a **EC2**, **Lambda** y **Fargate**.

##### 4. Spot Instances

- **Mechanism**: Puja por la capacidad de **EC2** no utilizada.
- **Discount**: Hasta un 90% comparado con los precios **On-Demand**.
- **Interruption**: AWS puede terminarlas con un aviso de 2 minutos.
- **Ideal para**:
  - Aplicaciones tolerantes a fallos.
  - Procesamiento por lotes (**Batch processing**).
- **No apto para**: Cargas de trabajo críticas o bases de datos.

##### 5. Dedicated Hosts

- **Definition**: Servidor físico de **EC2** dedicado exclusivamente para tu uso.
- **Cost**: La opción más cara.
- **Casos de Uso**: Requisitos de cumplimiento normativo o licencias de software vinculadas al servidor físico.

#### Auto Scaling

**Ajusta automáticamente la capacidad basándose en la demanda**

- **Scaling Actions**:
  - **Scale Out**: Añadir instancias cuando aumenta la demanda.
  - **Scale In**: Eliminar instancias cuando disminuye la demanda.
- **Benefits**: Mejora la disponibilidad y optimiza los costes.

### AWS Lambda

**Servicio de cómputo serverless**

Ejecuta código sin aprovisionar ni gestionar servidores.

**Características Clave**:
- **Zero Server Management**: Sin infraestructura que gestionar.
- **Automatic Scaling**: Escala automáticamente de unas pocas solicitudes a miles.
- **Subsecond Metering**: Paga solo por el tiempo de cómputo consumido (se factura por cada 100ms).
- **Event-Driven**: Se activa por eventos de servicios de AWS o aplicaciones personalizadas.

**Lambda vs EC2 Comparison**

| Aspecto | AWS Lambda | Amazon EC2 |
|--------|------------|------------|
| **Management** | Totalmente gestionado, cero administración | Tú gestionas el OS, parches, escalado |
| **Scaling** | Automático, instantáneo | Manual o Auto Scaling (minutos) |
| **Pricing** | Por solicitud + duración | Por hora/segundo de ejecución |
| **Max Duration** | 15 minutos por invocación | Ilimitado |

### Amazon Lightsail

**Plataforma en la nube simplificada para cargas de trabajo sencillas**

- Servidores virtuales privados (**VPS**) fáciles de usar.
- Precios mensuales predecibles.
- Ideal para: Blogs, sitios web sencillos y entornos de prueba.

### AWS Elastic Beanstalk

**Platform as a Service (PaaS)**

Despliega y gestiona aplicaciones sin la complejidad de la infraestructura. AWS gestiona el despliegue, desde el aprovisionamiento de capacidad y el equilibrio de carga hasta el auto-escalado y el monitoreo de salud.

### Amazon ECS (Elastic Container Service)

**Orquestación de contenedores totalmente gestionada**

Ejecuta y escala contenedores Docker en AWS.
- **EC2 Launch Type**: Tú gestionas las instancias de **EC2** subyacentes.
- **Fargate Launch Type**: Despliegue de contenedores **serverless** (AWS gestiona la infraestructura).

### Amazon EKS (Elastic Kubernetes Service)

**Servicio de Kubernetes gestionado**

- Plano de control de Kubernetes totalmente gestionado.
- Compatible con las herramientas estándar de Kubernetes.

### AWS Fargate

**Motor de cómputo serverless para contenedores**

- Funciona con **ECS** y **EKS**.
- Elimina la necesidad de aprovisionar o escalar servidores.
- Paga solo por los recursos de vCPU y memoria que consumen tus contenedores.

---

## Servicios de Almacenamiento (Storage Services)

### Amazon S3 (Simple Storage Service)

**Servicio de almacenamiento de objetos para cualquier cantidad de datos**

#### Clases de Almacenamiento de S3 (S3 Storage Classes)

| Clase de Almacenamiento | Caso de Uso | Coste de Almacenamiento |
|---------------|----------|---------------|
| **S3 Standard** | Datos de acceso frecuente | Más alto |
| **S3 Intelligent-Tiering** | Patrones de acceso desconocidos | Automático |
| **S3 Standard-IA** | Acceso infrecuente, disponibilidad inmediata | Bajo |
| **S3 One Zone-IA** | Datos recreables, una sola AZ | El más bajo de IA |
| **S3 Glacier Instant Retrieval** | Archivo con acceso en milisegundos | Muy bajo |
| **S3 Glacier Flexible Retrieval** | Archivo, minutos a horas | Ultra bajo |
| **S3 Glacier Deep Archive** | Archivo a largo plazo, 12-48 horas | El más bajo de todos |

#### Características de S3
- **Versioning**: Mantener múltiples versiones de un objeto.
- **Lifecycle Policies**: Transición automática entre clases de almacenamiento para ahorrar costes.
- **Durability**: Diseñado para una durabilidad del 99.999999999% (11 nueves).

### Amazon EBS (Elastic Block Store)

**Volúmenes de almacenamiento a nivel de bloque para instancias EC2**

- Almacenamiento persistente que existe independientemente de la vida de la instancia.
- Se conecta a una instancia **EC2** a la vez (dentro de la misma **AZ**).
- Las **Snapshots** (instantáneas) se guardan en **S3**.

### Amazon EFS (Elastic File System)

**Sistema de archivos NFS gestionado para EC2**

- Puede ser accedido por múltiples instancias **EC2** simultáneamente (**Shared Access**).
- Escala automáticamente.
- Servicio regional (múltiples **AZs**).

### AWS Storage Gateway

**Servicio de almacenamiento en nube híbrida**

Conecta entornos **on-premises** con el almacenamiento en la nube de AWS.
- **File Gateway**: Archivos en **S3**.
- **Volume Gateway**: Almacenamiento de bloque (**iSCSI**).
- **Tape Gateway**: Reemplazo de cintas físicas (**VTL**).

### AWS Snow Family

**Dispositivos físicos para migración de datos y computación en el borde**

- **Snowcone**: 8 TB, portátil.
- **Snowball Edge**: 80 TB, para grandes migraciones.
- **Snowmobile**: 100 PB, camión para migraciones masivas.

---

## Servicios de Bases de Datos (Database Services)

### Amazon RDS (Relational Database Service)

**Servicio de base de datos relacional gestionado**

Soporta: MySQL, PostgreSQL, MariaDB, Oracle, SQL Server y **Amazon Aurora**.
- **Multi-AZ**: Para alta disponibilidad y recuperación ante desastres (**DR**).
- **Read Replicas**: Para escalar las cargas de lectura.

### Amazon Aurora

**Base de datos relacional nativa de AWS**

- Compatible con MySQL y PostgreSQL.
- 5 veces más rápida que MySQL estándar.
- Almacenamiento auto-reparable y altamente disponible.
- **Aurora Serverless**: Configuración que escala automáticamente bajo demanda.

### Amazon DynamoDB

**Servicio de base de datos NoSQL totalmente gestionado**

- Base de datos de clave-valor y documentos.
- Latencia de milisegundos de un solo dígito a cualquier escala.
- **Serverless**: Sin servidores que gestionar.
- **Global Tables**: Replicación multi-región.

### Amazon ElastiCache

**Servicio de caché en memoria**

- Mejora el rendimiento de las aplicaciones almacenando datos frecuentemente accedidos en memoria.
- Motores: **Redis** y **Memcached**.

### Amazon Redshift

**Almacén de datos (Data Warehouse) totalmente gestionado**

- Diseñado para analítica de datos a escala de petabytes.
- Utiliza almacenamiento en columnas y procesamiento paralelo masivo (**MPP**).

---

## Redes y Entrega de Contenido (Networking and Content Delivery)

### Amazon VPC (Virtual Private Cloud)

**Red virtual aislada en AWS**

- **Subnets**: Divisiones de la **VPC** (**Public** vs **Private**).
- **Internet Gateway (IGW)**: Permite la comunicación entre la **VPC** e Internet.
- **NAT Gateway**: Permite que las instancias en subredes privadas accedan a Internet (solo salida).
- **Security Groups**: Firewall a nivel de instancia (**Stateful**).
- **Network ACLs**: Firewall a nivel de subred (**Stateless**).

### Amazon CloudFront

**Red de entrega de contenido (CDN) global**

- Entrega contenido (imágenes, videos, datos) a los usuarios con baja latencia.
- Utiliza **Edge Locations** para cachear el contenido.

### Amazon Route 53

**Servicio de DNS web altamente disponible y escalable**

- Registro de dominios y enrutamiento de tráfico.
- **Routing Policies**: Simple, Weighted, Latency, Failover, Geolocation.

### Elastic Load Balancing (ELB)

**Distribuye automáticamente el tráfico entrante entre múltiples objetivos**

- **Application Load Balancer (ALB)**: Nivel 7 (HTTP/HTTPS).
- **Network Load Balancer (NLB)**: Nivel 4 (TCP/UDP), alto rendimiento.
- **Gateway Load Balancer**: Nivel 3, para appliances virtuales.

### AWS Direct Connect

**Conexión de red privada dedicada**

- Conexión física dedicada desde tu centro de datos a AWS.
- Omite la Internet pública para mayor seguridad y rendimiento consistente.

---

## Gestión y Gobernanza (Management and Governance)

### AWS CloudFormation

**Infrastructure as Code (IaC)**

Define y aprovisiona infraestructura de AWS usando plantillas (JSON o YAML).

### AWS CloudTrail

**Gobernanza, cumplimiento y auditoría**

Registra todas las llamadas a la **API** en tu cuenta de AWS (quién hizo qué, cuándo y desde dónde).

### Amazon CloudWatch

**Servicio de monitoreo y observabilidad**

- **Metrics**: Recopila datos de rendimiento.
- **Alarms**: Activa acciones basadas en umbrales.
- **Logs**: Recopila y almacena archivos de log.

### AWS Systems Manager

**Hub operativo para recursos de AWS**

Gestiona parches, ejecuta comandos y almacena parámetros de configuración de forma centralizada.

### AWS Trusted Advisor

**Recomendaciones de mejores prácticas**

Inspecciona tu entorno y ofrece recomendaciones en 5 categorías: **Cost Optimization**, **Performance**, **Security**, **Fault Tolerance** y **Service Limits**.

---

## Servicios Adicionales (Additional Services)

- **Amazon SNS (Simple Notification Service)**: Servicio de mensajería **pub/sub** (notificaciones).
- **Amazon SQS (Simple Queue Service)**: Servicio de colas de mensajes (desacoplamiento).
- **AWS Step Functions**: Orquestación de flujos de trabajo **serverless**.
- **Amazon Athena**: Consulta datos en **S3** usando **SQL** estándar (sin servidor).
- **AWS Glue**: Servicio de **ETL** (Extraer, Transformar y Cargar) totalmente gestionado.
- **Amazon QuickSight**: Servicio de Business Intelligence (**BI**) para visualizaciones.
- **Amazon SageMaker**: Plataforma para construir, entrenar y desplegar modelos de **Machine Learning**.
- **Amazon Rekognition**: Análisis de imágenes y videos.

---

## Preguntas de Repaso

**1. ¿Qué servicio de cómputo de AWS es serverless y escala automáticamente basándose en las solicitudes?**
   - A) Amazon EC2
   - B) **AWS Lambda**
   - C) Amazon Lightsail
   *(Respuesta: B)*

**2. ¿Qué clase de almacenamiento de S3 es la más económica para datos que se conservan por 10 años y rara vez se acceden?**
   - A) S3 Standard
   - B) S3 Standard-IA
   - C) **S3 Glacier Deep Archive**
   *(Respuesta: C)*

**3. ¿Qué servicio permite definir infraestructura mediante código (Templates)?**
   - A) AWS CloudTrail
   - B) **AWS CloudFormation**
   - C) AWS Trusted Advisor
   *(Respuesta: B)*

---

[← Anterior: Seguridad y Cumplimiento](03-security-compliance.md) | [Volver al Inicio](README.md) | [Siguiente: Facturación, Precios y Soporte →](05-billing-pricing-support.md)
