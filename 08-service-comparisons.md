# Comparaciones Detalladas de Servicios y Árboles de Decisión

[← Anterior: Laboratorios Prácticos](07-hands-on-labs.md) | [Volver al Inicio](README.md) | [Siguiente: Escenarios de Examen →](09-exam-scenarios.md)

Este capítulo proporciona comparaciones detalladas de los servicios de **AWS** para ayudarle a elegir el servicio adecuado para casos de uso específicos. Estas comparaciones son críticas para el examen.

## Comparación Detallada de Servicios de Almacenamiento

| Característica | Amazon S3 | Amazon EBS | Amazon EFS | Instance Store |
|----------------|-----------|------------|------------|----------------|
| **Tipo** | Almacenamiento de Objetos | Almacenamiento en Bloque | Almacenamiento de Archivos | Bloque Efímero |
| **Caso de Uso** | Copias de seguridad, archivos, contenido web, lagos de datos | Volúmenes de arranque, bases de datos, datos transaccionales | Sistemas de archivos compartidos, gestión de contenido | Datos temporales, cachés, buffers |
| **Acceso** | HTTP/S API, SDK, CLI | Adjunto a la instancia **EC2** | Protocolo **NFSv4**, múltiples instancias **EC2** | Adjunto directo a la instancia **EC2** |
| **Durabilidad** | 11 nueves (99.999999999%) | Replicado dentro de una **AZ** | Replicado entre **AZs** en la Región | Se pierde cuando la instancia se detiene |
| **Escalabilidad** | Ilimitada | Hasta 64 TB por volumen | Escala de petabytes | Fija al tipo de instancia |
| **Rendimiento** | Varía según la clase | Hasta 256,000 **IOPS** | Escala con el tamaño | Los **IOPS** más altos por instancia |
| **Costo** | Bajo por GB, varía según la clase | $0.08-0.125/GB-mes | $0.30/GB-mes | Incluido con la instancia |
| **Disponibilidad** | 99.9-99.99% **SLA** | 99.8-99.9% | 99.99% | Depende de la instancia |
| **Respaldo** | **Versioning**, **Lifecycle** | **Snapshots** a **S3** | **AWS Backup** | No persistente |
| **Multi-AZ** | Sí | No (una sola **AZ**) | Sí | No |
| **Acceso Concurrente** | Ilimitado | Una sola instancia **EC2** | Miles de instancias | Una sola instancia |

### Cuándo Usar Cada Servicio de Almacenamiento

**Use S3 cuando:**
- Almacene contenido de sitios web estáticos.
- Copias de seguridad y archivado.
- Lagos de datos (**Data Lakes**) para analítica.
- Distribución de software/medios.
- Recuperación ante desastres.

**Use EBS cuando:**
- Se requieran volúmenes de arranque de **EC2**.
- Bases de datos que requieren **IOPS** consistentes.
- Aplicaciones que necesitan almacenamiento en bloque.
- El acceso desde una sola instancia sea suficiente.

**Use EFS cuando:**
- Múltiples instancias necesitan acceso compartido.
- Sistemas de gestión de contenido (**CMS**).
- Servicio web desde almacenamiento compartido.
- Analítica de **Big Data** que requiere acceso compartido.
- Migración de aplicaciones locales (**Lift-and-shift**).

**Use Instance Store cuando:**
- Necesidades de almacenamiento temporal.
- Caché y buffers.
- Datos de trabajo (**Scratch data**).
- Datos replicados entre instancias.
- Se necesiten los **IOPS** más altos posibles.

## Comparación Detallada de Servicios de Bases de Datos

