# Capítulo 10: Servicios Adicionales de AWS y Temas Avanzados

[← Anterior: Capítulo 9](09-management-governance.md) | [Siguiente: Apéndice →](appendix.md)

---

## Herramientas de Desarrollo y CI/CD

### AWS CodeCommit
- Repositorio de control de código fuente basado en Git
- Seguro y de alta disponibilidad
- Sin límites de tamaño en los repositorios
- Se integra con herramientas Git existentes
- Cifrado en reposo y en tránsito
- Nivel gratuito: 5 usuarios activos por mes

### AWS CodeBuild
- Servicio de compilación completamente administrado
- Compila código fuente, ejecuta pruebas y produce paquetes
- Escala automáticamente
- Paga solo por el tiempo de compilación
- Entornos preconfigurados o imágenes Docker personalizadas
- Se integra con CodeCommit, GitHub, Bitbucket

### AWS CodeDeploy
- Servicio de despliegue automatizado
- Despliega en EC2, Lambda, servidores locales
- Estrategias de despliegue: In-place, azul/verde
- Reversión automática en caso de fallo
- Integración con herramientas CI/CD existentes
- Sin cargo adicional (pagas por los recursos)

### AWS CodePipeline
- Servicio de entrega continua
- Automatiza el pipeline de lanzamiento
- Se integra con CodeCommit, CodeBuild, CodeDeploy
- Integraciones de terceros (GitHub, Jenkins)
- Constructor visual de flujos de trabajo
- $1 por pipeline activo por mes

### AWS CodeStar
- Interfaz unificada para actividades de desarrollo
- Desarrolla, compila y despliega aplicaciones rápidamente
- Plantillas de proyectos para varios lenguajes y plataformas
- Panel integrado para monitoreo
- Funciones de colaboración en equipo
- Sin cargo adicional

### AWS Cloud9
- IDE (Entorno de Desarrollo Integrado) basado en la nube
- Escribe, ejecuta y depura código en el navegador
- Compatible con más de 40 lenguajes de programación
- Terminal integrado con AWS CLI
- Funciones de programación colaborativa
- Paga solo por la instancia EC2 subyacente

---

## Servicios de Integración de Aplicaciones

### Amazon EventBridge
- Servicio de bus de eventos sin servidor
- Conecta aplicaciones mediante eventos
- Anteriormente conocido como CloudWatch Events
- Fuentes de eventos: servicios de AWS, aplicaciones personalizadas, apps SaaS
- Patrones de eventos para filtrado
- Múltiples destinos por regla
- Pago por evento

### Amazon MQ
- Servicio administrado de intermediario de mensajes
- Compatible con Apache ActiveMQ y RabbitMQ
- Para migrar aplicaciones existentes que usan intermediarios de mensajes
- Alternativa a SNS/SQS para protocolos específicos
- APIs y protocolos estándar de la industria (MQTT, AMQP, STOMP)
- Despliegue de instancia única o activo/en espera

### AWS App Mesh
- Malla de servicios para microservicios
- Monitorea y controla las comunicaciones
- Funciona con ECS, EKS, EC2
- Proporciona observabilidad y gestión de tráfico
- Basado en el proxy Envoy
- Sin cargo adicional (pagas por los recursos)

---

## Cómputo para Usuarios Finales

### Amazon WorkSpaces
- Escritorio como Servicio (DaaS) administrado
- Escritorios virtuales Windows o Linux
- Acceso desde cualquier dispositivo
- Almacenamiento de escritorio persistente
- Integrado con Active Directory
- Precios: mensual o por horas
- Casos de uso: trabajo remoto, contratistas, BYOD

### Amazon AppStream 2.0
- Servicio de transmisión de aplicaciones
- Transmite aplicaciones de escritorio al navegador
- No requiere instalar aplicaciones localmente
- Escala automáticamente
- Pago por uso
- Casos de uso: capacitación, POC, pruebas de software

### Amazon WorkDocs
- Almacenamiento y colaboración segura de documentos
- Similar a Dropbox o Google Drive
- 1 TB de almacenamiento por usuario
- Comentarios y retroalimentación en archivos
- Integración con Active Directory
- Aplicaciones móviles y de escritorio

