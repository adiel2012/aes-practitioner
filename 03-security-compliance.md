# Dominio 2: Seguridad y Cumplimiento (30%)

[Anterior: Conceptos de la Nube](02-cloud-concepts.md) | [Volver al Inicio](README.md) | [Siguiente: Tecnología y Servicios](04-technology-services.md)

---

La seguridad es el "Trabajo Cero" en AWS. Este dominio cubre el Modelo de Responsabilidad Compartida, la gestión de identidades y accesos (IAM), la protección de datos, la seguridad de la infraestructura y los marcos de cumplimiento.

## Tabla de Contenidos

- [Modelo de Responsabilidad Compartida de AWS](#modelo-de-responsabilidad-compartida-de-aws)
- [AWS Identity and Access Management (IAM)](#aws-identity-and-access-management-iam)
  - [Usuarios, Grupos y Roles](#usuarios-grupos-y-roles)
  - [Políticas y Permisos](#politicas-y-permisos)
  - [Mejores Prácticas de IAM](#mejores-practicas-de-iam)
- [Seguridad de Datos](#seguridad-de-datos)
  - [Cifrado en Reposo y en Tránsito](#cifrado-en-reposo-y-en-transito)
  - [AWS Key Management Service (KMS)](#aws-key-management-service-kms)
- [Seguridad de Infraestructura](#seguridad-de-infraestructura)
  - [VPC Security y Servicios de Red](#vpc-security-y-servicios-de-red)
- [Detección y Monitoreo](#deteccion-y-monitoreo)
  - [GuardDuty, Inspector y Macie](#guardduty-inspector-y-macie)
- [Cumplimiento y Gobernanza](#cumplimiento-y-gobernanza)
  - [AWS Artifact y AWS Config](#aws-artifact-y-aws-config)
- [Preguntas de Repaso](#preguntas-de-repaso)

---

## Modelo de Responsabilidad Compartida de AWS (AWS Shared Responsibility Model)

El **Shared Responsibility Model** es fundamental para entender la seguridad en la nube. Divide las tareas de seguridad entre AWS y el cliente.

### Responsabilidad de AWS: "Seguridad DE la Nube"

AWS es responsable de proteger la infraestructura que ejecuta todos los servicios ofrecidos en la Nube de AWS. Esta infraestructura está compuesta por el hardware, el software, las redes y las instalaciones que ejecutan los servicios de AWS.

**Elementos bajo responsabilidad de AWS:**
- **Infraestructura Global**: Regiones, Zonas de Disponibilidad (AZs) y Ubicaciones de Borde (Edge Locations).
- **Hardware Físico**: Servidores, almacenamiento y cableado en los centros de datos.
- **Software de Virtualización**: La capa de **Hypervisor** que permite ejecutar múltiples instancias en un solo host físico.
- **Seguridad Física de las Instalaciones**: Control de acceso biométrico, vigilancia 24/7, protección contra incendios y desastres.
- **Gestión de Servicios de Red**: Redes físicas y virtualizadas que conectan los centros de datos.
- **Servicios Gestionados (Abstracción)**: En servicios como **S3**, **DynamoDB**, **Lambda**, **RDS** y **Redshift**, AWS asume responsabilidades adicionales como:
  - Parcheado del sistema operativo subyacente.
  - Actualizaciones de software de la base de datos.
  - Gestión de la configuración de red y almacenamiento.

### Responsabilidad del Cliente: "Seguridad EN la Nube"

El cliente es responsable de todo lo que pone en la nube y de cómo configura los servicios que utiliza.

**Elementos bajo responsabilidad del cliente:**
- **Datos del Cliente**: Cifrado, integridad, privacidad y copias de seguridad de los datos almacenados.
- **Gestión de Identidades y Accesos (IAM)**: Configuración de usuarios, grupos, roles, contraseñas y **MFA**.
- **Configuración de Firewall de Red**: Gestión de **Security Groups** y **Network ACLs**.
- **Sistema Operativo (en IaaS)**: Parcheado y actualizaciones de seguridad para instancias **EC2**.
- **Cifrado del Lado del Cliente (Client-Side Encryption)**: Cifrar los datos antes de enviarlos a AWS.
- **Cifrado del Lado del Servidor (Server-Side Encryption)**: Configurar AWS para que cifre los datos en reposo.
- **Seguridad del Tráfico de Red**: Configuración de cifrado en tránsito (**SSL/TLS**).

> [!TIP]
> **Analogía para el Examen:** AWS te proporciona una "casa segura" (infraestructura), pero tú eres responsable de "cerrar las puertas con llave", "poner la alarma" y elegir "a quién dejas entrar" (datos y configuración).

---

## AWS Identity and Access Management (IAM)

**IAM** es el servicio central para controlar el acceso a tus recursos de AWS. Es un servicio global (no regional).

### Componentes de IAM

#### 1. Usuario Root (Root User)
- Se crea automáticamente al abrir la cuenta.
- Tiene acceso total y sin restricciones a todos los recursos y facturación.
- **Regla de Oro**: Solo usa el usuario Root para las tareas iniciales. Después, crea un usuario Administrador en **IAM**.

#### 2. Usuarios de IAM (IAM Users)
- Representan a personas o aplicaciones dentro de tu organización.
- Cada usuario tiene su propio nombre y credenciales.
- **Credenciales**:
  - **Password**: Para iniciar sesión en la Consola de Administración de AWS.
  - **Access Keys**: Para interactuar con AWS vía **CLI**, **SDK** o **API**. (Consiste en un **Access Key ID** y un **Secret Access Key**).

#### 3. Grupos de IAM (IAM Groups)
- Colecciones de usuarios de **IAM**.
- Se usan para asignar permisos a múltiples usuarios a la vez de forma eficiente.
- **Ejemplo**: Crear un grupo llamado "Admins" y asignarle permisos de administrador. Todos los usuarios en ese grupo heredarán esos permisos.

#### 4. Roles de IAM (IAM Roles)
- Identidades temporales que no tienen credenciales permanentes.
- Se usan para otorgar permisos a:
  - **Servicios de AWS**: Por ejemplo, permitir que una instancia **EC2** escriba en un bucket de **S3**.
  - **Usuarios de otras cuentas**: Para acceso entre cuentas (**Cross-account access**).
  - **Identidades Federadas**: Usuarios que inician sesión mediante **Active Directory**, Google o Facebook.

#### 5. Políticas de IAM (IAM Policies)
- Documentos en formato **JSON** que definen qué acciones están permitidas o denegadas.
- **Tipos de Políticas**:
  - **AWS Managed Policies**: Creadas y mantenidas por AWS (ej. `AdministratorAccess`).
  - **Customer Managed Policies**: Creadas por ti para tus necesidades específicas.
  - **Inline Policies**: Insertadas directamente en un solo usuario, grupo o rol.

### Estructura de una Política JSON

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "s3:ListBucket",
      "Resource": "arn:aws:s3:::mi-bucket-de-ejemplo"
    }
  ]
}
```
- **Effect**: `Allow` o `Deny`.
- **Action**: La acción específica (ej. `ec2:RunInstances`).
- **Resource**: El recurso específico identificado por su **ARN** (**Amazon Resource Name**).

---

## Mejores Prácticas de IAM

Seguir estas prácticas es vital para mantener la seguridad de tu cuenta:

#### Implementar el Principio de Privilegio Mínimo (Least Privilege)
- Otorga solo los permisos necesarios para realizar el trabajo, y nada más.
- Comienza con cero permisos y añade solo lo que se requiera.
- Usa políticas gestionadas por AWS como punto de partida y luego personalízalas.
- Revisa y audita los permisos regularmente.
- Usa **IAM Access Analyzer** para identificar permisos no utilizados.
- Elimina permisos que no se hayan usado en más de 90 días.

**Escenario de Ejemplo**: Un desarrollador necesita leer logs de aplicaciones de **S3**. Dale permiso `s3:GetObject` en el bucket específico, no acceso total a **S3** ni derechos de administrador.

#### Implementar Políticas de Contraseña Fuertes
- Longitud mínima: 14+ caracteres (AWS permite de 8 a 128).
- Requerir mayúsculas, minúsculas, números y símbolos.
- Evitar la reutilización (recordar al menos las últimas 24 contraseñas).
- Forzar la expiración de contraseñas (recomendado cada 60-90 días).
- Impedir que los usuarios cambien su contraseña con demasiada frecuencia.

---

#### Habilitar Multi-Factor Authentication (MFA)

**MFA** añade una capa extra de protección al requerir un segundo factor además de la contraseña.

**Opciones de Dispositivos MFA:**
1. **Dispositivo MFA Virtual**: Aplicaciones como **Google Authenticator**, **Authy** o **Microsoft Authenticator** ejecutándose en un smartphone.
2. **Llave de Seguridad U2F**: Dispositivos físicos que se conectan al puerto USB (ej. **Yubikey**).
3. **Dispositivo de Token de Hardware**: Pequeños dispositivos físicos que generan códigos de un solo uso.

> [!IMPORTANT]
> Habilitar **MFA** en el usuario **Root** es la recomendación de seguridad número uno de AWS.

---

## Seguridad de Datos (Data Security)

AWS proporciona herramientas robustas para proteger tus datos mediante el cifrado.

### Cifrado en Reposo (Encryption at Rest)

El cifrado en reposo protege los datos almacenados en medios físicos.

#### Opciones de Cifrado del Lado del Servidor (SSE)

1. **SSE-S3 (S3-Managed Keys)**:
   - AWS gestiona las claves de cifrado y el proceso.
   - Utiliza cifrado **AES-256**.
   - **Coste**: Sin cargos adicionales por el cifrado.

2. **SSE-KMS (KMS-Managed Keys)**:
   - Utiliza **AWS Key Management Service** para gestionar las claves.
   - Proporciona una pista de auditoría detallada en **CloudTrail**.
   - Permite la rotación de claves y control de acceso granular.
   - **Coste**: Se aplican cargos por el uso de **KMS**.

3. **SSE-C (Customer-Provided Keys)**:
   - Tú proporcionas la clave de cifrado con cada solicitud.
   - AWS realiza el cifrado pero no almacena la clave.
   - Eres responsable de la gestión de las claves fuera de AWS.

#### Cifrado del Lado del Cliente (Client-Side Encryption)

- Tú cifras los datos antes de enviarlos a AWS.
- Tienes control total sobre el proceso y las claves.

#### Cifrado en otros servicios

- **Amazon EBS**: El cifrado de volúmenes protege los datos en reposo, en tránsito entre la instancia y el volumen, y en las instantáneas (**Snapshots**).
- **Amazon RDS**: Cifra la instancia de base de datos, las copias de seguridad automatizadas, las réplicas de lectura y las instantáneas. Debe habilitarse al crear la base de datos.
- **Amazon DynamoDB**: Todos los datos están cifrados en reposo por defecto mediante claves de AWS o de **KMS**.

### Cifrado en Tránsito (Encryption in Transit)

Protege los datos mientras se mueven a través de la red utilizando protocolos **SSL/TLS**.

#### Mejores Prácticas de TLS/SSL

- **Usar TLS 1.2 o superior**: Las versiones 1.0 y 1.1 están obsoletos y son inseguras.
- Configura siempre la versión mínima de **TLS** en tus servicios.
- Utiliza conjuntos de cifrado (**Cipher Suites**) fuertes.
- Actualiza regularmente los certificados **SSL/TLS**.

#### AWS Certificate Manager (ACM)

- Proporciona certificados **SSL/TLS** públicos y privados gratuitos para su uso con servicios de AWS.
- **Gestión Automática**: Renovación y despliegue automático de certificados.
- **Integración**: Funciona nativamente con **CloudFront**, **ALB**, **API Gateway** y **AppSync**.

#### Cifrado en Conexiones de Red

- **AWS Site-to-Site VPN**: Crea un túnel cifrado **IPsec** sobre el internet público. Utiliza **IKE** para la gestión de claves y soporta múltiples algoritmos de cifrado.
- **AWS Client VPN**: Servicio gestionado basado en cliente que utiliza cifrado **TLS**. Integrado con **Active Directory** y **MFA**.
- **Direct Connect con MACsec**: Proporciona cifrado de Capa 2 punto a punto en conexiones dedicadas de alta velocidad.

---

### AWS Key Management Service (KMS) - Análisis Profundo

**KMS** es un servicio gestionado para crear y controlar claves criptográficas.

#### Conceptos de Claves de KMS

1. **Customer Master Keys (CMKs)**:
   - Recurso principal de **KMS**.
   - Puede cifrar/descifrar hasta 4 KB de datos.
   - Usada para generar claves de datos (**Data Keys**).

2. **Tipos de CMKs**:
   - **AWS Managed CMK**: Creada por AWS para servicios específicos (ej. `aws/s3`). Gratis. Rotación anual automática. No se puede borrar.
   - **Customer Managed CMK**: Creada por ti. Control total sobre políticas de acceso y rotación. Coste: $1/mes. Rotación anual opcional.
   - **AWS Owned CMK**: Usada internamente por AWS. No visible en tu cuenta. Gratis.

3. **Políticas de Clave (Key Policies)**:
   - Controlan el acceso a las claves de **KMS**.
   - No puedes usar una clave de **KMS** sin una política de clave, incluso si tienes permisos de **IAM**.

#### Casos de Uso Avanzados

**Compartir Claves entre Cuentas (Cross-Account Key Sharing):**
- La Cuenta A (propietaria de la clave) otorga permisos a la Cuenta B (usuario de la clave) en la política de clave.
- La Cuenta B otorga permisos a sus usuarios en sus propias políticas de **IAM**.
- Permite descifrar objetos de **S3** o volúmenes de **EBS** compartidos de forma segura.

**Cifrado de Sobre (Envelope Encryption):**
- Estrategia para cifrar archivos grandes:
  1. **KMS** genera una **Data Key**.
  2. La **Data Key** cifra el archivo.
  3. **KMS** cifra la **Data Key** con la **CMK**.
  4. Se almacena el archivo cifrado junto con la **Data Key** cifrada.

---

## Identidad y Federación de Accesos (Federation)

La federación permite que los usuarios externos accedan a los recursos de AWS de forma segura sin tener que crear un usuario de **IAM**.

### Estrategias de Federación

#### 1. Federación con SAML 2.0 (Directorio Activo)
- Permite el **Single Sign-On (SSO)** para usuarios empresariales.
- El usuario se autentica contra su proveedor de identidad local (**IdP**), como **Active Directory**.
- El **IdP** envía una aserción **SAML** a AWS.
- AWS otorga credenciales temporales mediante **AWS STS** (**Security Token Service**).

#### 2. AWS Managed Microsoft AD
- Directorio de **Active Directory** real gestionado por AWS.
- Permite extender tu directorio local a la nube.
- Soporta relaciones de confianza (**Trust Relationships**).

#### 3. AD Connector
- Un "proxy" que redirige las solicitudes de directorio a tu **Active Directory** local.
- No se almacenan datos en AWS.
- Ideal para usar el directorio local existente sin migrarlo.

#### 4. Simple AD
- Directorio independiente basado en **Samba 4**.
- Proporciona características básicas de **AD**.
- No permite relaciones de confianza con el directorio local.

### Amazon Cognito

- Servicio para gestionar el registro, inicio de sesión y control de acceso de usuarios en aplicaciones móviles y web.
- **User Pools**: Directorio de usuarios para la autenticación (ej. iniciar sesión con email).
- **Identity Pools**: Otorgan credenciales temporales de AWS a los usuarios para que puedan acceder a otros servicios de AWS.

---

## Estrategias de Acceso Entre Cuentas (Cross-Account)

#### 1. Roles de IAM (Recomendado)
- La Cuenta A (que confía) crea un **Role** con una política de confianza que apunta a la Cuenta B.
- La Cuenta B (en la que se confía) asume el rol mediante la acción `sts:AssumeRole`.
- No hay credenciales permanentes que gestionar.

#### 2. Políticas Basadas en Recursos
- Se adjuntan directamente al recurso (ej. **S3 Bucket Policy**, **SQS Queue Policy**).
- Permiten acceso directo desde otra cuenta sin tener que asumir un rol.

---

## Gestión Multi-Cuenta con AWS Organizations

**AWS Organizations** permite consolidar y gestionar centralizadamente múltiples cuentas de AWS.

### Service Control Policies (SCPs)

- Las **SCPs** definen los permisos máximos (barreras de protección) para todas las cuentas de la organización.
- **Importante**: Las **SCPs** no otorgan permisos; solo los limitan. Para que una acción esté permitida, debe estar permitida tanto por la **SCP** como por la política de **IAM** local.
- Se aplican a nivel de cuenta, de Unidad Organizativa (**OU**) o de toda la organización.
- Afectan a todos los usuarios y roles, incluido el usuario **Root** de las cuentas miembro.

**Caso de Uso**: Impedir que los usuarios de cualquier cuenta de la organización eliminen los logs de **CloudTrail** o deshabiliten servicios de seguridad críticos.

---

## Servicios de Seguridad Avanzada

### Protección de Red y Aplicaciones

#### AWS WAF (Web Application Firewall)
- Protege tus aplicaciones web contra exploits comunes (Capa 7).
- Permite crear reglas para bloquear tráfico basándose en:
  - Direcciones IP.
  - Cabeceras HTTP.
  - Cuerpo de la solicitud.
  - Reglas de tasa (**Rate-based rules**).
- Despliegue en: **CloudFront**, **ALB**, **API Gateway**, **AppSync**.

#### AWS Shield
- Servicio gestionado de protección contra **DDoS** (Capa 3 y 4).
- **Shield Standard**: Gratuito. Protección automática contra ataques comunes.
- **Shield Advanced**: Servicio de pago. Protección avanzada, visibilidad 24/7 y acceso al equipo de respuesta de AWS (**DRT**). Incluye protección financiera contra picos de costos por DDoS.

#### AWS Firewall Manager
- Servicio de gestión centralizada para configurar y gestionar reglas de firewall en todas tus cuentas.
- Gestiona: **WAF**, **Shield Advanced**, **VPC Security Groups** y **AWS Network Firewall**.

### Detección y Monitoreo de Amenazas

#### Amazon GuardDuty
- Servicio de detección de amenazas inteligente que monitorea continuamente tus cuentas en busca de actividad maliciosa.
- Utiliza **Machine Learning** y detección de anomalías.
- Analiza: **VPC Flow Logs**, Logs de **CloudTrail**, Logs de **DNS**.
- No requiere agentes.

#### Amazon Inspector
- Servicio de gestión de vulnerabilidades que escanea continuamente tus cargas de trabajo.
- Escanea: Instancias **EC2**, imágenes de contenedores en **ECR**, funciones **Lambda**.
- Identifica vulnerabilidades de software y exposición de red no deseada.

#### Amazon Macie
- Servicio de seguridad de datos que utiliza aprendizaje automático para descubrir y proteger datos sensibles en **Amazon S3**.
- Identifica automáticamente **PII** (Información de Identificación Personal).
- Alerta sobre buckets públicos o no cifrados.

---

## Cumplimiento y Gobernanza (Compliance & Governance)

### AWS Artifact
- Portal de autoservicio para informes de cumplimiento.
- Acceso a informes **ISO**, **SOC**, **PCI DSS**, etc.
- Permite aceptar acuerdos como el de **HIPAA**.

### AWS Config
- Evalúa, audita y califica las configuraciones de los recursos.
- Mantiene un historial de cambios.
- **Config Rules**: Verifica automáticamente si los recursos cumplen con las configuraciones deseadas.

### AWS CloudTrail
- Registra todas las acciones de la API de AWS.
- Responde a: "¿Quién hizo qué, cuándo y desde dónde?".
- Esencial para auditoría y respuesta a incidentes.

---

## Respuesta a Incidentes de Seguridad

AWS proporciona un marco y herramientas para responder a incidentes.

### Pasos en la Respuesta a Incidentes

1. **Detección**: Identificar el incidente mediante **GuardDuty**, **Security Hub** o alarmas de **CloudWatch**.
2. **Contención**: Limitar el alcance del incidente (ej. aislar una instancia **EC2** cambiando su **Security Group** para bloquear todo el tráfico).
3. **Investigación**: Analizar los logs de **CloudTrail** y **VPC Flow Logs** para entender el origen. Realizar análisis forense en snapshots de **EBS**.
4. **Remediación**: Eliminar la amenaza y restaurar el servicio.
5. **Post-incidente**: Analizar las lecciones aprendidas y mejorar las políticas de seguridad.

---

## Errores Comunes de Seguridad y Cómo Evitarlos

A continuación se detallan los errores más comunes cometidos por los clientes y las recomendaciones para evitarlos:

1. **Uso excesivo del usuario Root**: Evítalo creando usuarios de **IAM** y usando **MFA**.
2. **Access Keys en el código**: Nunca guardes claves de acceso en el código fuente o en archivos de configuración. Usa **IAM Roles**.
3. **Políticas de IAM demasiado permisivas**: Sigue siempre el **Principio de Privilegio Mínimo**.
4. **Buckets de S3 públicos**: Usa **S3 Block Public Access** a nivel de cuenta.
5. **No rotar credenciales**: Configura la rotación automática en **KMS** y **Secrets Manager**.
6. **No habilitar MFA**: Obligatorio para cuentas con privilegios.

---

## Lista de Verificación de Seguridad para el Examen

- [ ] Entender el Modelo de Responsabilidad Compartida (Seguridad DE vs EN la nube).
- [ ] Diferenciar entre Usuarios, Grupos, Roles y Políticas de **IAM**.
- [ ] Conocer las mejores prácticas de **IAM** (Privilegio Mínimo, MFA, Roles).
- [ ] Identificar los servicios de detección (**GuardDuty**, **Inspector**, **Macie**).
- [ ] Identificar los servicios de protección de red (**WAF**, **Shield**, **Firewall Manager**).
- [ ] Entender el propósito de **KMS** (Gestión de claves) y **CloudTrail** (Auditoría de API).
- [ ] Conocer el uso de **AWS Artifact** para descargar informes de cumplimiento.

---

## Preguntas de Repaso (Review Questions)

**1. Según el Modelo de Responsabilidad Compartida, ¿quién es responsable del parcheado del sistema operativo de una instancia EC2?**
- A. AWS.
- B. El cliente.
- C. Ambos.
- D. El proveedor del sistema operativo.
*(Respuesta: B)*

**2. ¿Qué servicio usarías para identificar automáticamente números de tarjetas de crédito almacenados en S3?**
- A. **Amazon GuardDuty**.
- B. **Amazon Inspector**.
- C. **Amazon Macie**.
- D. **AWS Config**.
*(Respuesta: C)*

**3. ¿Qué componente de IAM es el más adecuado para otorgar permisos a una aplicación que se ejecuta en AWS?**
- A. Usuario de **IAM**.
- B. Grupo de **IAM**.
- C. Rol de **IAM**.
- D. Usuario **Root**.
*(Respuesta: C)*

**4. ¿Qué servicio te permite bloquear el acceso a tu aplicación web desde una dirección IP específica?**
- A. **AWS Shield**.
- B. **AWS WAF**.
- C. **Amazon GuardDuty**.
- D. **AWS CloudTrail**.
*(Respuesta: B)*

**5. ¿Cuál es la principal ventaja de usar AWS Organizations con SCPs?**
- A. Permite crear usuarios de **IAM** automáticamente.
- B. Establece barreras de seguridad centralizadas para múltiples cuentas.
- C. Ofrece descuentos automáticos en todos los servicios.
- D. Cifra todos los datos en reposo por defecto.
*(Respuesta: B)*

---

[← Anterior: Conceptos de la Nube](02-cloud-concepts.md) | [Volver al Inicio](README.md) | [Siguiente: Tecnología y Servicios en la Nube →](04-technology-services.md)