| Servicio | Tipo | Caso de Uso | Escalado | Modelo de Precios | Administrado |
|----------|------|-------------|----------|-------------------|--------------|
| **RDS** | Relacional (SQL) | Aplicaciones tradicionales, **OLTP** | Vertical, **Read Replicas** | Instancia + almacenamiento | Totalmente administrado |
| **Aurora** | Relacional (compatible con **MySQL**/**PostgreSQL**) | **OLTP** de alto rendimiento | Escalado automático de almacenamiento | **Serverless** o provisionado | Totalmente administrado |
| **DynamoDB** | **NoSQL** Clave-Valor | Apps web/móviles, juegos, **IoT** | Horizontal automático | Bajo demanda o provisionado | Totalmente administrado |
| **Redshift** | Almacén de datos (**OLAP**) | Analítica, **BI**, **Big Data** | Añadir nodos | Nodos + almacenamiento | Totalmente administrado |
| **ElastiCache** | Caché en memoria | Almacenes de sesiones, tablas de clasificación | Añadir nodos | Nodos (por hora) | Totalmente administrado |
| **Neptune** | Base de datos de grafos | Redes sociales, recomendaciones | Vertical | Instancias | Totalmente administrado |
| **DocumentDB** | Documental (compatible con **MongoDB**) | Gestión de contenido, catálogos | Vertical, réplicas | Instancias | Totalmente administrado |
| **Timestream** | Series temporales | **IoT**, métricas de **DevOps** | Automático | Almacenamiento + consultas | Totalmente administrado |

### Árbol de Decisión para la Selección de Bases de Datos

```
¿Necesita SQL y transacciones ACID?
├─ Sí
│  ├─ Aplicación tradicional, base de datos existente → RDS
│  ├─ Necesita rendimiento extremo → Aurora
│  └─ Necesidades específicas: Oracle/SQL Server → RDS con ese motor
│
└─ ¿Necesita NoSQL?
   ├─ Clave-valor, escala a millones de solicitudes/seg → DynamoDB
   ├─ Base de datos documental, compatible con MongoDB → DocumentDB
   └─ Relaciones de grafos → Neptune

¿Necesita caché?
├─ Caché en memoria → ElastiCache
├─ Características de Redis necesarias → ElastiCache para Redis
└─ Caché simple → ElastiCache para Memcached

¿Necesita almacén de datos/analítica?
├─ OLAP, BI, Big Data → Redshift
├─ Consultar datos en S3 → Athena
└─ Analítica en tiempo real → Kinesis Data Analytics

¿Datos de series temporales?
└─ IoT, métricas, logs → Timestream
```

## Árbol de Decisión de Servicios de Cómputo

### Elija su Servicio de Cómputo

**¿Necesita control total sobre el sistema operativo y la configuración?**
- Sí → Use **EC2**
  - ¿Quiere ahorrar costos para cargas de trabajo predecibles? → Use **Reserved Instances**
  - ¿Cargas de trabajo por lotes tolerantes a fallos? → Use **Spot Instances**

**¿Quiere ejecutar código sin administrar servidores?**
- Basado en eventos, ejecución < 15 min → Use **Lambda**
- Larga duración, sin estado → Use **Fargate**

**¿Necesita desplegar aplicaciones web rápidamente?**
- Despliegue simple, no quiere administración de infraestructura → Use **Elastic Beanstalk**
- Necesita precios predecibles para proyectos pequeños → Use **Lightsail**

**¿Usa contenedores?**
- Quiere orquestación nativa de AWS → Use **ECS**
- Necesita compatibilidad con Kubernetes → Use **EKS**
- No quiere administrar servidores → Use **Fargate** (con **ECS** o **EKS**)

**¿Ejecuta trabajos de procesamiento por lotes?**
- Use **AWS Batch**

### Tabla de Selección de Servicios de Cómputo

| Requisito | Mejor Opción | Por qué |
|-----------|--------------|---------|
| Control total del SO | EC2 | Flexibilidad completa |
| **Serverless**, basado en eventos | Lambda | Sin servidores, pago por uso |
| Desplegar app rápidamente | Elastic Beanstalk | **PaaS**, administrado |
| Contenedores, Kubernetes | EKS | Compatible con Kubernetes |
| Contenedores, nativo de AWS | ECS | Más simple que **EKS** |
| Contenedores, sin servidores | Fargate | Contenedores **Serverless** |
| Procesamiento por lotes | AWS Batch | Optimizado para lotes |
| Sitio web simple, blog | Lightsail | Precios predecibles |

## Profundización en Componentes de Red

| Componente | Propósito | Puntos Clave |
|------------|-----------|--------------|
| **Internet Gateway (IGW)** | Conectar **VPC** a internet | Uno por **VPC**; habilita el acceso a internet para subredes públicas |
| **NAT Gateway** | Internet de salida desde subredes privadas | Altamente disponible; colocado en subred pública; se cobra por hora + datos |
| **NAT Instance** | Alternativa a **NAT Gateway** | Instancia **EC2**; administrada por usted; menor costo pero menos confiable |
| **VPC Peering** | Conectar dos **VPCs** | No transitivo; puede ser entre cuentas/regiones; sin **CIDRs** superpuestos |
| **Transit Gateway** | Hub para conectar **VPCs** | Simplifica topologías de red complejas; gestión centralizada |
| **VPN Gateway** | Conexión **VPN** a instalaciones locales | **VPN IPsec**; cifrada a través de internet; configuración rápida |
| **Direct Connect** | Conexión dedicada a instalaciones locales | Privada, ancho de banda consistente; costosa; tarda semanas en provisionarse |
| **VPC Endpoints** | Conexión privada a servicios de **AWS** | No requiere internet; **Interface** o **Gateway endpoints**; reduce costos |
| **PrivateLink** | Conectividad privada a servicios | Acceda a servicios en otras **VPCs**; no requiere **VPC peering** |

### Guía de Decisión de Componentes de VPC

**Subred Pública vs. Privada:**
- **Subred Pública**: Tiene ruta al **Internet Gateway** (0.0.0.0/0 → **IGW**)
- **Subred Privada**: Sin acceso directo a internet; usa **NAT** para salida

**NAT Gateway vs. NAT Instance:**
- **NAT Gateway**: Administrado por **AWS**, altamente disponible, más fácil, más costoso
- **NAT Instance**: Instancia **EC2** que usted administra, más barata, menos confiable

**VPC Peering vs. Transit Gateway:**
- **VPC Peering**: Conecta dos **VPCs** directamente, no transitivo
- **Transit Gateway**: Modelo **hub-and-spoke**, simplifica múltiples conexiones de **VPC**

**VPN vs. Direct Connect:**
- **VPN**: Configuración rápida (horas), cifrada, a través de internet, rendimiento variable
- **Direct Connect**: Enlace dedicado, rendimiento consistente, costoso, tarda semanas

**Tipos de VPC Endpoint:**
- **Interface Endpoint**: **ENI** con IP privada, utiliza **PrivateLink**
- **Gateway Endpoint**: Entrada en la tabla de rutas, solo para **S3** y **DynamoDB**

## Comparación Detallada de Equilibradores de Carga

| Característica | ALB | NLB | Gateway LB |
|----------------|-----|-----|------------|
| **Capa OSI** | Capa 7 (Aplicación) | Capa 4 (Transporte) | Capa 3 (Red) |
| **Protocolo** | HTTP, HTTPS, WebSocket | TCP, UDP, TLS | IP |
| **Enrutamiento** | Basado en ruta, host, cadena de consulta | Dirección IP, puerto | N/A |
| **Caso de Uso** | Aplicaciones web, microservicios | Alto rendimiento, baja latencia, IP estática | Aplicaciones de terceros |
| **Tipos de Destino** | IP, instancia, Lambda | IP, instancia, **ALB** | IP, instancia |
| **Rendimiento** | Bueno | Extremo (millones de solicitudes/seg) | Alto |
| **IP Estática** | No (solo DNS) | Sí (**Elastic IP**) | N/A |
| **Terminación SSL** | Sí | Sí | No |
| **WebSocket** | Sí | Sí | No |
| **Health Checks** | Avanzados | Básicos | Avanzados |
| **Precios** | Por hora + **LCU** | Por hora + **LCU** | Por hora + **LCU** |

### Cuándo Usar Cada Equilibrador de Carga

**Application Load Balancer (ALB):**
- Aplicaciones web.
- Arquitecturas de microservicios.
- Necesita enrutamiento avanzado (basado en ruta, host, cabecera).
- Destino de funciones **Lambda**.
- Necesita soporte para **WebSocket**.
- Se requiere enrutamiento basado en contenido.

**Network Load Balancer (NLB):**
- Se requiere un rendimiento extremo (millones de solicitudes por segundo).
- Se necesita una latencia ultra baja.
- Se requieren direcciones IP estáticas.
- Tráfico **TCP**/**UDP**.
- Protocolos que no son **HTTP**.
- Preservar la dirección IP de origen.

**Gateway Load Balancer:**
- Desplegar aplicaciones virtuales de terceros.
- Firewalls, **IDS**/**IPS**.
- Inspección profunda de paquetes.
- Pasarela de red transparente.

**Classic Load Balancer (CLB):**
- Aplicaciones heredadas.
- Red **EC2-Classic**.
- Se está eliminando gradualmente; migre a **ALB** o **NLB**.

## Matriz Completa de Servicios de Seguridad

| Servicio | Qué hace | Cuándo usarlo |
|----------|----------|---------------|
| **IAM** | Gestión de identidad y acceso | Controlar quién puede acceder a qué en **AWS** |
| **AWS Organizations** | Gestión de múltiples cuentas | Centralizar facturación, aplicar políticas en todas las cuentas |
| **AWS SSO** | Inicio de sesión único | Gestionar centralmente el acceso a múltiples cuentas y aplicaciones |
| **Cognito** | Autenticación de usuarios para apps | Añadir registro/inicio de sesión a apps móviles y web |
| **Directory Service** | Active Directory administrado | Integrar **AWS** con el **Microsoft AD** existente |
| **Secrets Manager** | Almacenar y rotar secretos | Rotar automáticamente las credenciales de la base de datos |
| **KMS** | Gestión de claves de cifrado | Crear y controlar claves de cifrado |
| **CloudHSM** | Módulos de seguridad de hardware | Hardware dedicado para cumplimiento normativo |
| **Certificate Manager** | Certificados **SSL**/**TLS** | Certificados gratuitos para **ELB**, **CloudFront**, **API Gateway** |
| **WAF** | Firewall de aplicaciones web | Proteger contra inyección **SQL**, ataques **XSS** |
| **Shield Standard** | Protección **DDoS** | Protección automática (gratis) |
| **Shield Advanced** | Protección **DDoS** mejorada | Equipo de respuesta **DDoS** 24/7, protección de costos ($3,000/mes) |
| **GuardDuty** | Detección de amenazas | Monitoreo continuo de actividad maliciosa |
| **Inspector** | Evaluación de vulnerabilidades | Escanear **EC2** e imágenes de contenedores en busca de vulnerabilidades |
| **Macie** | Privacidad y protección de datos | Descubrir y proteger datos sensibles en **S3** |
| **Detective** | Investigación de seguridad | Analizar e investigar problemas de seguridad |
| **Security Hub** | Gestión de la postura de seguridad | Vista centralizada de alertas de seguridad y cumplimiento |
| **Firewall Manager** | Gestión centralizada de firewalls | Gestionar **WAF**, **Shield** en todas las cuentas |

### Guía de Selección de Servicios de Seguridad

**Identidad y Acceso:**
- Usuarios/servicios internos de AWS → **IAM**
- Usuarios de aplicaciones externas → **Cognito**
- Múltiples cuentas de AWS → **Organizations** + **SSO**
- Integración con **AD** local → **Directory Service**

**Protección de Datos:**
- Gestión de claves de cifrado → **KMS**
- Cifrado para cumplimiento normativo → **CloudHSM**
- Almacenar secretos → **Secrets Manager**
- Descubrir datos sensibles → **Macie**

**Protección contra Amenazas:**
- Protección **DDoS** → **Shield** (**Standard** para todos, **Advanced** para críticos)
- Ataques a aplicaciones web → **WAF**
- Detección de amenazas → **GuardDuty**
- Escaneo de vulnerabilidades → **Inspector**
- Investigación de seguridad → **Detective**

**Cumplimiento y Gobernanza:**
- Seguridad multi-cuenta → **Security Hub**
- Firewall multi-cuenta → **Firewall Manager**
- Cumplimiento de configuración → **Config**
- Registro de auditoría → **CloudTrail**

## Referencia Rápida de Límites de Servicio

| Servicio | Límite por Defecto | Notas |
|----------|-------------------|-------|
| **Instancias EC2 (Bajo Demanda)** | 20 por región | Puede solicitar aumento |
| **VPCs por Región** | 5 | Puede solicitar aumento |
| **Internet Gateways por Región** | 5 | Típicamente uno por **VPC** |
| **Buckets S3 por Cuenta** | 100 | Límite elástico, puede aumentar |
| **Tamaño de Objeto S3** | 5 TB máx | Use carga multiparte para > 100 MB |
| **Instancias de DB RDS** | 40 por región | Puede solicitar aumento |
| **Ejecuciones Concurrentes de Lambda** | 1,000 | Puede solicitar aumento |
| **Tiempo de Espera de Función Lambda** | 15 minutos máx | No puede aumentarse |
| **Stacks de CloudFormation** | 200 por región | Puede solicitar aumento |
| **Usuarios IAM por Cuenta** | 5,000 | Use roles/identidades federadas en su lugar |
| **Grupos IAM por Cuenta** | 300 | Planifique la estructura de grupos cuidadosamente |

> **Consejo de Examen:** La mayoría de los límites de servicio pueden aumentarse solicitándolo a través del Soporte de **AWS**. Algunos límites fijos (como el tiempo de espera de 15 minutos de **Lambda**) no pueden cambiarse.

## Guía de Selección de Almacenamiento

| Caso de Uso | Servicio | Razón |
|-------------|----------|-------|
| Copias de seguridad, archivos | S3 Glacier | Costo más bajo |
| Contenido de sitio web | S3 + CloudFront | Escalable, global |
| Volumen de arranque de **EC2** | EBS | Almacenamiento en bloque |
| Sistema de archivos compartido | EFS | Multi-adjunto |
| Recursos compartidos de Windows | FSx for Windows | **SMB**, integración con **AD** |
| Cargas de trabajo **HPC** | FSx for Lustre | Alto rendimiento |
| Datos temporales | Instance Store | Los **IOPS** más altos |
| Transferencia fuera de línea | Snow Family | Grandes volúmenes de datos |

## Resumen de Comparación Clave

### Matriz de Decisión de Almacenamiento
- **Objetos/Archivos** → **S3**
- **Almacenamiento en bloque para EC2** → **EBS**
- **Sistema de archivos compartido** → **EFS**
- **Archivo** → **S3 Glacier**
- **Gran migración** → **Snow Family**

### Matriz de Decisión de Base de Datos
- **Relacional/SQL** → **RDS** o **Aurora**
- **NoSQL clave-valor** → **DynamoDB**
- **Caché** → **ElastiCache**
- **Almacén de datos** → **Redshift**
- **Grafos** → **Neptune**

### Matriz de Decisión de Cómputo
- **Control total** → **EC2**
- **Funciones Serverless** → **Lambda**
- **Contenedores con orquestación** → **ECS**/**EKS**
- **Contenedores Serverless** → **Fargate**
- **Apps web simples** → **Lightsail**
- **Despliegue rápido de apps** → **Elastic Beanstalk**

### Matriz de Decisión de Redes
- **Red privada** → **VPC**
- **Entrega de contenido** → **CloudFront**
- **DNS** → **Route 53**
- **Equilibrio de carga** → **ALB**/**NLB**
- **Conectividad híbrida** → **Direct Connect** o **VPN**

## Comparación de Servicios Serverless

| Servicio | Tipo | Caso de Uso | Modelo de Ejecución | Precios | Integración |
|----------|------|-------------|---------------------|---------|-------------|
| **Lambda** | Función como Servicio | Ejecución de código basada en eventos, APIs, automatización | Hasta 15 min por ejecución | Por solicitud + duración | Todos los servicios de AWS, **API Gateway** |
| **Fargate** | Contenedores **Serverless** | Contenedores de larga duración, microservicios | Continuo | Por vCPU + memoria por hora | **ECS**, **EKS** |
| **AppSync** | GraphQL API | Aplicaciones móviles/web en tiempo real y fuera de línea | Basado en solicitudes | Por consulta + transferencia de datos | **DynamoDB**, **Lambda**, **RDS** |
| **Step Functions** | Orquestación de flujos de trabajo | Coordinar múltiples funciones **Lambda**, servicios | Ejecución de máquina de estados | Por transición de estado | **Lambda**, **ECS**, **SNS**, **SQS**, etc. |

### Selección de Servicio Serverless

**Use Lambda cuando:**
- Procesamiento basado en eventos (cargas de **S3**, flujos de **DynamoDB**).
- Backends de **API** a través de **API Gateway**.
- Tareas programadas (**CloudWatch Events**).
- Procesamiento de archivos/flujos en tiempo real.
- Tiempo de ejecución < 15 minutos.
- Operaciones sin estado.

**Use Fargate cuando:**
- Aplicaciones de larga duración.
- Microservicios que se ejecutan continuamente.
- Migración de contenedores **Docker** a **Serverless**.
- Necesita más de 15 minutos de ejecución.
- La aplicación requiere conexiones persistentes.
- Quiere beneficios de contenedores sin administrar servidores.

**Use AppSync cuando:**
- Construcción de **APIs GraphQL**.
- Se necesita sincronización de datos en tiempo real.
- Se requiere soporte para aplicaciones móviles fuera de línea.
- Múltiples fuentes de datos (**DynamoDB**, **Lambda**, **RDS**).
- Necesita almacenamiento en caché y suscripciones integradas.

**Use Step Functions cuando:**
- Coordinación de múltiples funciones **Lambda**.
- Flujos de trabajo complejos con lógica de ramificación.
- Necesita reintentos y manejo de errores.
- Flujos de trabajo de larga duración (hasta 1 año).
- Se requiere gestión visual del flujo de trabajo.
- Orquestación de microservicios.

## Comparación de Servicios de Analítica

| Servicio | Tipo | Mejor para | Fuente de Datos | Método de Consulta | Precios |
|----------|------|------------|-----------------|-------------------|---------|
| **Athena** | Consulta interactiva | Consultas **SQL ad-hoc** en **S3** | **S3** (**CSV**, **JSON**, **Parquet**, **ORC**) | **SQL** a través de **Presto** | Por TB escaneado |
| **EMR** | Plataforma de **Big Data** | Cargas de trabajo de **Hadoop**, **Spark** | **S3**, **HDFS** | **MapReduce**, **Spark**, **Hive**, **Pig** | Por hora de instancia |
| **Redshift** | Almacén de datos | **OLAP**, **BI**, consultas complejas | **S3**, bases de datos a través de **COPY** | **SQL** (compatible con **PostgreSQL**) | Por hora de nodo |
| **QuickSight** | Visualización de **BI** | Cuadros de mando e informes | **Athena**, **Redshift**, **RDS**, **S3** | Interfaz visual | Por usuario/sesión |
| **Glue** | Servicio **ETL** | Preparación de datos, catalogación | **S3**, **RDS**, **Redshift** | **Python**/**Scala** (**Spark**) | Por hora de **DPU** |
| **Kinesis** | Streaming en tiempo real | Procesamiento de datos en tiempo real | Streams, aplicaciones, **IoT** | **Kinesis API**, **SQL** | Por hora de **shard** |
| **Lake Formation** | Lago de datos | Construir y asegurar lagos de datos | **S3** | Integrado con **Athena**, **EMR** | Costos de almacenamiento + consulta |
| **MSK** | **Kafka** administrado | Streaming con **Kafka** | Aplicaciones | **Kafka API** | Por hora de **broker** |

### Guía de Selección de Servicios de Analítica

**Use Athena cuando:**
- Consulte datos que ya están en **S3**.
- **Serverless**, sin infraestructura que administrar.
- Análisis y exploración **ad-hoc**.
- Rentable para consultas poco frecuentes.
- No hay necesidad de cargar datos en una base de datos.

**Use EMR cuando:**
- Necesita el ecosistema completo de **Hadoop**/**Spark**.
- Pipelines de procesamiento d**Use Redshift cuando:**
- Almacén de datos para cargas de trabajo **OLAP**.
- Joins complejos a través de grandes conjuntos de datos.
- Las herramientas de **BI** necesitan un rendimiento de consulta rápido.
- Analítica a escala de petabytes.
- Se necesita un rendimiento de consulta consistente.

**Use QuickSight cuando:**
- Cree cuadros de mando empresariales.
- Visualice datos de múltiples fuentes.
- Comparta información con las partes interesadas.
- Se necesite **BI** amigable para dispositivos móviles.
- Precios rentables por usuario.

**Use Glue cuando:**
- Trabajos **ETL** para preparar datos.
- Se necesite descubrimiento automático de esquemas.
- Catálogo de datos para servicios de analítica.
- Mueva datos entre almacenes de datos.
- **ETL Serverless** sin administrar servidores.

**Use Kinesis cuando:**
- Streaming de datos en tiempo real.
- Recolección de datos de registros y eventos.
- Analítica en tiempo real.
- Análisis de flujos de clics (**Clickstream**).
- Procesamiento de telemetría de **IoT**.

## Comparación de Herramientas de Migración

| Herramienta | Propósito | Caso de Uso | Tamaño de Datos | Método de Transferencia | Plazo |
|-------------|-----------|-------------|-----------------|-------------------------|-------|
| **MGN (Application Migration Service)** | Migración de servidores **Lift-and-shift** | Migrar servidores físicos/virtuales a **EC2** | Cualquiera | Red (replicación basada en agentes) | Días a semanas |
| **DMS (Database Migration Service)** | Migración de bases de datos | Migrar bases de datos con tiempo de inactividad mínimo | Bases de datos | Red (replicación continua) | Horas a días |
| **DataSync** | Transferencia de datos | Migrar datos de archivos hacia/desde **AWS** | **GBs** a **PBs** | Red (acelerada) | Horas a días |
| **Snow Family** | Transferencia física de datos | Transferencia masiva de datos fuera de línea | **TBs** a **Exabytes** | Envío de dispositivo físico | 1-2 semanas |
| **Transfer Family** | **SFTP**/**FTPS**/**FTP** | Migrar flujos de trabajo de transferencia de archivos | Cualquiera | Red (protocolos heredados) | Inmediato |
| **Migration Hub** | Seguimiento de migración | Planificación y seguimiento de migración centralizados | N/A | Cuadro de mando | Continuo |

### Detalles de las Herramientas de Migración

**AWS Application Migration Service (MGN):**
- Anteriormente **CloudEndure Migration**.
- **Lift-and-shift** automatizado para servidores.
- Replicación continua a nivel de bloque.
- Tiempo de inactividad mínimo (minutos).
- Soporta servidores físicos, virtuales y en la nube.
- Gratis durante 90 días (pague solo por los recursos).

**Database Migration Service (DMS):**
- Migraciones homogéneas (**Oracle** a **Oracle**).
- Migraciones heterogéneas (**Oracle** a **Aurora**).
- Replicación continua para un tiempo de inactividad mínimo.
- Herramienta de conversión de esquemas (**SCT**) para diferentes motores.
- Soporta la mayoría de las bases de datos comerciales y de código abierto.

**DataSync:**
- 10 veces más rápido que las herramientas de código abierto.
- Automatiza la transferencia de datos.
- Valida la integridad de los datos.
- Uso para: **NFS**/**SMB** a **EFS**, **FSx**.
- Optimización de ancho de banda y cifrado.
- Programación de transferencias.

**Snow Family:**
- **Snowcone**: 8 TB utilizables, computación en el borde (**Edge computing**).
- **Snowball Edge**: 80 TB utilizables, opciones de cómputo.
- **Snowmobile**: 100 PB, contenedor del tamaño de un camión.
- Úselo cuando: Ancho de banda limitado, semanas para transferir por red.
- Incluye cómputo para procesamiento en el borde.
- Rugerizado para entornos hostiles.

**Transfer Family:**
- Servicio administrado de **SFTP**, **FTPS**, **FTP**.
- Almacena archivos en **S3** o **EFS**.
- Migre flujos de trabajo de transferencia de archivos heredados.
- Intégrelo con la autenticación existente.
- Totalmente administrado, sin servidores que mantener.

### Árbol de Decisión de Migración

```
¿Cuántos datos desea migrar?
├─ < 10 TB con buen ancho de banda → DataSync por red
├─ 10-80 TB o ancho de banda limitado → Snowcone/Snowball
└─ > 80 TB o ancho de banda muy limitado → Snowball Edge/Snowmobile

¿Qué tipo de datos?
├─ Servidores/VMs → MGN (Application Migration Service)
├─ Bases de datos → DMS (Database Migration Service)
├─ Archivos/NAS → DataSync o Snow Family
└─ Flujos de trabajo SFTP → Transfer Family

¿Necesita sincronización continua?
├─ Sí → DataSync (programado) o DMS (continuo)
└─ No → Transferencia única con Snow Family

¿Ancho de banda de red disponible?
├─ > 100 Mbps → DataSync
├─ 10-100 Mbps → Considere Snowcone
└─ < 10 Mbps → Se requiere Snow Family
```

## Comparación Detallada de Orquestación de Contenedores

| Característica | ECS | EKS | Fargate (con ECS) | Fargate (con EKS) |
|----------------|-----|-----|-------------------|-------------------|
| **Orquestador** | Propietario de AWS | **Kubernetes** | **ECS** (**Serverless**) | **Kubernetes** (**Serverless**) |
| **Curva de Aprendizaje** | Más fácil | Más pronunciada | La más fácil | Moderada |
| **Plano de Control** | Gratis | $0.10/hora por clúster | Gratis | $0.10/hora por clúster |
| **Gestión de Infraestructura** | Usted administra **EC2** | Usted administra los nodos | **AWS** administra | **AWS** administra |
| **Compatibilidad con Kubernetes** | No | Sí | No | Sí |
| **Caso de Uso** | Contenedores nativos de AWS | Cargas de trabajo de **Kubernetes** | Contenedores **Serverless** | **Kubernetes Serverless** |
| **Precios** | Solo costos de **EC2** | Clúster + costos de **EC2** | Por **vCPU** + memoria | Por **vCPU** + memoria |
| **Complementos/Ecosistema** | Servicios de AWS | Ecosistema de **Kubernetes** | Servicios de AWS | Ambos |
| **Multi-nube** | Solo AWS | Portable | Solo AWS | Más portable |

### Selección de Servicio de Contenedores

**Use ECS cuando:**
- Se prefiera una integración profunda con AWS.
- No necesite **Kubernetes**.
- Quiera una orquestación de contenedores más simple.
- El equipo esté familiarizado con los servicios de AWS.
- No haya inversión previa en **Kubernetes**.
- Sea sensible al costo (sin costo de plano de control).

**Use EKS cuando:**
- Necesite compatibilidad con **Kubernetes**.
- Tenga cargas de trabajo de **Kubernetes** existentes.
- Quiera portabilidad entre nubes.
- El equipo tenga experiencia en **Kubernetes**.
- Necesite herramientas del ecosistema de **Kubernetes**.
- Tenga una estrategia híbrida o multi-nube.

**Use Fargate (con ECS o EKS) cuando:**
- No quiera administrar servidores.
- Tenga cargas de trabajo variables.
- Quiera precios de pago por tarea.
- Necesite aislamiento de seguridad por tarea.
- Despliegue rápido sin infraestructura.
- Entornos de desarrollo y pruebas.

**Tipos de Lanzamiento de Contenedores:**
- **EC2 Launch Type**: Usted administra las instancias **EC2**, más control, costo potencialmente menor.
- **Fargate Launch Type**: **Serverless**, sin gestión de **EC2**, pago por tarea.

### Patrones de Arquitectura de Contenedores

**Arquitectura ECS:**
- **Task Definition** (configuración del contenedor).
- **Service** (mantiene el recuento deseado).
- **Cluster** (agrupación lógica).
- Tipo de lanzamiento **EC2** o **Fargate**.

**Arquitectura EKS:**
- Plano de control de **Kubernetes** administrado.
- Nodos trabajadores (**EC2** o **Fargate**).
- Objetos estándar de **Kubernetes** (**Pods**, **Deployments**).
- Compatible con **kubectl** y herramientas de **Kubernetes**.

## Comparación de Servicios de Monitoreo y Registro

| Servicio | Propósito | Qué monitorea | Retención de Datos | Caso de Uso | Precios |
|----------|-----------|---------------|--------------------|-------------|---------|
| **CloudWatch** | Monitoreo y observabilidad | Métricas, logs, eventos, alarmas | 15 meses (métricas), personalizada (logs) | Monitoreo de rendimiento, alarmas | Por métrica, por **GB** ingerido |
| **CloudTrail** | Registro de actividad de **API** | Todas las llamadas **API** en la cuenta de **AWS** | 90 días (historial), indefinido (**S3**) | Auditoría de seguridad, cumplimiento | Primer rastro gratis, $2/100k eventos |
| **Config** | Seguimiento de configuración de recursos | Configuraciones de recursos de **AWS** | Configurable | Cumplimiento, seguimiento de cambios | Por ítem de configuración registrado |
| **X-Ray** | Rastreo distribuido | Rendimiento de aplicaciones, solicitudes | 30 days | Depuración, análisis de rendimiento | Por rastro registrado + recuperado |
| **VPC Flow Logs** | Registro de tráfico de red | Interfaces de red en la **VPC** | **S3** o **CloudWatch** | Resolución de problemas de red, seguridad | Solo costos de almacenamiento |
| **EventBridge** | Bus de eventos | Eventos de AWS, **SaaS**, personalizados | N/A (enrutamiento) | Arquitecturas basadas en eventos | Por millón de eventos |

### Detalles de los Servicios de Monitoreo

**CloudWatch:**
- **Métricas**: **CPU**, disco, red para **EC2**, **RDS**, etc.
- **Logs**: Registros de aplicaciones, registros del sistema.
- **Alarmas**: Activan acciones basadas en métricas.
- **Dashboards**: Visualización de métricas.
- **Events**/**EventBridge**: Reacción a cambios de estado.
- **Insights**: Consulta y análisis de datos de registro.
- **Synthetics**: Monitoreo de puntos finales y **APIs**.
- Monitoreo predeterminado vs. detallado.

**CloudTrail:**
- Registra cada llamada a la **API** en su cuenta.
- Quién, qué, cuándo, dónde para todas las acciones.
- Esencial para seguridad y cumplimiento.
- Se integra con **CloudWatch Logs** para alertas.
- Rastros multirregión y de organización.
- Validación de integridad con hashing de archivos de registro.

**Config:**
- Registra los cambios de configuración a lo largo del tiempo.
- Historial de configuración para los recursos.
- Verificación de cumplimiento con **Config Rules**.
- Se integra con **Systems Manager** para la remediación.
- Seguimiento de relaciones entre recursos.
- Soporte multi-cuenta y multi-región.

**X-Ray:**
- Rastreo de solicitudes de extremo a extremo.
- Visualización del mapa de servicios.
- Identificación de cuellos de botella de rendimiento.
- Seguimiento de solicitudes a través de microservicios.
- Se integra con **Lambda**, **ECS**, **Elastic Beanstalk**.
- Anotaciones y metadatos para filtrado.

### Guía de Decisión de Monitoreo

**Use CloudWatch cuando:**
- Necesite métricas de rendimiento y alarmas.
- Monitoree la utilización de recursos.
- Configure respuestas automatizadas a las métricas.
- Recopile y analice registros.
- Cree cuadros de mando operativos.

**Use CloudTrail cuando:**
- Se requiera auditoría de seguridad.
- Necesidades de cumplimiento y gobernanza.
- Rastree quién hizo qué en AWS.
- Investigue incidentes de seguridad.
- Análisis forense de la actividad de la **API**.

**Use Config cuando:**
- Necesite un inventario de recursos.
- Rastree cambios de configuración a lo largo del tiempo.
- Auditoría de cumplimiento contra reglas.
- Comprenda las relaciones entre recursos.
- Historial de configuración en un punto en el tiempo.

**Use X-Ray cuando:**
- Depure aplicaciones distribuidas.
- Analice el rendimiento de microservicios.
- Identifique problemas de latencia.
- Rastree solicitudes a través de servicios.
- Comprenda el comportamiento de la aplicación.

## Comparación de Servicios de IA/ML

| Servicio | Tipo | Caso de Uso | Experiencia Requerida | Entrenamiento Requerido | Precios |
|----------|------|-------------|-----------------------|-------------------------|---------|
| **SageMaker** | Plataforma completa de **ML** | Modelos de **ML** personalizados, entrenamiento, despliegue | Alta (científicos de datos) | Sí, modelos personalizados | Por hora de instancia + almacenamiento |
| **Rekognition** | Visión por computadora | Análisis de imagen/video, detección facial | Baja (llamadas a la **API**) | No, pre-entrenado | Por imagen/video analizado |
| **Comprehend** | **NLP** | Análisis de texto, sentimiento, entidades | Baja (llamadas a la **API**) | No, pre-entrenado | Por unidad de texto |
| **Lex** | IA Conversacional | Chatbots, asistentes de voz | Media | Sí, entrenar intenciones | Por solicitud |
| **Polly** | Texto a voz | Convertir texto en habla realista | Baja (llamadas a la **API**) | No, pre-entrenado | Por carácter |
| **Transcribe** | Voz a texto | Convertir audio en texto | Baja (llamadas a la **API**) | No, pre-entrenado | Por segundo de audio |
| **Translate** | Traducción | Traducción de texto en tiempo real | Baja (llamadas a la **API**) | No, pre-entrenado | Por carácter |
| **Forecast** | Series temporales | Pronóstico de series temporales | Media | Sí, entrenar con sus datos | Por pronóstico + horas de entrenamiento |
| **Personalize** | Recomendaciones | Motores de recomendación | Media | Sí, entrenar con sus datos | Por hora de entrenamiento + uso |
| **Textract** | Análisis de documentos | Extraer texto de documentos | Baja (llamadas a la **API**) | No, pre-entrenado | Por página analizada |
| **Kendra** | Búsqueda inteligente | Búsqueda empresarial con **NLP** | Baja | Sí, indexar documentos | Por hora + consultas |

### Detalles de los Servicios de IA/ML

**SageMaker (ML Personalizado):**
- Construir, entrenar y desplegar modelos de **ML** personalizados.
- Cuadernos **Jupyter** para el desarrollo.
- Algoritmos y frameworks integrados.
- **AutoML** con **Autopilot**.
- Monitoreo y depuración de modelos.
- Para científicos de datos e ingenieros de **ML**.

**Rekognition (Visión por Computadora):**
- Detección de objetos y escenas.
- Análisis y reconocimiento facial.
- Texto en imágenes (**OCR**).
- Moderación de contenido.
- Reconocimiento de celebridades.
- Análisis de video.

**Comprehend (Procesamiento de Lenguaje Natural):**
- Análisis de sentimiento (positivo, negativo, neutral).
- Reconocimiento de entidades (personas, lugares, fechas).
- Extracción de frases clave.
- Detección de idioma.
- Modelado de temas.
- Detección de **PII**.

**Lex (Interfaces Conversacionales):**
- Construir chatbots y asistentes de voz.
- Impulsa a Amazon **Alexa**.
- Comprensión del lenguaje natural.
- Conversaciones de múltiples turnos.
- Integración con **Lambda** para el cumplimiento.
- Desplegar en plataformas móviles, web y de mensajería.

**Polly (Texto a Voz):**
- Más de 60 voces en más de 30 idiomas.
- **TTS** neuronal para una voz más natural.
- Soporte **SSML** para personalización.
- Marcas de voz para sincronización labial.
- Léxicos para pronunciación.
- Ideal para accesibilidad, sistemas **IVR**.

**Transcribe (Voz a Texto):**
- Transcripción en tiempo real y por lotes.
- Identificación de hablantes.
- Vocabulario personalizado.
- Identificación automática de idioma.
- Identificación de canales.
- Versiones para analítica médica y de llamadas.

### Guía de Selección de IA/ML

**Use SageMaker cuando:**
- Construya modelos de **ML** personalizados.
- Tenga un equipo de ciencia de datos.
- Necesite control total sobre el entrenamiento.
- Requisitos de **ML** complejos.
- Quiera usar algoritmos específicos.

**Use Servicios de IA Pre-entrenados cuando:**
- No se requiera experiencia en **ML**.
- Integración rápida a través de la **API**.
- Se cubran casos de uso estándar.
- Sea rentable para la mayoría de los escenarios.
- No quiera gestionar el entrenamiento.

**Por Caso de Uso:**
- Analizar imágenes/videos → **Rekognition**
- Analizar el sentimiento del texto → **Comprehend**
- Construir un chatbot → **Lex**
- Texto a voz → **Polly**
- Voz a texto → **Transcribe**
- Traducir idiomas → **Translate**
- Extraer de documentos → **Textract**
- Recomendaciones de productos → **Personalize**
- Pronóstico de ventas → **Forecast**
- Búsqueda empresarial → **Kendra**
- **ML** personalizado → **SageMaker**

## Árboles de Decisión Mejorados

### Árbol de Decisión de Arquitectura de Redes

```
¿Qué necesita conectar?

Recursos de AWS a Internet
├─ Desde subred pública → Internet Gateway
├─ Desde subred privada → NAT Gateway (o NAT Instance)
└─ Acceso privado a servicios de AWS → VPC Endpoints

Múltiples VPCs
├─ 2 VPCs → VPC Peering
├─ 3+ VPCs → Transit Gateway
└─ Servicios compartidos a muchas VPCs → PrivateLink

De las instalaciones locales a AWS
├─ Configuración rápida, cifrada → VPN Gateway
├─ Ancho de banda consistente, privado → Direct Connect
├─ Necesidad de redundancia → Múltiples VPN o Direct Connect + VPN
└─ Baja latencia requerida → Direct Connect

Entrega de Contenido
├─ Contenido estático globalmente → CloudFront + S3
├─ Contenido dinámico → CloudFront + ALB
└─ Enrutamiento DNS → Route 53

Equilibrio de Carga
├─ HTTP/HTTPS → Application Load Balancer (ALB)
├─ TCP/UDP, rendimiento extremo → Network Load Balancer (NLB)
├─ Aplicaciones de terceros → Gateway Load Balancer
└─ Heredado (evitar) → Classic Load Balancer

Seguridad
├─ Protección de aplicaciones web → WAF + ALB/CloudFront
├─ Protección DDoS → Shield (Standard/Advanced)
├─ ACLs de red → Firewall a nivel de subred (sin estado)
└─ Grupos de Seguridad → Firewall a nivel de instancia (con estado)
```

### Selector de Estrategia de Recuperación ante Desastres

```
¿Cuál es su requisito de RTO/RPO?

RTO: Minutos, RPO: Segundos
└─ Multi-Sitio Activo/Activo
   - Standby caliente en múltiples regiones
   - Costo más alto, conm   - Versión reducida ejecutándose en la región de **DR**.
   - Puede escalar rápidamente.
   - Uso: **EC2** reducido, réplicas de lectura promovidas.
   - Costo: 50-100% de un solo sitio.

RTO: Horas, RPO: Minutos a Horas
└─ Pilot Light
   - Componentes principales siempre en ejecución (ej., bases de datos).
   - Entorno completo provisionado en caso de fallo.
   - Uso: **RDS standby**, datos replicados en **S3**.
   - Costo: 20-50% de un solo sitio.

RTO: Días, RPO: Horas
└─ Backup and Restore
   - Copias de seguridad regulares en **S3**/**Glacier**.
   - Restauración desde la copia de seguridad en caso de desastre.
   - Uso: **AWS Backup**, replicación entre regiones de **S3**.
   - Costo: < 10% de un solo sitio (solo almacenamiento).

Patrones de Implementación:
- Replicación de base de datos → **RDS Multi-AZ**, réplicas de **Aurora**.
- Replicación de datos → **S3 Cross-Region Replication**.
- Infraestructura como Código → **CloudFormation** para una reconstrucción rápida.
- Gestión de tráfico → Verificaciones de estado y conmutación por error de **Route 53**.
- Sincronización continua → **DataSync**, **Storage Gateway**.
```

### Selector de Plataforma de Análisis de Datos

```
¿Qué tipo de analítica?

Analítica en Tiempo Real
├─ Streaming de datos → Kinesis Data Streams + Analytics
├─ Cuadros de mando en tiempo real → QuickSight con SPICE
└─ ML en streams → Kinesis + Lambda + SageMaker

Analítica por Lotes (Batch)
├─ SQL sobre datos de S3 → Athena
├─ ETL complejo → Glue
├─ Procesamiento a gran escala → EMR (Spark/Hadoop)
└─ Almacén de datos → Redshift

Exploración Interactiva
├─ Consultas ad-hoc → Athena
├─ Basado en cuadernos → Cuadernos de SageMaker o EMR
└─ Información rápida → QuickSight

Inteligencia de Negocios (BI)
├─ Cuadros de mando e informes → QuickSight
├─ Consultas complejas → Redshift + QuickSight
└─ BI de autoservicio → QuickSight con conjuntos de datos

Preparación de Datos
├─ ETL visual → Glue DataBrew
├─ ETL basado en código → Glue (PySpark)
├─ Descubrimiento de esquemas → Glue Crawlers
└─ Catálogo de datos → Glue Data Catalog

Aprendizaje Automático (ML)
├─ Ingeniería de características → Glue, SageMaker Data Wrangler
├─ Entrenamiento de modelos → SageMaker
├─ ML en Big Data → EMR con Spark MLlib
└─ AutoML → SageMaker Autopilot

Consideraciones de Volumen de Datos:
- < 1 TB → Athena, RDS con analítica
- 1-100 TB → Redshift
- 100+ TB → Redshift, EMR, o lago de datos en S3 con Athena
- Escala de Petabytes → EMR, lago de datos en S3
```

### Flujograma de Decisión de Despliegue de Contenedores

```
¿Necesita Kubernetes?
├─ Sí
│  ├─ Quiere administrar nodos → EKS con EC2
│  └─ Serverless → EKS con Fargate
│
└─ No
   ├─ Quiere administrar hosts → ECS con EC2
   └─ Serverless → ECS con Fargate o simplemente Lambda

¿Cuánto control necesita?
├─ Control total del SO → EC2 con Docker (sin orquestar)
├─ Orquestación de contenedores → ECS o EKS
└─ Solo ejecutar código → Lambda

¿Cuánto tiempo se ejecuta?
├─ < 15 minutos, basado en eventos → Lambda
├─ Continuo → ECS/EKS con Fargate o EC2
└─ Trabajos por lotes → AWS Batch

¿Experiencia del equipo?
├─ Conocen bien Kubernetes → EKS
├─ Experiencia en AWS → ECS
├─ Quieren lo más simple → Fargate con ECS
└─ DevOps mínimo → Lambda o Elastic Beanstalk

¿Optimización de costos?
├─ Spot Instances → ECS/EKS con EC2 Spot
├─ Reserved Instances → ECS/EKS con EC2 RIs
├─ Pago por uso → Fargate o Lambda
└─ Savings Plans → Fargate con Compute Savings Plans

Patrones de despliegue:
ECS con EC2:
- Registrar instancias de contenedor al clúster.
- Definir definiciones de tareas (Task Definitions).
- Crear servicios con el recuento deseado.
- Usar Application Load Balancer.

ECS con Fargate:
- Sin instancias que administrar.
- Definir definiciones de tareas (CPU/memoria).
- Crear servicios.
- AWS maneja la infraestructura.

EKS con EC2:
- Plano de control de Kubernetes administrado.
- Nodos trabajadores autogestionados.
- Comandos kubectl estándar.
- Usar servicios/ingreso de Kubernetes.

EKS con Fargate:
- Sin nodos que administrar.
- Definir perfiles de Fargate.
- Desplegar pods de Kubernetes.
- AWS provisiona el cómputo.
```

### Selector de Soluciones de Monitoreo

```
¿Qué necesita monitorear?

Rendimiento de Aplicaciones
├─ Métricas y alarmas → CloudWatch
├─ Rastreo distribuido → X-Ray
├─ Monitoreo de usuarios reales → CloudWatch RUM
└─ Monitoreo sintético → CloudWatch Synthetics

Seguridad y Cumplimiento
├─ Actividad de la API → CloudTrail
├─ Cambios de configuración → Config
├─ Detección de amenazas → GuardDuty
├─ Escaneo de vulnerabilidades → Inspector
└─ Seguridad centralizada → Security Hub

Cambios en los Recursos
├─ Historial de configuración → Config
├─ Notificaciones de cambios → EventBridge
└─ Detección de desviaciones → CloudFormation

Tráfico de Red
├─ Tráfico de VPC → VPC Flow Logs
├─ Consultas DNS → Route 53 Resolver Query Logs
└─ Acceso a CloudFront → CloudFront Logs

Monitoreo de Costos
├─ Seguimiento de costos → Cost Explorer
├─ Alertas de presupuesto → AWS Budgets
├─ Detección de anomalías → Cost Anomaly Detection
└─ Dimensionamiento correcto → Compute Optimizer

Estrategia de Registro (Logging):
Registros de aplicaciones → CloudWatch Logs
Registros de acceso → S3, CloudWatch Logs
Registros de auditoría → CloudTrail a S3
Flow Logs → CloudWatch Logs o S3

Arquitectura de Monitoreo:
1. Recopilar: CloudWatch Agent, X-Ray SDK, CloudTrail.
2. Almacenar: CloudWatch Logs, S3, CloudWatch Metrics.
3. Analizar: CloudWatch Insights, Athena en S3.
4. Alertar: CloudWatch Alarms, SNS, EventBridge.
5. Visualizar: CloudWatch Dashboards, QuickSight.
```

## Matrices de Comparación de Costos

### Comparación de Costos de Almacenamiento (por GB-mes)

| Tipo de Almacenamiento | Caliente/Frecuente | Tibio/Poco Frecuente | Frío/Archivo | Caso de Uso |
|-----------------------|--------------------|-----------------------|--------------|-------------|
| **S3 Standard** | $0.023 | - | - | Datos activos, acceso frecuente |
| **S3 Intelligent-Tiering** | $0.023 + $0.0025 monitoreo | Movimiento automático | Movimiento automático | Patrones de acceso desconocidos |
| **S3 Standard-IA** | - | $0.0125 + $0.01/GB recuperación | - | Acceso poco frecuente pero rápido |
| **S3 One Zone-IA** | - | $0.01 + $0.01/GB recuperación | - | No crítico, poco frecuente |
| **S3 Glacier Instant** | - | $0.004 + $0.03/GB recuperación | - | Archivo, recuperación instantánea |
| **S3 Glacier Flexible** | - | - | $0.0036 + costos recuperación | Archivo, recuperación en 1-5 min |
| **S3 Glacier Deep Archive** | - | - | $0.00099 + costos recuperación | Archivo a largo plazo, 12 hrs |
| **EBS gp3** | $0.08 | - | - | **SSD** de propósito general |
| **EBS io2** | $0.125 + costo **IOPS** | - | - | **SSD** de alto rendimiento |
| **EBS st1** | $0.045 | - | - | **HDD** optimizado para rendimiento |
| **EBS sc1** | $0.015 | - | - | **HDD** frío |
| **EFS Standard** | $0.30 | - | - | Sistema de archivos compartido |
| **EFS IA** | - | $0.025 | - | Archivos de acceso poco frecuente |
| **FSx for Windows** | $0.13 | - | $0.013 (respaldo) | Recursos compartidos de **Windows** |
| **FSx for Lustre** | $0.145-0.240 | - | - | Cargas de trabajo **HPC** |

**Consejos de Optimización de Costos de Almacenamiento:**
- Use políticas de **Lifecycle** de **S3** para mover a niveles más baratos.
- Habilite **S3 Intelligent-Tiering** para accesos impredecibles.
- Use **snapshots** de **EBS** a **S3** para respaldos (más barato que mantener volúmenes sin usar).
- Elimine volúmenes **EBS** no utilizados.
- Use **EFS IA** para archivos que no se han accedido en más de 30 días.

### Comparación de Costos de Bases de Datos (por hora)

| Base de Datos | Instancia Pequeña | Instancia Mediana | Instancia Grande | Almacenamiento | Notas |
|---------------|-------------------|-------------------|------------------|----------------|-------|
| **RDS MySQL db.t3.micro** | $0.017 | - | - | $0.115/GB-mes | Propósito general |
| **RDS MySQL db.t3.medium** | - | $0.068 | - | $0.115/GB-mes | |
| **RDS MySQL db.m5.large** | - | - | $0.188 | $0.115/GB-mes | |
| **Aurora MySQL** | - | $0.082 (db.t3.medium) | $0.29 (db.r5.large) | $0.10/GB-mes | Escalado auto de almacenamiento |
| **Aurora Serverless v2** | - | - | - | $0.12/ACU-hora + $0.10/GB | Escala auto, mín 0.5 **ACU** |
| **DynamoDB Bajo Demanda** | - | - | - | $1.25/millón escrituras, $0.25/millón lecturas | Pago por solicitud |
| **DynamoDB Provisionado** | - | - | - | $0.00065/WCU-hora, $0.00013/RCU-hora | Capacidad reservada disponible |
| **Redshift dc2.large** | - | $0.25 | - | 160 GB **SSD** incluido | Mín 1 nodo |
| **Redshift ra3.xlplus** | - | - | $1.086 | Almacenamiento admin $0.024/GB | Almacenamiento escalable |
| **ElastiCache t3.micro** | $0.017 | - | - | Incluido | **Redis** o **Memcached** |
| **ElastiCache r5.large** | - | - | $0.243 | Incluido | Optimizado para memoria |
| **DocumentDB r5.large** | - | - | $0.277 | $0.10/GB-mes | Compatible con **MongoDB** |
| **Neptune db.t3.medium** | - | $0.073 | - | $0.10/GB-mes | Base de datos de grafos |

**Optimización de Costos de Bases de Datos:**
- Use **Aurora Serverless v2** para cargas de trabajo variables.
- **Reserved Instances** para **RDS** (hasta un 69% de ahorro).
- **DynamoDB Bajo Demanda** para tráfico impredecible.
- **DynamoDB Provisionado** con escalado automático para tráfico predecible.
- Detenga las instancias de **RDS** de desarrollo/pruebas cuando no estén en uso.
- Use réplicas de lectura en lugar de instancias más grandes.

### Comparación de Precios de Cómputo (por hora)

| Tipo de Instancia | Bajo Demanda | Spot (prom. 70% menos) | Reservada 1 Año | Reservada 3 Años | Savings Plan 1 Año |
|-------------------|--------------|------------------------|-----------------|------------------|-------------------|
| **t3.micro** | $0.0104 | ~$0.003 | $0.0062 (40% desc) | $0.0042 (60% desc) | Similar a **RI** |
| **t3.medium** | $0.0416 | ~$0.012 | $0.0250 (40% desc) | $0.0166 (60% desc) | Similar a **RI** |
| **m5.large** | $0.096 | ~$0.029 | $0.058 (40% desc) | $0.038 (60% desc) | Similar a **RI** |
| **m5.xlarge** | $0.192 | ~$0.058 | $0.115 (40% desc) | $0.077 (60% desc) | Similar a **RI** |
| **c5.xlarge** | $0.17 | ~$0.051 | $0.102 (40% desc) | $0.068 (60% desc) | Similar a **RI** |
| **r5.xlarge** | $0.252 | ~$0.076 | $0.151 (40% desc) | $0.101 (60% desc) | Similar a **RI** |
| **Lambda** | $0.20/1M solicitudes + $0.0000166667/GB-seg | - | - | - | **Compute Savings Plan** |
| **Fargate vCPU** | $0.04048/vCPU-hora | - | - | - | **Compute Savings Plan** (hasta 50% desc) |
| **Fargate Memoria** | $0.004445/GB-hora | - | - | - | **Compute Savings Plan** |

**Optimización de Costos de Cómputo:**
- **Spot Instances** para cargas de trabajo tolerantes a fallos (hasta un 90% de ahorro).
- **Reserved Instances** para cargas de trabajo de estado estable (hasta un 72% de ahorro).
- **Savings Plans** para compromiso flexible (hasta un 72% de ahorro).
- Dimensione correctamente las instancias usando **Compute Optimizer**.
- Use **Auto Scaling** para hacer coincidir la capacidad con la demanda.
- **Lambda** para ejecución basada en eventos (sin costos de inactividad).
- Programe el inicio/parada para entornos de desarrollo/pruebas.

### Escenarios de Costo de Transferencia de Datos (por GB)

| Escenario | Costo | Notas |
|-----------|-------|-------|
| **Entrada de datos a AWS (desde internet)** | GRATIS | Toda la transferencia de datos entrantes |
| **Salida de datos de AWS a internet** | $0.09 (primeros 10 TB) | Disminuye con el volumen |
| **Salida de datos de AWS a internet** | $0.085 (siguientes 40 TB) | Nivel de 10-50 TB |
| **Salida de datos de AWS a internet** | $0.07 (siguientes 100 TB) | Nivel de 50-150 TB |
| **Entre S3 y EC2 (misma región)** | GRATIS | Sin cargos por transferencia de datos |
| **Entre S3 y CloudFront** | GRATIS | **CloudFront** a **S3** en la misma región |
| **CloudFront a internet** | $0.085 | Menor que directo desde la región |
| **Entre regiones** | $0.02 | Transferencia de datos entre regiones |
| **Entre AZs dentro de una región** | $0.01 (cada dirección) | $0.02 ida y vuelta |
| **Entre pares de VPC (misma región)** | $0.01 (cada dirección) | Igual que entre **AZs** |
| **Salida de datos Direct Connect (DX)** | $0.02-0.03 | Menor que por internet |
| **Salida de datos vía NAT Gateway** | $0.045 + costos internet | Tarifa de procesamiento de **NAT Gateway** |

**Optimización de Costos de Transferencia de Datos:**
- Use **CloudFront** para la entrega de contenido (menores costos de egreso).
- Mantenga los recursos en la misma **AZ** cuando sea posible.
- Use **VPC endpoints** para **S3**/**DynamoDB** (sin cargos de **NAT Gateway** o internet).
- Comprima los datos antes de la transferencia.
- Use **Direct Connect** para grandes transferencias de datos.
- Aproveche **S3 Transfer Acceleration** para las cargas.
- Use **AWS DataSync** para migraciones (sin tarifa de transferencia adicional).

### Variaciones de Costo Regionales

**Nota:** Los precios varían según la región. Los ejemplos anteriores son para **US East (N. Virginia)**.

| Región | Multiplicador de Costo | Notas |
|--------|------------------------|-------|
| **US East (N. Virginia)** | 1.0x | Línea base, generalmente la más barata |
| **US West (Oregon)** | 1.0x | Similar a **US East** |
| **EU (Ireland)** | ~1.1x | Ligeramente más alta |
| **Asia Pacific (Singapore)** | ~1.15x | Más alta que en **EE. UU.** |
| **Asia Pacific (Sydney)** | ~1.2x | Mayores costos de transferencia de datos |
| **South America (São Paulo)** | ~1.5x | Costos más altos en general |

**Optimización de Costos por Región:**
- Despliegue en **US East (N. Virginia)** para los costos más bajos.
- Elija la región más cercana a los usuarios para el rendimiento.
- Considere los requisitos de soberanía de datos.
- Use **S3 Intelligent-Tiering** en todas las regiones.

## Cuándo Usar Qué - Escenarios Detallados

### Escenarios de Servicios de Cómputo

**Lambda - Perfecto Para:**
- Generación de miniaturas de imágenes cuando se cargan en **S3**.
- Procesamiento de flujos de **DynamoDB**.
- Tareas programadas (cada 5 minutos, cada hora, diariamente).
- Backends de **API** con **API Gateway**.
- Procesamiento de archivos en tiempo real.
- Chatbots y habilidades de **Alexa**.
- Procesamiento de backend de **IoT**.
- Trabajos **ETL** de menos de 15 minutos.

**Lambda - NO es Bueno Para:**
- Procesos de larga duración (> 15 min).
- Aplicaciones con tráfico constante (mejor usar **EC2**/contenedores).
- Procesos que requieren almacenamiento local > 10 GB.
- Aplicaciones que necesitan **GPUs**.
- Aplicaciones heredadas con dependencias complejas.

