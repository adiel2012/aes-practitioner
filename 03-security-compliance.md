# Dominio 2: Seguridad y Cumplimiento (30%)

[← Anterior: Conceptos de la Nube](02-cloud-concepts.md) | [Volver al Inicio](README.md) | [Siguiente: Tecnología y Servicios en la Nube →](04-technology-services.md)

---

## Modelo de Responsabilidad Compartida de AWS (AWS Shared Responsibility Model)

> **CRÍTICO PARA EL EXAMEN:** Este es uno de los conceptos más importantes. Comprende qué gestiona AWS frente a lo que gestiona el cliente.

El **Shared Responsibility Model** divide las responsabilidades de seguridad entre AWS y el cliente. Piénsalo como "**Security OF the Cloud**" (AWS) frente a "**Security IN the Cloud**" (Cliente).

### Responsabilidad de AWS: Seguridad de la Nube (Security OF the Cloud)

AWS es responsable de proteger la infraestructura que ejecuta todos los servicios:

- **Seguridad física de los centros de datos**
  - Controles de acceso a los edificios
  - Personal de seguridad
  - Salvaguardas ambientales (climatización, incendios)

- **Hardware y componentes de red**
  - Servidores físicos
  - Dispositivos de almacenamiento
  - Equipos de red (routers, switches)

- **Infraestructura de cómputo, almacenamiento, bases de datos y redes**
  - Capa del hipervisor (Virtualización)
  - Infraestructura de servicios gestionados

- **Infraestructura global de AWS (AWS global infrastructure)**
  - **Regions**
  - **Availability Zones**
  - **Edge Locations**

- **Servicios gestionados**
  - **RDS**, **DynamoDB**, **Lambda**, etc.
  - AWS se encarga del parcheo del sistema operativo (**OS**) y el mantenimiento de estos servicios.

### Responsabilidad del Cliente: Seguridad en la Nube (Security IN the Cloud)

Los clientes son responsables de:

- **Datos del cliente (Customer data)**
  - Todos los datos que almacenas en AWS
  - Clasificación y protección de los mismos

- **Plataforma, aplicaciones, gestión de identidad y acceso (Identity and Access Management - IAM)**
  - Código de la aplicación
  - Usuarios, grupos, roles y políticas de **IAM**

- **Configuración del sistema operativo, red y firewall**
  - Parches y actualizaciones del **OS** (para **EC2**)
  - Reglas de **Security Groups**
  - **Network ACLs** (**NACLs**)

- **Client-side data encryption e integridad de datos**
  - Cifrar los datos antes de subirlos
  - Validación de datos

- **Cifrado del lado del servidor (Server-side encryption - sistema de archivos y/o datos)**
  - Cifrado en reposo (**at rest**)
  - Elección de la gestión de claves

- **Protección del tráfico de red (Network traffic protection)**
  - Cifrado en tránsito (HTTPS, TLS)
  - Seguridad de la red

- **Configuración de Security Groups**
  - Reglas de firewall
  - Controles de acceso

- **Gestión de acceso de usuarios (User access management)**
  - Creación y gestión de usuarios
  - Políticas de contraseñas
  - Aplicación de **MFA**

### Controles Compartidos (Shared Controls)

Tanto AWS como los clientes tienen responsabilidades sobre:

| Control | Responsabilidad de AWS | Responsabilidad del Cliente |
|---------|-------------------|------------------------|
| **Patch Management** | Parchea los componentes de la infraestructura | Parchea el **guest OS** y las aplicaciones |
| **Configuration Management** | Configura los dispositivos de infraestructura | Configura las bases de datos y aplicaciones |
| **Awareness and Training** | Entrena a los empleados de AWS | Entrena a su propio personal |

> **Consejo para el Examen:** Para cualquier pregunta de seguridad, pregunta: "¿Quién es el responsable?". AWS maneja la infraestructura; tú manejas lo que pones en la nube.

---

## Gestión de Identidad y Acceso de AWS (AWS Identity and Access Management - IAM)

**IAM** te permite controlar de forma segura el acceso a los servicios y recursos de AWS. Es un **global service** (no depende de una región) y su uso es **gratuito**.

### Componentes Principales

#### Usuarios (Users)

- **Personas o servicios individuales**
- Operadores con nombre permanente
- Pueden tener credenciales a largo plazo:
  - Contraseña (para acceso a la consola)
  - **Access keys** (para acceso programático/CLI)
- Debe representar a una persona física o una aplicación
- Por defecto, los nuevos usuarios NO tienen permisos

#### Grupos (Groups)

- **Colección de usuarios**
- Simplifica la gestión de permisos
- Características clave:
  - Los grupos no pueden anidarse
  - Los usuarios pueden pertenecer a múltiples grupos
  - Aplicar políticas a grupos para una gestión más fácil
  - No hay grupos por defecto

#### Roles de IAM (IAM Roles)

- **Credenciales temporales** para usuarios, aplicaciones o servicios
- Sin nombre de usuario/contraseña ni **access keys**
- Pueden ser asumidos por cualquiera que lo necesite
- **Best practice** para instancias **EC2** que acceden a servicios de AWS
- Se pueden usar para acceso entre cuentas (**cross-account**)
- Las credenciales de seguridad temporales se rotan automáticamente

#### Políticas (Policies)

