# Capítulo 9: Escenarios Comunes del Examen y Soluciones del Mundo Real

[← Anterior: Preparación para el Examen](08-exam-preparation.md) | [Siguiente: Recursos Adicionales →](10-additional-resources.md)

---

## Tabla de Contenidos
- [Aprendizaje Basado en Escenarios](#scenario-based-learning)
  - [Escenario 1: Optimización de Costos para Cargas de Trabajo Predecibles](#scenario-1-cost-optimization-for-predictable-workloads)
  - [Escenario 2: Diseño para Alta Disponibilidad](#scenario-2-designing-for-high-availability)
  - [Escenario 3: Migración de Grandes Volúmenes de Datos](#scenario-3-large-data-migration)
  - [Escenario 4: Arquitectura de Aplicación Serverless](#scenario-4-serverless-application-architecture)
  - [Escenario 5: Cumplimiento y Gobernanza](#scenario-5-compliance-and-governance)
  - [Escenario 6: Estrategia de Recuperación ante Desastres](#scenario-6-disaster-recovery-strategy)
  - [Escenario 7: Conectividad de Nube Híbrida](#scenario-7-hybrid-cloud-connectivity)
  - [Escenario 8: Arquitectura Multi-Región para Aplicación Global](#scenario-8-multi-region-architecture-for-global-application)
  - [Escenario 9: Respuesta y Prevención de Incidentes de Seguridad](#scenario-9-security-incident-response-and-prevention)
  - [Escenario 10: Modernización de una Aplicación Monolítica Heredada](#scenario-10-modernizing-legacy-monolith-application)
  - [Escenario 11: Plataforma de Analítica de Big Data](#scenario-11-big-data-analytics-platform)
  - [Escenario 12: Implementación de Pipeline DevOps CI/CD](#scenario-12-devops-cicd-pipeline-implementation)
- [Escenarios Comunes de Resolución de Problemas](#common-troubleshooting-scenarios)
  - [No se puede conectar a la instancia EC2](#cannot-connect-to-ec2-instance)
  - [Errores de Acceso Denegado en S3](#s3-access-denied-errors)
  - [Problemas con Funciones Lambda](#lambda-function-issues)
  - [Problemas de Conexión en RDS](#rds-connection-problems)
  - [Fallos en los Stacks de CloudFormation](#cloudformation-stack-failures)
  - [Auto Scaling no funciona](#auto-scaling-not-working)
  - [Factura de AWS Inesperadamente Alta](#high-aws-bill-unexpectedly)
  - [Errores 502/504 en API Gateway](#api-gateway-502504-errors)

---

## Aprendizaje Basado en Escenarios

### Escenario 1: Optimización de Costos para Cargas de Trabajo Predecibles

**Situación**: Una empresa ejecuta una aplicación web en instancias **EC2** que experimenta un tráfico predecible de lunes a viernes de 9 a.m. a 5 p.m. EST. El tráfico es mínimo los fines de semana y las noches.

**Configuración Actual**:
- 10 instancias **m5.large** ejecutándose las 24 horas del día, los 7 días de la semana.
- Precios **On-Demand**.
- Costo mensual: $1,200.

**Pregunta**: ¿Cuál es la solución MÁS rentable?

**Análisis**:
- Horario predecible = oportunidad de optimización.
- No se ejecuta las 24 horas del día, los 7 días de la semana = **On-Demand** podría ser un desperdicio.
- Horas de oficina regulares = escalado programado.
- Capacidad base necesaria = candidato para **Reserved Instances**.

**Solución Recomendada**:

1. **Comprar Standard Reserved Instances de 3 años para 2-3 instancias (capacidad base)**
   - Ahorros: Hasta un 75% en estas instancias.

2. **Configurar EC2 Auto Scaling con acciones programadas**:
   - Escalar hacia arriba de lunes a viernes a las 8:30 a.m. EST (antes de que comience el tráfico).
   - Escalar hacia abajo a las 5:30 p.m. EST (después de que termine el tráfico).
   - Capacidad mínima los fines de semana: 2-3 instancias.

3. **Usar On-Demand para periodos pico durante el horario de oficina**

4. **Almacenar los datos de sesión en ElastiCache o DynamoDB (no en las instancias)**

**Ahorros Esperados**: Reducción del 40-60% en los costos mensuales.

---

### Escenario 2: Diseño para Alta Disponibilidad

**Situación**: La aplicación de una empresa de comercio electrónico debe permanecer disponible incluso si falla una Zona de Disponibilidad (**AZ**) completa. La aplicación se ejecuta actualmente en una sola instancia **EC2** con una base de datos **MySQL**.

**Pregunta**: ¿Cómo debería diseñar esta arquitectura para lograr alta disponibilidad?

**Problemas Actuales**:
- Punto único de fallo (una instancia **EC2**).
- Base de datos no redundante.
- Sin conmutación por error automática.
- Datos de sesión vinculados a la instancia.

**Solución Recomendada**:

#### 1. Nivel de Aplicación Multi-AZ
- Desplegar un **Application Load Balancer** (**ALB**) que abarque múltiples **AZs**.
- Crear un **Auto Scaling group** con un mínimo de 2 instancias en diferentes **AZs**.
- Establecer la capacidad deseada basada en los patrones de tráfico.
- Configurar verificaciones de estado en el **ALB** y el **Auto Scaling**.

#### 2. Base de Datos Multi-AZ
- Migrar **MySQL** a **Amazon RDS Multi-AZ**.
- Conmutación por error automática a una instancia en espera en una **AZ** diferente.
- Replicación síncrona.
- Tiempo de inactividad mínimo durante la conmutación por error.

#### 3. Diseño de Aplicación sin Estado (Stateless)
- Almacenar los datos de sesión en **ElastiCache** (**Redis** con **Multi-AZ**).
- O usar **DynamoDB** para el almacenamiento de sesiones.
- Habilitar sesiones pegajosas (**sticky sessions**) en el **ALB** si es necesario (pero es preferible sin estado).

#### 4. Activos Estáticos
- Almacenar en **S3** (automáticamente multi-AZ).
- Usar **CloudFront** para la distribución global.

#### 5. Monitoreo
- Configurar alarmas de **CloudWatch** para las verificaciones de estado.
- Configurar notificaciones de **SNS** para los fallos.

**Beneficios de la Arquitectura**:
- Sobrevive al fallo de una **AZ**.
- Escalado automático para picos de tráfico.
- Conmutación por error automática para la base de datos.
- Sin punto único de fallo.

---

### Escenario 3: Migración de Grandes Volúmenes de Datos

**Situación**: Una empresa de atención médica necesita migrar 80 TB de datos de imágenes médicas desde el almacenamiento local a **S3**. El cumplimiento normativo requiere que los datos estén cifrados y que la migración se complete en un plazo de 2 semanas.

**Restricciones**:
- Conexión a internet: 100 Mbps.
- La carga a través de internet tomaría: ~74 días.
- Fecha límite: 2 semanas.
- Los datos deben estar cifrados.
- Se requiere cumplimiento de **HIPAA**.

**Pregunta**: ¿Cuál es el mejor enfoque de migración?

**Análisis**:
- El volumen de datos es demasiado grande para la carga por internet.
- La restricción de tiempo elimina las soluciones basadas en internet.
- Requisitos de seguridad y cumplimiento.
- Necesidad de un dispositivo físico para la transferencia.

**Solución Recomendada**:

#### 1. Usar AWS Snowball Edge Storage Optimized
- 80 TB de capacidad utilizable por dispositivo.
- Pedir 1-2 dispositivos (por redundancia).
- Cifrado de 256 bits integrado.
- Compatible con **HIPAA**.

#### 2. Proceso de Migración

1. Pedir el dispositivo **Snowball** a través de la consola de **AWS**.
2. **AWS** envía el dispositivo (2-3 días).
3. Conectar a la red, desbloquear con las credenciales.
4. Copiar los datos usando el cliente de **Snowball** (2-4 días para 80 TB).
5. Devolver el dispositivo a **AWS** (2-3 días).
6. **AWS** carga los datos a **S3** (1-2 días).

#### 3. Configuración de S3
- Habilitar el cifrado del lado del servidor de **S3** (**SSE-S3** o **SSE-KMS**).
- Habilitar el control de versiones (**versioning**) para la protección de datos.
- Configurar políticas de ciclo de vida (**lifecycle**) para la transición de datos antiguos a **Glacier**.
- Habilitar **S3 Object Lock** para el cumplimiento (**WORM**).

#### 4. Cumplimiento
- Usar **AWS Artifact** para acceder al **BAA** de **HIPAA**.
- Firmar el apéndice para asociados comerciales (**BAA**).
- Habilitar **CloudTrail** para el registro de auditoría.
- Usar **AWS Config** para el monitoreo del cumplimiento.

**Cronograma**: 7-12 días (c### Escenario 4: Arquitectura de Aplicación Serverless

**Situación**: Una startup quiere crear el backend de una aplicación móvil con una **API REST**. Tienen recursos de **DevOps** limitados y quieren minimizar los gastos operativos pagando solo por el uso real.

**Requisitos**:
- **API REST** para la aplicación móvil.
- Autenticación de usuarios.
- Almacenamiento de datos.
- Almacenamiento de imágenes.
- Escalable a millones de usuarios.
- Gestión operativa mínima.
- Precios de pago por uso.

**Pregunta**: ¿Qué servicios de AWS deberían usar?

**Arquitectura Serverless Recomendada**:

#### 1. Capa de API
- **Amazon API Gateway**: Crear y administrar la **API REST**.
- Características: Estrangulamiento de solicitudes (**throttling**), claves de **API**, almacenamiento en caché, **CORS**.
- Pago por cada millón de llamadas a la **API**.

#### 2. Capa de Cómputo
- **AWS Lambda**: Ejecutar la lógica de negocio sin servidores.
- Lenguajes: **Node.js**, **Python**, **Java**, **Go**, etc.
- Escalado automático integrado.
- Pague solo por el tiempo de ejecución.

#### 3. Autenticación
- **Amazon Cognito**: Registro de usuarios, inicio de sesión, control de acceso.
- Grupos de usuarios (**User pools**) para la autenticación.
- Grupos de identidad (**Identity pools**) para el acceso a los recursos de AWS.
- Proveedores de identidad social (**Facebook**, **Google**).
- Nivel gratuito: 50,000 **MAUs**.

#### 4. Almacenamiento de Datos
- **Amazon DynamoDB**: Base de datos **NoSQL**.
- Latencia de milisegundos de un solo dígito.
- Escalado automático.
- Capacidad bajo demanda o provisionada.
- Nivel siempre gratuito: 25 GB de almacenamiento.

#### 5. Almacenamiento de Imágenes
- **Amazon S3**: Almacenar imágenes cargadas por el usuario.
- Políticas de **lifecycle** para mover imágenes antiguas a **Glacier**.
- **CloudFront** para una entrega rápida de imágenes.

#### 6. Mejoras Opcionales
- **Amazon CloudFront**: **CDN** para la **API** y activos estáticos.
- **AWS AppSync**: **API GraphQL** (alternativa a **API Gateway** + **Lambda**).
- **Amazon SES**: Enviar correos electrónicos transaccionales.
- **Amazon SNS**: Notificaciones **push** a dispositivos móviles.

**Beneficios**:
- Gestión de servidores cero.
- Escalado automático de 0 a millones de usuarios.
- Pague solo por el uso real.
- Alta disponibilidad integrada.
- Enfoque en el código de la aplicación, no en la infraestructura.
- Despliegue e iteración rápidos.

**Ejemplo de Costo**:
- 1 millón de solicitudes de **API**: ~$3.50.
- Ejecuciones de **Lambda**: ~$0.20.
- **DynamoDB**: ~$1.25.
- Almacenamiento de **S3** (100 GB): ~$2.30.
- **Total: ~$7.25/mes para 1 millón de solicitudes.**

---

### Escenario 5: Cumplimiento y Gobernanza

**Situación**: Una empresa de servicios financieros con 50 cuentas de AWS necesita asegurarse de que ningún bucket de **S3** sea accesible públicamente en toda la organización. También necesitan rastrear todos los cambios y demostrar el cumplimiento.

**Requisitos**:
- Aplicar que no haya buckets de **S3** públicos.
- Aplicar a todas las cuentas.
- Monitorear el cumplimiento continuamente.
- Auditar todos los cambios.
- Se prefiere la remediación automatizada.

**Pregunta**: ¿Cómo pueden aplicar y monitorear esta política?

**Solución Recomendada**:

#### 1. Configuración de AWS Organizations
- Agrupar cuentas usando Unidades Organizativas (**OUs**).
- Ejemplo de estructura: **Production OU**, **Development OU**, **Test OU**.

#### 2. Service Control Policies (SCPs)
- Crear una **SCP** que deniegue `s3:PutBucketPublicAccessBlock` con el valor **False**.
- Denegar `s3:PutBucketPolicy` si permite el acceso público.
- Aplicar a la raíz o a **OUs** específicas.
- Las **SCPs** definen los permisos máximos (incluso los administradores no pueden anularlos).

#### 3. S3 Block Public Access
- Habilitar **S3 Block Public Access** a nivel de organización.
- Se aplica a todas las cuentas de la organización.
- Evita la exposición pública accidental.

#### 4. Monitoreo Continuo
- Habilitar **AWS Config** en todas las cuentas.
- Desplegar la regla **s3-bucket-public-read-prohibited**.
- Desplegar la regla **s3-bucket-public-write-prohibited**.
- Informes de cumplimiento automáticos.

#### 5. Remediación Automatizada
- Configurar la **auto-remediación** de **AWS Config**.
- Usar documentos de **AWS Systems Manager Automation**.
- Desactivar automáticamente el acceso público cuando se detecte.

#### 6. Auditoría y Registro
- Habilitar **CloudTrail** en todas las cuentas.
- Centralizar los registros en una cuenta de seguridad dedicada.
- Rastrear todas las llamadas a la **API** de **S3**.
- Configurar alarmas de **CloudWatch** para las violaciones de políticas.

#### 7. Seguridad Centralizada
- Usar **AWS Security Hub** para una visión de seguridad centralizada.
- Agrega hallazgos de **Config**, **GuardDuty**, **Inspector**.
- Cuadros de mando de cumplimiento para estándares (**PCI DSS**, **CIS**).

**Recomendaciones Adicionales**:
- Informes de cumplimiento regulares usando **AWS Artifact**.
- Revisiones de acceso periódicas.
- Capacitación de empleados en las mejores prácticas de seguridad.
- Implementar políticas **IAM** de privilegio mínimo.

---

### Escenario 6: Estrategia de Recuperación ante Desastres

**Situación**: Una empresa de comercio electrónico necesita una recuperación ante desastres para su aplicación. Su negocio requiere:
- **RPO** (Objetivo de Punto de Recuperación): 1 hora.
- **RTO** (Objetivo de Tiempo de Recuperación): 4 horas.
- Actualmente se ejecuta en **us-east-1**.

**Pregunta**: ¿Qué estrategia de **DR** deberían implementar?

**Opciones de Estrategia de DR**:

| Estrategia | RPO | RTO | Costo | Mejor para |
|------------|-----|-----|-------|------------|
| **Backup and Restore** | Horas a días | Horas a días | El más bajo | Cargas de trabajo no críticas |
| **Pilot Light** ⭐ | Minutos a horas | Horas | Bajo-Medio | Este escenario |
| **Warm Standby** | Segundos a minutos | Minutos | Medio-Alto | Aplicaciones críticas |
| **Multi-Site Activo/Activo** | Cerca de cero | Cerca de cero | El más alto | Sistemas de misión crítica |

> **Recomendación**: **Pilot Light** es la estrategia óptima para este escenario, cumpliendo con los requisitos de **RPO** de 1 hora y **RTO** de 4 horas a un costo razonable.

**Implementación de Pilot Light Recomendada**:

#### 1. Replicación de Datos
- Usar réplicas de lectura entre regiones de **RDS**.
- Replicar de **us-east-1** a **us-west-2**.
- Cumple con el requisito de **RPO** de 1 hora.

#### 2. AMIs de la Aplicación
- Copiar regularmente las **AMIs** a la región de **DR**.
- Mantener las **AMIs** actualizadas.
- Automatizar con **Lambda**.

#### 3. Infraestructura como Código
- Usar plantillas de **CloudFormation**.
- Pre-crear **VPC**, subredes, grupos de seguridad en la región de **DR**.
- Mantener los **Auto Scaling groups** en la región de **DR** con capacidad 0.

#### 4. Conmutación por Error de DNS
- Usar verificaciones de estado de **Route 53**.
- Configurar la política de enrutamiento de conmutación por error (**failover**).
- Conmutación por error de **DNS** automática a la región de **DR**.

#### 5. Pruebas
- Simulacros de **DR** trimestrales.
- Documentar manuales de procedimientos (**runbooks**).
- Medir el **RTO**/**RPO** real.

**Proceso de Conmutación por Error**:

1. Detectar el fallo de la región primaria (verificación de estado de **Route 53**).
2. Promover la réplica de lectura de **RDS** a maestro.
3. Actualizar el stack de **CloudFormation** para escalar el **Auto Scaling**.
4. **Route 53** redirige automáticamente el tráfico.
5. **Tiempo total: ~2-3 horas (cumple con el RTO de 4 horas)**.

---

### Escenario 7: Conectividad de Nube Híbrida

**Situación**: Una empresa de fabricación quiere ampliar su centro de datos local a AWS manteniendo un rendimiento de red constante para su sistema **ERP**.

**Requisitos**:
- Latencia de red constante.
- Conexión privada (sin internet).
- Ancho de banda: 1 Gbps.
- Acceso a múltiples **VPCs**.

**Análisis de Opciones de Conexión**:

| Solución | Pros | Contras | Mejor para |
|----------|------|---------|------------|
| **Site-to-Site VPN** | Configuración rápida (horas), bajo costo, cifrada | Latencia variable, basada en internet, ancho de banda limitado | Des/pruebas, conexiones temporales |
| **AWS Direct Connect** ⭐ | Rendimiento constante, alto ancho de banda, privada | Costosa, tarda semanas, no cifrada por defecto | Producción, necesidades de alto ancho de banda |
| **Direct Connect + VPN** | Lo mejor de ambos mundos | La más costosa, compleja | Industrias reguladas que requieren cifrado |

**Solución Recomendada: AWS Direct Connect**

#### 1. Configuración de Direct Connect
- Pedir un puerto de **Direct Connect** de 1 Gbps.
- Trabajar con un socio de **AWS Direct Connect**.
- El aprovisionamiento tarda de 2 a 4 semanas.
- Configurar la interconexión (**cross-connect**) en la instalación de coubicación.

#### 2. Acceso a Múltiples VPC
- Usar **Direct Connect Gateway**.
- Conectarse a múltiples **VPCs** en todas las regiones.
- Una sola conexión de **Direct Connect**.
- Simplifica la conectividad.

#### 3. Alta Disponibilidad
- Pedir una segunda conexión de **Direct Connect** (ubicación diferente).
- Configurar **BGP** para la conmutación por error automática.
- O usar una **VPN** como conexión de respaldo.

#### 4. Seguridad
- Superponer la **VPN** sobre **Direct Connect** para el cifrado.
- O usar cifrado **MACsec**.
- **VIF** privada para el acceso a la **VPC**.
- **VIF** pública para los servicios públicos de AWS.

---
t for encryption
- Or use **MACsec** encryption
- Private VIF for VPC access
- Public VIF for public AWS services

---

### Escenario 8: Arquitectura Multi-Región para Aplicación Global

**Situación**: Una empresa de redes sociales va a lanzar una nueva aplicación para compartir fotos que debe dar servicio a usuarios de Norteamérica, Europa y Asia. Esperan un crecimiento rápido y necesitan proporcionar un acceso de baja latencia al contenido manteniendo la consistencia de los datos.

**Estado Actual**:
- Despliegue en una sola región en **us-east-1**.
- Latencia de más de 200 ms para los usuarios de Asia y Europa.
- Quejas de los clientes sobre la lentitud en la carga de imágenes.
- Base de usuarios creciente: 100,000 usuarios → se esperan 5 millones en 6 meses.

**Requisitos**:

**Requisitos Funcionales**:
- Los usuarios pueden cargar/ver fotos desde cualquier región.
- Funciones sociales: me gusta, comentarios, seguimientos.
- Perfil de usuario y configuración.
- Funcionalidad de búsqueda.
- Acceso móvil y web.

**Requisitos No Funcionales**:
- Latencia: <100 ms para la entrega de contenido.
- Disponibilidad: 99.95%.
- Cumplimiento de la residencia de datos (**GDPR** para la UE).
- **RPO**: 1 hora, **RTO**: 2 horas.
- Soporte para 10 millones de usuarios concurrentes.
- Escalado rentable.

**Pregunta**: ¿Cómo deberían diseñar una arquitectura multi-región?

**Arquitectura Recomendada**:

#### 1. Entrega de Contenido Global

**Amazon CloudFront**:
- Desplegar distribuciones de **CloudFront** con ubicaciones de borde en todo el mundo.
- Almacenar en caché activos estáticos (imágenes, **CSS**, **JavaScript**).
- Cachés de borde regionales para archivos grandes.
- Configurar orígenes personalizados que apunten a los endpoints regionales.
- Habilitar **HTTP/2** y compresión.

**Amazon S3**:
- Crear buckets de **S3** en cada región principal (**us-east-1**, **eu-west-1**, **ap-southeast-1**).
- Habilitar **S3 Transfer Acceleration** para cargas más rápidas.
- Usar **S3 Intelligent-Tiering** para la optimización automática de costos.
- Implementar políticas de ciclo de vida para el contenido antiguo.

**Replicación entre Regiones**:
- Habilitar **S3 Cross-Region Replication** para la recuperación ante desastres.
- Replicar fotos de forma bidireccional entre regiones.
- Usar el control de tiempo de replicación para una replicación predecible.
- Replicar solo el contenido activo (fotos de <30 días).

#### 2. Arquitectura de Base de Datos

**Amazon DynamoDB Global Tables**:
- Desplegar **Global Tables** en 3 regiones.
- Tablas: Usuarios, Publicaciones, Me gusta, Comentarios, Seguimientos.
- Replicación multi-maestro (escrituras en cualquier región).
- Latencia de replicación típica: <1 segundo.
- Resolución automática de conflictos (el último en escribir gana).
- Usar capacidad bajo demanda para el tráfico impredecible.

**Alternativa: Amazon Aurora Global Database**:
- Si se necesitan consultas complejas.
- Región primaria: **us-east-1**.
- Réplicas de lectura en **eu-west-1** y **ap-southeast-1**.
- Retraso (**lag**): <1 segundo.
- Conmutación por error: <1 minuto.
- Mejor para datos relacionales y uniones (**joins**) complejas.

**Cumplimiento de la Residencia de Datos**:
- Crear tablas de **DynamoDB** separadas para los usuarios de la UE.
- Almacenar los datos de los usuarios de la UE solo en **eu-west-1**.
- Usar políticas **IAM** para aplicar los límites de datos.
- Documentar el flujo de datos para el cumplimiento de **GDPR**.

#### 3. Capa de Aplicación y API

**Amazon API Gateway**:
- Desplegar endpoints regionales de **API Gateway**.
- Optimizado para el borde para la integración con **CloudFront**.
- Nombres de dominio personalizados por región.
- Estrangulamiento de solicitudes (**throttling**) y almacenamiento en caché.

**AWS Lambda o ECS Fargate**:
- **Lambda** para cargas de trabajo esporádicas impulsadas por eventos.
- **ECS Fargate** para aplicaciones contenedorizadas.
- Desplegar en múltiples **AZs** por región.
- Escalado automático basado en el volumen de solicitudes.

#### 4. Enrutamiento y Gestión del Tráfico

**Amazon Route 53**:
- Crear una política de enrutamiento por geolocalización.
- Norteamérica → **us-east-1**.
- Europa → **eu-west-1**.
- Asia → **ap-southeast-1**.
- Configurar verificaciones de estado para la conmutación por error.
- Enrutamiento basado en la latencia para un rendimiento óptimo.

**Implementación**:
```
Usuario en Alemania
→ Route 53 (geolocalización: Europa)
→ CloudFront (borde de Frankfurt)
→ API Gateway (eu-west-1)
→ Lambda/ECS (eu-west-1)
→ DynamoDB Global Table (eu-west-1)
→ S3 (eu-west-1) a través de CloudFront
```

#### 5. Funcionalidad de Búsqueda

**Amazon OpenSearch Service**:
- Desplegar el dominio en cada región.
- Indexar datos de usuarios y publicaciones.
- Instantánea (**snapshot**) entre regiones para respaldo.
- O usar **Amazon CloudSearch**.

**Alternativa: Amazon Kendra**:
- Para búsqueda inteligente con **ML**.
- Consultas en lenguaje natural.

#### 6. Monitoreo y Operaciones

**Amazon CloudWatch**:
- Cuadros de mando entre regiones.
- Registro unificado con **CloudWatch Logs Insights**.
- Alarmas por latencia, errores, costos.
- Métricas personalizadas para **KPIs** de negocio.

**AWS X-Ray**:
- Rastreo distribuido entre regiones.
- Identificar cuellos de botella.
- Visualización del mapa de servicios.

**Implementación Paso a Paso**:

**Fase 1: Fundación (Semanas 1-2)**
1. Configurar **AWS Organizations** y la estructura de múltiples cuentas.
2. Crear **VPCs** en las regiones objetivo.
3. Desplegar plantillas de **CloudFormation** para la infraestructura.
4. Configurar el monitoreo y registro centralizados.
5. Configurar roles y políticas **IAM**.

**Fase 2: Capa de Datos (Semanas 3-4)**
1. Crear **DynamoDB Global Tables**.
2. Configurar buckets de **S3** con replicación.
3. Configurar **Aurora Global Database** (si se elige).
4. Probar la replicación y consistencia de los datos.
5. Implementar estrategias de respaldo.

**Fase 3: Despliegue de la Aplicación (Semanas 5-6)**
1. Desplegar **API Gateway** en todas las regiones.
2. Desplegar funciones **Lambda** o servicios **ECS**.
3. Configurar políticas de **Auto Scaling**.
4. Implementar estrategias de almacenamiento en caché.
5. Configurar distribuciones de **CloudFront**.

**Fase 4: Enrutamiento y DNS (Semana 7)**
1. Configurar el enrutamiento por geolocalización de **Route 53**.
2. Configurar verificaciones de estado y conmutación por error.
3. Probar el enrutamiento desde diferentes regiones.
4. Configurar certificados **SSL**/**TLS**.

**Fase 5: Pruebas y Optimización (Semana 8)**
1. Pruebas de carga desde múltiples regiones.
2. Mediciones de latencia.
3. Pruebas de conmutación por error.
4. Optimización de costos.
5. Refuerzo de la seguridad.

**Desglose de Costos (Estimación Mensual para 5 Millones de Usuarios)**:

| Servicio | Configuración | Costo Mensual |
|----------|---------------|---------------|
| **CloudFront** | 10 TB de transferencia de datos, 100M de solicitudes | $850 |
| **S3** | 50 TB de almacenamiento, **Transfer Acceleration** | $1,250 |
| **DynamoDB Global Tables** | 1,000 millones de solicitudes, 500 GB | $1,800 |
| **Lambda** | 500M de solicitudes, 1GB de memoria | $900 |
| **API Gateway** | 500M de solicitudes | $1,750 |
| **Route 53** | Zonas alojadas, verificaciones de estado | $100 |
| **CloudWatch** | Registros, métricas, alarmas | $250 |
| **Transferencia de Datos** | Replicación entre regiones | $450 |
| **Total** | | **~$7,350/mes** |

**Estrategias de Optimización de Costos**:
1. Usar **S3 Intelligent-Tiering** para transiciones automáticas de clases de almacenamiento.
2. Habilitar la compresión de **CloudFront** para reducir la transferencia de datos.
3. Implementar precios bajo demanda de **DynamoDB** para cargas de trabajo variables.
4. Usar capacidad reservada para la carga base predecible.
5. Configurar alertas de **AWS Budgets**.
6. Archivar contenido antiguo en **S3 Glacier**.

**Beneficios**:
- **Rendimiento**: <100 ms de latencia en todo el mundo.
- **Disponibilidad**: 99.99% con conmutación por error multi-región.
- **Escalabilidad**: Maneja picos de tráfico sin problemas.
- **Soberanía de Datos**: Cumplimiento de **GDPR** con almacenamiento de datos regional.
- **Experiencia de Usuario**: Entrega rápida de contenido independientemente de la ubicación.
- **Continuidad del Negocio**: Conmutación por error automática entre regiones.

**Compromisos (Trade-offs)**:
- **Complejidad**: Gestión de la infraestructura multi-región.
- **Costo**: Más alto que un despliegue en una sola región (3-4 veces).
- **Consistencia de Datos**: Consistencia eventual con **Global Tables**.
- **Desarrollo**: Pruebas y despliegue más complejos.
- **Gasto Operativo**: Monitoreo y resolución de problemas en múltiples regiones.

**Enfoques Alternativos**:

**Opción 1: Enfoque Híbrido**
- Región primaria con **CloudFront** para la entrega de contenido.
- Menor costo pero mayor latencia para las escrituras.
- El mejor para aplicaciones con muchas lecturas.

**Opción 2: Multi-Región Activo-Pasivo**
- La región activa maneja todo el tráfico.
- Región pasiva solo para recuperación ante desastres.
- Menor costo, más simple pero mayor tiempo de conmutación por error.

**Opción 3: Aislamiento Regional**
- Despliegues completamente separados por región.
- Sin replicación de datos entre regiones.
- El mejor para requisitos estrictos de residencia de datos.

**Errores Comunes a Evitar**:
1. **No probar la conmutación por error**: Practique regularmente la conmutación por error de región.
2. **Ignorar los costos de transferencia de datos**: Pueden ser el 30-40% de los costos totales.
3. **Suposiciones de replicación síncrona**: Las **DynamoDB Global Tables** son eventualmente consistentes.
4. **Sobre-ingeniería**: Comience con 2 regiones, amplíe según sea necesario.
5. **Descuidar el monitoreo**: Configure cuadros de mando integrales de **CloudWatch** desde el principio.
6. **Endpoints codificados (hardcoded)**: Use el descubrimiento de servicios o la configuración.
7. **Ignorar las leyes de residencia de datos**: Consulte al equipo legal para el cumplimiento.
8. **No considerar la latencia para las escrituras**: Las **Global Tables** tienen un retraso de replicación de ~1s.

---

### Escenario 9: Respuesta y Prevención de Incidentes de Seguridad

**Situación**: Una empresa de tecnología de atención médica experimentó un incidente de seguridad en el que un bucket de **S3** que contenía datos de pacientes estuvo expuesto públicamente por un breve periodo. El **CISO** ha ordenado una revisión exhaustiva de la seguridad para prevenir futuros incidentes y mejorar las capacidades de detección y respuesta.

**Estado Actual**:
- 25 cuentas de AWS con prácticas de seguridad inconsistentes.
- Sin monitoreo de seguridad centralizado.
- Revisiones de seguridad manuales.
- Visibilidad limitada de los cambios de configuración.
- Enfoque de seguridad reactivo.

**Impacto del Incidente**:
- 10,000 registros de pacientes potencialmente expuestos.
- 4 horas hasta la detección.
- Investigación por violación de **HIPAA**.
- Daño a la reputación.
- Multas potenciales de hasta $1.5 millones.

**Requisitos**:

**Requisitos Funcionales**:
- Detectar amenazas de seguridad en tiempo real.
- Prevenir el acceso no autorizado.
- Respuesta automatizada ante incidentes.
- Monitoreo continuo del cumplimiento.
- Pista de auditoría para todas las acciones.
- Cifrado en reposo y en tránsito.

**Requisitos No Funcionales**:
- Tiempo de detección: <5 minutos.
- Respuesta automatizada: <1 minuto.
- 100% de cumplimiento de la configuración.
- Retención de registros por 7 años.
- Cumplimiento de **SOC 2**, **HIPAA**.
- Arquitectura de confianza cero (**Zero Trust**).

**Pregunta**: ¿Cómo deberían implementar controles de seguridad integrales?

**Arquitectura de Seguridad Recomendada**:

#### 1. Controles Detectivos - Detección de Amenazas

**Amazon GuardDuty**:
- Habilitar en todas las cuentas y regiones.
- Monitorea **VPC Flow Logs**, **CloudTrail**, registros de **DNS**.
- Detección de anomalías basada en **ML**.
- Detecta:
  - Instancias **EC2** comprometidas (minería de criptomonedas).
  - Actividad de reconocimiento.
  - Intentos de acceso no autorizados.
  - Exfiltración de datos.
  - Comunicaciones con IPs maliciosas.

**AWS Security Hub**:
- Panel de seguridad centralizado.
- Agrega hallazgos de:
  - **GuardDuty**
  - **Amazon Inspector**
  - **Amazon Macie**
  - **IAM Access Analyzer**
  - **AWS Config**
  - Herramientas de terceros.
- Verificaciones de cumplimiento contra:
  - **CIS AWS Foundations Benchmark**
  - **PCI DSS**
  - **HIPAA**
  - **AWS Foundational Security Best Practices**.

**Amazon Macie**:
- Descubrimiento automatizado de datos sensibles.
- Escanea buckets de **S3** en busca de **PII** (Información de Identificación Personal), **PHI** (Información de Salud Protegida).
- Clasificación mediante aprendizaje automático.
- Identifica:
  - Números de tarjetas de crédito.
  - Números de seguridad social.
  - Registros de salud de pacientes.
  - Claves de **API** y secretos.

**AWS CloudTrail**:
- Habilitar en todas las regiones.
- Registrar todas las llamadas a la **API**.
- Trail multi-región.
- Validación de la integridad del archivo de registro.
- Registro centralizado en una cuenta de seguridad dedicada.
- Bucket de **S3** con **MFA Delete** habilitado.
- Política de ciclo de vida: retención de 7 años.

#### 2. Controles Preventivos - Gestión de Accesos

**AWS Organizations con SCPs**:
- Jerarquía organizacional:
  - Raíz
    - **Security OU**
    - **Production OU**
    - **Development OU**
    - **Sandbox OU**

**Service Control Policies (SCPs)**:
```json
// Prevenir la desactivación de los servicios de seguridad
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Deny",
      "Action": [
        "guardduty:DeleteDetector",
        "securityhub:DisableSecurityHub",
        "cloudtrail:StopLogging",
        "cloudtrail:DeleteTrail",
        "config:DeleteConfigRule",
        "config:StopConfigurationRecorder"
      ],
      "Resource": "*"
    }
  ]
}

// Obligar al cifrado
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Deny",
      "Action": "s3:PutObject",
      "Resource": "*",
      "Condition": {
        "StringNotEquals": {
          "s3:x-amz-server-side-encryption": [
            "AES256",
            "aws:kms"
          ]
        }
      }
    }
  ]
}

// Prevenir el acceso público a S3
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Deny",
      "Action": [
        "s3:PutAccountPublicAccessBlock"
      ],
      "Resource": "*",
      "Condition": {
        "StringNotEquals": {
          "s3:PublicAccessBlock": "true"
        }
      }
    }
  ]
}
```

**IAM Access Analyzer**:
- Monitorea continuamente las políticas de **IAM**.
- Identifica recursos compartidos con entidades externas.
- Valida las políticas contra las mejores prácticas.
- Genera recomendaciones de políticas.

**AWS IAM Identity Center (SSO)**:
- Gestión centralizada del acceso de usuarios.
- Autenticación multifactor (**MFA**) obligatoria.
- Integración con proveedores de identidad corporativos (**Okta**, **Azure AD**).
- Acceso elevado limitado en el tiempo.
- Control de acceso basado en atributos (**ABAC**).

#### 3. Monitoreo Continuo del Cumplimiento

**AWS Config**:
- Habilitar en todas las regiones y cuentas.
- Registro de configuración para todos los recursos.
- Reglas de cumplimiento:
  - `s3-bucket-public-read-prohibited`
  - `s3-bucket-public-write-prohibited`
  - `s3-bucket-server-side-encryption-enabled`
  - `rds-encryption-enabled`
  - `ec2-encrypted-volumes`
  - `cloudtrail-enabled`
  - `multi-region-cloudtrail-enabled`
  - `root-account-mfa-enabled`
  - `iam-password-policy`
  - `vpc-flow-logs-enabled`

**AWS Config Aggregator**:
- Vista de cumplimiento centralizada en todas las cuentas.
- Desplegado en la cuenta de seguridad.
- Acceso entre cuentas mediante roles **IAM**.

#### 4. Respuesta Automatizada ante Incidentes

**AWS Lambda para Auto-Remediación**:

**Escenario: Bucket de S3 hecho público**
```python
# Función Lambda activada por la violación de una regla de Config
import boto3

def lambda_handler(event, context):
    s3 = boto3.client('s3')
    config = boto3.client('config')

    # Extraer el nombre del bucket del evento de Config
    bucket_name = event['configRuleEvaluations'][0]['resourceId']

    # Bloquear todo el acceso público
    s3.put_public_access_block(
        Bucket=bucket_name,
        PublicAccessBlockConfiguration={
            'BlockPublicAcls': True,
            'IgnorePublicAcls': True,
            'BlockPublicPolicy': True,
            'RestrictPublicBuckets': True
        }
    )

    # Enviar notificación por SNS
    sns = boto3.client('sns')
    sns.publish(
        TopicArn='arn:aws:sns:us-east-1:123456789012:SecurityAlerts',
        Subject='SEGURIDAD: Bucket de S3 Público Auto-Remediado',
        Message=f'El bucket {bucket_name} se hizo público y ha sido asegurado automáticamente.'
    )

    return {
        'statusCode': 200,
        'body': f'Acceso público remediado para {bucket_name}'
    }
```

**Reglas de Amazon EventBridge**:
- Activar **Lambda** tras los hallazgos de **GuardDuty**.
- Respuestas automatizadas:
  - Aislar instancias **EC2** comprometidas (cambiar grupo de seguridad).
  - Revocar credenciales de usuario **IAM**.
  - Crear instantáneas (**snapshots**) de volúmenes **EBS** para análisis forense.
  - Bloquear IPs maliciosas en **NACLs**.

**AWS Systems Manager Incident Manager**:
- Planes de respuesta ante incidentes automatizados.
- Políticas de escalada.
- Horarios de guardia.
- Análisis post-incidente.

#### 5. Protección de Datos

**Cifrado en Reposo**:
- **S3**: Cifrado predeterminado con **KMS**.
- **EBS**: Volúmenes cifrados obligatorios.
- **RDS**: Cifrado habilitado para todas las bases de datos.
- **DynamoDB**: Cifrado habilitado.
- **EFS**: Cifrado habilitado.

**AWS Key Management Service (KMS)**:
- Claves administradas por el cliente (**CMKs**).
- Rotación automática de claves.
- Políticas de claves que restringen el acceso.
- Registro en **CloudTrail** del uso de las claves.
- Claves separadas por entorno.

**Cifrado en Tránsito**:
- **TLS 1.2**+ para toda la comunicación.
- **AWS Certificate Manager** para **SSL**/**TLS**.
- Endpoints de **VPC** para comunicación privada.
- **PrivateLink** para acceso a servicios.

**AWS Secrets Manager**:
- Rotar credenciales de base de datos automáticamente.
- Almacenar claves de **API** y secretos.
- Integración con **RDS**, **Redshift**, **DocumentDB**.
- Auditar el acceso a los secretos a través de **CloudTrail**.

#### 6. Seguridad de Red

**Seguridad de la VPC**:
- Subredes privadas para las capas de aplicación y base de datos.
- Subredes públicas solo para balanceadores de carga.
- **VPC Flow Logs** habilitados (en todas las **VPCs**).
- **Network ACLs** para filtrado a nivel de subred.

**AWS Network Firewall**:
- Firewall con estado a nivel de **VPC**.
- Sistema de prevención de intrusiones (**IPS**).
- Bloqueo de dominios maliciosos.
- Grupos de reglas personalizados.

**AWS WAF (Web Application Firewall)**:
- Proteger las aplicaciones web de exploits comunes.
- Reglas administradas:
  - **OWASP Top 10**.
  - Entradas maliciosas conocidas.
  - Inyección **SQL**.
  - Cross-site scripting (**XSS**).
- Limitación de tasa (**rate limiting**).
- Bloqueo geográfico.

**Implementación Paso a Paso**:

**Fase 1: Fundación (Semana 1)**
1. Habilitar **CloudTrail** en todas las cuentas.
2. Crear la cuenta de seguridad.
3. Configurar un bucket de **S3** para el registro centralizado.
4. Habilitar **GuardDuty** en todas las cuentas/regiones.
5. Documentar la postura de seguridad actual.

**Fase 2: Detección y Monitoreo (Semana 2)**
1. Habilitar **Security Hub**.
2. Habilitar **Macie** para el escaneo de **S3**.
3. Desplegar **Config** con reglas de cumplimiento.
4. Configurar **Config Aggregator**.
5. Crear cuadros de mando de **CloudWatch**.

**Fase 3: Controles Preventivos (Semana 3)**
1. Implementar **SCPs** en **AWS Organizations**.
2. Habilitar **S3 Block Public Access** en toda la organización.
3. Desplegar **IAM Access Analyzer**.
4. Obligar al uso de **MFA** para todos los usuarios.
5. Implementar **IAM Identity Center**.

**Fase 4: Respuesta Automatizada (Semana 4)**
1. Crear funciones **Lambda** de remediación.
2. Configurar reglas de **EventBridge**.
3. Configurar temas de **SNS** para alertas.
4. Desplegar **Systems Manager Incident Manager**.
5. Probar las respuestas automatizadas.

**Fase 5: Protección de Datos (Semana 5)**
1. Habilitar cifrado predeterminado en **S3**.
2. Cifrar todos los volúmenes **EBS**.
3. Desplegar **KMS CMKs** con rotación.
4. Migrar los secretos a **Secrets Manager**.
5. Implementar **AWS Backup**.

**Fase 6: Pruebas y Validación (Semana 6)**
1. Realizar simulacros de seguridad.
2. Pruebas de penetración.
3. Ejercicios de **Red Team**.
4. Actualizar los manuales de respuesta ante incidentes.
5. Capacitar al equipo de seguridad.

**Desglose de Costos (Mensual para 25 cuentas)**:

| Servicio | Configuración | Costo Mensual |
|----------|---------------|---------------|
| **GuardDuty** | 25 cuentas, 500GB **VPC Flow Logs** | $650 |
| **Security Hub** | 25 cuentas, 10K verificaciones de cumplimiento | $200 |
| **Macie** | 1TB de datos de **S3** escaneados | $300 |
| **CloudTrail** | Multi-región, 1M de eventos | $50 |
| **Config** | 25 cuentas, 500 reglas | $800 |
| **AWS WAF** | 5 web ACLs, 100M de solicitudes | $150 |
| **KMS** | 100 **CMKs** | $100 |
| **Lambda** | Funciones de auto-remediación | $50 |
| **S3** | Almacenamiento de registros (1TB) | $25 |
| **Total** | | **~$2,325/mes** |

**Beneficios**:
- **Detección Rápida**: Amenazas de seguridad detectadas en <5 minutos.
- **Respuesta Automatizada**: Incidentes remediados en <1 minuto.
- **Cumplimiento**: Monitoreo continuo contra estándares.
- **Visibilidad**: Vista centralizada de la postura de seguridad.
- **Prevención**: Los controles proactivos previenen incidentes.
- **Pista de Auditoría**: Historial completo para el cumplimiento.
- **Costo de Prevención**: $2,325/mes vs. multa potencial de $1.5 millones.

**Compromisos (Trade-offs)**:
- **Complejidad Inicial**: Configurar la seguridad centralizada lleva tiempo.
- **Falsos Positivos**: **GuardDuty** puede marcar actividad legítima.
- **Cambios Operativos**: Los equipos deben adaptarse a los nuevos controles de seguridad.
- **Costo**: Gasto continuo en seguridad vs. mitigación de riesgos.

**Enfoques Alternativos**:

**Opción 1: SIEM de Terceros**
- **Splunk**, **Sumo Logic** o **Datadog**.
- Analítica más avanzada.
- Costo más alto.
- Mantenimiento adicional.

**Opción 2: Respuesta Manual**
- Costo más bajo.
- Tiempos de respuesta más lentos.
- No recomendado para cumplimiento normativo.

**Errores Comunes a Evitar**:
1. **Fatiga de alertas de Security Hub**: Comience solo con hallazgos críticos.
2. **No probar la auto-remediación**: Pruebe primero en desarrollo.
3. **SCPs demasiado restrictivas**: Pueden bloquear operaciones legítimas.
4. **Ignorar los hallazgos de GuardDuty**: Revise y actúe sobre todos los hallazgos.
5. **Sin plan de respuesta ante incidentes**: Documente los procedimientos antes de los incidentes.
6. **Despliegue en una sola región**: Habilite los servicios de seguridad en todas las regiones.
7. **Sin capacitación en seguridad**: Eduque a los desarrolladores sobre prácticas seguras.
8. **Olvidar las amenazas internas**: Monitoree la actividad de los usuarios privilegiados.

---

### Escenario 10: Modernización de una Aplicación Monolítica Heredada

**Situación**: Una empresa de seguros ejecuta una aplicación de 15 años basada en **.NET Framework** en servidores locales. La aplicación se encarga de la gestión de pólizas, el procesamiento de reclamaciones y el portal del cliente. Quieren migrar a AWS y modernizar la arquitectura para mejorar la escalabilidad, reducir los costos y acelerar el desarrollo de funciones.

**Estado Actual**:
- Aplicación monolítica **.NET Framework 4.8**.
- Base de datos **SQL Server 2014** (2 TB).
- **Windows Server 2012 R2**.
- 10 servidores de aplicaciones detrás de un balanceador de carga de hardware.
- Carga pico: 5,000 usuarios concurrentes.
- Despliegue: Manual, lanzamientos mensuales, 4 horas de tiempo de inactividad.
- Sin pruebas automatizadas.
- Tiempo de respuesta promedio: 2-3 segundos.
- Costo anual de infraestructura: $500,000.

**Desafíos Actuales**:
- Ciclos de desarrollo lentos.
- Difícil de escalar componentes individuales.
- Altos costos de infraestructura.
- Problemas frecuentes en producción.
- Stack tecnológico obsoleto.
- Desafíos de contratación (tecnología antigua).

**Requisitos**:

**Requisitos Funcionales**:
- Migrar toda la funcionalidad a AWS.
- Mantener la paridad de funciones durante la migración.
- Soportar integraciones existentes (**SOAP**, **APIs REST**).
- Preservar la integridad de los datos.
- Integración con autenticación de **Windows**.

**Requisitos No Funcionales**:
- Cero tiempo de inactividad durante la migración.
- Tiempo de respuesta: <1 segundo.
- 99.9% de disponibilidad.
- Soporte para 10,000 usuarios concurrentes.
- Reducir los costos de infraestructura en un 40%.
- Despliegues semanales con cero tiempo de inactividad.
- Pruebas y reversión (**rollback**) automatizadas.

**Pregunta**: ¿Cómo deberían abordar la migración y modernización?

**Estrategia de Migración Recomendada: Patrón Strangler Fig**

#### Fase 1: Lift and Shift (Fundación)

**Paso 1: Migración de Base de Datos**

**AWS Database Migration Service (DMS)**:
- RDS SQL Server Enterprise Edition
- Multi-AZ deployment for high availability
- db.r5.4xlarge (16 vCPU, 128 GB RAM)
- 3TB storage with Provisioned IOPS (10,000 IOPS)
- Automated backups (7-day retention)
- Automated patching in maintenance window

**Migration Process**:
1. Set up DMS replication instance
2. Create source endpoint (on-premises SQL Server)
3. Create target endpoint (RDS)
4. Crear la tarea de migración con carga completa + **CDC**.
5. Monitorear el retraso de replicación.
6. Realizar la conmutación en un periodo de bajo tráfico.

**Paso 2: Migración de la Aplicación con App2Container**

**AWS App2Container**:
- Analiza las aplicaciones **.NET Framework**.
- Crea la imagen del contenedor.
- Genera definiciones de tareas de **ECS**.
- Crea plantillas de **CloudFormation**.
- Se requieren cambios mínimos de código.

**Proceso**:
1. Instalar **App2Container** en el servidor de aplicaciones.
2. Ejecutar inventario: `app2container inventory`.
3. Analizar la aplicación: `app2container analyze --application-id <id>`.
4. Personalizar el despliegue (app2container-config.json).
5. Generar artefactos: `app2container containerize`.
6. Enviar a **Amazon ECR**.

**Paso 3: Orquestación de Contenedores**

**Amazon ECS on Fargate**:
- Cómputo sin servidor para contenedores.
- Sin instancias **EC2** que administrar.
- Escalado automático.
- Integrado con **Application Load Balancer**.

**Configuración de ECS**:
- Definición de tarea (**Task definition**):
  - 4 vCPU, 8 GB de memoria por tarea.
  - Contenedor **Windows Server 2019 Core**.
  - Variables de entorno para la configuración.
  - Secretos de **AWS Secrets Manager**.
- Servicio:
  - Recuento deseado: 10 tareas.
  - **Auto Scaling**: 10-50 tareas basadas en CPU.
  - Distribuido en 3 **AZs**.
  - Periodo de gracia de verificación de estado: 60 segundos.

#### Fase 2: Modernización (Incremental)

**Implementación del Patrón Strangler Fig**:
1. Identificar contextos delimitados (**bounded contexts**) en el monolito.
2. Extraer un servicio a la vez.
3. Dirigir el tráfico al nuevo servicio.
4. Reemplazar gradualmente los componentes del monolito.

**Servicios Prioritarios a Extraer**:

**1. Servicio de Autenticación**
- Alto nivel de reutilización en todas las funciones.
- Extraer primero para uso compartido.
- Tecnología: **ASP.NET Core Web API**.
- Base de datos: **Amazon Aurora PostgreSQL**.
- Despliegue: **ECS Fargate**.

**2. Servicio de Procesamiento de Reclamaciones**
- Intensivo en CPU.
- Necesidades de escalado independientes.
- Se beneficia del procesamiento basado en colas.
- Tecnología: **ASP.NET Core** + **AWS Lambda**.
- Cola: **Amazon SQS**.
- Base de datos: **DynamoDB** para el estado de las reclamaciones.

**3. Servicio de Almacenamiento de Documentos**
- Cargas de archivos grandes (documentos de reclamaciones, PDFs de pólizas).
- Extraer para reducir la carga del monolito.
- Tecnología: **ASP.NET Core API**.
- Almacenamiento: **Amazon S3**.
- **OCR**: **Amazon Textract**.

**4. Servicio de Notificaciones**
- Notificaciones por correo electrónico, **SMS**.
- Alto volumen, esporádico.
- Tecnología: **AWS Lambda**.
- Correo electrónico: **Amazon SES**.
- **SMS**: **Amazon SNS**.
- Cola: **Amazon SQS**.

**5. Servicio de Informes (Reporting)**
- Consultas que consumen muchos recursos.
- Extraer a una réplica de lectura dedicada.
- Tecnología: **ASP.NET Core** + **Lambda**.
- Base de datos: Réplica de lectura de **RDS**.
- Almacenamiento en caché: **Amazon ElastiCache**.

**Evolución de la Arquitectura**:

```
Inicial (Meses 0-3):
Monolito (ECS) → RDS SQL Server

Fase 1 (Meses 3-6):
ALB → Servicio de Autenticación (ECS)
    ↓
    → Monolito (ECS) → RDS SQL Server

Fase 2 (Meses 6-9):
ALB → Servicio de Autenticación (ECS)
    → Servicio de Reclamaciones (ECS + Lambda + SQS)
    → Monolito (ECS) → RDS SQL Server

Fase 3 (Meses 9-12):
ALB → Servicio de Autenticación (ECS)
    → Servicio de Reclamaciones (ECS + Lambda + SQS)
    → Servicio de Documentos (ECS + S3 + Textract)
    → Servicio de Notificaciones (Lambda + SQS + SES/SNS)
    → Servicio de Informes (Lambda + ElastiCache)
    → Monolito (ECS) → RDS SQL Server (funcionalidad reducida)
```

#### Fase 3: Infraestructura de Soporte

**Capa de Almacenamiento en Caché**:

**Amazon ElastiCache for Redis**:
- Almacenar en caché los datos a los que se accede con frecuencia.
- Almacenamiento de sesiones.
- Reducir la carga de la base de datos en un 60%.
- Configuración:
  - **cache.r5.large** (2 nodos).
  - **Multi-AZ** con conmutación por error automática.
  - Cifrado en tránsito y en reposo.

**Application Load Balancer**:
- Enrutamiento basado en rutas (**path-based**).
- Rutas de ejemplo:
  - `/api/auth/*` → Servicio de Autenticación.
  - `/api/claims/*` → Servicio de Reclamaciones.
  - `/api/documents/*` → Servicio de Documentos.
  - `/*` → Monolito (predeterminado).
- Sesiones pegajosas (**sticky sessions**) para la compatibilidad con el monolito.
- Terminación **SSL**/**TLS**.
- Integración con **WAF**.

**API Gateway**:
- Para socios externos que acceden a las **APIs**.
- Limitación de tasa y cuotas.
- Gestión de claves de **API**.
- Transformación de solicitud/respuesta.
- Registro en **CloudWatch**.

**Observabilidad**:

**AWS X-Ray**:
- Rastreo distribuido.
- Identificar cuellos de botella de rendimiento.
- Visualización del mapa de servicios.
- Análisis del flujo de solicitudes.

**Amazon CloudWatch**:
- Registro centralizado.
- Métricas personalizadas (**KPIs** de negocio).
- Cuadros de mando para cada servicio.
- Alarmas por errores y latencia.

**AWS CloudTrail**:
- Pista de auditoría para todas las llamadas a la **API**.
- Cumplimiento y seguridad.

#### Fase 4: Pipeline CI/CD

**AWS CodePipeline**:
```
Origen (CodeCommit)
  ↓
Compilación (CodeBuild)
  - Compilar .NET Core
  - Ejecutar pruebas unitarias
  - Construir imagen de Docker
  - Enviar a ECR
  ↓
Pruebas (CodeBuild)
  - Pruebas de integración
  - Escaneo de seguridad (Snyk, Aqua)
  ↓
Despliegue en Dev (CodeDeploy + ECS)
  - Despliegue Blue/green
  - Pruebas de humo (Smoke tests)
  ↓
Aprobación Manual
  ↓
Despliegue en Prod (CodeDeploy + ECS)
  - Despliegue Blue/green
  - Cambio de tráfico gradual (10% → 50% → 100%)
  - Rollback automático en caso de errores
```

**AWS CodeBuild buildspec.yml**:
```yaml
version: 0.2
phases:
  pre_build:
    commands:
      - echo Iniciando sesión en Amazon ECR...
      - aws ecr get-login-password --region $AWS_DEFAULT_REGION | docker login --username AWS --password-stdin $AWS_ACCOUNT_ID.dkr.ecr.$AWS_DEFAULT_REGION.amazonaws.com
  build:
    commands:
      - echo La compilación comenzó el `date`
      - echo Construyendo la imagen de Docker...
      - docker build -t $IMAGE_REPO_NAME:$IMAGE_TAG .
      - docker tag $IMAGE_REPO_NAME:$IMAGE_TAG $AWS_ACCOUNT_ID.dkr.ecr.$AWS_DEFAULT_REGION.amazonaws.com/$IMAGE_REPO_NAME:$IMAGE_TAG
  post_build:
    commands:
      - echo La compilación se completó el `date`
      - echo Enviando la imagen de Docker...
      - docker push $AWS_ACCOUNT_ID.dkr.ecr.$AWS_DEFAULT_REGION.amazonaws.com/$IMAGE_REPO_NAME:$IMAGE_TAG
      - echo Escribiendo el archivo de definiciones de imagen...
      - printf '[{"name":"app-container","imageUri":"%s"}]' $AWS_ACCOUNT_ID.dkr.ecr.$AWS_DEFAULT_REGION.amazonaws.com/$IMAGE_REPO_NAME:$IMAGE_TAG > imagedefinitions.json
artifacts:
  files: imagedefinitions.json
```

**Implementación Paso a Paso**:

**Fase 1: Preparación (Meses 1-2)**
1. Evaluar la arquitectura de la aplicación.
2. Identificar dependencias e integraciones.
3. Configurar cuentas de AWS y redes.
4. Crear el plan de migración.
5. Capacitar al equipo en los servicios de AWS.

**Fase 2: Migración de la Base de Datos (Mes 3)**
1. Configurar la instancia de **RDS**.
2. Probar la replicación de **DMS**.
3. Migrar la base de datos con **CDC**.
4. Verificar la integridad de los datos.
5. Actualizar las cadenas de conexión.

**Fase 3: Contenerizar el Monolito (Mes 4)**
1. Usar **App2Container**.
2. Probar la aplicación contenerizada.
3. Desplegar en **ECS Fargate**.
4. Ejecución paralela con la infraestructura local.
5. Cambio de tráfico gradual (20% → 50% → 100%).

**Fase 4: Extraer Servicios (Meses 5-12)**
1. Extraer el servicio de autenticación (Mes 5).
2. Extraer el servicio de reclamaciones (Meses 6-7).
3. Extraer el servicio de documentos (Meses 8-9).
4. Extraer el servicio de notificaciones (Mes 10).
5. Extraer el servicio de informes (Mes 11).
6. Desmantelar los componentes del monolito (Mes 12).

**Fase 5: Optimización (Continuo)**
1. Implementar estrategias de almacenamiento en caché.
2. Optimizar las consultas a la base de datos.
3. Ajustar el tamaño de los recursos de cómputo (**right-size**).
4. Implementar el escalado automático (**auto-scaling**).
5. Optimización de costos.

**Comparación del Desglose de Costos**:

**Local (Anual)**:
- Amortización de hardware: $200,000.
- Mantenimiento y soporte: $150,000.
- Costos del centro de datos: $100,000.
- Personal (4 **FTEs**): $400,000 (asignado parcialmente).
- **Total: $500,000/año**

**Arquitectura Modernizada de AWS (Anual)**:
| Servicio | Configuración | Mensual | Anual |
|----------|---------------|---------|-------|
| **ECS Fargate** | Promedio de 30 tareas, Windows | $3,600 | $43,200 |
| **RDS SQL Server** | **Multi-AZ**, **db.r5.4xlarge** | $5,500 | $66,000 |
| **Application Load Balancer** | 2 **ALBs** | $150 | $1,800 |
| **ElastiCache** | Redis, 2 nodos | $250 | $3,000 |
| **S3** | 10 TB de almacenamiento, solicitudes | $300 | $3,600 |
| **Lambda** | 10M de solicitudes | $200 | $2,400 |
| **CloudWatch** | Registros, métricas | $400 | $4,800 |
| **Transferencia de Datos** | Salida (**Outbound**) | $500 | $6,000 |
| **Total** | | **~$10,900/mes** | **~$131,000/año** |

**Costos Adicionales**:
- Herramientas de migración y servicios profesionales: $50,000 (pago único).
- Capacitación: $20,000 (pago único).

**Total Año 1**: $200,000
**Total Año 2+**: $131,000/año

**Ahorros**:
- Año 1: $300,000 (reducción del 60%).
- Año 2+: $369,000 (reducción del 74%).

**Beneficios**:
- **Reducción de Costos**: Ahorro del 74% en costos de infraestructura.
- **Escalabilidad**: El escalado automático maneja el doble de tráfico sin intervención manual.
- **Rendimiento**: Tiempo de respuesta reducido de 3s a <1s.
- **Velocidad de Despliegue**: Despliegues mensuales → semanales.
- **Disponibilidad**: 99.5% → 99.9%.
- **Innovación**: El equipo de desarrollo se centra en las funciones, no en la infraestructura.
- **Contratación**: El stack tecnológico moderno atrae talento.
- **Recuperación ante Desastres**: Integrada con el despliegue **Multi-AZ**.

**Compromisos (Trade-offs)**:
- **Tiempo de Migración**: Proyecto de 12 meses.
- **Curva de Aprendizaje**: El equipo debe aprender sobre AWS, contenedores y microservicios.
- **Complejidad**: Los sistemas distribuidos son más complejos que el monolito.
- **Cambios Operativos**: Nuevos procesos de monitoreo y despliegue.
- **Inversión Inicial**: Tiempo y recursos para la migración.

**Enfoques Alternativos**:

**Opción 1: Reescritura Completa**
- Reconstruir la aplicación desde cero.
- Pros: Última tecnología, arquitectura limpia.
- Contras: Alto riesgo, 2-3 años, costoso.
- Recomendación: Evitar a menos que sea absolutamente necesario.

**Opción 2: Solo Lift and Shift**
- Migrar a **EC2** sin contenerización.
- Pros: Migración más rápida (3 meses).
- Contras: Beneficios limitados, se siguen administrando **VMs**.
- Recomendación: Solo si hay limitaciones de tiempo.

**Opción 3: Prioridad a lo Sin Servidor (Serverless-First)**
- Convertir a **Lambda** + **API Gateway** + **DynamoDB**.
- Pros: Máxima escalabilidad, menor gasto operativo.
- Contras: Requiere una reescritura significativa, inicios en frío (**cold starts**).
- Recomendación: Para nuevas funciones, no para el monolito existente.

**Errores Comunes a Evitar**:
1. **Migración "Big Bang"**: La migración incremental reduce el riesgo.
2. **Ignorar la complejidad de la migración de datos**: Las pruebas de **DMS** son críticas.
3. **No modernizar la arquitectura**: El **lift-and-shift** por sí solo ofrece beneficios limitados.
4. **Subestimar la capacitación del equipo**: Reserve tiempo para el aprendizaje.
5. **Sin plan de reversión (Rollback)**: Tenga siempre una forma de volver atrás.
6. **Omitir las pruebas de carga**: Pruebe con el doble de la carga pico esperada.
7. **No involucrar a las partes interesadas del negocio**: Obtenga su apoyo temprano.
8. **Ignorar la observabilidad**: Implemente el monitoreo desde el primer día.

---

### Escenario 11: Plataforma de Analítica de Big Data

**Situación**: Una empresa minorista recopila cantidades masivas de datos de transacciones en línea, uso de aplicaciones móviles, sensores de **IoT** en tiendas y redes sociales. Quieren construir una plataforma de analítica integral para obtener información en tiempo real sobre el comportamiento de los clientes, optimizar el inventario y mejorar la eficacia del marketing.

**Estado Actual**:
- Múltiples fuentes de datos que generan 5 TB al día.
- Datos dispersos en diferentes sistemas.
- Informes manuales (tardan 2-3 días).
- Sin analítica en tiempo real.
- Capacidades limitadas de ciencia de datos.
- Herramientas de analítica de terceros costosas ($500,000/año).

**Fuentes de Datos**:
- Clickstream web/móvil: 2,000 millones de eventos al día.
- Registros de transacciones: 10 millones de transacciones al día.
- Sensores de **IoT** (tráfico peatonal, temperatura): 50 millones de lecturas al día.
- Menciones en redes sociales: **APIs** y web scraping.
- Datos de **CRM**: Perfiles e interacciones de los clientes.
- Sistemas de inventario: Niveles de stock y envíos.

**Requisitos**:

**Requisitos Funcionales**:
- Ingerir datos de múltiples fuentes.
- Cuadros de mando en tiempo real para operaciones.
- Procesamiento por lotes (**batch**) para informes diarios/semanales.
- Consultas **SQL** ad-hoc para analistas.
- Aprendizaje automático para recomendaciones.
- Retención de datos: Caliente (90 días), Tibio (1 año), Frío (7 años).

**Requisitos No Funcionales**:
- Latencia en tiempo real: <1 minuto.
- Rendimiento de consultas: <5 segundos para consultas interactivas.
- Escalabilidad: Manejar un crecimiento de datos de 10 veces.
- Rentable a gran escala.
- Gobernanza y seguridad de los datos.
- Analítica de autoservicio para usuarios de negocio.

**Pregunta**: ¿Cómo deberían diseñar una arquitectura de plataforma de analítica de **Big Data**?

**Arquitectura Recomendada**:

#### 1. Capa de Ingesta de Datos

**Datos en Streaming en Tiempo Real**:

**Amazon Kinesis Data Streams**:
- Para clickstream, sensores de **IoT**.
- Fragmentos (**shards**): 50 (1 MB/s por fragmento = 50 MB/s en total).
- Retención: 7 días para capacidad de repetición.
- Productores: Aplicaciones web/móviles, dispositivos de **IoT** a través de **Kinesis Agent**.

**Amazon Kinesis Data Firehose**:
- Entrega de transmisiones a **S3**, **Redshift**, **OpenSearch**.
- Agrupación por lotes y compresión automáticas.
- Transformar datos con **Lambda**.
- Tamaño del búfer: 5 MB o 60 segundos.

**Ingesta de Datos por Lotes (Batch)**:

**AWS Glue ETL Jobs**:
- Extraer de bases de datos de origen.
- Transformar y limpiar datos.
- Cargar en el lago de datos de **S3**.
- Programación: Nocturna para registros de transacciones, datos de **CRM**.

**AWS Database Migration Service (DMS)**:
- Replicación continua desde bases de datos transaccionales.
- Captura de datos modificados (**CDC**).
- Impacto mínimo en los sistemas de origen.

**Ingesta Basada en API**:

**AWS Lambda**:
- Obtener datos de las **APIs** de redes sociales.
- Analizar y normalizar.
- Escribir en **Kinesis** o **S3**.
- Programar con **EventBridge** (cada hora).

#### 2. Capa de Almacenamiento - Lago de Datos (Data Lake)

**Amazon S3**:
- Repositorio central del lago de datos.
- Organizado por:
  - Fuente de datos.
  - Particionamiento por fecha (año/mes/día).
  - Formato de archivo (**Parquet**, **ORC** para analítica).

**Estructura de Buckets de S3**:
```
s3://retail-datalake-raw/
  ├── clickstream/year=2025/month=01/day=15/
  ├── transactions/year=2025/month=01/day=15/
  ├── iot-sensors/year=2025/month=01/day=15/
  └── social-media/year=2025/month=01/day=15/

s3://retail-datalake-processed/
  ├── customer-360/
  ├── sales-analytics/
  └── inventory-metrics/

s3://retail-datalake-curated/
  ├── marketing-reports/
  └── executive-dashboards/
```

**Clases de Almacenamiento de S3**:
- Standard: Últimos 90 días (datos calientes).
- Standard-IA: 91 días - 1 año (datos tibios).
- Glacier Flexible Retrieval: 1-7 años (datos fríos).
- Políticas de ciclo de vida para transiciones automáticas.

**Funciones de S3**:
- Control de versiones habilitado para la protección de datos.
- Cifrado del lado del servidor (**SSE-S3** o **SSE-KMS**).
- **S3 Object Lock** para cumplimiento normativo.
- **S3 Access Points** para diferentes equipos.
- **S3 Inventory** para el catálogo de datos.

#### 3. Capa de Procesamiento de Datos

**Procesamiento en Tiempo Real**:

**Amazon Kinesis Data Analytics**:
- Consultas **SQL** sobre datos en streaming.
- Ventanas de tiempo (**tumbling**/**sliding windows**).
- Agregaciones en tiempo real.
- Detección de anomalías.
- Salida a **Lambda**, **Kinesis**, **S3**.

**AWS Lambda**:
- Procesar eventos individuales.
- Enriquecer con datos de referencia (**DynamoDB**).
- Alertas en tiempo real a través de **SNS**.
- Activar flujos de trabajo posteriores.

**Procesamiento por Lotes (Batch)**:

**AWS Glue**:
- **ETL** basado en **Spark** sin servidor.
- Descubre el esquema automáticamente.
- **Glue Data Catalog** (repositorio de metadatos).
- **Glue Studio** para **ETL** visual.
- **Glue DataBrew** para la preparación de datos.

**Amazon EMR (Elastic MapReduce)**:
- Para trabajos complejos de **Spark**, **Hadoop**.
- **EMR on EKS** para cargas de trabajo contenerizadas.
- **Instancias Spot** para el ahorro de costos (reducción del 70%).
- Configuración del clúster:
  - Maestro: **m5.xlarge** (1 instancia).
  - Core: **r5.2xlarge** (5 instancias, **On-Demand**).
  - Tarea: **r5.2xlarge** (20 instancias, **Spot**).

**Trabajo Típico de Glue ETL**:
```python
import sys
from awsglue.transforms import *
from awsglue.utils import getResolvedOptions
from pyspark.context import SparkContext
from awsglue.context import GlueContext
from awsglue.job import Job

args = getResolvedOptions(sys.argv, ['JOB_NAME'])
sc = SparkContext()
glueContext = GlueContext(sc)
spark = glueContext.spark_session
job = Job(glueContext)
job.init(args['JOB_NAME'], args)

# Leer desde el Data Catalog
datasource0 = glueContext.create_dynamic_frame.from_catalog(
    database = "retail_raw",
    table_name = "clickstream"
)

# Transformar
applymapping1 = ApplyMapping.apply(
    frame = datasource0,
    mappings = [
        ("user_id", "string", "customer_id", "string"),
        ("event_timestamp", "long", "event_time", "timestamp"),
        ("page_url", "string", "page_url", "string"),
        ("session_id", "string", "session_id", "string")
    ]
)

# Filtrar registros inválidos
filtered = Filter.apply(
    frame = applymapping1,
    f = lambda x: x["customer_id"] is not None
)

# Escribir en S3 en formato Parquet
glueContext.write_dynamic_frame.from_options(
    frame = filtered,
    connection_type = "s3",
    connection_options = {
        "path": "s3://retail-datalake-processed/customer-sessions/",
        "partitionKeys": ["year", "month", "day"]
    },
    format = "parquet"
)

job.commit()
```

#### 4. Catálogo de Datos y Gobernanza

**AWS Glue Data Catalog**:
- Repositorio de metadatos centralizado.
- Registro de esquemas (**Schema Registry**).
- Integración con **Athena**, **Redshift**, **EMR**.
- **Glue Crawlers** para el descubrimiento automático de esquemas.

**AWS Lake Formation**:
- Control de acceso de grano fino.
- Seguridad a nivel de columna.
- Seguridad a nivel de fila.
- Filtrado de datos.
- Registro de auditoría.
- Tablas gobernadas para transacciones **ACID**.

**Ejemplo de Control de Acceso**:
```
Administrador del Lago de Datos:
  - Acceso total a todas las tablas.

Equipo de Marketing:
  - Acceso de lectura a: customer_360, campaign_analytics.
  - Filtrado de columnas: Ocultar PII (SSN, tarjeta de crédito).

Equipo de Ciencia de Datos:
  - Acceso de lectura a: todas las tablas.
  - Acceso de escritura al bucket: ml_models.

Equipo de Finanzas:
  - Acceso de lectura a: sales_analytics, inventory_metrics.
  - Filtrado de filas: Solo los datos de su región.
```

#### 5. Analítica y Consultas

**Amazon Athena**:
- Consultas **SQL** interactivas sobre datos de **S3**.
- Sin servidor (sin infraestructura).
- Pago por consulta ($5 por TB escaneado).
- Integración con **QuickSight**.
- Grupos de trabajo (**Workgroups**) para el control de costos.
- Almacenamiento en caché de resultados de consultas.

**Técnicas de Optimización**:
- Particionar los datos por fecha.
- Usar formatos columnares (**Parquet**, **ORC**).
- Comprimir los datos (**Snappy**, **ZSTD**).
- Limitar las columnas en el `SELECT`.
- Usar funciones aproximadas (`approx_distinct` vs `COUNT DISTINCT`).

**Comparación de Rendimiento de Consultas**:
- **CSV**, sin comprimir: $5/TB, 45 segundos.
- **Parquet**, **Snappy**: $0.50/TB, 5 segundos.
- **90% de reducción de costos, 9 veces más rápido.**

**Amazon Redshift**:
- Almacén de datos (**Data Warehouse**) para consultas complejas.
- Procesamiento masivamente paralelo.
- Configuración:
  - Tipo de nodo: **ra3.4xlarge**.
  - Nodos: 5 (640 GB de RAM, 128 TB de almacenamiento).
  - **Redshift Spectrum** para consultas en **S3**.
  - **Concurrency Scaling** para cargas pico.
  - Vistas materializadas para agregaciones.

**Casos de Uso**:
- **Athena**: Consultas ad-hoc, exploración, consultas poco frecuentes.
- **Redshift**: Informes regulares, uniones (**joins**) complejas, rendimiento constante.

#### 6. Inteligencia de Negocio (BI) y Visualización

**Amazon QuickSight**:
- Servicio de **BI** sin servidor.
- Conexión a **Athena**, **Redshift**, **S3**.
- **SPICE** (motor en memoria) para visualizaciones rápidas.
- Información impulsada por **ML** (**ML-powered insights**).
- Analítica integrada para aplicaciones.
- Precios: $5/autor/mes, $0.30/lector/sesión.

**Cuadros de Mando (Dashboards)**:
1. **Cuadro de Mando Ejecutivo**:
   - Tendencias de ventas diarias.
   - Ingresos por región.
   - Productos más vendidos.
   - Costo de adquisición de clientes.

2. **Cuadro de Mando de Operaciones**:
   - Tráfico peatonal en tiendas en tiempo real.
   - Niveles de inventario.
   - Alertas de falta de stock (**stockout**).
   - Métricas de la cadena de suministro.

3. **Cuadro de Mando de Marketing**:
   - Rendimiento de las campañas.
   - Segmentación de clientes.
   - Embudos de conversión.
   - Sentimiento en redes sociales.

4. **Cuadro de Mando de Ciencia de Datos**:
   - Métricas de rendimiento del modelo.
   - Resultados de pruebas **A/B**.
   - Eficacia de las recomendaciones.

#### 7. Pipeline de Aprendizaje Automático (ML)

**Amazon SageMaker**:
- Entrenar modelos de recomendación.
- Detección de fraude.
- Previsión de la demanda.
- Predicción de fuga de clientes (**churn**).

**Flujo de Trabajo de ML**:
1. **Preparación de Datos**: **Glue DataBrew** o **SageMaker Data Wrangler**.
2. **Ingeniería de Características**: Trabajos de procesamiento de **SageMaker**.
3. **Entrenamiento del Modelo**: Trabajos de entrenamiento de **SageMaker** (**Instancias Spot**).
4. **Evaluación del Modelo**: **SageMaker Experiments**.
5. **Registro de Modelos**: **SageMaker Model Registry**.
6. **Despliegue**: Endpoints de **SageMaker** (en tiempo real o por lotes).
7. **Monitoreo**: **SageMaker Model Monitor**.

**Amazon Personalize**:
- Motor de recomendaciones pre-construido.
- No se requiere experiencia en **ML**.
- Recomendaciones en tiempo real y por lotes.
- Casos de uso:
  - Recomendaciones de productos.
  - Clasificaciones personalizadas.
  - Artículos similares.

#### 8. Orquestación y Flujo de Trabajo

**AWS Step Functions**:
- Coordinar pipelines de datos de múltiples pasos.
- Diseñador visual de flujos de trabajo.
- Manejo de errores y lógica de reintento.
- Integración con **Lambda**, **Glue**, **EMR**, **SageMaker**.

**Ejemplo de Pipeline Diario**:
```
1. Ingerir datos (Lambda, Glue)
   ↓
2. Verificaciones de calidad de datos (Lambda)
   ↓
3. Procesamiento ETL (Glue o EMR)
   ↓
4. Cargar en Redshift (Glue)
   ↓
5. Actualizar vistas materializadas (Redshift)
   ↓
6. Actualizar modelos de ML (SageMaker)
   ↓
7. Actualizar conjuntos de datos de QuickSight
   ↓
8. Enviar notificación de finalización (SNS)
```

**Amazon Managed Workflows for Apache Airflow (MWAA)**:
- Alternativa a **Step Functions**.
- Para **DAGs** (Grafos Acíclicos Dirigidos) complejos.
- Definiciones de flujo de trabajo basadas en **Python**.
- Mejor para equipos de ingeniería de datos familiarizados con **Airflow**.

#### 9. Monitoreo y Optimización

**Amazon CloudWatch**:
- Métricas de trabajos de **Glue**.
- Utilización del clúster de **EMR**.
- Métricas de transmisiones de **Kinesis**.
- Rendimiento de consultas de **Athena**.
- Métricas de negocio personalizadas.

**AWS Cost Explorer**:
- Analizar el gasto por servicio.
- Identificar oportunidades de optimización.
- Recomendaciones de instancias reservadas.

**AWS Trusted Advisor**:
- Verificaciones de optimización de costos.
- Mejores prácticas de seguridad.

**Implementación Paso a Paso**:

**Fase 1: Fundación (Meses 1-2)**
1. Configurar cuentas de AWS y redes.
2. Crear la estructura del lago de datos de **S3**.
3. Desplegar el **AWS Glue Data Catalog**.
4. Configurar permisos de **Lake Formation**.
5. Implementar políticas de gobernanza de datos.

**Fase 2: Ingesta (Mes 3)**
1. Desplegar transmisiones de **Kinesis** para datos en tiempo real.
2. Configurar trabajos de **Glue ETL** para datos por lotes.
3. Implementar **Lambda** para la ingesta de **APIs**.
4. Probar el flujo de datos de extremo a extremo.
5. Monitorear la calidad de los datos.

**Fase 3: Procesamiento (Meses 4-5)**
1. Construir pipelines de **Glue ETL**.
2. Desplegar clústeres de **EMR** para procesamiento complejo.
3. Implementar verificaciones de calidad de datos.
4. Configurar la orquestación con **Step Functions**.
5. Optimizar el rendimiento de los trabajos.

**Fase 4: Analítica (Mes 6)**
1. Crear tablas de **Athena**.
2. Desplegar el clúster de **Redshift**.
3. Construir los cuadros de mando iniciales en **QuickSight**.
4. Capacitar a los usuarios de negocio en el autoservicio.
5. Recopilar comentarios e iterar.

**Fase 5: ML (Meses 7-8)**
1. Configurar el entorno de **SageMaker**.
2. Construir el modelo de recomendación.
3. Desplegar la detección de fraude.
4. Implementar la previsión de la demanda.
5. Monitorear el rendimiento del modelo.

**Fase 6: Optimización (Continuo)**
1. Ajustar el tamaño de los recursos (**right-size**).
2. Implementar estrategias de almacenamiento en caché.
3. Optimizar los formatos de datos.
4. Usar **Instancias Spot**.
5. Monitoreo continuo de costos.

**Desglose de Costos (Mensual)**:

| Servicio | Configuración | Costo Mensual |
|----------|---------------|---------------|
| **Kinesis Data Streams** | 50 fragmentos, 5TB de ingesta | $1,200 |
| **Kinesis Firehose** | 5TB de entrega | $125 |
| **S3 Storage** | 100TB (por niveles) | $2,000 |
| **AWS Glue** | 200 horas-DPU de **ETL** | $880 |
| **Amazon EMR** | 25 nodos, 8 hrs/día, 70% **Spot** | $2,400 |
| **Redshift** | 5 nodos **ra3.4xlarge** | $12,000 |
| **Athena** | 10TB escaneados/mes | $50 |
| **QuickSight** | 50 autores, 500 lectores | $400 |
| **SageMaker** | Entrenamiento + endpoints | $1,500 |
| **Lambda** | 50M de invocaciones | $100 |
| **Transferencia de Datos** | Salida (**Outbound**) | $500 |
| **CloudWatch** | Registros y métricas | $300 |
| **Total** | | **~$21,455/mes** |

**Estrategias de Optimización de Costos**:
1. Usar **Instancias Spot** para **EMR** (70% de ahorro).
2. Convertir los datos a **Parquet** (90% de reducción de almacenamiento).
3. Particionar los datos de forma eficaz (80% de reducción de costos de consulta).
4. Usar **S3 Intelligent-Tiering**.
5. Ajustar el tamaño de **Redshift** con pausa/reanudación.
6. Usar **Athena** para consultas poco frecuentes vs. **Redshift**.
7. Implementar políticas de ciclo de vida de **S3**.

**Costo Optimizado**: ~$14,000/mes (reducción del 35%).

**Beneficios**:
- **Información en Tiempo Real**: Latencia de <1 minuto para decisiones operativas.
- **Ahorro de Costos**: $500,000/año (herramientas de terceros) → $168,000/año (50% de ahorro).
- **Escalabilidad**: Maneja un crecimiento de datos de 10 veces sin cambios arquitectónicos.
- **Autoservicio**: Los usuarios de negocio ejecutan sus propias consultas.
- **Decisiones Basadas en Datos**: Las recomendaciones impulsadas por **ML** aumentan los ingresos en un 15%.
- **Tiempo para Obtener Información**: 2-3 días → <1 hora para los informes.
- **Cumplimiento**: Control de acceso de grano fino y pistas de auditoría.

**Compromisos (Trade-offs)**:
- **Complejidad**: Los sistemas distribuidos requieren un equipo capacitado.
- **Curva de Aprendizaje**: Se requiere capacitación para **Spark**, **SQL**, **ML**.
- **Costo Inicial**: Mayor inversión inicial.
- **Calidad de los Datos**: Si entra basura, sale basura; se necesitan verificaciones de calidad sólidas.

**Enfoques Alternativos**:

**Opción 1: Centrado en Redshift**
- Cargar todos los datos en **Redshift**.
- Arquitectura más simple.
- Mayor costo de almacenamiento.
- El mejor para: Conjuntos de datos más pequeños (<10TB).

**Opción 2: Centrado en EMR**
- Usar **EMR** para todo el procesamiento.
- Más control y flexibilidad.
- Mayor gasto operativo.
- El mejor para: Equipos con experiencia en **Hadoop**/**Spark**.

**Opción 3: Terceros (Snowflake, Databricks)**
- Servicios administrados.
- Excelente rendimiento.
- Costo más alto.
- Menos control.
- El mejor para: Equipos que deseen una carga operativa mínima.

**Errores Comunes a Evitar**:
1. **No particionar los datos**: Da como resultado consultas lentas y costos elevados.
2. **Ignorar los formatos de datos**: **CSV** vs. **Parquet** marca una diferencia de 10 veces.
3. **Sobre-aprovisionamiento**: Comience pequeño, escale según sea necesario.
4. **Sin gobernanza de datos**: Implemente controles de acceso desde el primer día.
5. **Ignorar la calidad de los datos**: Integre la validación en los pipelines.
6. **No usar Instancias Spot**: 70% de ahorro de costos para **EMR**.
7. **Almacenar todo en Redshift**: Use el lago de datos de **S3** + **Redshift Spectrum**.
8. **Sin monitoreo**: Implemente alarmas y cuadros de mando de **CloudWatch** desde el principio.

---

### Escenario 12: Implementación de Pipeline CI/CD de DevOps

**Situación**: Una empresa de **SaaS** con 20 desarrolladores tiene problemas con los procesos de despliegue manuales. El código se despliega en producción una vez al mes, con frecuentes reversiones (**rollbacks**) debido a errores. Los despliegues tardan entre 4 y 6 horas y requieren pasos manuales. El equipo quiere implementar prácticas modernas de **DevOps** con pipelines de **CI/CD** automatizados.

**Estado Actual**:
- Despliegues manuales a través de **SSH** y scripts.
- Sin pruebas automatizadas.
- Frecuencia de despliegue: Mensual.
- Duración del despliegue: 4-6 horas.
- Tasa de reversión: 30%.
- Incidentes en producción: 2-3 por mes.
- Frustración de los desarrolladores: Alta.
- Tiempo de comercialización (**Time to market**): 4-6 semanas para las funciones.

**Proceso Actual**:
1. Los desarrolladores envían cambios (**commit**) a una rama de Git compartida.
2. Revisión de código manual (informal).
3. El equipo de **QA** realiza pruebas durante 1 semana.
4. El equipo de operaciones despliega los fines de semana.
5. Frecuentes problemas en producción los lunes.

**Problemas**:
- Bucles de retroalimentación largos.
- Despliegues manuales propensos a errores.
- Sin consistencia en el despliegue.
- Reversiones difíciles.
- Miedo a desplegar.
- Cuello de botella en el equipo de operaciones.

**Requisitos**:

**Requisitos Funcionales**:
- Construcción (**build**) y pruebas automatizadas en cada **commit**.
- Despliegue automatizado a **Dev**/**Staging**/**Prod**.
- Verificaciones de calidad de código (**linting**, escaneo de seguridad).
- Capacidad de reversión automatizada.
- Infraestructura como Código (**IaC**).
- Gestión de secretos.
- Soporte para múltiples entornos.

**Requisitos No Funcionales**:
- Frecuencia de despliegue: Varias veces al día.
- Duración del despliegue: <15 minutos.
- Reversión automatizada: <5 minutos.
- Tasa de reversión: <5%.
- Despliegues sin tiempo de inactividad (**zero-downtime**).
- Pista de auditoría para el cumplimiento normativo.
- Rentable.

**Pregunta**: ¿Cómo deberían implementar un pipeline de **CI/CD** moderno?

**Arquitectura de CI/CD Recomendada**:

#### 1. Control de Origen y Estrategia de Ramificación

**AWS CodeCommit**:
- Control de origen basado en Git.
- Integración con los servicios de AWS.
- Cifrado en reposo y en tránsito.
- Control de acceso basado en **IAM**.
- Soporta **Git LFS** para archivos grandes.
- Flujos de trabajo de solicitudes de extracción (**Pull Requests**).

**Alternativa**: **GitHub**, **GitLab**, **Bitbucket**.
- Si ya están usando estas plataformas.
- **CodePipeline** se integra con todas ellas.

**Estrategia de Ramificación (Desarrollo Basado en el Tronco - Trunk-Based Development)**:
```
main (producción)
  ├── feature/user-auth (vida corta)
  ├── feature/payment-integration (vida corta)
  └── hotfix/critical-bug (vida corta)
```

**Directrices de Trunk-Based**:
- **Commits** pequeños y frecuentes a la rama **main**.
- Marcadores de funciones (**feature flags**) para funciones incompletas.
- Ramas de funciones de vida corta (<2 días).
- Solicitudes de extracción con verificaciones automatizadas.
- Fusionar (**merge**) solo si las pruebas pasan.

#### 2. Pipeline de Integración Continua (CI)

**AWS CodeBuild**:
- Servicio de construcción totalmente administrado.
- Entornos de construcción basados en **Docker**.
- Pago por minuto de construcción.
- Escala automáticamente.
- Integración con herramientas de escaneo de seguridad.

**Especificación de Construcción (buildspec.yml)**:
```yaml
version: 0.2

phases:
  install:
    runtime-versions:
      nodejs: 18
      docker: 20
    commands:
      - echo Instalando dependencias...
      - npm install

  pre_build:
    commands:
      - echo Ejecutando verificaciones previas a la construcción...
      - npm run lint
      - npm run security-check
      - echo Iniciando sesión en Amazon ECR...
      - aws ecr get-login-password --region $AWS_DEFAULT_REGION | docker login --username AWS --password-stdin $AWS_ACCOUNT_ID.dkr.ecr.$AWS_DEFAULT_REGION.amazonaws.com

  build:
    commands:
      - echo La compilación comenzó el `date`
      - echo Ejecutando pruebas unitarias...
      - npm test -- --coverage
      - echo Construyendo la aplicación...
      - npm run build
      - echo Construyendo la imagen de Docker...
      - docker build -t $IMAGE_REPO_NAME:$CODEBUILD_RESOLVED_SOURCE_VERSION .
      - docker tag $IMAGE_REPO_NAME:$CODEBUILD_RESOLVED_SOURCE_VERSION $AWS_ACCOUNT_ID.dkr.ecr.$AWS_DEFAULT_REGION.amazonaws.com/$IMAGE_REPO_NAME:$CODEBUILD_RESOLVED_SOURCE_VERSION
      - docker tag $IMAGE_REPO_NAME:$CODEBUILD_RESOLVED_SOURCE_VERSION $AWS_ACCOUNT_ID.dkr.ecr.$AWS_DEFAULT_REGION.amazonaws.com/$IMAGE_REPO_NAME:latest

  post_build:
    commands:
      - echo La compilación se completó el `date`
      - echo Enviando la imagen de Docker...
      - docker push $AWS_ACCOUNT_ID.dkr.ecr.$AWS_DEFAULT_REGION.amazonaws.com/$IMAGE_REPO_NAME:$CODEBUILD_RESOLVED_SOURCE_VERSION
      - docker push $AWS_ACCOUNT_ID.dkr.ecr.$AWS_DEFAULT_REGION.amazonaws.com/$IMAGE_REPO_NAME:latest
      - echo Generando artefactos de construcción...
      - printf '[{"name":"app-container","imageUri":"%s"}]' $AWS_ACCOUNT_ID.dkr.ecr.$AWS_DEFAULT_REGION.amazonaws.com/$IMAGE_REPO_NAME:$CODEBUILD_RESOLVED_SOURCE_VERSION > imagedefinitions.json

artifacts:
  files:
    - imagedefinitions.json
    - appspec.yml
    - taskdef.json
    - '**/*'
  discard-paths: no

reports:
  test-results:
    files:
      - 'test-results/**/*'
    file-format: 'JUNITXML'
  coverage-report:
    files:
      - 'coverage/clover.xml'
    file-format: 'CLOVERXML'

cache:
  paths:
    - '/root/.npm/**/*'
    - 'node_modules/**/*'
```

**Verificaciones Automatizadas en CI**:
1. **Pruebas Unitarias**: **Jest**, **Mocha**, **pytest**.
2. **Cobertura de Código**: Umbral mínimo del 80%.
3. **Linting**: **ESLint**, **Prettier**, **Black**.
4. **Escaneo de Seguridad**:
   - **Snyk** para vulnerabilidades de dependencias.
   - **OWASP Dependency-Check**.
   - **SonarQube** para la calidad del código.
5. **Escaneo de Contenedores**: Escaneo de imágenes de **Amazon ECR**.
6. **Validación de Infraestructura**: **cfn-lint**, **terraform validate**.

#### 3. Pipeline de Despliegue Continuo (CD)

**AWS CodePipeline**:
- Orquesta el flujo de trabajo de **CI/CD**.
- Editor visual de pipelines.
- Integración con herramientas de terceros.
- Etapas paralelas y secuenciales.
- Puertas de aprobación (**approval gates**).
- Reversión automatizada.

**Etapas del Pipeline**:

```
┌─────────────────────────────────────────────────────────────────┐
│                       ETAPA DE ORIGEN                           │
│  - Activador de CodeCommit/GitHub al enviar a main              │
│  - Obtener el código fuente                                     │
└────────────────────┬────────────────────────────────────────────┘
                     │
                     ▼
┌─────────────────────────────────────────────────────────────────┐
│                    ETAPA DE CONSTRUCCIÓN                        │
│  - CodeBuild compila, prueba y construye la imagen de Docker    │
│  - Enviar a ECR                                                 │
│  - Generar artefactos                                           │
└────────────────────┬────────────────────────────────────────────┘
                     │
                     ▼
┌─────────────────────────────────────────────────────────────────┐
│                      ETAPA DE PRUEBA (Dev)                      │
│  - Desplegar en el entorno de Dev (ECS/EKS)                     │
│  - CodeBuild: Pruebas de integración                            │
│  - CodeBuild: Pruebas de API (Postman/Newman)                   │
│  - CodeBuild: Pruebas de rendimiento (k6, JMeter)               │
└────────────────────┬────────────────────────────────────────────┘
                     │
                     ▼
┌─────────────────────────────────────────────────────────────────┐
│                  ETAPA DE DESPLIEGUE (Staging)                  │
│  - CodeDeploy al entorno de Staging                             │
│  - Despliegue Blue/green                                        │
│  - Pruebas de humo (Smoke tests)                                │
└────────────────────┬────────────────────────────────────────────┘
                     │
                     ▼
┌─────────────────────────────────────────────────────────────────┐
│                        APROBACIÓN MANUAL                        │
│  - Notificación por SNS a los aprobadores                       │
│  - Revisar los resultados de las pruebas                        │
│  - Aprobar o rechazar el despliegue a producción                │
└────────────────────┬────────────────────────────────────────────┘
                     │
                     ▼
┌─────────────────────────────────────────────────────────────────┐
│                 ETAPA DE DESPLIEGUE (Production)                │
│  - CodeDeploy a producción                                      │
│  - Despliegue Blue/green                                        │
│  - Cambio de tráfico: 10% → 50% → 100%                          │
│  - Reversión automática ante alarmas de CloudWatch              │
└─────────────────────────────────────────────────────────────────┘
```

#### 4. Estrategia de Despliegue

**AWS CodeDeploy**:
- Despliegues automatizados.
- Múltiples tipos de despliegue:
  - En el sitio (**In-place**).
  - Azul/Verde (**Blue/green**).
  - Canario (**Canary**).
  - Lineal.
- Reversión automática.
- Integración con **ECS**, **Lambda**, **EC2**, instalaciones locales (**on-premises**).

**Despliegue Blue/Green (ECS)**:

**Archivo AppSpec (appspec.yml)**:
```yaml
version: 0.0
Resources:
  - TargetService:
      Type: AWS::ECS::Service
      Properties:
        TaskDefinition: <TASK_DEFINITION>
        LoadBalancerInfo:
          ContainerName: "app-container"
          ContainerPort: 8080
        PlatformVersion: "LATEST"
        NetworkConfiguration:
          AwsvpcConfiguration:
            Subnets:
              - subnet-12345678
              - subnet-87654321
            SecurityGroups:
              - sg-12345678
            AssignPublicIp: "DISABLED"

Hooks:
  - BeforeInstall: "LambdaFunctionToValidateBeforeInstall"
  - AfterInstall: "LambdaFunctionToValidateAfterInstall"
  - AfterAllowTestTraffic: "LambdaFunctionToRunIntegrationTests"
  - BeforeAllowTraffic: "LambdaFunctionToWarmUpCache"
  - AfterAllowTraffic: "LambdaFunctionToValidateProduction"
```

**Estrategia de Cambio de Tráfico**:
- **Canary**: 10% del tráfico durante 5 minutos, luego el 100%.
- **Linear**: Aumento del 10% cada 5 minutos.
- **All-at-once**: Cambio inmediato (no recomendado para producción).

**Activadores de Reversión Automática**:
- Alarma de **CloudWatch**: Tasa de error >5%.
- Alarma de **CloudWatch**: Tiempo de respuesta >2 segundos.
- Alarma de **CloudWatch**: Utilización de CPU >80%.
- Fallo en el despliegue.

#### 5. Infraestructura como Código (IaC)

**AWS CloudFormation**:
- Definir la infraestructura en **YAML**/**JSON**.
- Control de versiones de la infraestructura.
- Actualizaciones de la pila con reversión.
- Detección de desviaciones (**Drift detection**).

**Alternativa: AWS CDK (Cloud Development Kit)**:
- Definir la infraestructura en lenguajes de programación.
- **Python**, **TypeScript**, **Java**, **C#**.
- Abstracciones de nivel superior.
- Sintetiza a **CloudFormation**.

**Ejemplo de Pila de CDK (TypeScript)**:
```typescript
import * as cdk from 'aws-cdk-lib';
import * as ec2 from 'aws-cdk-lib/aws-ec2';
import * as ecs from 'aws-cdk-lib/aws-ecs';
import * as ecsPatterns from 'aws-cdk-lib/aws-ecs-patterns';
import * as codedeploy from 'aws-cdk-lib/aws-codedeploy';

export class AppInfraStack extends cdk.Stack {
  constructor(scope: cdk.App, id: string, props?: cdk.StackProps) {
    super(scope, id, props);

    // VPC
    const vpc = new ec2.Vpc(this, 'AppVPC', {
      maxAzs: 3,
      natGateways: 2
    });

    // Clúster de ECS
    const cluster = new ecs.Cluster(this, 'AppCluster', {
      vpc: vpc,
      containerInsights: true
    });

    // Fargate Service with ALB
    const fargateService = new ecsPatterns.ApplicationLoadBalancedFargateService(
      this,
      'AppService',
      {
        cluster: cluster,
        cpu: 512,
        desiredCount: 3,
        taskImageOptions: {
          image: ecs.ContainerImage.fromRegistry('amazon/amazon-ecs-sample'),
          containerPort: 8080,
          environment: {
            ENVIRONMENT: 'production'
          }
        },
        memoryLimitMiB: 1024,
        publicLoadBalancer: true,
        deploymentController: {
          type: ecs.DeploymentControllerType.CODE_DEPLOY
        }
      }
    );

    // Auto Scaling
    const scaling = fargateService.service.autoScaleTaskCount({
      minCapacity: 3,
      maxCapacity: 20
    });

    scaling.scaleOnCpuUtilization('CpuScaling', {
      targetUtilizationPercent: 70
    });

    scaling.scaleOnMemoryUtilization('MemoryScaling', {
      targetUtilizationPercent: 80
    });

    // CodeDeploy Deployment Group
    const deploymentGroup = new codedeploy.EcsDeploymentGroup(
      this,
      'AppDeploymentGroup',
      {
        service: fargateService.service,
        blueGreenDeploymentConfig: {
          blueTargetGroup: fargateService.targetGroup,
          greenTargetGroup: fargateService.targetGroup,
          listener: fargateService.listener,
          terminationWaitTime: cdk.Duration.minutes(5)
        },
        deploymentConfig: codedeploy.EcsDeploymentConfig.CANARY_10PERCENT_5MINUTES,
        autoRollback: {
          failedDeployment: true,
          stoppedDeployment: true,
          deploymentInAlarm: true
        },
        alarms: [
          new cloudwatch.Alarm(this, 'ErrorAlarm', {
            metric: fargateService.targetGroup.metrics.httpCodeTarget(
              elb.HttpCodeTarget.TARGET_5XX_COUNT
            ),
            threshold: 10,
            evaluationPeriods: 2
          })
        ]
      }
    );
  }
}
```

#### 6. Secrets Management

**AWS Secrets Manager**:
- Store database credentials, API keys
- Automatic rotation
- Encryption with KMS
- Fine-grained access control
- Integration with RDS, Redshift

**Alternative: AWS Systems Manager Parameter Store**:
- Free for standard parameters
- Hierarchical storage
- No automatic rotation
- Good for configuration values
**Ejemplo de Uso en una Tarea de ECS**:
```json
{
  "containerDefinitions": [
    {
      "name": "app-container",
      "secrets": [
        {
          "name": "DB_PASSWORD",
          "valueFrom": "arn:aws:secretsmanager:us-east-1:123456789012:secret:prod/db/password-AbCdEf"
        },
        {
          "name": "API_KEY",
          "valueFrom": "arn:aws:secretsmanager:us-east-1:123456789012:secret:prod/api/key-XyZaBc"
        }
      ]
    }
  ]
}
```

#### 7. Monitoreo y Observabilidad

**Amazon CloudWatch**:
- Registro de logs unificado desde todos los entornos.
- Métricas personalizadas.
- Cuadros de mando para el estado del pipeline.
- Alarmas para fallos en el despliegue.

**AWS X-Ray**:
- Rastreo distribuido (**Distributed tracing**).
- Identificar cuellos de botella en el rendimiento.
- Visualización del flujo de solicitudes.

**Métricas Clave a Monitorear**:
- **Frecuencia de Despliegue**: Despliegues por día.
- **Tiempo de Espera (Lead Time)**: Tiempo desde el **commit** hasta producción.
- **Tiempo Medio de Recuperación (MTTR)**: Tiempo para solucionar un problema en producción.
- **Tasa de Fallos por Cambio**: % de despliegues que causan fallos.
- **Duración de la Construcción**: Tiempo del pipeline de construcción.
- **Cobertura de Pruebas**: % de código cubierto por pruebas.

**Cuadro de Mando de CloudWatch**:
```json
{
  "widgets": [
    {
      "type": "metric",
      "properties": {
        "metrics": [
          [ "AWS/CodePipeline", "PipelineExecutionSuccess", { "stat": "Sum" } ],
          [ ".", "PipelineExecutionFailure", { "stat": "Sum" } ]
        ],
        "period": 300,
        "stat": "Sum",
        "region": "us-east-1",
        "title": "Ejecuciones del Pipeline"
      }
    },
    {
      "type": "metric",
      "properties": {
        "metrics": [
          [ "AWS/ECS", "CPUUtilization", { "stat": "Average" } ],
          [ ".", "MemoryUtilization", { "stat": "Average" } ]
        ],
        "period": 300,
        "stat": "Average",
        "region": "us-east-1",
        "title": "Utilización de Recursos de ECS"
      }
    }
  ]
}
```

#### 8. Estrategia Multientorno

**Estructura de Cuentas**:
- Cuenta de **Dev**: Despliegues frecuentes, recursos de menor costo.
- Cuenta de **Staging**: Entorno similar al de producción.
- Cuenta de **Production**: Control de cambios estricto.

**Configuración Específica del Entorno**:
```
config/
  ├── dev.json
  ├── staging.json
  └── production.json
```

**Jerarquía del Parameter Store**:
```
/app/dev/database/host
/app/dev/database/port
/app/staging/database/host
/app/staging/database/port
/app/production/database/host
/app/production/database/port
```

#### 9. Estrategia de Pruebas

**Pirámide de Pruebas**:
```
         ┌─────────────┐
         │ Pruebas E2E │  ← Pocas, lentas, costosas
         │  (Cypress)  │
         └─────────────┘
       ┌─────────────────┐
       │   Pruebas de    │
       │   integración   │
       │ (API, Base de   │
       │     datos)      │
       └─────────────────┘
    ┌──────────────────────┐
    │  Pruebas unitarias   │  ← Muchas, rápidas, baratas
    │ (Jest, pytest, JUnit)│
    └──────────────────────┘
```

**Tipos de Pruebas**:
1. **Pruebas Unitarias**: 80% de las pruebas, <100ms cada una.
2. **Pruebas de Integración**: 15% de las pruebas, <5s cada una.
3. **Pruebas E2E**: 5% de las pruebas, <30s cada una.

**Etapa de Prueba de CodeBuild**:
```yaml
phases:
  build:
    commands:
      - echo Ejecutando pruebas unitarias...
      - npm test -- --coverage --maxWorkers=4
      - echo Ejecutando pruebas de integración...
      - npm run test:integration
      - echo Ejecutando pruebas E2E...
      - npm run test:e2e

  post_build:
    commands:
      - echo Comprobando el umbral de cobertura de pruebas...
      - npm run coverage:check -- --lines 80 --functions 80 --branches 75
```

#### 10. Estrategia de Reversión (Rollback)

**Reversión Automática**:
- Las alarmas de **CloudWatch** activan la reversión.
- **CodeDeploy** vuelve automáticamente a la versión anterior.
- <5 minutos para revertir.

**Reversión Manual**:
```bash
# Revertir al despliegue anterior
aws deploy stop-deployment \
  --deployment-id d-1234567890 \
  --auto-rollback-enabled

# O volver a desplegar la versión anterior
aws ecs update-service \
  --cluster my-cluster \
  --service my-service \
  --task-definition my-task:42  # Versión anterior
```

**Marcadores de Funciones (Feature Flags)**:
- Usar **AWS AppConfig** o **Launch Darkly**.
- Alternar funciones sin necesidad de volver a desplegar.
- Despliegue gradual para los usuarios.
- Desactivación rápida si surgen problemas.

**Implementación Paso a Paso**:

**Fase 1: Control de Origen (Semana 1)**
1. Migrar el código a **CodeCommit**/**GitHub**.
2. Definir la estrategia de ramificación.
3. Configurar los flujos de trabajo de solicitudes de extracción.
4. Configurar las reglas de protección de ramas.
5. Capacitar al equipo en las mejores prácticas de **Git**.

**Fase 2: Pipeline de CI (Semana 2)**
1. Crear el archivo `buildspec.yml`.
2. Configurar el proyecto de **CodeBuild**.
3. Integrar el **linting** y las pruebas.
4. Añadir escaneo de seguridad.
5. Configurar las notificaciones de construcción.

**Fase 3: Contenerización (Semana 3)**
1. Crear el `Dockerfile`.
2. Configurar **Amazon ECR**.
3. Construir las imágenes de contenedor en el pipeline.
4. Probar localmente con **Docker Compose**.
5. Documentar la configuración del contenedor.

**Fase 4: Infraestructura como Código (Semana 4)**
1. Definir la infraestructura en **CloudFormation**/**CDK**.
2. Crear **VPC**, subredes y grupos de seguridad.
3. Desplegar el clúster de **ECS** y los servicios.
4. Configurar el **Application Load Balancer**.
5. Probar el aprovisionamiento de la infraestructura.

**Fase 5: Pipeline de CD (Semana 5)**
1. Crear el **CodePipeline**.
2. Añadir etapas de despliegue (**Dev**, **Staging**, **Prod**).
3. Configurar **CodeDeploy**.
4. Configurar las puertas de aprobación.
5. Probar el despliegue de extremo a extremo.

**Fase 6: Monitoreo (Semana 6)**
1. Configurar los cuadros de mando de **CloudWatch**.
2. Configurar las alarmas.
3. Integrar el rastreo con **X-Ray**.
4. Configurar la agregación de registros (**logs**).
5. Definir **KPIs** y métricas.

**Fase 7: Pruebas y Optimización (Semanas 7-8)**
1. Probar escenarios de fallo.
2. Practicar los procedimientos de reversión.
3. Optimizar los tiempos de construcción.
4. Ajustar los parámetros de escalado automático.
5. Documentar los manuales de procedimientos (**runbooks**).

**Desglose de Costos (Mensual)**:

| Servicio | Configuración | Costo Mensual |
|----------|---------------|---------------|
| **CodeCommit** | 5 usuarios activos, 10 GB | $2 |
| **CodeBuild** | 500 minutos de construcción (**general1.small**) | $25 |
| **CodePipeline** | 10 pipelines, 200 ejecuciones | $10 |
| **CodeDeploy** | Gratuito para **ECS**, **Lambda** | $0 |
| **ECR** | 50 GB de almacenamiento | $5 |
| **ECS Fargate** | 6 tareas (0.5 vCPU, 1 GB) | $140 |
| **ALB** | 2 equilibradores de carga | $40 |
| **S3** | Almacenamiento de artefactos | $5 |
| **CloudWatch** | Registros, métricas, cuadros de mando | $50 |
| **Secrets Manager** | 20 secretos | $8 |
| **Total** | | **~$285/mes** |

**Beneficios**:
- **Frecuencia de Despliegue**: Mensual → Varias veces al día.
- **Duración del Despliegue**: 4-6 horas → <15 minutos.
- **Tiempo de Reversión**: Horas → <5 minutos.
- **Tasa de Reversión**: 30% → <5%.
- **Productividad de los Desarrolladores**: +40% (menos tiempo en despliegues).
- **Tiempo Medio de Recuperación**: 4 horas → <30 minutos.
- **Incidentes en Producción**: 2-3/mes → <1/mes.
- **Tiempo de Comercialización**: 4-6 semanas → 1-2 semanas.
- **Satisfacción de los Desarrolladores**: Significativamente mejorada.

**Compromisos (Trade-offs)**:
- **Configuración Inicial**: De 6 a 8 semanas para la implementación completa.
- **Curva de Aprendizaje**: El equipo debe aprender nuevas herramientas y prácticas.
- **Cambio Cultural**: Transición de procesos manuales a automatizados.
- **Cambio de Responsabilidades**: Los desarrolladores están más involucrados en las operaciones.

**Enfoques Alternativos**:

**Opción 1: Jenkins en EC2**
- **CI/CD** de código abierto.
- Más complementos (**plugins**) y flexibilidad.
- Requiere la gestión de servidores.
- Mayor gasto operativo.
- El mejor para: Equipos con experiencia previa en **Jenkins**.

**Opción 2: GitHub Actions**
- Integración nativa con **GitHub**.
- Fácil de configurar.
- Integración limitada con AWS en comparación con **CodePipeline**.
- El mejor para: Flujos de trabajo centrados en **GitHub**.

**Opción 3: GitLab CI/CD**
- Plataforma de **DevOps** todo en uno.
- Registro de contenedores integrado.
- Requiere alojamiento independiente.
- El mejor para: Equipos que deseen una única plataforma.

**Opción 4: Terceros (CircleCI, Travis CI)**
- Fácil de configurar.
- Gran experiencia para el desarrollador.
- Costo adicional.
- Control limitado.
- El mejor para: Startups que buscan una configuración rápida.

**Errores Comunes a Evitar**:
1. **Sin pruebas de reversión**: Practique las reversiones con regularidad.
2. **Omitir las pruebas de integración**: Detecte problemas antes de llegar a producción.
3. **Pasos manuales en el pipeline**: Automatice todo.
4. **Ignorar los tiempos de construcción**: Optimice para construcciones de menos de 10 minutos.
5. **Sin monitoreo**: Implemente un monitoreo integral desde el principio.
6. **Sobre-ingeniería**: Empiece de forma sencilla y añada complejidad según sea necesario.
7. **Ignorar la seguridad**: Busque vulnerabilidades en el pipeline.
8. **Sin documentación**: Documente la arquitectura y los manuales de procedimientos (**runbooks**).
9. **Olvidar las notificaciones**: Alerte al equipo sobre fallos en el pipeline.
10. **No medir**: Realice un seguimiento de las métricas **DORA** (frecuencia de despliegue, tiempo de espera, **MTTR**, tasa de fallos por cambio).

---

## Escenarios Comunes de Resolución de Problemas

### No se puede conectar a la instancia de EC2

**Síntomas**: El tiempo de espera de la conexión **SSH** o **RDP** se agota o la conexión es rechazada.

**Pasos de Resolución de Problemas**:

#### 1. Verificar el Estado de la Instancia
- Compruebe que el estado de la instancia sea "running" (en ejecución).
- Compruebe que las comprobaciones de estado (**status checks**) se estén superando.
- Vea el registro del sistema en busca de errores de arranque.

#### 2. Comprobar el Grupo de Seguridad (Security Group)
- Asegúrese de que la regla de entrada permita **SSH** (puerto 22) o **RDP** (puerto 3389).
- Verifique que la **IP** de origen esté permitida (0.0.0.0/0 o su **IP** específica).
- Compruebe si el grupo de seguridad cambió recientemente.

#### 3. Comprobar la ACL de Red (NACL)
- Asegúrese de que la **NACL** permita el tráfico de entrada en el puerto correspondiente.
- Asegúrese de que la **NACL** permita los puertos de salida efímeros (1024-65535).
- **Importante**: ¡Las **NACLs** no tienen estado (**stateless**)!

#### 4. Verificar la Configuración de Red
- La instancia tiene una **IP** pública (si se conecta desde Internet).
- La instancia está en una subred pública (tiene una ruta hacia un **IGW**).
- O se está utilizando un **bastion host** para la subred privada.

#### 5. Comprobar el Par de Claves (Key Pair)
- Está utilizando el archivo `.pem`/`.ppk` correcto.
- Los permisos del archivo son correctos (`chmod 400` para `.pem`).
- El par de claves coincide con el de la instancia.

#### 6. Comprobar la Tabla de Rutas (Route Table)
- La subred tiene una ruta hacia el **IGW** (0.0.0.0/0 → **igw-xxx**).
- O una ruta hacia un **NAT Gateway** para la subred privada.

> **Consejo**: Utilice **EC2 Instance Connect** o **Systems Manager Session Manager** como alternativas a **SSH**/**RDP** cuando solucione problemas de conectividad.

---

### Errores de "Acceso Denegado" (Access Denied) en S3

**Causas y Soluciones Comunes**:

#### 1. Permisos de IAM
- Verifique que la política de **IAM** otorgue permisos como `s3:GetObject` o `s3:PutObject`.
- Compruebe si hay declaraciones de denegación explícita (**explicit Deny**).
- Verifique que el **ARN** del recurso en la política coincida con el del bucket.

#### 2. Política de Bucket (Bucket Policy)
- Compruebe que la política del bucket no deniegue el acceso.
- Verifique el **Principal** en la política.
- Compruebe si hay restricciones basadas en la **IP**.

#### 3. Bloqueo de Acceso Público (Block Public Access)
- Si se necesita acceso público, desactive el **Block Public Access**.
- Verifique la configuración tanto a nivel de bucket como a nivel de cuenta.

#### 4. Cifrado
- Si utiliza **SSE-KMS**, verifique la política de la clave de **KMS**.
- Asegúrese de que el usuario tenga el permiso `kms:Decrypt`.

#### 5. Acceso entre Cuentas (Cross-Account Access)
- La política del bucket debe permitir el acceso entre cuentas.
- Asuma el rol con los permisos correctos.

> **Consejo de depuración**: Utilice **AWS CloudTrail** para revisar la llamada a la **API** y ver el motivo exacto de la denegación de acceso. Busque los campos `errorCode` y `errorMessage` en los registros de **CloudTrail**.

---

### Problemas con Funciones Lambda

#### Problema 1: La Función Agota el Tiempo de Espera (Timeout)

**Soluciones**:
- Aumente el tiempo de espera (por defecto 3 seg, máx 15 min).
- Optimice el rendimiento del código.
- Compruebe la configuración de la **VPC** (puede añadir latencia).
- Aumente la memoria (también aumenta la **CPU**).
- Investigue los retrasos por inicio en frío (**cold start**).

> **Mejor Práctica**: Establezca el tiempo de espera ligeramente por encima del tiempo de ejecución esperado, pero no innecesariamente alto para evitar ejecuciones fallidas prolongadas.

#### Problema 2: Permisos Insuficientes

**Soluciones**:
- Compruebe que el rol de ejecución de **Lambda** tenga los permisos necesarios.
- Revise los registros de **CloudWatch Logs** en busca de errores de permisos.
- Añada las políticas de **IAM** necesarias al rol de ejecución.
- Para **VPC**: Asegúrese de que el rol tenga permisos de ejecución en la **VPC**.

**Permisos Comunes Requeridos**:
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "logs:CreateLogGroup",
        "logs:CreateLogStream",
        "logs:PutLogEvents"
      ],
      "Resource": "arn:aws:logs:*:*:*"
    }
  ]
}
```

- Use SQS to buffer requests
- Consider reserved concurrency for critical functions

**Understanding Throttling**:
- **Account-level limit**: 1,000 concurrent executions per region (default)
- **Function-level limit**: Can be set with reserved concurrency
- **Unreserved pool**: Shared across all functions without reserved concurrency

> **Tip**: Monitor the `ConcurrentExecutions` and `Throttles` metrics in CloudWatch to identify throttling issues early.

---

### RDS Connection Problems

**Symptoms**: Cannot connect to RDS database from application

**Troubleshooting Steps**:

#### 1. Verify Endpoint and Port
- Check endpoint hostname is correct
- Default ports: MySQL (3306), PostgreSQL (5432), SQL Server (1433)
- Verify database is available (not stopped or in maintenance)

#### 2. Security Group Configuration
- Inbound rule must allow traffic on database port
- Source should be application security group or IP range
- Example: MySQL on 3306 from application SG

#### 3. Network Accessibility
- **Public Accessibility**: Set to Yes if connecting from internet
- **Private Subnet**: Application must be in same VPC or have connectivity
- **VPC Peering/VPN**: Required for cross-VPC or on-premises access

#### 4. Database Credentials
- Verify username and password
- Check if password has special characters needing escaping
- Master user vs. database-specific users
- For Aurora, use cluster endpoint for writes, reader endpoint for reads

#### 5. Network ACLs
- Check subnet NACL allows traffic on database port
- Both inbound and outbound rules needed (stateless)

#### 6. SSL/TLS Requirements
- Some databases require SSL connections
- Download RDS certificate bundle
- Configure application to use SSL

#### 7. Connection Limits
- RDS has maximum connections based on instance class
- MySQL: `{DBInstanceClassMemory/12582880}`
- Check CloudWatch `DatabaseConnections` metric
- If maxed out, scale up instance or optimize connection pooling

**Testing Connection**:
```bash
# From EC2 instance in same VPC
# MySQL
mysql -h mydb.abc123.us-east-1.rds.amazonaws.com -P 3306 -u admin -p

# PostgreSQL
psql -h mydb.abc123.us-east-1.rds.amazonaws.com -p 5432 -U admin -d mydb

# Test connectivity
telnet mydb.abc123.us-east-1.rds.amazonaws.com 3306
```

**Common Solutions**:
- Add application security group to RDS security group inbound rules
- Enable public accessibility (for testing only, not production)
- Check VPC routing and internet gateway configuration
- Verify database is in same VPC as application
- Use AWS Systems Manager Session Manager to connect to EC2, then test RDS connection

---

### CloudFormation Stack Failures

**Symptoms**: CloudFormation stack creation or update fails, rolls back

**Common Failure Reasons**:

#### 1. Insufficient IAM Permissions
**Error**: `User is not authorized to perform: [action]`

**Solutions**:
- User/role needs permissions for all resources being created
- CloudFormation also needs permissions via service role
- Add required permissions to IAM policy
- Use CloudFormation service role with necessary permissions

**Example IAM Policy**:
```json
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "cloudformation:*",
        "ec2:*",
        "iam:*",
        "s3:*"
      ],
      "Resource": "*"
    }
  ]
}
```

#### 2. Límites de Recursos Excedidos
**Error**: `LimitExceeded` o `ResourceLimitExceeded`

**Soluciones**:
- Compruebe las cuotas de servicio (**VPCs**, **Elastic IPs**, instancias de **EC2**).
- Solicite un aumento del límite a través de la consola de **Service Quotas**.
- Utilice recursos existentes en lugar de crear otros nuevos.
- Despliegue en una región diferente con capacidad disponible.

#### 3. Errores de Validación de Parámetros
**Error**: `Parameters: [parameter] must match pattern [regex]`

**Soluciones**:
- Verifique que los valores de los parámetros coincidan con las restricciones.
- Compruebe `AllowedValues`, `MinLength` y `MaxLength`.
- Asegúrese de que los bloques **CIDR** no se solapen.
- Valide que los **IDs** de **AMI** existan en la región de destino.

#### 4. El Recurso ya Existe
**Error**: `Resource already exists`

**Soluciones**:
- Elimine el recurso existente o impórtelo.
- Utilice diferentes nombres de recursos o **IDs** lógicos.
- Compruebe si hay recursos de pilas fallidas anteriores.
- Use `DeletionPolicy: Retain` para conservar los recursos al eliminar la pila.

#### 5. Dependencias Circulares
**Error**: `Circular dependency between resources`

**Soluciones**:
- Revise los atributos `DependsOn`.
- Elimine dependencias innecesarias.
- Reestructure la plantilla para romper las referencias circulares.
- Use pilas anidadas (**nested stacks**) para separar los recursos dependientes.

#### 6. Capacidad Insuficiente
**Error**: `Insufficient capacity` para instancias de **EC2**.

**Soluciones**:
- Pruebe con una zona de disponibilidad diferente.
- Use un tipo de instancia diferente.
- Pruebe con múltiples tipos de instancias con plantillas de lanzamiento (**launch templates**).
- Despliegue en múltiples **AZs**.

#### 7. Problemas de Tiempo de Espera (Timeout)
**Error**: El tiempo de espera para la creación del recurso se ha agotado.

**Soluciones**:
- Aumente el tiempo de espera en la `CreationPolicy`.
- Compruebe que el recurso se esté creando realmente (**CloudWatch Logs**).
- Para un **ASG**, verifique que las instancias puedan llegar al servicio de metadatos.
- Use `cfn-signal` desde los **user data** de la instancia.

**Herramientas de Resolución de Problemas**:

**Eventos de CloudFormation**:
- Vea los eventos de la pila para obtener mensajes de error detallados.
- Identifique qué recurso falló.
- Compruebe el motivo del estado (**status reason**) para conocer la causa del fallo.

**Conjuntos de Cambios (Change Sets)**:
- Previsualice los cambios antes de ejecutarlos.
- Identifique los recursos que serán reemplazados.
- Valide la plantilla antes de la actualización de la pila.

**Detección de Desviaciones de la Pila (Stack Drift Detection)**:
- Detecte si los recursos fueron modificados manualmente.
- Compare la configuración real con la plantilla.
- Resuelva la desviación antes de actualizar la pila.

**Validación de Plantillas**:
```bash
# Validar la sintaxis de la plantilla
aws cloudformation validate-template --template-body file://template.yaml

# Usar cfn-lint para una validación avanzada
pip install cfn-lint
cfn-lint template.yaml
```

**Mejores Prácticas**:
1. Valide siempre las plantillas antes del despliegue.
2. Use conjuntos de cambios para las actualizaciones de la pila.
3. Implemente activadores de reversión con alarmas de **CloudWatch**.
4. Establezca tiempos de espera adecuados para la creación de recursos.
5. Use `DeletionPolicy: Retain` para recursos críticos.
6. Pruebe primero las plantillas en el entorno de desarrollo.
7. Use pilas anidadas para infraestructuras complejas.
8. Habilite la protección contra la eliminación para las pilas de producción.

---

### El Escalado Automático (Auto Scaling) no Funciona

**Síntomas**: El **Auto Scaling Group** no lanza ni termina las instancias como se esperaba.

**Pasos de Resolución de Problemas**:

#### 1. Verificar las Políticas de Escalado

**Comprobar la Configuración de la Política**:
- Escalado por seguimiento de objetivos (**target tracking**) vs. escalado por pasos (**step scaling**) vs. escalado simple.
- Métrica monitoreada (**CPU**, memoria, personalizada).
- Valor del objetivo o ajustes de los pasos.
- Períodos de enfriamiento (**cooldown**) que impiden un escalado rápido.

**Ejemplo de Problema**:
- Objetivo: 70% de utilización de **CPU**.
- Actual: 85% de **CPU**.
- Pero no se produce el escalado de salida (**scale-out**).

**Soluciones**:
- Compruebe si se encuentra en un período de enfriamiento (por defecto 300 segundos).
- Verifique que el estado de la alarma de **CloudWatch** sea `ALARM`.
- Compruebe que la alarma tenga puntos de datos que superen el umbral.
- Asegúrese de que la política esté habilitada.

#### 2. Comprobar la Configuración del Auto Scaling Group

**Límites de Capacidad**:
- Capacidad mínima: No puede escalar por debajo de esto.
- Capacidad máxima: No puede escalar por encima de esto.
- Capacidad deseada: Objetivo actual.

**Problema Común**: Capacidad máxima alcanzada.
```
Actual: 10 instancias
Capacidad máxima: 10
Resultado: No se puede escalar hacia fuera, incluso si la CPU es alta.
```

**Soluciones**:
- Aumente la capacidad máxima.
- Revise si los límites de capacidad son apropiados.
- Compruebe las cuotas de servicio para las instancias de **EC2**.

#### 3. Problemas en la Plantilla/Configuración de Lanzamiento

**AMI Inválida**:
- **AMI** eliminada o no disponible en la región.
- **AMI** compartida desde otra cuenta que ya no es accesible.

**Permisos de IAM Insuficientes**:
- El perfil de instancia no tiene los permisos requeridos.
- No se puede acceder a **S3**, **Parameter Store** o **Secrets Manager**.

**User Data Inválido**:
- Errores de sintaxis en el script de **user data**.
- El script falla, lo que provoca que falle la inicialización de la instancia.

**Soluciones**:
- Compruebe que la **AMI** exista: `aws ec2 describe-images --image-ids ami-xxx`.
- Revise los registros de **CloudWatch Logs** para ver la salida del script de **user data**.
- Pruebe la plantilla de lanzamiento manualmente lanzando una instancia.
- Verifique que los grupos de seguridad y los pares de claves sean válidos.

#### 4. Problemas en la Zona de Disponibilidad (AZ)

**Sin Capacidad**:
- Capacidad de **EC2** no disponible en las **AZs** especificadas.
- Solo algunas **AZs** tienen capacidad.

**Soluciones**:
- Distribuya entre múltiples **AZs**.
- Use múltiples tipos de instancias (política de instancias mixtas).
- Habilite el reequilibrio de capacidad.

#### 5. Fallos en las Comprobaciones de Estado (Health Checks)

**Instancias que Terminan Inmediatamente**:
- Tipo de comprobación de estado: **EC2** vs. **ELB**.
- Período de gracia de la comprobación de estado demasiado corto.
- Instancias que fallan las comprobaciones de estado.

**Síntomas**:
- Las instancias se lanzan y luego se terminan repetidamente.
- **CloudWatch** muestra las instancias como no saludables (**unhealthy**).

**Soluciones**:
- Aumente el período de gracia de la comprobación de estado (300-600 segundos).
- Corrija los problemas de la aplicación que provocan los fallos en las comprobaciones de estado.
- Verifique la configuración de la comprobación de estado del **target group** del **ELB**.
- Compruebe que los grupos de seguridad permitan el tráfico de las comprobaciones de estado.

#### 6. Cuotas de Servicio

**Límites de Instancias de EC2**:
- Límites de **vCPU** bajo demanda.
- Límites de instancias **Spot**.
- Límites por región.

**Comprobar el Uso Actual**:
```bash
# Comprobar las cuotas de servicio
aws service-quotas get-service-quota \
  --service-code ec2 \
  --quota-code L-1216C47A  # Running On-Demand Standard instances
```

**Soluciones**:
- Solicite un aumento de la cuota.
- Use diferentes tipos de instancias.
- Despliegue en una región diferente.

#### 7. Escalado Suspendido

**Comprobar Procesos Suspendidos**:
- `ReplaceUnhealthy`.
- `Launch`.
- `Terminate`.
- `AddToLoadBalancer`.

**Reanudar Procesos**:
```bash
aws autoscaling resume-processes \
  --auto-scaling-group-name my-asg
```

#### 8. Problemas con las Alarmas de CloudWatch

**La Alarma no se Activa**:
- Datos insuficientes.
- Métrica no publicada.
- El umbral no se ha superado durante los períodos de evaluación requeridos.
- Alarma en estado `INSUFFICIENT_DATA`.

**Soluciones**:
- Compruebe que las métricas de **CloudWatch** se estén publicando.
- Verifique la configuración de la alarma (umbral, períodos).
- Revise el historial de la alarma.
- Pruebe temporalmente con un umbral más bajo.
**Comandos de Depuración**:
```bash
# Describir el Auto Scaling Group
aws autoscaling describe-auto-scaling-groups \
  --auto-scaling-group-names my-asg

# Ver las actividades de escalado
aws autoscaling describe-scaling-activities \
  --auto-scaling-group-name my-asg \
  --max-records 20

# Comprobar las políticas de escalado
aws autoscaling describe-policies \
  --auto-scaling-group-name my-asg

# Ver las alarmas de CloudWatch
aws cloudwatch describe-alarms \
  --alarm-names my-cpu-alarm
```

**Soluciones Comunes**:
1. Asegúrese de que las capacidades mínima, máxima y deseada sean apropiadas.
2. Verifique que las alarmas de **CloudWatch** estén en estado `ALARM`.
3. Compruebe si hay procesos suspendidos.
4. Aumente el período de gracia de la comprobación de estado.
5. Corrija los problemas de la plantilla de lanzamiento (**AMI**, grupos de seguridad, **user data**).
6. Distribuya entre múltiples **AZs** para mejorar la disponibilidad.
7. Use múltiples tipos de instancias para mejorar la disponibilidad de la capacidad.
8. Monitoree con **CloudWatch** y configure alertas.

---

### Factura de AWS Inesperadamente Alta

**Síntomas**: La factura de AWS es significativamente más alta de lo esperado o de lo habitual.

**Principales Causas de los Costos**:

#### 1. Recursos sin Etiquetas o Huérfanos

**Instancias de EC2 en Ejecución**:
- Instancias que se dejaron ejecutándose después de las pruebas.
- El **Auto Scaling** no reduce la escala (**scale-down**).
- Solicitudes **Spot** que crean instancias.

**Comprobación**:
```bash
# Listar todas las instancias en ejecución
aws ec2 describe-instances \
  --filters "Name=instance-state-name,Values=running" \
  --query 'Reservations[].Instances[].[InstanceId,InstanceType,Tags[?Key==`Name`].Value|[0]]'
```

**Volúmenes de EBS**:
- Volúmenes desconectados de instancias terminadas.
- **Snapshots** que se acumulan con el tiempo.
- Volúmenes más grandes de lo necesario.

**Comprobación**:
```bash
# Encontrar volúmenes no conectados
aws ec2 describe-volumes \
  --filters "Name=status,Values=available" \
  --query 'Volumes[].[VolumeId,Size,CreateTime]'
```

**Soluciones**:
- Termine las instancias de **EC2** que no utilice.
- Elimine los volúmenes de **EBS** desconectados.
- Configure políticas de ciclo de vida para los **snapshots**.
- Use el **Tag Editor** de **AWS Resource Groups** para encontrar recursos sin etiquetas.

#### 2. Costos de Transferencia de Datos

**Transferencia entre Regiones**:
- Transferencia de datos entre regiones ($0.02/GB).
- No utilizar recursos de la misma región.

**Transferencia de Datos por Internet**:
- Transferencia de datos de salida hacia Internet ($0.09/GB para los primeros 10TB).
- Descargas de archivos grandes.
- Streaming de vídeo o audio.

**Soluciones**:
- Mantenga los recursos en la misma región.
- Use **CloudFront** para la entrega de contenido (salida o **egress** más barata).
- Comprima los datos antes de la transferencia.
- Use **VPC endpoints** para evitar los cargos por procesamiento de datos del **NAT Gateway**.
- Revise la transferencia de datos de **CloudFront**, **S3** y **EC2** en el **Cost Explorer**.

#### 3. Costos del NAT Gateway

**Alto Procesamiento de Datos**:
- El **NAT Gateway** cobra por los datos procesados ($0.045/GB).
- Instancias en subredes privadas que acceden a Internet con frecuencia.

**Soluciones**:
- Use **VPC endpoints** para los servicios de AWS (**S3**, **DynamoDB**).
- Consolide los **NAT Gateways** (uno por **AZ** es suficiente).
- Revise qué tráfico está pasando a través del **NAT Gateway**.
- Considere cambiar a instancias **NAT** para casos de uso de gran volumen.

#### 4. CloudWatch Logs

**Alta Ingesta de Registros**:
- La aplicación registra demasiada información (**verbose**).
- Período de retención demasiado largo.
- Muchos grupos de registros.

**Comprobar Costos**:
- Ingesta: $0.50/GB.
- Almacenamiento: $0.03/GB al mes.
- Consultas de **Insights**: $0.005/GB escaneado.

**Soluciones**:
- Reduzca el nivel de detalle de los registros.
- Establezca políticas de retención (lo típico es de 7 a 30 días).
- Exporte los registros antiguos a **S3** (almacenamiento más barato).
- Use el muestreo (**sampling**) para registros de gran volumen.
- Elimine los grupos de registros innecesarios.

#### 5. Elastic Load Balancers (ELB)

**Equilibradores de Carga Inactivos**:
- Equilibrador de carga funcionando sin tráfico.
- Utilizar múltiples equilibradores de carga cuando uno solo es suficiente.

**Costos**:
- **ALB**/**NLB**: ~$0.0225/hora (~$16/mes) + procesamiento de datos.
- **Classic LB**: ~$0.025/hora (~$18/mes).

**Soluciones**:
- Elimine los equilibradores de carga que no utilice.
- Consolide las aplicaciones detrás de menos equilibradores de carga.
- Use el enrutamiento basado en rutas en el **ALB**.

#### 6. Instancias de RDS

**Sobre-aprovisionamiento**:
- Instancia de base de datos demasiado grande.
- **Multi-AZ** cuando no es necesario para desarrollo o pruebas.
- No utilizar **Instancias Reservadas**.

**Soluciones**:
- Ajuste el tamaño de la instancia basándose en las métricas de **CloudWatch**.
- Use **Single-AZ** para entornos que no sean de producción.
- Detenga las instancias de **RDS** cuando no estén en uso (desarrollo/pruebas).
- Adquiera **Instancias Reservadas** para producción (ahorre hasta un 72%).

#### 7. Costos de Almacenamiento en S3

**Clase de Almacenamiento Incorrecta**:
- Usar **Standard** para datos a los que se accede con poca frecuencia.
- No utilizar **Intelligent-Tiering**.

**Muchos Objetos Pequeños**:
- **S3** cobra por cada solicitud.
- Millones de archivos diminutos son más costosos.

**Soluciones**:
- Use políticas de ciclo de vida para realizar la transición a clases de almacenamiento más baratas.
- Habilite **S3 Intelligent-Tiering** para patrones de acceso desconocidos.
- Consolide los objetos pequeños.
- Elimine las cargas multiparte (**multipart uploads**) incompletas.
- Use **S3 Storage Lens** para obtener información.

#### 8. Costos de Lambda

**Altas Invocaciones**:
- Bucle infinito o llamadas recursivas.
- Activadores de **CloudWatch Events** demasiado frecuentes.
- Memoria asignada en exceso.

**Soluciones**:
- Revise los registros de **CloudWatch Logs** en busca de errores que provoquen reintentos.
- Optimice el tiempo de ejecución de la función.
- Ajuste la asignación de memoria.
- Use la concurrencia reservada para limitar los costos.
- Implemente un retroceso exponencial para los reintentos.

**Herramientas de Análisis de Costos**:

**AWS Cost Explorer**:
- Vea los costos por servicio, región y etiqueta.
- Identifique tendencias y anomalías.
- Filtre por período de tiempo.

**AWS Budgets**:
- Configure alertas de presupuesto.
- Reciba notificaciones al superar un umbral.
- Pronostique el gasto.

**AWS Cost Anomaly Detection**:
- Detección de anomalías basada en **ML**.
- Alertas automáticas para gastos inusuales.
- Análisis de la causa raíz.

**AWS Trusted Advisor**:
- Recomendaciones de optimización de costos.
- Identifique recursos inactivos.
- Sugerencias para ajustar el tamaño (**right-sizing**) (con soporte Business o Enterprise).

**Asignación de Costos Basada en Etiquetas**:
- Etiquete los recursos por: Proyecto, Entorno, Propietario.
- Habilite los informes de asignación de costos basados en etiquetas.
- Identifique los costos por unidad de negocio.

**Pasos de Investigación**:

1. **Abrir el Cost Explorer**:
   - Agrupar por servicio.
   - Identificar los servicios con mayores costos.
   - Comparar con el mes anterior.

2. **Comprobar si hay Anomalías**:
   - Buscar picos repentinos.
   - Identificar días u horas específicos.

3. **Revisar los Servicios Principales**:
   - **EC2**: Instancias en ejecución, volúmenes de **EBS**.
   - **S3**: Almacenamiento, solicitudes, transferencia de datos.
   - **Transferencia de Datos**: Entre regiones, salida a Internet.
   - **RDS**: Bases de datos en ejecución.

4. **Análisis de Etiquetas**:
   - Identificar recursos sin etiquetas.
   - Realizar un seguimiento de los costos por proyecto o equipo.

5. **Habilitar Facturación Detallada**:
   - Granularidad a nivel de recurso.
   - Entender qué es lo que está impulsando los costos.

**Prevención**:
1. Configure **AWS Budgets** con alertas por correo electrónico.
2. Etiquete todos los recursos de forma adecuada.
3. Configure la detección de anomalías de costos (**Cost Anomaly Detection**).
4. Revise el **Cost Explorer** mensualmente.
5. Implemente políticas de **IAM** de mínimo privilegio (para evitar la creación accidental de recursos costosos).
6. Use **CloudFormation** con restricciones de presupuesto.
7. Habilite el **AWS Cost Optimization Hub**.
8. Reuniones periódicas de revisión de costos con las partes interesadas.

---

### Errores 502/504 en API Gateway

**Síntomas**: **API Gateway** devuelve `502 Bad Gateway` o `504 Gateway Timeout`.

**Tipos de Error**:

#### 1. 502 Bad Gateway

**Causas**:
- El **endpoint** de backend (**Lambda**, **HTTP**) devuelve una respuesta inválida.
- Error/excepción en la función **Lambda**.
- Respuesta malformada de la integración.
- Fallo en la validación del certificado (para integración **HTTP**).

**Soluciones**:

**Integración con Lambda**:
- Revise los registros de **CloudWatch Logs** en busca de errores en **Lambda**.
- Asegúrese de que **Lambda** devuelva el formato de respuesta adecuado:
```json
{
  "statusCode": 200,
  "headers": {
    "Content-Type": "application/json"
  },
  "body": "{\"message\":\"Success\"}"
}
```
- Verifique que el rol de ejecución de **Lambda** tenga los permisos necesarios.
- Compruebe si la función **Lambda** está en una **VPC** y puede llegar a los recursos requeridos.

**Integración HTTP**:
- Verifique que el **endpoint** de backend sea accesible.
- Compruebe que el certificado **SSL** sea válido.
- Pruebe el **endpoint** directamente desde una **EC2** en la misma **VPC**.
- Verifique que los grupos de seguridad permitan que **API Gateway** llegue al backend.
- Compruebe si se está utilizando el método **HTTP** correcto.

**Problemas con VPC Link**:
- Las comprobaciones de estado del **Network Load Balancer** están fallando.
- Los grupos de seguridad están bloqueando el tráfico.
- El **target group** no tiene objetivos saludables (**healthy targets**).

#### 2. 504 Gateway Timeout

**Causas**:
- El backend tarda más que el tiempo de espera de **API Gateway** (máximo 29 segundos).
- Tiempo de espera agotado en la función **Lambda**.
- El **endpoint HTTP** no responde.
- Problemas de conectividad de red.

**Soluciones**:

**Tiempo de Espera de Lambda**:
- Compruebe el ajuste de tiempo de espera de **Lambda** (máximo 15 minutos, pero el límite de **API Gateway** es de 29 segundos).
- Establezca el tiempo de espera de **Lambda** en menos de 29 segundos para invocaciones síncronas.
- Para tareas de larga duración, use la invocación asíncrona o **Step Functions**.
- Optimice el rendimiento de **Lambda**.

**Tiempo de Espera del Endpoint HTTP**:
- Reduzca el tiempo de procesamiento del backend.
- Implemente el almacenamiento en caché en el backend.
- Use el procesamiento asíncrono para operaciones largas.
- Devuelva una respuesta inmediata y procese en segundo plano.

**Configuración de la VPC**:
- Si la función **Lambda** está en una **VPC**, compruebe que pueda llegar a los **endpoints** (usar **NAT Gateway** para Internet).
- Verifique que la resolución de **DNS** esté funcionando.
- Compruebe los registros de **VPC Flow Logs** en busca de paquetes descartados.

#### 3. Problemas en la Respuesta de Integración

**Transformación de Respuesta Inválida**:
- Error en el mapeo de **VTL** (**Velocity Template Language**).
- Cabeceras no formateadas correctamente.
- El cuerpo de la respuesta no es un **JSON** válido.

**Soluciones**:
- Pruebe las plantillas de mapeo en la consola de **API Gateway**.
- Verifique que la estructura de la respuesta coincida con el modelo definido.
- Busque errores de sintaxis en las plantillas **VTL**.
- Habilite el registro en **CloudWatch** para **API Gateway**.

#### 4. Política de Recursos o Autorización

**Error 403 Forbidden disfrazado de 502**:
- La política de recursos deniega la solicitud.
- El autorizador de **Lambda** deniega el acceso pero no devuelve la respuesta adecuada.

**Soluciones**:
- Revise la política de recursos.
- Compruebe los registros de **CloudWatch Logs** del autorizador de **Lambda**.
- Asegúrese de que el autorizador devuelva el documento de política adecuado.

#### 5. Estrangulamiento (Throttling)

**TooManyRequestsException**:
- Estrangulamiento a nivel de cuenta (10,000 **RPS** por defecto).
- Límite de ráfaga superado (5,000 por defecto).
- Estrangulamientos a nivel de etapa (**stage**) o de método.

**Soluciones**:
- Implemente reintentos en el lado del cliente con retroceso exponencial.
- Solicite un aumento del límite.
- Use planes de uso (**usage plans**) para controlar el acceso.
- Implemente el almacenamiento en caché para reducir las llamadas al backend.

**Pasos de Depuración**:

**1. Habilitar CloudWatch Logs**:
```bash
# Habilitar el registro de ejecución
aws apigateway update-stage \
  --rest-api-id abc123 \
  --stage-name prod \
  --patch-operations \
    op=replace,path=/*/logging/loglevel,value=INFO \
    op=replace,path=/*/logging/dataTrace,value=true
```

**2. Comprobar las Métricas de CloudWatch**:
- `4XXError`: Errores del cliente.
- `5XXError`: Errores del servidor.
- `IntegrationLatency`: Tiempo de respuesta del backend.
- `Latency`: Latencia total de la solicitud.
- `Count`: Número de solicitudes.

**3. Probar el Endpoint**:
```bash
# Probar la API directamente
curl -X POST https://api-id.execute-api.region.amazonaws.com/stage/path \
  -H "Content-Type: application/json" \
  -d '{"key":"value"}' \
  -v

# Buscar códigos de error específicos
# 502: Bad Gateway (error de integración)
# 504: Gateway Timeout (tiempo de espera del backend agotado)
```

**4. Revisar los Registros de Lambda** (si se usa la integración con **Lambda**):
```bash
# Obtener el flujo de registros más reciente
aws logs describe-log-streams \
  --log-group-name /aws/lambda/my-function \
  --order-by LastEventTime \
  --descending \
  --max-items 1

# Ver los registros
aws logs tail /aws/lambda/my-function --follow
```

**5. Probar Lambda Directamente**:
```bash
# Invocar Lambda con un evento de prueba
aws lambda invoke \
  --function-name my-function \
  --payload '{"key":"value"}' \
  response.json
```

**Soluciones Comunes**:

1. **Formato de Respuesta de Lambda**:
   - Use la integración de tipo **proxy** para casos sencillos.
   - Asegúrese de que `statusCode`, `headers` y `body` estén formateados correctamente.
   - Convierta el cuerpo **JSON** a una cadena (**string**).

2. **Configuración del Tiempo de Espera**:
   - Tiempo de espera de **Lambda**: < 29 segundos.
   - Use la invocación asíncrona para tareas de larga duración.
   - Implemente el almacenamiento en caché.

3. **Configuración de la VPC**:
   - Añada un **NAT Gateway** para el acceso a Internet.
   - Use **VPC endpoints** para los servicios de AWS.
   - Verifique los grupos de seguridad.

4. **Gestión de Errores**:
   - Implemente `try-catch` en **Lambda**.
   - Devuelva respuestas de error adecuadas.
   - Registre los errores en **CloudWatch**.

5. **Monitoreo**:
   - Configure alarmas de **CloudWatch** para errores **5XX**.
   - Habilite el rastreo con **X-Ray** para un análisis detallado.
   - Revisión periódica de los registros de **CloudWatch Logs**.

**Mejores Prácticas**:
1. Habilite siempre los registros de **CloudWatch Logs** (al menos para los errores).
2. Implemente una gestión de errores adecuada en el backend.
3. Establezca tiempos de espera apropiados (**Lambda** < 29 segundos).
4. Use **X-Ray** para el rastreo distribuido.
5. Pruebe la **API** a fondo antes de pasar a producción.
6. Monitoree la latencia y las tasas de error.
7. Implemente el almacenamiento en caché para reducir la carga del backend.
8. Use planes de uso para controlar el acceso y evitar abusos.

---

## Puntos Clave a Recordar

1. **Optimización de Costos**: Combine **Instancias Reservadas**, **Instancias Spot** e **Instancias bajo demanda** basándose en los patrones de carga de trabajo.
2. **Alta Disponibilidad**: Diseñe siempre a través de múltiples Zonas de Disponibilidad (**Availability Zones**).
3. **Migración de Datos**: Utilice dispositivos físicos de AWS (**Snowball**/**Snowmobile**) para grandes conjuntos de datos.
4. **Serverless**: Ideal para cargas de trabajo impredecibles y un gasto operativo mínimo.
5. **Cumplimiento**: Use **AWS Organizations**, **SCPs** y **AWS Config** para la gobernanza a escala.
6. **Recuperación ante Desastres (DR)**: Elija la estrategia de **DR** basándose en los requisitos de **RPO**/**RTO** y el presupuesto.
7. **Conectividad Híbrida**: **Direct Connect** para producción, **VPN** para desarrollo/pruebas.
8. **Multi-Región**: Use **Global Tables**, **CloudFront** y **Route 53** para un acceso global de baja latencia.
9. **Seguridad**: Implemente defensa en profundidad con **GuardDuty**, **Security Hub**, **Config** y remediación automatizada.
10. **Modernización**: Use el patrón **Strangler Fig** para una migración incremental desde monolitos.
11. **Big Data**: Construya lagos de datos con **S3**, procéselos con **Glue**/**EMR** y analícelos con **Athena**/**Redshift**.
12. **CI/CD**: Automatice los despliegues con **CodePipeline**, implemente despliegues **blue/green** y habilite reversiones rápidas.
13. **Resolución de Problemas**: Siga enfoques sistemáticos para problemas de conectividad, seguridad y rendimiento.
14. **Gestión de Costos**: Use el **Cost Explorer**, **Budgets** y el etiquetado para realizar un seguimiento y optimizar el gasto.

---

[← Anterior: Preparación para el Examen](08-exam-preparation.md) | [Siguiente: Recursos Adicionales →](10-additional-resources.md)