**EC2 - Perfecto Para:**
- Aplicaciones personalizadas que requieren un **SO** específico.
- Aplicaciones que necesitan **GPUs**.
- Migraciones **Lift-and-shift**.
- Cargas de trabajo constantes (use **Reserved Instances**).
- Aplicaciones con restricciones de licencia.
- Clústeres de computación de alto rendimiento.
- Aplicaciones basadas en **Windows**.

**EC2 - NO es Bueno Para:**
- Funciones simples basadas en eventos (use **Lambda**).
- Picos de tráfico impredecibles (a menos que se use **Auto Scaling**).
- Microservicios que podrían ser contenedorizados.
- Cuando quiera administración cero.

**Fargate - Perfecto Para:**
- Arquitecturas de microservicios.
- Contenedores de larga duración.
- Trabajos por lotes y tareas programadas.
- Agentes de **CI**/**CD**.
- Aplicaciones con carga variable.
- Cuando no quiera administrar servidores.

**Fargate - NO es Bueno Para:**
- Cargas de trabajo altas constantes (**EC2** con contenedores es más barato).
- Cargas de trabajo de **GPU** (use **ECS** con **EC2**).
- Contenedores **Windows** que requieren personalización del host.
- Aplicaciones extremadamente sensibles a la latencia (arranques en frío).

### Escenarios de Servicios de Almacenamiento

**S3 Standard - Perfecto Para:**
- Contenido y activos de sitios web activos.
- Datos a los que se accede con frecuencia.
- Distribución de contenido con **CloudFront**.
- Nivel caliente de lago de datos.
- Aplicaciones móviles y de juegos.
- Analítica de **Big Data** (consultada con frecuencia).

**S3 Standard-IA - Perfecto Para:**
- Copias de seguridad a las que se accede ocasionalmente.
- Archivos de recuperación ante desastres.
- Datos conservados para cumplimiento normativo.
- Datos antiguos a los que se accede pocas veces al mes.
- Datos de larga duración pero de acceso poco frecuente.

**S3 Glacier - Perfecto Para:**
- Preservación digital.
- Datos de archivo a los que rara vez se accede.
- Reemplazo de cintas.
- Archivos regulatorios.
- Copias de seguridad a largo plazo.

**S3 Glacier - NO es Bueno Para:**
- Datos a los que se accede con frecuencia (recuperaciones costosas).
- Datos necesarios de inmediato (tiempo de recuperación).
- Archivos pequeños (cargos por duración mínima de almacenamiento).

**EFS - Perfecto Para:**
- Servicio web y gestión de contenido.
- Almacenamiento persistente de contenedores.
- Datos compartidos entre instancias.
- Analítica de **Big Data** que necesita acceso compartido.
- Datos de entrenamiento de **ML** accedidos por múltiples instancias.
- Aplicaciones **Lift-and-shift** que esperan **NFS**.

**EFS - NO es Bueno Para:**
- Cargas de trabajo de una sola instancia (**EBS** es más barato).
- Aplicaciones **Windows** (use **FSx for Windows**).
- Necesidades de **IOPS** más altas (use **EBS io2**).
- Necesidades de almacenamiento de objetos (use **S3**).

### Escenarios de Servicios de Bases de Datos

**RDS - Perfecto Para:**
- Bases de datos relacionales tradicionales.
rage Service Scenarios

**S3 Standard - Perfect For:**
- Active website content and assets
- Frequently accessed data
- Content distribution with CloudFront
- Data lake hot tier
- Mobile and gaming applications
- Big data analytics (frequently queried)

**S3 Standard-IA - Perfect For:**
- Backups accessed occasionally
- Disaster recovery files
- Data retained for compliance
- Old data accessed few times per month
- Long-lived but infrequently accessed

**S3 Glacier - Perfect For:**
- Digital preservation
- Archive data rarely accessed
- Tape replacement
- Regulatory archives
- Long-term backups

**S3 Glacier - NOT Good For:**
- Frequently accessed data (expensive retrievals)
- Data needed immediately (retrieval time)
- Small files (minimum storage duration charges)

**EFS - Perfecto Para:**
- Servicio web y gestión de contenido.
- Almacenamiento persistente de contenedores.
- Datos compartidos entre instancias.
- Analítica de **Big Data** que necesita acceso compartido.
- Datos de entrenamiento de **ML** accedidos por múltiples instancias.
- Aplicaciones **Lift-and-shift** que esperan **NFS**.

**EFS - NO es Bueno Para:**
- Cargas de trabajo de una sola instancia (**EBS** es más barato).
- Aplicaciones **Windows** (use **FSx for Windows**).
- Necesidades de **IOPS** más altas (use **EBS io2**).
- Necesidades de almacenamiento de objetos (use **S3**).

### Escenarios de Servicios de Bases de Datos

**RDS - Perfecto Para:**
- Bases de datos relacionales tradicionales.
- Aplicaciones existentes que usan **MySQL**/**PostgreSQL**/**Oracle**/**SQL Server**.
- Cargas de trabajo **OLTP**.
- Cuando necesite **Multi-AZ** para alta disponibilidad (**HA**).
- Réplicas de lectura para cargas de trabajo con muchas lecturas.
- **Lift-and-shift** de bases de datos existentes.