### Amazon WorkLink
- Acceso móvil seguro a sitios web internos
- No requiere VPN
- Renderiza el contenido en el navegador en AWS
- Solo los píxeles se envían al dispositivo
- Protege la red corporativa
- Precio por usuario por mes

---

## Servicios IoT

### AWS IoT Core
- Conecta dispositivos IoT a la nube
- Compatible con miles de millones de dispositivos
- Protocolos MQTT, HTTPS, WebSockets
- Device shadow para gestión de estado
- Motor de reglas para procesamiento de datos
- Integración con otros servicios de AWS

### AWS IoT Greengrass
- Extiende AWS a dispositivos de borde
- Cómputo local, mensajería y caché de datos
- Ejecuta funciones Lambda en el borde
- Funciona sin conexión
- Comunicación segura con la nube
- Inferencia de ML en el borde

### AWS IoT Analytics
- Analítica para datos de IoT
- Procesa y analiza datos de IoT
- Plantillas de análisis prediseñadas
- Integración con QuickSight
- Consultas SQL sobre datos de series de tiempo
- Integración con aprendizaje automático

---

## Servicios de Medios

### Amazon Elastic Transcoder
- Convierte archivos multimedia entre formatos
- Transcodificación de medios escalable
- Presets preconfigurados
- Pago por minuto de transcodificación
- Integración con S3, CloudFront

### AWS Elemental MediaConvert
- Transcodificación de video basada en archivos
- Características de nivel broadcast
- Compatible con varios formatos y códecs
- Precios bajo demanda o reservados
- Más avanzado que Elastic Transcoder

### Amazon Kinesis Video Streams
- Captura, procesa y almacena transmisiones de video
- Millones de dispositivos
- Reproducción, análisis, integración con ML
- Casos de uso: cámaras de seguridad, transmisión en vivo
- Pago por datos ingestados y consumidos

---

## Servicios Adicionales de IA/ML

### Amazon Polly
- Servicio de texto a voz
- Voz con sonido natural
- Múltiples idiomas y voces
- TTS neuronal para sonido más natural
- Pago por carácter
- Casos de uso: e-learning, accesibilidad

### Amazon Transcribe
- Reconocimiento automático de voz (ASR)
- Convierte voz a texto
- Procesamiento en tiempo real y por lotes
- Identificación de hablantes
- Vocabulario personalizado
- Pago por segundo de audio

### Amazon Translate
- Traducción automática neuronal
- Traduce texto entre idiomas
- Compatible con más de 75 idiomas
- Terminología personalizada
- Pago por carácter
- Casos de uso: localización, creación de contenido

### Amazon Forecast
- Servicio de pronóstico de series de tiempo
- Usa aprendizaje automático
- No requiere experiencia en ML
- Más preciso que los métodos tradicionales
- Casos de uso: previsión de demanda, planificación de recursos

### Amazon Kendra
- Servicio de búsqueda inteligente
- Búsqueda empresarial impulsada por ML
- Consultas en lenguaje natural
- Aprende de las interacciones del usuario
- Se conecta a varias fuentes de datos

### Amazon Personalize
- Recomendaciones en tiempo real
- La misma tecnología que Amazon.com
- No requiere experiencia en ML
- Recomendaciones en tiempo real y por lotes
- Casos de uso: recomendaciones de productos, contenido personalizado

### Amazon Textract
- Extrae texto y datos de documentos
- OCR y reconocimiento de formularios
- Preserva estructura y relaciones
- Funciona con PDFs e imágenes
- Pago por página

---

## Aplicaciones Empresariales

### Amazon Connect
- Centro de contacto basado en la nube
- Servicio al cliente omnicanal
- Chatbots impulsados por IA
- Pago por uso
- Integración con otros servicios de AWS
- Casos de uso: soporte al cliente, helpdesk

### Amazon Simple Email Service (SES)
- Envío y recepción de correos electrónicos
- Correos transaccionales y de marketing
- Alta capacidad de entrega
- Pago por correo enviado
- Nivel gratuito: 62,000 correos/mes (desde EC2)
- Validación y filtrado de correos