- **Documentos JSON** que definen permisos
- Se adjuntan a usuarios, grupos o roles
- Definen qué acciones están permitidas o denegadas en qué recursos
- Siguen el **principle of least privilege** (mínimo privilegio)
- Dos tipos principales:
  - **AWS Managed Policies:** Creadas y mantenidas por AWS
  - **Customer Managed Policies:** Creadas y mantenidas por ti

**Ejemplo de Estructura de Política:**
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::my-bucket/*"
    }
  ]
}
```

### Ejemplos Detallados de Políticas IAM

Comprender las políticas de IAM es fundamental para el examen. Aquí tienes escenarios comunes.

#### Ejemplo 1: Acceso de Solo Lectura a un Bucket de S3 Específico

Esta política otorga acceso de solo lectura a los objetos en un bucket de S3 específico.

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "S3ReadOnlyAccess",
      "Effect": "Allow",
      "Action": [
        "s3:GetObject",
        "s3:GetObjectVersion",
        "s3:ListBucket"
      ],
      "Resource": [
        "arn:aws:s3:::company-reports/*",
        "arn:aws:s3:::company-reports"
      ]
    }
  ]
}
```

**Puntos Clave:**
- `Sid`: ID de la declaración (opcional, para documentación)
- `Effect`: Puede ser "Allow" (Permitir) o "Deny" (Denegar)
- `Action`: Qué operaciones están permitidas
- `Resource`: A qué recursos de AWS se aplica la política

#### Ejemplo 2: Gestión de Instancias EC2 con Condiciones

Esta política permite iniciar y detener instancias **EC2** solo durante el horario comercial.

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "ec2:StartInstances",
        "ec2:StopInstances",
        "ec2:DescribeInstances"
      ],
      "Resource": "*",
      "Condition": {
        "DateGreaterThan": {
          "aws:CurrentTime": "2024-01-01T08:00:00Z"
        },
        "DateLessThan": {
          "aws:CurrentTime": "2024-12-31T18:00:00Z"
        },
        "IpAddress": {
          "aws:SourceIp": [
            "203.0.113.0/24",
            "198.51.100.0/24"
          ]
        }
      }
    }
  ]
}
```

**Elementos de Condición:**
- Restricciones basadas en el tiempo
- Restricciones de dirección IP
- Requisitos de **MFA**
- Restricciones de la **VPC** de origen

#### Ejemplo 3: Política de Denegación para Acciones Sensibles

Esta política deniega explícitamente la eliminación de recursos de producción (Deny siempre anula Allow).

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Deny",
      "Action": [
        "rds:DeleteDBInstance",
        "ec2:TerminateInstances",
        "s3:DeleteBucket"
      ],
      "Resource": "*",
      "Condition": {
        "StringEquals": {
          "aws:ResourceTag/Environment": "Production"
        }
      }
    }
  ]
}
```

**Importante:** Un Denegar explícito siempre gana sobre un Permitir en la evaluación de políticas de IAM.

#### Ejemplo 4: Política con Requisito de MFA

Esta política requiere **MFA** para operaciones sensibles.

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "ec2:TerminateInstances",
        "rds:DeleteDBInstance"
      ],
      "Resource": "*",
      "Condition": {
        "BoolIfExists": {
          "aws:MultiFactorAuthPresent": "true"
        }
      }
    }
  ]
}
```

#### Ejemplo 5: Política de Acceso entre Cuentas (Cross-Account)

Esta política permite asumir un rol desde otra cuenta de AWS.

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": {
        "AWS": "arn:aws:iam::123456789012:root"
      },
      "Action": "sts:AssumeRole",
      "Condition": {
        "StringEquals": {
          "sts:ExternalId": "UniqueExternalId123"
        }
      }
    }
  ]
}
```

**Caso de Uso:** Permitir que los usuarios de la Cuenta A accedan a recursos en la Cuenta B.

#### Ejemplo 6: Acceso de Administrador Completo

Esta política otorga acceso total a todos los servicios de AWS (usar con precaución extrema).

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "*",
      "Resource": "*"
    }
  ]
}
```

**Advertencia:** Solo asignar a administradores de confianza. Esto es equivalente al acceso **root**.

#### Ejemplo 7: Acceso de Solo Lectura en Todos los Servicios

Esta política proporciona acceso de solo lectura con fines de auditoría.

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "ec2:Describe*",
        "s3:Get*",
        "s3:List*",
        "rds:Describe*",
        "cloudwatch:Get*",
        "cloudwatch:List*",
        "cloudtrail:LookupEvents"
      ],
      "Resource": "*"
    }
  ]
}
```

#### Ejemplo 8: S3 Bucket Policy para Acceso de Lectura Público

Esta es una política basada en recursos adjunta a un bucket de S3.

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "PublicReadGetObject",
      "Effect": "Allow",
      "Principal": "*",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::my-public-website/*"
    }
  ]
}
```

**Caso de Uso:** Alojamiento de un sitio web estático o contenido público.

#### Ejemplo 9: Service Control Policy (SCP)

Esta **SCP** evita que cualquier persona en una **OU** abandone la organización.

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Deny",
      "Action": [
        "organizations:LeaveOrganization"
      ],
      "Resource": "*"
    }
  ]
}
```

**Importante:** Las **SCPs** afectan a todos los usuarios y roles, incluido el usuario **root** de la cuenta.

#### Ejemplo 10: Control de Acceso Basado en Etiquetas (Tag-Based)

Esta política permite acciones solo en recursos con etiquetas específicas.

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "ec2:StartInstances",
        "ec2:StopInstances"
      ],
      "Resource": "arn:aws:ec2:*:*:instance/*",
      "Condition": {
        "StringEquals": {
          "aws:ResourceTag/Owner": "${aws:username}",
          "aws:ResourceTag/Department": "Engineering"
        }
      }
    }
  ]
}
```

**Caso de Uso:** Los usuarios solo pueden gestionar sus propios recursos dentro de su departamento.

### Mejores Prácticas de IAM

1. **Protección de la cuenta root**
   - Úsala solo para la configuración inicial, luego guárdala bajo llave.
   - Habilita **MFA** en la cuenta **root**.
   - No crees **access keys** para **root**.
   - Crea usuarios de IAM individuales en su lugar.

2. **Principle of Least Privilege** (Mínimo Privilegio)
   - Otorga solo los permisos requeridos para realizar una tarea.
   - Comienza con permisos mínimos y añade según sea necesario.
   - Revisa y elimina regularmente los permisos innecesarios.

3. **Usa Grupos para la gestión de permisos**
   - Asigna permisos a grupos, no a usuarios individuales.
   - Añade usuarios a los grupos apropiados.
   - Es más fácil de gestionar y auditar.

4. **Habilita MFA (Multi-Factor Authentication)**
   - Especialmente para usuarios privilegiados.
   - Obligatorio para la cuenta **root**.
   - Añade una capa extra de seguridad.

5. **Usa Roles para aplicaciones**
   - Para aplicaciones que se ejecutan en **EC2**.
   - Mejor que incrustar credenciales.
   - Rotación automática de credenciales.

6. **Rota las credenciales regularmente**
   - Cambia las contraseñas periódicamente.
   - Rota las **access keys**.
   - Establece políticas de expiración de contraseñas.

7. **Elimina credenciales innecesarias**
   - Elimina usuarios no utilizados.
   - Elimina roles no utilizados.
   - Desactiva las **access keys** antiguas.

8. **Usa condiciones de política para seguridad extra**
   - Restricciones de dirección IP.
   - Acceso basado en el tiempo.
   - Requisitos de **MFA**.
   - Restricciones de la **VPC** de origen.

---

## Análisis Profundo de las Mejores Prácticas de Seguridad (Security Best Practices)

Mejores prácticas de seguridad integrales que debes conocer para el examen y el uso real de AWS.

### 1. Seguridad en la Gestión de Identidad y Acceso (IAM Security)

#### Implementar el Acceso de Mínimo Privilegio (Least Privilege)

**Qué significa:** Otorgar solo los permisos necesarios para realizar las tareas requeridas, nada más.

**Cómo implementarlo:**
- Comienza con cero permisos y añade lo que sea necesario.
- Usa las políticas gestionadas de AWS como punto de partida y luego personaliza.
- Revisa y audita regularmente los permisos.
- Usa **IAM Access Analyzer** para identificar permisos no utilizados.
- Elimina permisos que no se hayan usado en más de 90 días.

**Escenario de Ejemplo:** Un desarrollador necesita leer los logs de una aplicación desde S3. Dale `s3:GetObject` en el bucket específico, no acceso total a S3 ni derechos de administrador.

**Consejo para el Examen:** Las preguntas evaluarán si puedes identificar políticas excesivamente permisivas.

#### Implementar Políticas de Contraseñas Fuertes

**Requisitos:**
- Longitud mínima: 14+ caracteres (AWS permite de 8 a 128).
- Requerir mayúsculas, minúsculas, números y símbolos.
- Evitar la reutilización de contraseñas (recordar al menos 24 contraseñas anteriores).
- Imponer la expiración de contraseñas (se recomiendan 60-90 días).
- Evitar que los usuarios cambien su contraseña con demasiada frecuencia.

**Configuraciones de Política de Contraseñas de AWS:**
```
- Longitud mínima de la contraseña: 14 caracteres
- Requerir al menos una letra mayúscula: Sí
- Requerir al menos una letra minúscula: Sí
- Requerir al menos un número: Sí
- Requerir al menos un carácter no alfanumérico: Sí
- Permitir que los usuarios cambien su propia contraseña: Sí
- Habilitar la expiración de la contraseña: Sí (90 días)
- La expiración de la contraseña requiere reinicio por administrador: No
- Número de contraseñas a recordar: 24
```

#### Mejores Prácticas de Gestión de Credenciales

**Nunca hagas esto:**
- Incrustar credenciales en el código de la aplicación (**hard-coding**).
- Almacenar credenciales en el control de versiones (**Git**).
- Compartir credenciales entre usuarios o aplicaciones.
- Enviar credenciales por correo electrónico o mensajería.
- Usar credenciales a largo plazo cuando hay credenciales temporales disponibles.

**Haz siempre esto:**
- Usa **IAM Roles** para instancias **EC2** y funciones **Lambda**.
- Usa **AWS Secrets Manager** o **Systems Manager Parameter Store** para secretos.
- Rota las credenciales regularmente (**access keys** cada 90 días).
- Usa credenciales temporales a través de **AWS STS**.
- Elimina las credenciales no utilizadas inmediatamente.

#### Habilitar AWS CloudTrail en todas las regiones

**Por qué es crítico:**
- Proporciona un rastro de auditoría de todas las llamadas a la API.
- Ayuda con los requisitos de cumplimiento.
- Permite el análisis de seguridad y la resolución de problemas.
- Detecta intentos de acceso no autorizados.
- Necesario para la respuesta ante incidentes.

**Configuración:**
- Habilítalo en todas las regiones (incluso en las que no usas).
- Habilita la validación de archivos de log para integridad.
- Almacena los logs en un bucket de S3 separado y seguro.
- Habilita el control de versiones en el bucket de S3 de almacenamiento de logs.
- Restringe el acceso a los logs de **CloudTrail**.
- Configura la integración con **CloudWatch Logs** para monitoreo en tiempo real.

### 2. Mejores Prácticas de Seguridad de Red (Network Security)

#### Usar los Security Groups Correctamente

**Principios Clave:**
- Los **Security Groups** tienen estado (**stateful**): el tráfico de retorno se permite automáticamente.
- Denegación por defecto: Permite solo lo que sea necesario.
- Usa nombres descriptivos y etiquetas (**tags**).
- Referencia a otros **Security Groups** en lugar de direcciones IP cuando sea posible.
- Separa los **Security Groups** por capa (**tier**): web, aplicación, base de datos.

**Patrones Comunes:**

**Web Tier Security Group:**
```
Entrada:
- Puerto 80 (HTTP) desde 0.0.0.0/0
- Puerto 443 (HTTPS) desde 0.0.0.0/0
- Puerto 22 (SSH) solo desde Bastion-SG

Salida:
- Todo el tráfico (por defecto)
```

**Application Tier Security Group:**
```
Entrada:
- Puerto 8080 desde Web-Tier-SG
- Puerto 22 solo desde Bastion-SG

Salida:
- Puerto 3306 hacia Database-SG
- Puerto 443 hacia 0.0.0.0/0 (para llamadas a la API)
```

**Database Tier Security Group:**
```
Entrada:
- Puerto 3306 (MySQL) solo desde App-Tier-SG
- Puerto 22 solo desde Bastion-SG

Salida:
- Ninguna (lo más restrictivo)
```

#### Mejores Prácticas de Network ACL (NACL)

**Diferencias con los Security Groups:**
- Sin estado (**stateless**): debes permitir el tráfico de retorno explícitamente.
- Se aplica a nivel de subred.
- Las reglas se procesan en orden numérico.
- Puede tener reglas de DENEGACIÓN explícitas.

**Cuándo usar NACLs:**
- Bloquear direcciones IP específicas (los **Security Groups** no pueden denegar).
- Añadir una capa adicional de defensa.
- Cumplir con requisitos regulatorios de segmentación de red.

**Mejor Práctica:**
- Usa los **Security Groups** como defensa principal.
- Usa las **NACLs** para protección adicional a nivel de subred.
- Deja espacio entre los números de las reglas (100, 200, 300) para inserciones.
- Documenta todas las reglas personalizadas de las **NACLs**.

#### Implementar VPC Flow Logs

**Qué capturan:**
- Tráfico aceptado y rechazado.
- Direcciones IP de origen y destino.
- Puertos y protocolos.
- Recuentos de paquetes y bytes.

**Casos de uso:**
- Resolución de problemas de conectividad.
- Monitoreo de patrones de tráfico.
- Detección de comportamiento anómalo.
- Cumplir con requisitos de cumplimiento.
- Forense de seguridad.

**Configuración:**
- Habilítalo a nivel de **VPC**, subred o **ENI**.
- Publícalo en **CloudWatch Logs** o **S3**.
- Úsalo para análisis con **Amazon Athena**.
- Intégralo con herramientas de seguridad.

### 3. Mejores Prácticas de Protección de Datos (Data Protection)

#### Cifrar Datos en Reposo (at rest)

**Servicios con cifrado:**
- **S3**: SSE-S3, SSE-KMS, SSE-C
- **EBS**: Volúmenes cifrados
- **RDS**: Bases de datos cifradas
- **DynamoDB**: Cifrado en reposo
- **Redshift**: Clusters cifrados

**Mejores prácticas:**
- Habilita el cifrado por defecto para todos los nuevos recursos.
- Usa **AWS KMS** para la gestión de claves.
- Implementa la rotación automática de claves.
- Separa las claves para diferentes clasificaciones de datos.
- Otorga permisos mínimos para descifrar.

#### Cifrar Datos en Tránsito (in transit)

**Cómo implementarlo:**
- Usa HTTPS/TLS para todo el tráfico web.
- Usa SSL/TLS para las conexiones a bases de datos.
- Usa **VPN** o **Direct Connect** para la conectividad híbrida.
- Habilita el cifrado para todas las transferencias de datos.
- Usa **AWS Certificate Manager** (**ACM**) para certificados SSL/TLS.

**Servicios que imponen el cifrado en tránsito:**
- **CloudFront** (puede requerir HTTPS)
- **API Gateway** (solo HTTPS)
- **Application Load Balancer** (terminación SSL/TLS)
- **S3 Transfer Acceleration** (HTTPS)

#### Implementar Copia de Seguridad y Recuperación (Backup and Recovery)

**Mejores prácticas:**
- Habilita las copias de seguridad automatizadas para las bases de datos.
- Usa **AWS Backup** para la gestión centralizada de copias de seguridad.
- Almacena copias de seguridad en una región diferente para recuperación ante desastres (**DR**).
- Prueba los procedimientos de restauración regularmente.
- Implementa el control de versiones para los objetos de **S3**.
- Usa políticas de ciclo de vida para gestionar la retención de copias de seguridad.

### 4. Mejores Prácticas de Monitorización y Registro (Monitoring and Logging)

#### Implementar Logging Integral

**Logs esenciales para habilitar:**
- **CloudTrail**: Actividad de la API
- **VPC Flow Logs**: Tráfico de red
- **S3 Server Access Logs**: Acceso al bucket de S3
- **ELB Access Logs**: Solicitudes del equilibrador de carga
- **CloudFront Access Logs**: Solicitudes de la CDN
- **RDS Logs**: Consultas y errores de base de datos

**Gestión de logs:**
- Centraliza los logs en una cuenta dedicada.
- Habilita la validación de integridad de los archivos de log.
- Implementa políticas de retención de logs.
- Protege los logs contra la eliminación o modificación.
- Usa **CloudWatch Logs Insights** para el análisis.

#### Configurar Alertas de Seguridad

**Alertas críticas para configurar:**
- Uso de la cuenta **root**.
- Cambios en las políticas de **IAM**.
- Cambios en los **Security Groups**.
- Cambios en las **Network ACLs**.
- Intentos fallidos de inicio de sesión (múltiples).
- Llamadas a la API no autorizadas.
- Cambios en la configuración de **CloudTrail**.
- Cambios en la política del bucket de **S3**.
- Eliminaciones de claves de cifrado.

**Mecanismos de alerta:**
- Alarmas de **CloudWatch**.
- Notificaciones de **SNS**.
- Reglas de **EventBridge**.
- Hallazgos de **GuardDuty**.
- Alertas de **Security Hub**.

### 5. Mejores Prácticas de Cumplimiento y Gobernanza (Compliance and Governance)

#### Implementar Verificación Automática de Cumplimiento

**Herramientas para usar:**
- **AWS Config Rules** para cumplimiento continuo.
- **AWS Security Hub** para una vista de seguridad centralizada.
- **AWS Systems Manager** para el cumplimiento de parches.
- **Trusted Advisor** para verificaciones de mejores prácticas.

**Reglas de cumplimiento comunes:**
- Asegurar que los buckets de **S3** no sean accesibles públicamente.
- Asegurar que el cifrado esté habilitado en todos los volúmenes.
- Asegurar que **MFA** esté habilitado para la cuenta **root**.
- Asegurar que **CloudTrail** esté habilitado en todas las regiones.
- Asegurar que se eliminen las credenciales de **IAM** no utilizadas.

#### Etiquetar Todo para la Gobernanza (Tagging)

**Etiquetas esenciales:**
- **Environment** (Producción, Staging, Dev)
- **Owner** (equipo o individuo)
- **Cost Center** (para facturación)
- **Project** (nombre de la aplicación o proyecto)
- **Compliance** (programas de cumplimiento requeridos)
- **Data Classification** (Público, Interno, Confidencial)

**Beneficios:**
- Asignación y seguimiento de costes.
- Aplicación de políticas automatizada.
- Organización de recursos.
- Informes de cumplimiento.
- Gestión del ciclo de vida.

### 6. Mejores Prácticas de Respuesta ante Incidentes (Incident Response)

#### Prepararse para Incidentes de Seguridad

**Ten un plan:**
- Documenta los procedimientos de respuesta a incidentes.
- Define roles y responsabilidades.
- Mantén listas de contacto.
- Establece canales de comunicación.
- Practica con ejercicios de simulación.

**Herramientas de AWS para respuesta a incidentes:**
- **CloudWatch** para monitoreo y alertas.
- **CloudTrail** para análisis forense.
- **VPC Flow Logs** para análisis de red.
- **AWS Systems Manager** para remediación automatizada.
- Instantánea de **EC2** (**snapshot**) para investigación forense.

**Procedimientos de aislamiento:**
- Cambiar el **Security Group** para denegar todo el tráfico.
- Crear una instantánea de los recursos afectados antes de la investigación.
- Aislar en una **VPC** o subred separada.
- Preservar los logs y las evidencias.
- Seguir los procedimientos de cadena de custodia.

### 7. Mejores Prácticas de Seguridad de Aplicaciones (Application Security)

#### Implementar Defensa en Profundidad (Defense in Depth)

**Múltiples capas de seguridad:**
1. Seguridad en el borde (**Edge security**): **CloudFront**, **AWS WAF**, **Shield**.
2. Seguridad de red: **VPC**, **Security Groups**, **NACLs**.
3. Seguridad de la aplicación: **IAM Roles**, cifrado.
4. Seguridad de los datos: Cifrado en reposo, controles de acceso.
5. Monitoreo: **CloudTrail**, **GuardDuty**, **CloudWatch**.

**Beneficios:**
- Sin punto único de fallo.
- Múltiples oportunidades para detectar y detener ataques.
- Reduce el radio de explosión de las brechas.
- Requisito de cumplimiento para muchos marcos de trabajo.

#### Asegurar los Endpoints de la API

**Seguridad de API Gateway:**
- Usa **API keys** para identificación.
- Implementa **throttling** (estrangulamiento) y limitación de tasa.
- Habilita **AWS WAF** para protección.
- Usa autorizadores de **Lambda** para autenticación personalizada.
- Implementa validación de solicitudes.
- Habilita **CloudWatch Logs** para el monitoreo.

**Mejores prácticas:**
- Usa solo HTTPS.
- Implementa autenticación y autorización adecuadas.
- Valida todas las entradas.
- Usa el mínimo privilegio para los roles de ejecución de **Lambda**.
- Habilita **CORS** correctamente (no uses *).
- Implementa el versionado de la API.

### 8. Mejores Prácticas de Seguridad de Terceros (Third-Party Security)

#### Gestionar el Acceso de Terceros de Forma Segura

**Usa IDs externos para el acceso entre cuentas:**
- Evita el problema del "confused deputy".
- Identificador único por cliente.
- Incluir en la condición de la política de **AssumeRole**.

**Mejores prácticas:**
- Usa **IAM Roles** en lugar de compartir credenciales.
- Implementa el acceso de mínimo privilegio.
- Requiere **MFA** para operaciones sensibles.
- Monitorea el acceso de terceros con **CloudTrail**.
- Audita y revisa el acceso regularmente.
- Elimina el acceso cuando ya no sea necesario.

#### Asegurar Cargas de Trabajo de Contenedores y Serverless

**Seguridad de contenedores:**
- Escanea las imágenes en busca de vulnerabilidades (**Amazon ECR scanning**).
- Usa imágenes base mínimas.
- No ejecutes contenedores como **root**.
- Implementa el mínimo privilegio para los roles de las tareas (**task roles**).
- Usa la gestión de secretos para las credenciales.
- Habilita el registro de **CloudTrail** para **ECR**.

**Seguridad de Lambda:**
- Usa roles de ejecución separados por función.
- Almacena secretos en **Secrets Manager** o **Parameter Store**.
- Habilita el acceso a la **VPC** solo cuando sea necesario.
- Implementa el cifrado a nivel de función.
- Usa variables de entorno para la configuración.
- Monitorea con **CloudWatch** y **X-Ray**.

### Autenticación de Múltiples Factores (Multi-Factor Authentication - MFA)

**MFA** añade una capa extra de protección más allá del nombre de usuario y la contraseña.

**Factores de Autenticación:**
- **Algo que sabes:** Contraseña.
- **Algo que tienes:** Dispositivo **MFA**.

**Opciones de Dispositivos MFA:**

| Tipo | Descripción | Caso de Uso |
|------|-------------|----------|
| **Virtual MFA Device** | Aplicación móvil (Google Authenticator, Authy) | Más común, conveniente |
| **Hardware MFA Device** | Token físico (YubiKey) | Entornos de alta seguridad |
| **SMS Text Message** | Código enviado vía SMS | No recomendado para cuenta **root** |

> **Mejor Práctica:** Habilita siempre **MFA** en la cuenta **root** y para todos los usuarios con acceso a la consola, especialmente aquellos con privilegios administrativos.

---

## Mejores Prácticas de Cifrado de Datos

### Cifrado en Reposo (Encryption at Rest)

El cifrado en reposo protege los datos almacenados en disco contra accesos no autorizados.

#### Opciones de Cifrado de Amazon S3

**Server-Side Encryption con S3-Managed Keys (SSE-S3):**
- AWS gestiona las claves de cifrado.
- Cifrado AES-256.
- Se habilita con un solo clic.
- Sin coste adicional.
- Cada objeto se cifra con una clave única.
- Ideal para: Requisitos de cifrado simples.

**Server-Side Encryption con KMS (SSE-KMS):**
- **AWS KMS** gestiona las claves de cifrado.
- Tú controlas las políticas de claves y la rotación.
- Rastro de auditoría vía **CloudTrail**.
- Coste adicional por solicitud.
- Cifrado de sobre (**envelope encryption**) para archivos grandes.
- Ideal para: Requisitos de cumplimiento, necesidades de auditoría.

**Server-Side Encryption con Customer-Provided Keys (SSE-C):**
- Tú gestionas las claves de cifrado fuera de AWS.
- AWS realiza el cifrado pero no almacena las claves.
- Debes proporcionar la clave con cada solicitud.
- Ideal para: Cuando debes controlar las claves fuera de AWS.

**Client-Side Encryption:**
- Cifras los datos antes de subirlos a **S3**.
- Tú gestionas todo el proceso de cifrado.
- AWS almacena los datos ya cifrados.
- Ideal para: Máximo control sobre el cifrado.

#### Cifrado de EBS

**Características:**
- Cifra los datos en reposo dentro del volumen.
- Cifra los datos en tránsito entre la instancia y el volumen.
- Cifra todas las instantáneas (**snapshots**) creadas a partir del volumen.
- Usa **AWS KMS** para la gestión de claves.
- Impacto mínimo en el rendimiento.
- No se puede cifrar el volumen raíz de una instancia existente (debe crearse una **AMI**).

#### Cifrado de RDS

**Qué se cifra:**
- Almacenamiento de la base de datos.
- Copias de seguridad automatizadas.
- Réplicas de lectura.
- Instantáneas (**snapshots**).
- Logs.

**Notas importantes:**
- Debe habilitarse al momento de crear la base de datos.
- No se puede cifrar una base de datos existente no cifrada.
- Solución: Crear instantánea, copiar con cifrado, restaurar.

#### Cifrado de DynamoDB

**Características:**
- Cifrado en reposo habilitado por defecto.
- Usa claves propiedad de AWS (por defecto, sin coste).
- Puede usar una clave gestionada por AWS (aws/dynamodb).
- Puede usar una clave **KMS** gestionada por el cliente.

### Cifrado en Tránsito (Encryption in Transit)

El cifrado en tránsito protege los datos que se mueven entre sistemas.

#### Mejores Prácticas de TLS/SSL

**Usar TLS 1.2 o superior:**
- TLS 1.0 y 1.1 están obsoletos.
- Configurar la versión mínima de TLS.
- Usar suites de cifrado fuertes.
- Actualizar regularmente los certificados SSL/TLS.

**AWS Certificate Manager (ACM):**
- Certificados SSL/TLS gratuitos.
- Renovación automática.
- Integración con **CloudFront**, **ALB**, **API Gateway**.
- Despliegue fácil y sin sobrecarga de gestión.

#### Cifrado de VPN

**AWS Site-to-Site VPN:**
- Conexión **IPsec VPN**.
- Túnel cifrado sobre Internet.
- Soporta múltiples algoritmos de cifrado.

**AWS Client VPN:**
- **VPN** basada en cliente gestionada.
- Basada en **OpenVPN** con cifrado TLS.

#### Cifrado de Direct Connect

**MACsec para Direct Connect:**
- Cifrado de Capa 2 punto a punto.
- Conexiones de 10 Gbps y 100 Gbps.

**VPN sobre Direct Connect:**
- **IPsec VPN** sobre una conexión **DX**.
- Cifrado de extremo a extremo.

### Gestión de Claves con AWS KMS

#### Tipos de Claves de KMS

**Symmetric Keys (Simétricas):**
- Misma clave para cifrar y descifrar (AES-256).
- Nunca sale de **KMS** sin cifrar.
- Usada por la mayoría de los servicios de AWS.

**Asymmetric Keys (Asimétricas):**
- Par de claves pública y privada (RSA o Curva Elíptica).
- La clave privada nunca sale de **KMS**.
- Usada para firma y verificación.

#### Customer Master Keys (CMKs)

**AWS Managed CMK:**
- Creada y gestionada por AWS.
- Rotación automática cada año. No se puede eliminar.
- Sin coste por la clave (solo uso).

**Customer Managed CMK:**
- Tú la creas y gestionas. Control total sobre las políticas.
- Rotación automática opcional (anual).
- Se puede habilitar/deshabilitar o eliminar (periodo de espera de 7-30 días).
- Coste: $1/mes más uso.

**AWS Owned CMK:**
- AWS la posee y gestiona. Sin visibilidad ni control por parte del cliente. Sin coste.

---

## Análisis Profundo de Network Security

### Arquitectura de Seguridad de la VPC

#### Ejemplo de Arquitectura Multi-Capa (Multi-Tier)

**Public Subnet (DMZ):**
- **Internet Gateway** adjunto. Direcciones IP públicas.
- **Bastion hosts** (servidores de salto).
- **NAT Gateways**.
- **Load balancers**.

**Private Subnet (Application Tier):**
- Sin acceso directo a Internet. Instancias **EC2** para aplicaciones.
- Ruta hacia el **NAT Gateway** para salida.
- Acceso solo vía el equilibrador de carga.

**Private Subnet (Database Tier):**
- Seguridad más restrictiva. **RDS**, endpoints de **DynamoDB**.
- Sin acceso a Internet (ni entrada ni salida).
- Acceso solo desde la capa de aplicación.

#### Mejores Prácticas de Segmentación de Red

**Estrategia de Subredes:**
- Separa las subredes por capa (web, app, data).
- Separa las subredes por entorno (prod, staging, dev).
- Usa al menos 2 **AZs** para alta disponibilidad (**HA**).

#### VPC Endpoints para Seguridad

**Interface Endpoints (PrivateLink):**
- Direcciones IP privadas en tu **VPC**.
- Soporta muchos servicios de AWS. Sin necesidad de **Internet Gateway**.

**Gateway Endpoints:**
- Entrada en la tabla de rutas. Gratuito.
- Soporta **S3** y **DynamoDB**.

**Beneficios:**
- El tráfico se mantiene dentro de la red de AWS.
- Sin exposición a Internet. Mejor rendimiento.

#### AWS PrivateLink

**Qué es:**
- Conectividad privada a servicios.
- El tráfico se mantiene en la red de AWS.
- Impulsado por los **Interface VPC Endpoints**.

---

## Identity Federation y SSO

### AWS IAM Identity Center (anteriormente AWS SSO)

**Qué proporciona:**
- **Single sign-on** a múltiples cuentas de AWS y aplicaciones de negocio.
- Gestión de usuarios centralizada y soporte de **MFA**.

### Federation con SAML 2.0

**Arquitectura de Federación SAML:**
```
Usuario → Identity Provider (IdP) → AWS STS → Credenciales Temporales → Recursos de AWS
```
**Proveedores de Identidad (IdP):** Okta, Azure AD, Ping Identity, etc.

### Amazon Cognito

- **User pools** para autenticación.
- **Identity pools** para obtener credenciales de AWS.
- Soporte para proveedores sociales (Google, Facebook).

---

## Procedimientos de Respuesta ante Incidentes de Seguridad

#### 1. Fase de Preparación
- Crear un plan de respuesta a incidentes y definir niveles de severidad.
- Configurar herramientas: **CloudTrail**, **VPC Flow Logs**, **GuardDuty**, **Security Hub**.

#### 2. Detección y Análisis
- Hallazgos de **GuardDuty**, alarmas de **CloudWatch**, alertas de **Security Hub**.
- Confirmar si el incidente es real e identificar los recursos afectados.

#### 3. Estrategias de Contención
- Aislar instancias comprometidas (cambiar **Security Group** a denegar todo).
- Revocar credenciales comprometidas y bloquear IPs maliciosas en las **NACLs**.
- Crear instantáneas (**snapshots**) para el análisis forense.

#### 4. Erradicación
- Eliminar malware, cerrar puertas traseras y parchear vulnerabilidades.

#### 5. Recuperación
- Restaurar operaciones desde copias de seguridad limpias.

#### 6. Actividad Post-Incidente
- Reunión de lecciones aprendidas y actualización de documentación.

---

## Servicios de Seguridad

### AWS Organizations - Detallado
Gestión centralizada y gobernanza de múltiples cuentas de AWS.
- **Consolidated Billing:** Una sola factura para toda la organización.
- **Service Control Policies (SCPs):** Establecen los permisos máximos (barreras de protección) para las cuentas de la organización. No otorgan permisos, solo los limitan.

### Amazon GuardDuty
Servicio de detección de amenazas inteligente que usa **Machine Learning**.
- Monitorea **VPC Flow Logs**, logs de **CloudTrail**, logs de DNS, etc.

### Amazon Inspector
Servicio de evaluación de seguridad automatizado para aplicaciones (**EC2**, imágenes de contenedor en **ECR**, **Lambda**). Busca vulnerabilidades y desviaciones de las mejores prácticas.

### AWS WAF (Web Application Firewall)
Protege las aplicaciones web de exploits comunes (Inyección SQL, XSS). Se despliega en **CloudFront**, **ALB** o **API Gateway**.

### Amazon Macie
Servicio de seguridad y privacidad de datos que usa **Machine Learning** para descubrir y proteger datos sensibles (**PII**) en **Amazon S3**.

### AWS Artifact
Portal de autoservicio para el acceso bajo demanda a los informes de cumplimiento de AWS (ISO, SOC, PCI).

---

## Cumplimiento y Normativas (Compliance)

AWS cumple con numerosos programas de cumplimiento:
- **HIPAA:** Salud (EE. UU.). Requiere un **BAA** (**Business Associate Agreement**).
- **PCI DSS:** Procesamiento de tarjetas de pago.
- **SOC 1, 2, 3:** Informes de auditoría de controles.
- **ISO 27001:** Estándar internacional de seguridad de la información.
- **FedRAMP:** Gobierno federal de EE. UU.
- **GDPR:** Privacidad de datos de la UE.

---

## Errores Comunes de Seguridad y Cómo Evitarlos

1. **Usar la cuenta Root para tareas diarias:** Crea usuarios de **IAM** individuales y habilita **MFA** en **root**.
2. **Políticas IAM excesivamente permisivas:** Sigue el **Least Privilege**.
3. **Incrustar credenciales en el código:** Usa **IAM Roles** y **Secrets Manager**.
4. **Dejar buckets de S3 públicos:** Habilita **S3 Block Public Access**.
5. **No habilitar MFA:** Obligatorio para **root** y usuarios privilegiados.
6. **Ignorar los logs de CloudTrail:** Habilítalo en todas las regiones para auditoría.

---

## Preguntas de Repaso

**1. Según el Shared Responsibility Model, ¿de qué aspecto de seguridad es responsable AWS?**
   - A. Configuración de **Security Groups**.
   - B. Seguridad física de los centros de datos.
   - C. Cifrado de los datos del cliente.
   - D. Gestión de usuarios de **IAM**.
   *(Respuesta: B)*

**2. ¿Qué servicio proporciona protección contra DDoS sin coste adicional?**
   - A. **AWS WAF**.
   - B. **AWS Shield Advanced**.
   - C. **AWS Shield Standard**.
   - D. **Amazon GuardDuty**.
   *(Respuesta: C)*

**3. ¿Qué servicio usa Machine Learning para descubrir y proteger datos sensibles en S3?**
   - A. **Amazon GuardDuty**.
   - B. **Amazon Inspector**.
   - C. **Amazon Macie**.
   - D. **AWS Config**.
   *(Respuesta: C)*

**4. ¿Dónde puedes descargar los informes de cumplimiento y certificaciones de AWS?**
   - A. **AWS Config**.
   - B. **AWS Artifact**.
   - C. **AWS Inspector**.
   - D. **AWS Organizations**.
   *(Respuesta: B)*

---

[← Anterior: Conceptos de la Nube](02-cloud-concepts.md) | [Volver al Inicio](README.md) | [Siguiente: Tecnología y Servicios en la Nube →](04-technology-services.md)