**RDS - NO es Bueno Para:**
- Escala masiva (considere **DynamoDB** o **Aurora**).
- Necesidad de acceso root (use **EC2** con base de datos autogestionada).
- Acceso simple de clave-valor (use **DynamoDB**).
- Cambios frecuentes de esquema (considere **NoSQL**).

**Aurora - Perfecto Para:**
- **OLTP** de alto rendimiento.
- Aplicaciones que necesitan compatibilidad con **MySQL**/**PostgreSQL**.
- Aplicaciones globales (**Global Database**).
- Cargas de trabajo variables (**Serverless v2**).
- Cargas de trabajo con muchas lecturas (15 réplicas de lectura).
- Cuando necesite el mejor rendimiento de **RDS**.

**DynamoDB - Perfecto Para:**
- Aplicaciones **Serverless**.
- Backends móviles.
- Tablas de clasificación de juegos.
- Almacenes de sesiones.
- Carritos de compra.
- Datos de **IoT**.
- Se requiere latencia de milisegundos.
- Escala masiva (millones de solicitudes/seg).

**DynamoDB - NO es Bueno Para:**
- Consultas complejas con joins.
- Consultas **ad-hoc** y analítica (use **RDS** o **Athena**).
- Transacciones **ACID** entre elementos (use **RDS**).
- Cuando necesite **SQL**.
- Aplicaciones con patrones de consulta impredecibles.

**Redshift - Perfecto Para:**
- Inteligencia de negocios (**BI**).
- Cargas de trabajo **OLAP**.
- Consultas complejas a través de grandes conjuntos de datos.
- Consolidación de almacenes de datos.
- Análisis de datos históricos.
- Analítica a escala de petabytes.

**Redshift - NO es Bueno Para:**
- Cargas de trabajo **OLTP** (use **RDS** o **DynamoDB**).
- Ingesta de datos en tiempo real (use **Kinesis**).
- Conjuntos de datos pequeños < 100 GB (use **RDS** o **Athena**).
- Cargas de trabajo de alta concurrencia (use **RDS**).

### Escenarios de Servicios de Red

**CloudFront - Perfecto Para:**
- Entrega de contenido global.
- Aceleración de sitios web.
- Streaming de video.
- Aceleración de **APIs**.
- Distribución de software.
- Protección **DDoS** con **Shield**.

**CloudFront - NO es Bueno Para:**
- Tráfico en la misma región (sin beneficio).
- Contenido dinámico que no se puede cachear.
- Comunicación de backend a backend.

**Direct Connect - Perfecto Para:**
- Grandes migraciones de datos a **AWS**.
- Requisitos de ancho de banda consistentes.
- Arquitecturas híbridas con alto rendimiento.
- Cumplimiento que requiere conectividad privada.
- Reducción de costos para altos volúmenes de transferencia de datos.

**Direct Connect - NO es Bueno Para:**
- Necesidades de configuración rápida (use **VPN**).
- Bajos volúmenes de transferencia de datos.
- Conexiones temporales.
- Proyectos con presupuesto limitado.

**VPN - Perfecto Para:**
- Conectividad híbrida rápida.
- Respaldo para **Direct Connect**.
- Tráfico bajo a moderado.
- Conectividad cifrada.
- Conexiones de sucursales.

**VPN - NO es Bueno Para:**
- Requisitos de alto ancho de banda.
- Aplicaciones sensibles a la latencia.
- Volúmenes de transferencia de datos muy altos.

### Escenarios de Servicios de Seguridad

**IAM - Perfecto Para:**
- Control de acceso interno de AWS.
- Permisos de servicio a servicio.
- Acceso entre cuentas.
- Credenciales temporales con roles.

**IAM - NO es Bueno Para:**
- Autenticación de usuarios externos (use **Cognito**).
- Miles de usuarios (use federación).

**Cognito - Perfecto Para:**
- Autenticación de aplicaciones móviles.
- Registro/inicio de sesión de usuarios de aplicaciones web.
- Federación de identidad social.
- Acceso de usuarios invitados.
- Gestión de perfiles de usuario.

**Cognito - NO es Bueno Para:**
- Control de acceso a recursos de AWS (use **IAM**).
- Acceso de empleados internos (use **SSO**).

**Secrets Manager - Perfecto Para:**
- Credenciales de base de datos.
- Claves de **API**.
- Rotación automática de secretos.
- Replicación de secretos entre regiones.
- Aplicaciones que usan **SDKs** de **AWS**.

**Secrets Manager - NO es Bueno Para:**
- Datos de configuración (use **Parameter Store**).
- Datos públicos (use **Parameter Store**).
- Cuando no necesite rotación (**Parameter Store** es más barato).

## Anti-patrones de Servicio (Cuándo NO Usar)

### Errores Comunes y Anti-patrones

**Anti-patrones de Lambda:**
- Usar **Lambda** para procesos de larga duración (use **ECS**/**Fargate**).
- Almacenar estado en `/tmp` (use **S3** o **DynamoDB**).
- Cadenas de invocación síncronas (use **Step Functions**).
- No usar agrupación de conexiones (**connection pooling**) para bases de datos.
- Ignorar la optimización del arranque en frío (**cold start**).