### Amazon Pinpoint
- Servicio de comunicación de marketing
- Correo electrónico, SMS, notificaciones push
- Segmentación de clientes
- Gestión de campañas
- Métricas de análisis y participación
- Pago por mensajes enviados

### AWS WorkMail
- Servicio administrado de correo y calendario
- Alternativa a Exchange/Gmail
- Acceso basado en web
- Clientes móviles y de escritorio
- Integración con Active Directory
- $4 por usuario por mes

### Amazon Chime
- Videoconferencia y comunicación
- Reuniones, chat y llamadas empresariales
- Compartir pantalla
- Precio por usuario por día
- Alternativa a Zoom, Teams

---

## Gestión y Gobernanza Avanzada

### AWS License Manager
- Gestiona licencias de software
- Rastrea el uso de licencias
- Establece límites de uso
- Previene violaciones de licencias
- Funciona con Microsoft, Oracle, SAP, etc.

### AWS Service Catalog
- Crea y gestiona catálogos de servicios de TI
- Productos estandarizados
- Controla qué servicios pueden desplegar los usuarios
- Control de versiones para productos
- Gobernanza y cumplimiento
- Portal de autoservicio para usuarios finales

### AWS Well-Architected Tool
- Revisa la arquitectura de las cargas de trabajo
- Compara con las mejores prácticas
- Evaluación de los seis pilares
- Obtiene un plan de mejora
- Servicio gratuito
- Se recomiendan revisiones periódicas

### AWS Personal Health Dashboard
- Vista personalizada del estado de los servicios de AWS
- Alertas para eventos de servicio que te afectan
- Notificaciones proactivas
- Orientación para remediar problemas
- Disponible para todos los clientes
- Diferente del Service Health Dashboard (estado global)

### AWS Compute Optimizer
- Recomienda recursos óptimos de AWS
- Recomendaciones basadas en ML
- Analiza la utilización histórica
- Sugiere tipos de instancias EC2, volúmenes EBS, memoria Lambda
- Oportunidades de ahorro en costos
- Sin cargo adicional

---

## Servicios de Migración y Transferencia

### AWS Application Discovery Service
- Descubre aplicaciones locales
- Planifica migraciones
- Recopila datos de utilización del servidor y dependencias
- Descubrimiento sin agente o basado en agente
- Exporta datos para análisis
- Integración con Migration Hub

### AWS Migration Hub
- Rastrea las migraciones de aplicaciones
- Ubicación única para monitorear migraciones
- Funciona con herramientas de migración
- Visualiza el progreso de la migración
- Sin costo adicional

### AWS Server Migration Service (SMS)
- Migra servidores locales a AWS
- Replicación incremental
- Tiempo de inactividad mínimo
- Compatible con VMware, Hyper-V, Azure
- Sin cargo adicional
- Siendo reemplazado por Application Migration Service

### AWS DataSync
- Transfiere datos entre entornos locales y AWS
- Hasta 10 veces más rápido que herramientas de código abierto
- Transferencia de datos automatizada
- Compatible con protocolos NFS, SMB
- Transferencias a S3, EFS, FSx
- Pago por GB transferido

### AWS Transfer Family
- SFTP, FTPS, FTP completamente administrado
- Transfiere archivos hacia/desde S3 o EFS
- Sin infraestructura que gestionar
- Integración con sistemas de autenticación existentes
- Pago por protocolo habilitado + transferencia de datos

---

## Servicios de Redes Avanzados

### AWS Global Accelerator
- Mejora la disponibilidad y el rendimiento
- Usa la red global de AWS
- Direcciones IP anycast estáticas
- Conmutación por error automática
- Verificaciones de estado
- Casos de uso: juegos, IoT, VoIP
- Diferente de CloudFront (no hace caché)

### AWS App Mesh
- Malla de servicios para microservicios
- Monitorea y controla las comunicaciones
- Funciona con ECS, EKS, EC2
- Basado en el proxy Envoy
- Gestión de tráfico y observabilidad

### AWS Cloud Map
- Descubrimiento de servicios
- Registra recursos de aplicaciones
- Descubre servicios a través de API o DNS
- Verificación de estado
- Integración con ECS, EKS

