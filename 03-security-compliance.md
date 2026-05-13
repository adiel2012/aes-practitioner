# Dominio 2: Seguridad y Cumplimiento (30%)

[Anterior: Conceptos de la Nube](02-cloud-concepts.md) | [Volver al Inicio](README.md) | [Siguiente: Tecnología y Servicios](04-technology-services.md)

---

La seguridad es la prioridad número uno en AWS. Este dominio cubre el Modelo de Responsabilidad Compartida, la gestión de identidades (IAM), los servicios de seguridad de red y datos, y los marcos de cumplimiento.

## Tabla de Contenidos

- [Modelo de Responsabilidad Compartida de AWS](#modelo-de-responsabilidad-compartida-de-aws)
- [AWS Identity and Access Management (IAM)](#aws-identity-and-access-management-iam)
  - [Usuarios, Grupos y Roles](#usuarios-grupos-y-roles)
  - [Políticas y Permisos](#politicas-y-permisos)
  - [Mejores Prácticas de IAM](#mejores-practicas-de-iam)
- [Seguridad y Protección de Datos](#seguridad-y-proteccion-de-datos)
  - [Cifrado en Reposo y en Tránsito](#cifrado-en-reposo-y-en-transito)
  - [AWS Key Management Service (KMS)](#aws-key-management-service-kms)
- [Protección de Red e Infraestructura](#proteccion-de-red-e-infraestructura)
  - [AWS WAF, Shield y Firewall Manager](#aws-waf-shield-y-firewall-manager)
- [Detección y Monitoreo](#deteccion-y-monitoreo)
  - [GuardDuty, Inspector y Macie](#guardduty-inspector-y-macie)
- [Cumplimiento y Gobernanza](#cumplimiento-y-gobernanza)
  - [AWS Artifact y AWS Audit Manager](#aws-artifact-y-aws-audit-manager)
- [Preguntas de Repaso](#preguntas-de-repaso)

---

## Modelo de Responsabilidad Compartida de AWS (AWS Shared Responsibility Model)

El **Shared Responsibility Model** es fundamental para entender la seguridad en la nube. Divide las tareas de seguridad entre AWS y el cliente.

### Responsabilidad de AWS: "Seguridad DE la Nube"

AWS es responsable de proteger la infraestructura que ejecuta todos los servicios ofrecidos en la Nube de AWS. Esta infraestructura está compuesta por el hardware, el software, las redes y las instalaciones que ejecutan los servicios de AWS.

**Elementos bajo responsabilidad de AWS:**
- **Infraestructura Global**: Regiones, Zonas de Disponibilidad y Ubicaciones de Borde.
- **Hardware y Software**: Servidores físicos, almacenamiento y el software de virtualización (**Hypervisor**).
- **Redes**: Protección de la red física de AWS.
- **Seguridad Física**: Acceso a los centros de datos, vigilancia, protección contra incendios, etc.
- **Servicios Gestionados**: En servicios como **S3**, **DynamoDB** o **Lambda**, AWS gestiona más capas (parcheo del SO, configuración de red, etc.).

### Responsabilidad del Cliente: "Seguridad EN la Nube"

El cliente es responsable de lo que pone en la nube y de cómo configura los servicios de AWS.

**Elementos bajo responsabilidad del cliente:**
- **Datos del Cliente**: Cifrado, integridad y protección de la privacidad de los datos.
- **Gestión de Identidades (IAM)**: Control de quién tiene acceso a qué.
- **Configuración de Red**: **Security Groups**, **Network ACLs**, subredes y enrutamiento.
- **Sistema Operativo (en IaaS)**: Parcheo y actualizaciones de seguridad de las instancias **EC2**.
- **Cifrado**: Configuración de cifrado del lado del cliente y del lado del servidor.
- **Firewall de Aplicaciones**: Configuración de reglas en **AWS WAF**.

> [!TIP]
> **Regla de oro para el examen:** Si puedes configurarlo en la consola de AWS o mediante la API, probablemente sea tu responsabilidad. Si requiere acceso físico a un servidor, es responsabilidad de AWS.

---

## AWS Identity and Access Management (IAM)

**IAM** es el servicio que te permite gestionar de forma segura el acceso a los servicios y recursos de AWS. Con **IAM**, puedes crear y gestionar usuarios, grupos y roles de AWS, y utilizar permisos para permitir o denegar su acceso a los recursos de AWS.

### Componentes de IAM

#### 1. Usuarios (Users)
- Representan a una persona o aplicación que interactúa con AWS.
- Tienen credenciales permanentes (contraseña para la consola y/o **Access Keys** para la CLI/SDK).
- **Mejor Práctica:** Nunca uses el usuario **Root** para las tareas diarias. Crea un usuario de **IAM** para ti mismo.

#### 2. Grupos (Groups)
- Una colección de usuarios de **IAM**.
- Permiten asignar permisos a múltiples usuarios a la vez.
- Un usuario puede pertenecer a múltiples grupos.

#### 3. Roles (Roles)
- Identidades con permisos específicos que no tienen credenciales permanentes.
- Son "asumidos" por usuarios, aplicaciones o servicios de AWS.
- **Caso de uso común:** Permitir que una instancia **EC2** acceda a un bucket de **S3** sin guardar claves de acceso en el servidor.

#### 4. Políticas (Policies)
- Documentos **JSON** que definen los permisos.
- Se adjuntan a usuarios, grupos o roles.
- **Tipos de Políticas:**
  - **Managed Policies**: Gestionadas por AWS o por ti. Se pueden adjuntar a múltiples identidades.
  - **Inline Policies**: Insertadas directamente en un solo usuario, grupo o rol.

### Estructura de una Política IAM (JSON)

Una política estándar tiene los siguientes elementos:
- **Version**: Versión del lenguaje de la política (usa siempre `2012-10-17`).
- **Statement**: El contenedor principal de los elementos de la política.
  - **Sid** (opcional): Identificador del statement.
  - **Effect**: `Allow` (Permitir) o `Deny` (Denegar).
  - **Action**: Lista de acciones permitidas o denegadas (ej. `s3:GetItem`).
  - **Resource**: El recurso de AWS al que se aplica la acción (identificado por su **ARN**).
  - **Condition** (opcional): Circunstancias bajo las cuales la política es válida (ej. solo desde una IP específica).

### Principio de Privilegio Mínimo (Least Privilege)

Este es el concepto de seguridad más importante en **IAM**. Consiste en otorgar solo los permisos estrictamente necesarios para realizar una tarea específica, y nada más.

---

## Mejores Prácticas de Seguridad de IAM

Para proteger tu cuenta de AWS, sigue siempre estas recomendaciones:

1. **Protege tu Usuario Root**: Habilita **MFA** inmediatamente y elimina las **Access Keys**. No lo uses para tareas diarias.
2. **Crea Usuarios Individuales**: No compartas credenciales. Cada persona debe tener su propio usuario.
3. **Usa Grupos para Asignar Permisos**: Es más fácil gestionar permisos a nivel de grupo que individualmente.
4. **Otorga Privilegio Mínimo**: Empieza con cero permisos y añade solo lo necesario.
5. **Habilita MFA (Multi-Factor Authentication)**: Obligatorio para usuarios con privilegios y altamente recomendado para todos los demás.
6. **Usa Roles para Aplicaciones en EC2**: Nunca guardes **Access Keys** dentro de tus servidores.
7. **Rota las Credenciales Regularmente**: Cambia contraseñas y claves de acceso periódicamente.
8. **Usa IAM Access Analyzer**: Identifica recursos compartidos externamente.
9. **Establece una Política de Contraseñas Fuerte**: Longitud mínima, tipos de caracteres, etc.

---

## Seguridad y Protección de Datos

### Cifrado de Datos (Data Encryption)

AWS proporciona múltiples formas de cifrar tus datos, tanto en reposo como en tránsito.

#### Cifrado en Reposo (Encryption at Rest)

Protege los datos almacenados en discos, bases de datos o sistemas de archivos.

- **Server-Side Encryption (SSE)**: AWS cifra los datos por ti antes de guardarlos.
  - **SSE-S3**: Claves gestionadas por AWS.
  - **SSE-KMS**: Claves gestionadas a través de **AWS KMS**.
  - **SSE-C**: Claves proporcionadas por el cliente.
- **Client-Side Encryption**: Tú cifras los datos antes de enviarlos a AWS.

#### Cifrado en Tránsito (Encryption in Transit)

Protege los datos mientras se mueven a través de la red entre el cliente y el servidor, o entre servicios de AWS.

- Utiliza protocolos **SSL/TLS**.
- **HTTPS** es el estándar para la web.
- **AWS Certificate Manager (ACM)**: Proporciona y gestiona certificados **SSL/TLS** gratuitos.

### AWS Key Management Service (KMS)

Servicio gestionado que facilita la creación y el control de las claves criptográficas.

- Utiliza módulos de seguridad de hardware (**HSM**) para proteger las claves.
- Se integra con casi todos los servicios de AWS.
- Permite la rotación automática de claves.
- **Auditoría**: Todas las acciones de uso de claves quedan registradas en **CloudTrail**.

### AWS Secrets Manager

Servicio para gestionar secretos (contraseñas de BD, claves API) de forma segura.
- Permite la rotación automática de secretos.
- Elimina la necesidad de tener contraseñas en el código fuente.

---

## Protección de Red e Infraestructura

### Seguridad de la VPC (Virtual Private Cloud)

La seguridad de la red en AWS se implementa mediante múltiples capas de defensa.

#### 1. Security Groups (Grupos de Seguridad)
- Actúan como un firewall virtual para tus instancias **EC2**.
- Operan a nivel de **Instancia**.
- Son **Stateful**: Si permites la entrada, la salida se permite automáticamente.
- Solo soportan reglas de "Permitir" (**Allow**).
- Todas las reglas se evalúan antes de decidir si se permite el tráfico.

#### 2. Network Access Control Lists (NACLs)
- Actúan como un firewall para la subred.
- Operan a nivel de **Subred**.
- Son **Stateless**: Debes permitir explícitamente tanto la entrada como la salida.
- Soportan reglas de "Permitir" y "Denegar" (**Deny**).
- Las reglas se evalúan en orden numérico (de menor a mayor).

#### 3. AWS WAF (Web Application Firewall)
- Protege tus aplicaciones web contra ataques comunes como Inyección SQL o Cross-Site Scripting (XSS).
- Se puede desplegar en **CloudFront**, **Application Load Balancer (ALB)**, **API Gateway** o **AppSync**.
- Te permite bloquear IPs o regiones específicas.

#### 4. AWS Shield
- Protección gestionada contra ataques de denegación de servicio distribuido (**DDoS**).
- **Shield Standard**: Gratuito para todos los clientes de AWS. Protege contra ataques comunes en capas 3 y 4.
- **Shield Advanced**: Servicio de pago con mitigación avanzada, visibilidad 24/7 y acceso al equipo de respuesta de AWS.

#### 5. AWS Firewall Manager
- Servicio de gestión centralizada para configurar y gestionar reglas de firewall en todas tus cuentas y aplicaciones en **AWS Organizations**.

---

## Detección y Monitoreo de Amenazas

### Amazon GuardDuty
- Servicio de detección de amenazas inteligente que monitorea continuamente tus cuentas y cargas de trabajo.
- Utiliza **Machine Learning** para identificar actividades maliciosas (ej. minería de criptomonedas, acceso desde IPs sospechosas).
- Analiza logs de **CloudTrail**, **VPC Flow Logs** y logs de **DNS**.

### Amazon Inspector
- Servicio de evaluación de seguridad automatizado que ayuda a mejorar la seguridad y el cumplimiento de las aplicaciones desplegadas en AWS.
- Escanea instancias **EC2**, imágenes en **Amazon ECR** y funciones **AWS Lambda** en busca de vulnerabilidades de software y exposición de red.

### Amazon Macie
- Servicio de seguridad de datos que utiliza el aprendizaje automático para descubrir y proteger datos sensibles en **Amazon S3**.
- Identifica automáticamente información de identificación personal (**PII**) como números de tarjetas de crédito o pasaportes.

---

## Cumplimiento y Gobernanza (Compliance & Governance)

### AWS Artifact
- Portal de autoservicio para el acceso bajo demanda a los informes de cumplimiento de AWS.
- Aquí puedes descargar informes **SOC**, certificaciones **ISO**, y aceptar acuerdos como el de **HIPAA**.

### AWS Config
- Servicio que evalúa, audita y califica las configuraciones de tus recursos de AWS.
- Permite descubrir cambios en la configuración y verificar si los recursos cumplen con las políticas deseadas (**Config Rules**).
- Ejemplo: Recibir una alerta si un bucket de **S3** se vuelve público.

### AWS CloudTrail
- Registra todas las acciones realizadas por usuarios, roles o servicios de AWS.
- Proporciona un historial de eventos para auditoría, análisis de seguridad y solución de problemas.
- "Quién hizo qué, cuándo y desde dónde".

### AWS Organizations y SCPs
- **AWS Organizations** permite gestionar centralizadamente múltiples cuentas de AWS.
- **Service Control Policies (SCPs)**: Establecen los permisos máximos para las cuentas de la organización (guardarraíles).
- **Consolidated Billing**: Una sola factura para todas las cuentas, permitiendo descuentos por volumen de uso.

---

## Respuesta a Incidentes de Seguridad (Incident Response)

AWS proporciona herramientas para ayudarte a responder rápidamente a incidentes de seguridad.

### Pasos Recomendados:
1. **Detección**: Utiliza **GuardDuty**, **CloudWatch Alarms** y **Security Hub**.
2. **Contención**: Aislar los recursos afectados (ej. cambiar los **Security Groups** para denegar todo el tráfico, o poner la instancia en una subred de cuarentena).
3. **Análisis Forense**: Crear una instantánea (**Snapshot**) del volumen **EBS** para analizarlo sin afectar el entorno de producción.
4. **Remediación**: Eliminar el acceso malicioso y restaurar desde una copia de seguridad limpia.

> [!IMPORTANT]
> **CloudTrail** es tu herramienta principal para investigar "quién hizo qué" durante un incidente.

---

## Errores Comunes de Seguridad y Cómo Evitarlos

| Error Común | Cómo Evitarlo |
|-------------|---------------|
| Usar la cuenta **Root** para tareas diarias | Crear usuarios de **IAM** y usar **MFA**. |
| Compartir **Access Keys** | Usar **IAM Roles** para instancias y servicios. |
| Dejar buckets de **S3** públicos | Habilitar **S3 Block Public Access**. |
| Políticas de **IAM** demasiado permisivas | Seguir el **Principio de Privilegio Mínimo**. |
| No habilitar **MFA** | Obligatorio para **Root** y usuarios privilegiados. |

---

## Lista de Verificación de Seguridad para el Examen

- [ ] **Modelo de Responsabilidad Compartida**: Entender quién es responsable de qué.
- [ ] **IAM**: Diferencia entre Usuarios, Grupos y Roles.
- [ ] **MFA**: Importancia de habilitarlo en Root.
- [ ] **KMS**: Servicio para gestionar claves de cifrado.
- [ ] **Shield vs WAF**: Shield para DDoS, WAF para ataques web.
- [ ] **Inspector vs GuardDuty**: Inspector para vulnerabilidades, GuardDuty para amenazas activas.
- [ ] **Macie**: Descubrimiento de datos sensibles (**PII**).
- [ ] **Artifact**: Informes de cumplimiento y acuerdos.
- [ ] **Secrets Manager**: Rotación de contraseñas.

---

## Preguntas de Repaso (Review Questions)

**1. Según el Modelo de Responsabilidad Compartida, ¿cuál es una responsabilidad de AWS?**
- A. Gestión de parches del sistema operativo de una instancia **EC2**.
- B. Seguridad física de los centros de datos.
- C. Configuración de las reglas del firewall (**Security Groups**).
- D. Cifrado de los datos en el lado del cliente.
*(Respuesta: B)*

**2. ¿Qué componente de IAM usarías para otorgar permisos temporales a una aplicación que se ejecuta en una instancia EC2?**
- A. Usuario de **IAM**.
- B. Grupo de **IAM**.
- C. Rol de **IAM**.
- D. Política de contraseñas.
*(Respuesta: C)*

**3. ¿Qué servicio ayuda a identificar información de identificación personal (PII) en buckets de S3?**
- A. **Amazon GuardDuty**.
- B. **Amazon Inspector**.
- C. **Amazon Macie**.
- D. **AWS Config**.
*(Respuesta: C)*

**4. ¿Qué servicio te permite descargar informes de cumplimiento de AWS, como SOC 2 o ISO 27001?**
- A. **AWS Audit Manager**.
- B. **AWS Artifact**.
- C. **AWS Trusted Advisor**.
- D. **AWS Config**.
*(Respuesta: B)*

**5. ¿Cuál es la principal diferencia entre un Security Group y una NACL?**
- A. Los **Security Groups** son a nivel de subred, las **NACLs** a nivel de instancia.
- B. Los **Security Groups** son **Stateful**, las **NACLs** son **Stateless**.
- C. Solo las **NACLs** permiten reglas de "Permitir".
- D. Solo los **Security Groups** permiten reglas de "Denegar".
*(Respuesta: B)*

---

[← Anterior: Conceptos de la Nube](02-cloud-concepts.md) | [Volver al Inicio](README.md) | [Siguiente: Tecnología y Servicios en la Nube →](04-technology-services.md)