**Anti-patrones de S3:**
- Usar **S3** como una base de datos (use **DynamoDB** o **RDS**).
- Actualizaciones frecuentes de archivos pequeños (use **EFS** o base de datos).
- Nombramiento secuencial de archivos que causa particiones calientes (**hot partitions**).
- No usar políticas de **lifecycle** para datos antiguos.
- Buckets públicos sin una revisión cuidadosa.

**Anti-patrones de EC2:**
- No usar **Auto Scaling** para cargas de trabajo variables.
- Instancias **Bajo Demanda** para cargas de trabajo predecibles (use **RIs**).
- Sobredimensionamiento de los tamaños de las instancias.
- No usar **snapshots** de **EBS** para respaldos.
- Olvidar terminar las instancias no utilizadas.

**Anti-patrones de RDS:**
- No usar **Multi-AZ** para producción.
- Una sola instancia grande en lugar de réplicas de lectura.
- No monitorear el almacenamiento y los **IOPS**.
- Almacenar archivos/blobs en la base de datos (use **S3**).
- No usar grupos de parámetros para el ajuste.

**Anti-patrones de DynamoDB:**
- Usarlo como una base de datos relacional con joins complejos.
- Claves de partición calientes.
- Almacenar elementos grandes > 400 KB.
- No usar **DynamoDB Streams** para la replicación.
- Capacidad provisionada con tráfico impredecible.