---

## Servicios de Almacenamiento Adicionales

### Amazon FSx
Sistemas de archivos completamente administrados con dos opciones principales:

**FSx para Windows File Server**:
- Sistema de archivos nativo de Windows
- Protocolo SMB
- Integración con Active Directory
- Para aplicaciones Windows

**FSx para Lustre**:
- Sistema de archivos de alto rendimiento
- ML, HPC, procesamiento de video
- Integración con S3
- Latencias de submilisegundos

### AWS Backup
- Servicio centralizado de copias de seguridad
- Automatiza y gestiona copias de seguridad
- Respaldo en todos los servicios de AWS
- Políticas de respaldo y retención
- Informes de cumplimiento
- Pago por almacenamiento usado

---

## Blockchain y Computación Cuántica

### Amazon Managed Blockchain
- Crea y gestiona redes blockchain
- Compatible con Hyperledger Fabric y Ethereum
- Completamente administrado
- Escala automáticamente
- Casos de uso: cadena de suministro, finanzas

### Amazon Braket
- Servicio de computación cuántica
- Desarrolla algoritmos cuánticos
- Prueba en simuladores cuánticos
- Ejecuta en hardware cuántico
- Pago por simulación y tareas cuánticas

---

## Consejos del Examen para Selección de Servicios

### Árbol de Decisión para Bases de Datos

**Elige la Base de Datos Según los Requisitos**:

#### ¿Necesitas SQL y transacciones ACID?
- **Aplicación tradicional, base de datos existente** → **RDS**
- **Necesitas rendimiento extremo** → **Aurora**
- **Necesidades específicas: Oracle/SQL Server** → **RDS** con ese motor

#### ¿Necesitas NoSQL?
- **Clave-valor, escalar a millones de solicitudes/seg** → **DynamoDB**
- **Base de datos de documentos, compatible con MongoDB** → **DocumentDB**
- **Relaciones de grafos** → **Neptune**

#### ¿Necesitas caché?
- **Caché en memoria** → **ElastiCache**
- **Se necesitan funciones de Redis** → ElastiCache para Redis
- **Caché simple** → ElastiCache para Memcached

#### ¿Necesitas almacén de datos/analítica?
- **OLAP, BI, big data** → **Redshift**
- **Consultar datos en S3** → **Athena**
- **Analítica en tiempo real** → **Kinesis Data Analytics**

#### ¿Datos de series de tiempo?
- **IoT, métricas, registros** → **Timestream**

---

### Árbol de Decisión para Cómputo

| Requisito | Mejor Opción | Por Qué |
|-----------|--------------|---------|
| Control total del SO | EC2 | Flexibilidad completa |
| Sin servidor, orientado a eventos | Lambda | Sin servidores, pago por uso |
| Desplegar app rápidamente | Elastic Beanstalk | PaaS, administrado |
| Contenedores, Kubernetes | EKS | Compatible con Kubernetes |
| Contenedores, nativo AWS | ECS | Más simple que EKS |
| Contenedores, sin servidores | Fargate | Contenedores sin servidor |
| Procesamiento por lotes | AWS Batch | Optimizado para lotes |
| Sitio web simple, blog | Lightsail | Precios predecibles |

---

### Guía de Selección de Almacenamiento

| Caso de Uso | Servicio | Razón |
|-------------|---------|-------|
| Copias de seguridad, archivos | S3 Glacier | Menor costo |
| Contenido de sitio web | S3 + CloudFront | Escalable, global |
| Volumen de arranque de EC2 | EBS | Almacenamiento en bloque |
| Sistema de archivos compartido | EFS | Multi-adjunto |
| Recursos compartidos de archivos Windows | FSx para Windows | SMB, integración AD |
| Cargas de trabajo HPC | FSx para Lustre | Alto rendimiento |
| Datos temporales | Instance Store | Mayor IOPS |
| Transferencia sin conexión | Snow Family | Grandes volúmenes de datos |

---

[← Anterior: Escenarios de Examen](09-exam-scenarios.md) | [Volver al Inicio](README.md)