**Anti-patrones de VPC:**
- Bloques **CIDR** superpuestos que impiden el **peering**.
- Rangos **CIDR** demasiado pequeños.
- No usar múltiples **AZs** para alta disponibilidad.
- Subredes públicas para bases de datos.
- No usar **VPC endpoints** para **S3**/**DynamoDB**.

**Anti-patrones de ECS/EKS:**
- Usar **Fargate** para un volumen alto constante (**EC2** es más barato).
- No usar **Application Load Balancer** para servicios **HTTP**.
- Ignorar las verificaciones de estado de los contenedores.
- Contenedores monolíticos grandes.
- No usar **ECR** para imágenes privadas.

## Conceptos Erróneos Comunes

### Aclarando la Confusión

**Conceptos Erróneos sobre S3:**
- "S3 es solo para copias de seguridad" - FALSO: Es para cualquier almacenamiento de objetos, incluidas aplicaciones activas.
- "S3 es lento" - FALSO: Puede manejar miles de solicitudes por segundo por prefijo.
- "Necesitas administrar los servidores de S3" - FALSO: Totalmente **Serverless** y administrado.

**Conceptos Erróneos sobre Lambda:**
- "Lambda siempre es más barato" - FALSO: El tráfico constante hace que **EC2**/**Fargate** sean más baratos.
- "Lambda escala infinitamente" - FALSO: Limitado por el límite de ejecución concurrente (puede aumentarse).
- "Lambda es solo para funciones simples" - FALSO: Puede ejecutar aplicaciones complejas.

**Conceptos Erróneos sobre VPC:**
- "Todo el tráfico entre AZs es gratis" - FALSO: $0.01/GB en cada dirección.
- "Subred privada significa segura" - FALSO: Aún se necesitan Grupos de Seguridad y **NACLs**.
- "El VPC peering es transitivo" - FALSO: Debe crear un **peering** directo entre cada par de **VPC**.

**Conceptos Erróneos sobre RDS:**
- "Multi-AZ significa escalado de lectura" - FALSO: Es solo para alta disponibilidad, use réplicas de lectura para escalar.
- "Los respaldos automatizados son gratuitos" - FALSO: Se cobra por el almacenamiento de respaldo más allá del tamaño de la base de datos.
- "Todas las bases de datos deberían usar RDS" - FALSO: Las cargas de trabajo **NoSQL** son mejores en **DynamoDB**.

**Conceptos Erróneos sobre IAM:**
- "El usuario root debería usarse diariamente" - FALSO: Cree usuarios **IAM** y proteja la cuenta root.
- "IAM es gratis, así que los roles no importan" - FALSO: Los roles adecuados son cruciales para la seguridad.
- "Los usuarios necesitan contraseñas" - FALSO: Las cuentas de servicio deberían usar roles, no usuarios.

## Límites de Servicio Ampliados

### Límites Flexibles vs. Límites Fijos

**Límites Flexibles** (Pueden aumentarse bajo solicitud):
- Límites de instancias **EC2**.
- Límites de **VPC**.
- Límites de instancias **RDS**.
- Límite de buckets **S3**.
- Ejecuciones concurrentes de **Lambda**.
- Stacks de **CloudFormation**.
- Direcciones **IP Elásticas**.

**Límites Fijos** (No pueden aumentarse):
- Tamaño de objeto **S3** (5 TB).
- Tiempo de espera de ejecución de **Lambda** (15 minutos).
- Almacenamiento `/tmp` de **Lambda** (10 GB).
- Tamaño del paquete de despliegue de **Lambda** (250 MB).
- Longitud del nombre del bucket **S3** (63 caracteres).
- Tamaño de la política **IAM** (6,144 caracteres para políticas administradas).

### Tabla de Límites de Servicio Ampliada

| Servicio | Tipo de Límite | Por Defecto | Máx. tras Aumento | Cómo Solicitar |
|----------|----------------|-------------|-------------------|----------------|
| **Instancias EC2 Bajo Demanda** | Flexible | 20 vCPUs | Ilimitado | Consola Service Quotas |
| **Instancias EC2 Spot** | Flexible | 20 vCPUs | Ilimitado | Consola Service Quotas |
| **VPCs por Región** | Flexible | 5 | 100+ | Consola Service Quotas |
| **Subredes por VPC** | Flexible | 200 | 200+ | Consola Service Quotas |
| **Grupos de Seguridad por Región** | Flexible | 2,500 | 10,000 | Consola Service Quotas |
| **Reglas por Grupo de Seguridad** | Flexible | 60 | 250 | Consola Service Quotas |
| **Tablas de Rutas por VPC** | Flexible | 200 | 200+ | Consola Service Quotas |
| **Buckets S3** | Flexible | 100 | 1,000+ | Caso de Soporte |
| **Tamaño de Objeto S3** | Fijo | 5 TB | No se puede aumentar | N/A |
| **S3 PUT/COPY/POST/DELETE** | Flexible | 3,500 sol/seg | 5,500+ sol/seg | Automático |
| **S3 GET/HEAD** | Flexible | 5,500 sol/seg | Ilimitado | Automático |
| **Instancias de DB RDS** | Flexible | 40 | 100+ | Consola Service Quotas |
| **Réplicas de lectura RDS por Maestro** | Fijo | 15 (Aurora), 5 (otros) | No se puede aumentar | N/A |
| **Snapshots de DB RDS** | Flexible | 100 | 500+ | Consola Service Quotas |
| **Recuento de Tablas DynamoDB** | Flexible | 2,500 | 10,000+ | Consola Service Quotas |
| **Rendimiento de DynamoDB** | Flexible | 40,000 RCU, 40,000 WCU | Ilimitado | Automático con bajo demanda |
| **Ejecuciones Concurrentes Lambda** | Flexible | 1,000 | 100,000+ | Consola Service Quotas |
| **Tiempo de Espera Función Lambda** | Fijo | 15 minutos | No se puede aumentar | N/A |
| **Espacio /tmp de Lambda** | Fijo | 10 GB | No se puede aumentar | N/A |
| **Paquete de Despliegue Lambda** | Fijo | 250 MB (descomprimido) | No se puede aumentar | N/A |
| **Variables de Entorno Lambda** | Fijo | 4 KB | No se puede aumentar | N/A |
| **Capas de Lambda (Layers)** | Fijo | 5 por función | No se puede aumentar | N/A |
| **Volúmenes EBS por Instancia** | Flexible | 128 | 128 | No se puede aumentar |
| **Tamaño Volumen EBS (gp3/io2)** | Fijo | 64 TB | No se puede aumentar | N/A |
| **Snapshots de EBS** | Flexible | 10,000 | 100,000+ | Consola Service Quotas |
| **Stacks de CloudFormation** | Flexible | 2,000 | 10,000+ | Consola Service Quotas |
| **Stack Sets de CloudFormation** | Flexible | 100 | 500+ | Consola Service Quotas |
| **Alarmas de CloudWatch** | Flexible | 5,000 | 15,000+ | Consola Service Quotas |
| **Grupos de Registros CloudWatch** | Flexible | Ilimitado | Ilimitado | N/A |
| **Rastros (Trails) de CloudTrail** | Flexible | 5 | 5 | No se puede aumentar |
| **Usuarios IAM** | Flexible | 5,000 | 5,000 | No se puede aumentar |
| **Grupos IAM** | Flexible | 300 | 500 | Caso de Soporte |
| **Roles IAM** | Flexible | 1,000 | 5,000+ | Consola Service Quotas |
| **Políticas IAM por Usuario/Grupo/Rol** | Fijo | 10 admin, 1 inline | No se puede aumentar | N/A |
| **ELB (ALB/NLB) por Región** | Flexible | 50 | 100+ | Consola Service Quotas |
| **Target Groups por Región** | Flexible | 3,000 | 5,000+ | Consola Service Quotas |
| **Destinos por Target Group** | Flexible | 1,000 | 1,000+ | Consola Service Quotas |
| **Conexiones VPN por Región** | Flexible | 50 | 200+ | Consola Service Quotas |
| **Conexiones Direct Connect** | Flexible | 10 | 50+ | Caso de Soporte |
| **Zonas Alojadas de Route 53** | Flexible | 500 | 5,000+ | Consola Service Quotas |
| **Registros por Zona Alojada Route 53** | Flexible | 10,000 | 100,000+ | Consola Service Quotas |

### Cómo Solicitar Aumentos de Límites de Servicio

**Método 1: Consola Service Quotas** (Preferido)
1. Abra la consola de **Service Quotas**.
2. Elija el servicio de **AWS**.
3. Seleccione la cuota que desea aumentar.
4. Solicite el aumento de la cuota.
5. Proporcione una justificación del caso de uso.

**Método 2: Caso de Soporte**
- Para límites que no están en la consola de **Service Quotas**.
- Cree un caso de soporte bajo "Aumento de límite de servicio".
- Proporcione una justificación detallada.
- Incluya el uso proyectado.

**Método 3: Aumentos Automáticos**
- Algunos servicios se incrementan automáticamente según el uso (**S3**, **DynamoDB Bajo Demanda**).
- No se requiere ninguna acción.

**Mejores Prácticas:**
- Solicite aumentos de manera proactiva antes de alcanzar los límites.
- Monitoree el uso con **CloudWatch** y **Service Quotas**.
- Configure alarmas de **CloudWatch** para cuando se aproxime a los límites.
- Diseñe aplicaciones para manejar el **throttling** (estrangulamiento) de manera elegante.
- Use múltiples cuentas para multiplicar los límites (a través de **AWS Organizations**).

### Límites que Afectan a la Arquitectura

**Consideraciones de Diseño:**

**Límites de Instancias EC2:**
- Planifique el tamaño máximo de **Auto Scaling** dentro de los límites.
- Use múltiples tipos de instancias para la diversidad.
- Considere las **Reserved Instances** frente a los límites.

**Concurrencia de Lambda:**
- Reserve concurrencia para funciones críticas.
- Use **SQS** para el buffering.
- Diseñe para el **throttling** (**exponential backoff**).

**Rendimiento de DynamoDB:**
- Use **Bajo Demanda** para tráfico impredecible.
- Provisione con escalado automático para tráfico predecible.
- Particione los datos para evitar claves calientes.

**Tasas de Solicitud de S3:**
- Use prefijos aleatorios para tasas de solicitud altas.
- **CloudFront** para cargas de trabajo con muchas lecturas.
- **Transfer Acceleration** para las cargas.

**Límites de VPC:**
- Planifique el direccionamiento IP cuidadosamente (dimensionamiento **CIDR**).
- Use **Transit Gateway** para muchas **VPCs**.
- Monitoree el uso de **ENI** (**Lambda** en **VPC** consume **ENIs**).

**Límites de IAM:**
- Use roles en lugar de usuarios siempre que sea posible.
- Agrupe los permisos de manera eficiente.
- Considere **AWS SSO** para usuarios humanos.
- Federación para identidades externas.

---

[← Anterior: Laboratorios Prácticos](07-hands-on-labs.md) | [Volver al Inicio](README.md) | [Siguiente: Escenarios de Examen →](09-exam-scenarios.md)
