# Laboratorios de Práctica (Hands-On Practice Labs)

[← Volver al Plan de Estudio](06-study-plan.md) | [Regresar a la Guía Principal →](README.md)

---

## Tabla de Contenidos
- [Introducción](#introduccion)
- [Lista de Verificación Previa](#lista-de-verificacion-previa)
- [Requisitos Previos](#requisitos-previos)
- [Notas Importantes](#notas-importantes)
- [Guía de Dificultad de los Laboratorios](#guia-de-dificultad-de-los-laboratorios)
- [Laboratorio 1: Configurar Alertas de Facturación y Presupuesto](#laboratorio-1-configurar-alertas-de-facturacion-y-presupuesto)
- [Laboratorio 2: Usuarios, Grupos, Roles y MFA en IAM](#laboratorio-2-usuarios-grupos-roles-y-mfa-en-iam)
- [Laboratorio 3: Lanzar y Configurar una Instancia EC2](#laboratorio-3-lanzar-y-configurar-una-instancia-ec2)
- [Laboratorio 4: Almacenamiento en Amazon S3 y Alojamiento de Sitios Web](#laboratorio-4-almacenamiento-en-amazon-s3-y-alojamiento-de-sitios-web)
- [Laboratorio 5: VPC, Subredes y Configuración de Red](#laboratorio-5-vpc-subredes-y-configuracion-de-red)
- [Laboratorio 6: Base de Datos Amazon RDS](#laboratorio-6-base-de-datos-amazon-rds)
- [Laboratorio 7: Monitoreo y Alarmas con CloudWatch](#laboratorio-7-monitoreo-y-alarmas-con-cloudwatch)
- [Laboratorio 8: Herramientas de Gestión de Costos de AWS](#laboratorio-8-herramientas-de-gestion-de-costos-de-aws)
- [Laboratorio 9: Función Serverless con Lambda](#laboratorio-9-funcion-serverless-con-lambda)
- [Laboratorio 10: Infraestructura como Código con CloudFormation](#laboratorio-10-infraestructura-como-codigo-con-cloudformation)
- [Laboratorio 11: Auto Scaling y Balanceo de Carga](#laboratorio-11-auto-scaling-y-balanceo-de-carga)
- [Laboratorio 12: Práctica con DynamoDB](#laboratorio-12-practica-con-dynamodb)
- [Laboratorio 13: Mensajería con SNS y SQS](#laboratorio-13-mensajeria-con-sns-y-sqs)
- [Laboratorio 14: DNS y Verificaciones de Estado con Route 53](#laboratorio-14-dns-y-verificaciones-de-estado-con-route-53)
- [Laboratorio 15: AWS Organizations y Configuración Multi-Cuenta](#laboratorio-15-aws-organizations-y-configuracion-multi-cuenta)
- [Preguntas Frecuentes de Resolución de Problemas](#preguntas-frecuentes-de-resolucion-de-problemas)
- [Recomendaciones Adicionales de Práctica](#recomendaciones-adicionales-de-practica)

---

## Introducción

> **Importante:** La experiencia práctica es crucial para el éxito en el examen. Estos laboratorios le ayudarán a comprender los servicios de **AWS** más allá de la teoría y están diseñados para mantenerse dentro de los límites del **Free Tier** (Capa Gratuita) cuando se realizan con cuidado.

La experiencia práctica con los servicios de **AWS** proporciona:
- Una comprensión más profunda de las capacidades de los servicios.
- Familiaridad con la **AWS Management Console**.
- Confianza durante el examen.
- Habilidades del mundo real aplicables a empleos.
- Mejor retención de los conceptos.

**Inversión Total de Tiempo:** Aproximadamente 11-12 horas para los 15 laboratorios.

---

## Lista de Verificación Previa

Antes de comenzar cualquier laboratorio, asegúrese de haber completado lo siguiente:

### Requisitos Esenciales
- [ ] **Cuenta de AWS creada y activada** (puede tardar hasta 24 horas).
- [ ] **Tarjeta de crédito/débito válida** registrada (requerida incluso para el **Free Tier**).
- [ ] **Acceso a correo electrónico** para confirmar suscripciones de **SNS** y recibir alertas.
- [ ] **Alertas de facturación configuradas** (Laboratorio 1 - ¡haga esto PRIMERO!).
- [ ] **Panel de control del Free Tier marcado como favorito** para un monitoreo fácil.
- [ ] **ID de la cuenta anotado** y guardado de forma segura.

### Configuración Técnica
- [ ] **Navegador web moderno** (**Chrome**, **Firefox**, **Safari**, **Edge** - última versión).
- [ ] **Conexión a internet estable** (mínimo 5 Mbps recomendado).
- [ ] **Editor de texto** instalado (**VS Code**, **Sublime**, **Notepad++**, o cualquier editor).
- [ ] **Cliente SSH disponible** (integrado en **Mac**/**Linux**, **PuTTY** u **OpenSSH** para **Windows**).
- [ ] Acceso a la **terminal/símbolo del sistema** y familiaridad básica.

### Conocimientos Previos
- [ ] **Comprensión básica** de los conceptos de computación en la nube.
- [ ] **Familiaridad con las direcciones IP** y conceptos básicos de redes (para los laboratorios de **VPC**).
- [ ] Experiencia básica en **línea de comandos** (útil pero no obligatoria).
- [ ] **Comprensión de los formatos JSON/YAML** (para **CloudFormation**).

### Medidas de Seguridad
- [ ] **Gestor de contraseñas** o ubicación segura para las credenciales.
- [ ] **Bloc de notas listo** para documentar los IDs de recursos y URLs.
- [ ] **Recordatorio en el calendario** para revisar y limpiar recursos diariamente.
- [ ] **Límite de presupuesto decidido** (se recomienda un máximo de $10-20).
- [ ] **Comprensión de los cargos**: sepa qué cuesta dinero frente a qué es gratuito.

### Mejores Prácticas Antes de Empezar
- [ ] **Lea el laboratorio completo** antes de ejecutar cualquier paso.
- [ ] **Tome capturas de pantalla** a medida que avanza para su documentación.
- [ ] **Use convenciones de nombres consistentes** (incluya la fecha o el número de laboratorio).
- [ ] **Etiquete todos los recursos** con el nombre del proyecto para una fácil identificación.
- [ ] **Reserve tiempo ininterrumpido** para cada laboratorio.
- [ ] **Prepárese para tomar notas** sobre errores y soluciones.

### Selección de Región
- [ ] **Elija su región principal** (se recomienda **us-east-1** o **us-west-2**).
- [ ] **Verifique la disponibilidad del Free Tier** en la región elegida.
- [ ] **Anote la región para todos los laboratorios** para mantener la consistencia.
- [ ] **Marque el selector de regiones como favorito** para un acceso rápido.

### Planificación del Tiempo
- [ ] **Revise las estimaciones de duración** de los laboratorios.
- [ ] **Añada un margen de tiempo del 25%** para la resolución de problemas.
- [ ] **Planifique el tiempo de limpieza** (5-10 minutos por laboratorio).
- [ ] **Programe descansos** entre laboratorios complejos.
- [ ] **Evite comenzar laboratorios** tarde en la noche (podría olvidar la limpieza).

---

## Requisitos Previos

### Crear su Cuenta de AWS

Siga estos pasos para crear su cuenta de **AWS**:

1. Visite [https://aws.amazon.com](https://aws.amazon.com).
2. Haga clic en **"Create an AWS Account"**.
3. Proporcione:
   - Dirección de correo electrónico.
   - Contraseña.
   - Nombre de la cuenta de **AWS**.
4. Ingrese la información de contacto.
5. Proporcione el método de pago:
   - Se requiere tarjeta de crédito o débito.
   - No se le cobrará si se mantiene dentro del **Free Tier**.
6. Verifique su identidad mediante una llamada telefónica o **SMS**.
7. Seleccione el Plan de Soporte: **Basic (Free)**.
8. Espere la activación de la cuenta (puede tardar hasta 24 horas).
9. Revise su correo electrónico para la confirmación.

### AWS Free Tier

**Duración:** 12 meses a partir de la fecha de creación de la cuenta.

**Servicios clave del Free Tier:**
- **EC2**: 750 horas/mes de instancias **t2.micro** o **t3.micro**.
- **S3**: 5 GB de almacenamiento estándar (**Standard storage**).
- **RDS**: 750 horas/mes de **db.t2.micro**, **db.t3.micro** o **db.t4g.micro**.
- **Lambda**: 1 millón de solicitudes gratuitas por mes.
- **CloudWatch**: 10 métricas personalizadas y alarmas.

**Servicios siempre gratuitos (Always Free):**
- **DynamoDB**: 25 GB de almacenamiento.
- **Lambda**: 1 millón de solicitudes al mes.
- **CloudFormation**: Sin cargo (pague por los recursos creados).

> **Consejo de Examen:** Configure alertas de facturación inmediatamente para evitar cargos inesperados. El **Free Tier** de **AWS** es generoso, pero los errores pueden incurrir en costos.

---

## Notas Importantes

### Seguridad y Gestión de Costos

1. **Siempre limpie los recursos** después de cada laboratorio para evitar cargos.
2. **Configure alertas de facturación** antes de comenzar cualquier trabajo práctico.
3. **Use tipos de instancia t2.micro o t3.micro** (elegibles para el **Free Tier**).
4. **Elija regiones con Free Tier** (se recomiendan **us-east-1**, **us-west-2**).
5. **Monitoree el uso del Free Tier** en el panel de facturación regularmente.
6. **No deje recursos funcionando** durante la noche o cuando no estén en uso.

### Mejores Prácticas

- Complete los laboratorios en orden (existen dependencias).
- Tome notas y capturas de pantalla para referencia futura.
- Lea los mensajes de error cuidadosamente; a menudo contienen la solución.
- Use etiquetas (**tags**) para identificar los recursos del laboratorio para una limpieza fácil.
- Detenga los servicios en lugar de terminarlos si planea regresar.

### Recursos de Resolución de Problemas

- Documentación de **AWS**: [https://docs.aws.amazon.com](https://docs.aws.amazon.com)
- **AWS re:Post** (foro de la comunidad): [https://repost.aws](https://repost.aws)
- **Service Health Dashboard**: [https://status.aws.amazon.com](https://status.aws.amazon.com)
- Soporte de **AWS** (si tiene un plan de soporte de pago).

---

## Guía de Dificultad de los Laboratorios

### Comprender las Calificaciones de Dificultad

Cada laboratorio se califica en tres dimensiones:

**Complejidad Técnica:**
- **Principiante**: Pasos directos, se requiere un conocimiento técnico mínimo.
- **Intermedio**: Algunos conceptos técnicos, puede requerir resolución de problemas.
- **Avanzado**: Arquitectura/redes complejas, requiere atención cuidadosa.

**Inversión de Tiempo:**
- Corto: 15-30 minutos.
- Medio: 30-60 minutos.
- Largo: 60+ minutos.

**Requisitos Previos:**
- Mínimos: Solo la cuenta de **AWS**.
- Moderados: Es útil haber completado los laboratorios anteriores.
- Extensos: Requiere conocimientos específicos o laboratorios completados.

### Matriz de Dificultad de los Laboratorios

| Laboratorio | Dificultad | Duración | Requisitos Previos | Puntuación de Complejidad |
|-------------|------------|----------|--------------------|---------------------------|
| Lab 1: Alertas de Facturación | Principiante | 15 min | Ninguno | 1/5 |
| Lab 2: IAM & MFA | Principiante | 30 min | Ninguno | 2/5 |
| Lab 3: Instancia EC2 | Intermedio | 45 min | Lab 2 recomendado | 3/5 |
| Lab 4: Sitio Web en S3 | Intermedio | 40 min | HTML básico | 2/5 |
| Lab 5: Red VPC | Avanzado | 60 min | Conceptos básicos de redes | 4/5 |
| Lab 6: Base de Datos RDS | Intermedio | 30 min | Labs 3 & 5 | 3/5 |
| Lab 7: CloudWatch | Principiante | 25 min | Lab 3 | 2/5 |
| Lab 8: Herramientas de Costos | Principiante | 30 min | Ninguno | 1/5 |
| Lab 9: Lambda | Intermedio | 25 min | Python básico | 2/5 |
| Lab 10: CloudFormation | Intermedio | 20 min | YAML/JSON | 3/5 |
| Lab 11: Auto Scaling | Avanzado | 45 min | Labs 3 & 5 | 4/5 |
| Lab 12: DynamoDB | Intermedio | 35 min | Conceptos de bases de datos | 2/5 |
| Lab 13: SNS & SQS | Intermedio | 30 min | Lab 9 útil | 3/5 |
| Lab 14: Route 53 | Intermedio | 25 min | Conocimientos de DNS | 3/5 |
| Lab 15: Organizations | Avanzado | 40 min | Múltiples cuentas | 4/5 |

### Rutas de Aprendizaje Recomendadas

**Ruta 1: Principiantes Absolutos**
1. Lab 1 (Facturación) → Lab 2 (IAM) → Lab 8 (Herramientas de Costos) → Lab 4 (S3).
2. Luego continúe con: Lab 3 → Lab 7 → Lab 9 → Lab 12.

**Ruta 2: Desarrolladores**
1. Lab 1 → Lab 2 → Lab 3 → Lab 9 (Lambda).
2. Luego: Lab 4 → Lab 13 (Mensajería) → Lab 12 (DynamoDB) → Lab 10 (IaC).

**Ruta 3: Infraestructura/Operaciones**
1. Lab 1 → Lab 2 → Lab 3 → Lab 5 (VPC).
2. Luego: Lab 11 (Auto Scaling) → Lab 6 (RDS) → Lab 14 (Route 53) → Lab 15.

**Ruta 4: Secuencial Completa** (Recomendada para la mayoría)
- Siga los laboratorios 1-15 en orden para una comprensión integral.

### Seguimiento del Desarrollo de Habilidades

Después de completar cada laboratorio, evalúe su confianza:

**Escala de Calificación:**
- 1 = Necesito repasar.
- 2 = Entendido con ayuda.
- 3 = Cómodo, podría explicarlo a otros.
- 4 = Experto, podría resolver problemas de forma independiente.
- 5 = Podría enseñar este tema.

**Puntuaciones Objetivo para el Examen:**
- Laboratorios centrales (1-10): Apunte a un 4/5.
- Laboratorios avanzados (11-15): Apunte a un 3/5.

---

## Laboratorio 1: Configurar Alertas de Facturación y Presupuesto

**Duración:** 15 minutos
**Costo:** Gratis
**Dificultad:** Principiante

### Objetivos de Aprendizaje

Al final de este laboratorio, podrá:

1. Habilitar el acceso de usuarios de **IAM** a la información de facturación.
2. Crear alarmas de facturación de **CloudWatch** con notificaciones de **SNS**.
3. Configurar **AWS Budgets** con múltiples umbrales de alerta.
4. Comprender la diferencia entre las métricas de facturación de **CloudWatch** y **AWS Budgets**.
5. Monitorear el uso del **Free Tier** para evitar cargos inesperados.
6. Configurar un monitoreo de costos proactivo para la seguridad de la cuenta de **AWS**.

### Por Qué es Importante este Laboratorio

**Escenario del Mundo Real:** Un desarrollador dejó una base de datos **RDS** funcionando en una cuenta personal de **AWS** y acumuló $450 en cargos durante un fin de semana. Las alertas de facturación adecuadas habrían detectado esto en cuestión de horas, limitando el daño a menos de $20.

**Relevancia para el Examen:** El examen **Cloud Practitioner** enfatiza fuertemente la gestión de costos, la facturación y el monitoreo. Las preguntas a menudo ponen a prueba su comprensión de:
- **AWS Budgets** frente a **Cost Explorer** frente a las alarmas de facturación de **CloudWatch**.
- Límites y monitoreo del **Free Tier**.
- Mejores prácticas de alertas de facturación.
- Mecanismos de notificación de **SNS**.

### Objetivo

Protéjase de cargos inesperados configurando alertas de facturación y presupuestos.

### Requisitos Previos

- Cuenta de **AWS** activa.
- Acceso a la cuenta raíz (**root user**) o a un usuario de **IAM** con permisos de facturación.

### Instrucciones Paso a Paso

#### Parte 1: Habilitar Alertas de Facturación

> **Lo Que Verá:** La **AWS Management Console** con la barra de navegación en la parte superior mostrando el nombre de su cuenta y el selector de regiones.

1. Inicie sesión en la **AWS Management Console** como usuario raíz o administrador.
   - Debería ver la barra de búsqueda de servicios de **AWS** y el panel de control.

2. Haga clic en su **nombre de cuenta** (esquina superior derecha) → Seleccione **"Account"**.
   - Esto abre la página de configuración de la cuenta.
   - Ruta alternativa: Navegue a través del enlace "My Account" en el menú desplegable.

3. Desplácese hacia abajo hasta la sección **"IAM User and Role Access to Billing Information"**.
   - Esta sección está aproximadamente a la mitad de la página.
   - Verá una descripción sobre cómo permitir que los usuarios de **IAM** accedan a los datos de facturación.
   - **Por qué esto es importante:** Por defecto, solo los usuarios raíz pueden ver la 5. Marque la casilla para **"Activate IAM Access"**.
   - Cuando está habilitado, los usuarios de **IAM** con permisos de facturación pueden ver los costos.
   - **Mejor Práctica:** Esto le permite usar usuarios de **IAM** en lugar de la cuenta raíz para el monitoreo diario de la facturación.

6. Haga clic en **"Update"**.
   - Verá un mensaje de éxito: "Successfully updated IAM user/role access to billing information".

**Paso de Validación:**
- La configuración ahora debería aparecer como "Activated".
- Esta es una configuración única por cuenta de **AWS**.

**Problema Común:** Si no ve esta opción, verifique que haya iniciado sesión como usuario raíz (propietario de la cuenta), no como un usuario de **IAM**.

#### Parte 2: Crear una Alarma de Facturación de CloudWatch

1. Navegue al servicio **CloudWatch**.
2. Seleccione la región: **N. Virginia (us-east-1)** (las métricas de facturación solo están disponibles en **us-east-1**).
3. Vaya a **"Alarms"** → **"Billing"** → **"Create alarm"**.
4. Haga clic en **"Select metric"**.
5. Seleccione **"Billing"** → **"Total Estimated Charge"**.
6. Marque la casilla **USD**.
7. Haga clic en **"Select metric"**.
8. Configure la alarma:
   - **Threshold type**: Static (Estático).
   - **Whenever**: Greater than (Mayor que).
   - **Amount**: $5 (o su umbral preferido).
9. Haga clic en **"Next"**.

#### Parte 3: Configurar la Notificación de SNS

1. Bajo **"Notification"**:
   - **Select an SNS topic**: Create new topic.
   - **Topic name**: "Alertas-de-Facturacion".
   - **Email endpoints**: Ingrese su dirección de correo electrónico.
2. Haga clic en **"Create topic"**.
3. Haga clic en **"Next"**.
4. **Alarm name**: "Alerta-de-Facturacion-Mensual".
5. **Alarm description**: "Alerta cuando los cargos mensuales superen los $5".
6. Haga clic en **"Next"**.
7. Revise y haga clic en **"Create alarm"**.
8. **Revise su correo electrónico** y haga clic en el enlace de confirmación.

#### Parte 4: Crear un AWS Budget

1. Navegue a **"Billing and Cost Management"**.
2. Haga clic en **"Budgets"** en el menú de la izquierda.
3. Haga clic en **"Create budget"**.
4. Seleccione **"Cost budget"** → **"Next"**.
5. Configure el presupuesto:
   - **Budget name**: "Presupuesto-de-Costo-Mensual".
   - **Period**: Monthly (Mensual).
   - **Budget amount**: $10.
   - **Budget scope**: All AWS services.
6. Haga clic en **"Next"**.
7. Configure las alertas:
   - **Alert 1**: 80% del monto presupuestado.
   - **Email recipients**: Su correo electrónico.
   - **Alert 2**: 100% del monto presupuestado.
8. Haga clic en **"Next"**.
9. Revise y haga clic en **"Create budget"**.

### Resultados Esperados

- Alarma de facturación de **CloudWatch** creada y activa.
- Confirmación por correo electrónico recibida para la suscripción de **SNS**.
- Presupuesto creado con dos umbrales de alerta.
- Notificaciones por correo electrónico configuradas.

### Verificación

1. Vaya a **CloudWatch** → **Alarms** → Verifique que la alarma muestre el estado "OK".
2. Vaya a **Budgets** → Verifique que el presupuesto muestre el gasto actual frente al presupuesto.
3. Revise su correo electrónico para la confirmación de **SNS**.

### Resolución de Problemas

**Problema:** La métrica de facturación no aparece.
**Solución:** Asegúrese de estar en la región **us-east-1**; las métricas de facturación solo están disponibles allí.

**Problema:** No se recibió el correo electrónico de confirmación.
**Solución:** Revise la carpeta de correo no deseado; reenvíe la confirmación desde la consola de **SNS**.

**Problema:** No se puede acceder a la información de facturación.
**Solución:** Habilite el acceso de **IAM** a la facturación en la configuración de la cuenta.

### Qué Debería Ver en Cada Paso

**Después de Crear la Alarma de CloudWatch:**
- La alarma aparece en el panel de control de **CloudWatch** → **Alarms**.
- El estado muestra "Insufficient data" inicialmente (normal: se necesitan 24 horas de datos).
- Después de la confirmación, el estado cambia a "OK" (no se cumple la condición de alarma).
- El gráfico muestra los cargos estimados a lo largo del tiempo.

**Después de Confirmar la Suscripción de SNS:**
- Correo electrónico de "AWS Notifications" con el asunto "AWS Notification - Subscription Confirmation".
- Después de hacer clic en el enlace, el navegador muestra "Subscription confirmed!".
- La consola de **SNS** muestra el estado de la suscripción como "Confirmed".

**Después de Crear el Presupuesto:**
- El presupuesto aparece en el panel de control de **Budgets**.
- Muestra el gasto actual frente al monto presupuestado (probablemente $0.00 de $10.00).
- Umbrales de alerta mostrados al 80% ($8) y al 100% ($10).
- La previsión muestra el gasto proyectado al final del mes.

### Consejos del Mundo Real

**Umbrales de Alerta Recomendados:**
- Para estudiantes/aprendices: Alarma de $5, presupuesto de $10.
- Para cuentas de producción: Configure según el gasto mensual esperado.
- Enfoque conservador: Alerta al 50%, 80% y 100%.

**Estrategia de Múltiples Presupuestos:**
- Presupuesto total de la cuenta: $10.
- Presupuestos por servicio: **EC2** $3, **RDS** $3, **S3** $2, Otros $2.
- Ayuda a identificar qué servicio está impulsando los costos.

**Prevención de la Fatiga por Alertas:**
- No establezca umbrales demasiado bajos (evite alertas constantes).
- Revise y ajuste mensualmente según el uso real.
- Use presupuestos previstos para un monitoreo proactivo.

**Mejor Práctica - Múltiples Receptores de Notificaciones:**
- Agregue los correos electrónicos de los miembros del equipo al tema de **SNS**.
- Considere las notificaciones por **SMS** para presupuestos críticos (nota: el **SMS** tiene un costo).
- Configure la integración con **Slack**/**Teams** a través de **Lambda** (avanzado).

### Enfoques Alternativos

**Método 1: Solo AWS Budgets**
- Omita la alarma de facturación de **CloudWatch**.
- Use solo **AWS Budgets** (más simple para principiantes).
- Limitación: Menos granular, se actualiza 3 veces al día frente a **CloudWatch** cada 5 minutos.

**Método 2: Solo CloudWatch**
- Omita **AWS Budgets**.
- Cree múltiples alarmas de **CloudWatch** en diferentes umbrales.
- Limitación: No muestra previsiones ni la interfaz de seguimiento de presupuestos.

**Método 3: Cost Anomaly Detection (Avanzado)**
- **AWS** proporciona detección de anomalías basada en **ML**.
- Navegue a **Cost Management** → **Cost Anomaly Detection**.
- Cree un monitor → Configure las preferencias de alerta.
- Detecta automáticamente patrones de gasto inusuales.

### Limpieza

> **Nota:** Mantenga estas alertas activas para una protección continua. No se necesita limpieza.

**Si necesita eliminar las alertas más tarde:**

1. **Eliminar Alarma de CloudWatch:**
   - **CloudWatch** → **Alarms** → Seleccione la alarma → **Actions** → **Delete**.
   - Confirme la eliminación escribiendo el nombre de la alarma.
   - Alarma eliminada inmediatamente.

2. **Eliminar Presupuesto:**
   - **Billing** → **Budgets** → Seleccione el presupuesto → **Actions** → **Delete budget**.
   - Escriba "delete" para confirmar.
   - Presupuesto eliminado del panel de control.

3. **Eliminar Tema de SNS:**
   - **SNS** → **Topics** → Seleccione el tema → **Delete**.
   - Escriba "delete me" para confirmar.
   - Las suscripciones asociadas se eliminan automáticamente.

4. **Verificar Limpieza:**
   - Revise la lista de alarmas de **CloudWatch** (debería estar vacía).
   - Revise el panel de control de **Budgets** (no debería mostrar presupuestos).
   - Los correos electrónicos dejarán de llegar.

**Impacto en el Costo de la Eliminación:**
- No hay costos continuos para estos servicios gratuitos.
- Es seguro mantenerlos activos indefinidamente.

---

### Verificación de Conocimientos Post-Laboratorio

Ponga a prueba su comprensión de los conceptos del Laboratorio 1:

**Pregunta 1:** ¿Cuál es la diferencia entre las alarmas de facturación de **CloudWatch** y **AWS Budgets**?

<details>
<summary>Haga clic para revelar la respuesta</summary>

**Respuesta:**
- **Alarmas de CloudWatch**: Monitorean los cargos reales en tiempo casi real (cada 5-10 minutos), activan notificaciones de **SNS** cuando se supera el umbral. Solo están disponibles en la región **us-east-1**.
- **AWS Budgets**: Realizan un seguimiento de los costos y el uso frente a los presupuestos planificados, proporcionan previsiones, se actualizan 3 veces al día. Admiten presupuestos basados en el uso (no solo en el costo). Disponibles globalmente.
- **Use ambos juntos**: **CloudWatch** para alertas inmediatas, **Budgets** para el seguimiento y las previsiones.

</details>

**Pregunta 2:** ¿Por qué las métricas de facturación de **CloudWatch** deben crearse en la región **us-east-1**?

<details>
<summary>Haga clic para revelar la respuesta</summary>

**Respuesta:** La facturación es un servicio global, y **AWS** consolida todos los datos de facturación en la región **us-east-1** (N. Virginia). Las métricas de facturación solo se publican en **CloudWatch** en esta región. Esta es una decisión de diseño de **AWS** para centralizar los datos de facturación global.

**Consejo de Examen:** Recuerde esto para el examen; ¡es una pregunta trampa común!

</details>

**Pregunta 3:** Si su alarma muestra el estado "Insufficient data", ¿hay algo mal?

<details>
<summary>Haga clic para revelar la respuesta</summary>

**Respuesta:** No, esto es normal. "Insufficient data" significa que **CloudWatch** aún no tiene suficientes puntos de datos para evaluar la alarma. Esto ocurre típicamente:
- Dentro de las primeras 24 horas de la creación de la alarma.
- Para cuentas nuevas de **AWS** con un uso mínimo.
- Después de cambiar los parámetros de la alarma.

El estado cambiará a "OK" una vez que haya suficientes datos disponibles. Si los cargos superan el umbral, el estado cambia a "In alarm".

</details>

**Pregunta 4:** Usted configuró un presupuesto de $10 pero recibió una alerta a los $8. ¿Por qué?

<details>
<summary>Haga clic para revelar la respuesta</summary>

**Respuesta:** Usted configuró un umbral de alerta al 80% de su presupuesto. El 80% de $10 = $8. Esto es intencional y es una mejor práctica. Recibir alertas antes de llegar al 100% le da tiempo para investigar y tomar medidas antes de superar su presupuesto.

Múltiples umbrales (50%, 80%, 100%) proporcionan advertencias escalonadas a medida que se acerca a su límite.

</details>

**Pregunta 5:** ¿Se pueden configurar alarmas de facturación para servicios individuales como EC2 o S3?

<details>
<summary>Haga clic para revelar la respuesta</summary>

**Respuesta:**
- **Alarmas de facturación de CloudWatch**: Solo pueden monitorear los cargos totales estimados de la cuenta, no servicios individuales.
- **AWS Budgets**: SÍ, puede crear presupuestos específicos por servicio (ej: solo **EC2**, solo **S3**).
- **Mejor Práctica**: Use **AWS Budgets** para el seguimiento de costos a nivel de servicio y las alarmas de **CloudWatch** para el gasto total de la cuenta.

</details>

**Pregunta 6:** ¿Qué sucede si no confirma el correo electrónico de suscripción de SNS?

<details>
<summary>Haga clic para revelar la respuesta</summary>

**Respuesta:** La suscripción permanece en estado "Pending confirmation" y NO recibirá ninguna notificación de alarma. La alarma se evaluará y se activará, pero no se enviarán los correos electrónicos.

Siempre revise la carpeta de correo no deseado y confirme las suscripciones dentro de los 3 días. Después de 3 días, es posible que deba recrear la suscripción.

</details>

**Pregunta 7:** Su Free Tier incluye 10 alarmas de CloudWatch. ¿Qué sucede si crea 11?

<details>
<summary>Haga clic para revelar la respuesta</summary>

**Respuesta:** Se le cobrarán $0.10 por alarma al mes por la 11ª alarma (y cualquier otra posterior). Con 11 alarmas, el costo sería de $0.10/mes.

**Mejor Práctica para el Free Tier:**
- Manténgase dentro de las 10 alarmas.
- Use **AWS Budgets** (gratuito) para monitoreo adicional.
- Combine múltiples umbrales en menos alarmas.

</details>

**Pregunta 8:** ¿Con qué frecuencia debería revisar su panel de uso del Free Tier?

<details>
<summary>Haga clic para revelar la respuesta</summary>

**Respuesta:**
- **Mínimo**: Semanalmente.
- **Recomendado**: Cada 2-3 días cuando esté aprendiendo activamente.
- **Mejor Práctica**: Diariamente mientras realice laboratorios.
- **Configure un recordatorio en el calendario**: Agregue un recordatorio recurrente para revisar el panel.

El panel del **Free Tier** muestra el uso del mes actual frente a los límites con barras de progreso visuales. Detecta problemas antes de que se conviertan en cargos.

</details>

### Puntos Clave

- **Las alertas de facturación son su red de seguridad**: configúrelas antes de hacer cualquier otra cosa en **AWS**.
- **Use tanto las alarmas de CloudWatch como Budgets**: se complementan entre sí.
- **Siempre confirme las suscripciones de SNS**: revise la carpeta de correo no deseado si es necesario.
- **Monitoree el uso del Free Tier**: conviértalo en un hábito diario durante la fase de aprendizaje.
- **Sea conservador con los umbrales**: es mejor recibir una advertencia temprana que ser sorprendido por los cargos.
- **Las métricas de facturación son solo en us-east-1**: recuerde esto para el examen.
- **Los presupuestos pueden rastrear el uso, no solo los costos**: útil para monitorear las horas del **Free Tier**.
- **El Free Tier es por servicio**: 750 horas de **EC2** + 750 horas de **RDS**, no combinadas.

### Recursos Adicionales

- [Documentación de Gestión de Costos y Facturación de AWS](https://docs.aws.amazon.com/account-billing/)
- [Guía de Métricas de Facturación de CloudWatch](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/monitor_estimated_charges_with_cloudwatch.html)
- [Mejores Pr## Laboratorio 2: Usuarios, Grupos, Roles y MFA en IAM

**Duración:** 30 minutos
**Costo:** Gratis
**Dificultad:** Principiante

### Objetivo

Comprender las mejores prácticas de seguridad de **IAM** mediante la creación de usuarios, grupos, roles y la habilitación de **MFA**.

### Requisitos Previos

- Cuenta de **AWS** con acceso raíz.
- Teléfono inteligente con una aplicación de autenticación (**Google Authenticator**, **Authy**, etc.).

### Instrucciones Paso a Paso

#### Parte 1: Asegurar la Cuenta Raíz con MFA

1. Navegue al servicio **IAM** en la consola de **AWS**.
2. Haga clic en **"Dashboard"** → Revise las recomendaciones de seguridad.
3. Haga clic en **"Add MFA"** para la cuenta raíz.
4. **MFA device name**: "dispositivo-mfa-raiz".
5. Seleccione **"Virtual MFA device"** → **"Next"**.
6. Instale la aplicación de autenticación en su teléfono:
   - **Google Authenticator** (**iOS**/**Android**).
   - **Authy** (**iOS**/**Android**).
   - **Microsoft Authenticator**.
7. Haga clic en **"Show QR code"**.
8. Escanee el código QR con la aplicación de autenticación.
9. Ingrese **dos códigos MFA consecutivos** de la aplicación.
10. Haga clic en **"Add MFA"**.
11. **Verificación**: La insignia de **MFA** aparece en el panel de control.

> **Importante:** Guarde las credenciales de la cuenta raíz de forma segura y use usuarios de **IAM** para las tareas diarias.

#### Parte 2: Crear un Usuario Administrador de IAM

1. En **IAM**, haga clic en **"Users"** → **"Create user"**.
2. **User name**: "usuario-admin".
3. Seleccione **"Provide user access to the AWS Management Console"**.
4. Elija **"I want to create an IAM user"**.
5. Opciones de contraseña:
   - Contraseña personalizada O Autogenerada.
   - Desmarque "Users must create a new password at next sign-in".
6. Haga clic en **"Next"**.
7. **Permissions options**: "Attach policies directly".
8. Busque y seleccione **"AdministratorAccess"**.
9. Haga clic en **"Next"**.
10. Revise y haga clic en **"Create user"**.
11. **Descargue el archivo .csv** (contiene las credenciales).
12. **Copie la URL de inicio de sesión de la consola** (guárdela para más tarde).

#### Parte 3: Crear Grupos de IAM

**Crear el Grupo de Desarrolladores:**

1. Haga clic en **"User groups"** → **"Create group"**.
2. **Group name**: "Desarrolladores".
3. **Attach permissions policies**:
   - Busque y seleccione **"AmazonEC2ReadOnlyAccess"**.
   - Busque y seleccione **"AmazonS3FullAccess"**.
4. Haga clic en **"Create group"**.

**Crear el Grupo de Administradores:**

1. Haga clic en **"Create group"** nuevamente.
2. **Group name**: "Administradores".
3. **Attach permissions policy**:
   - Busque y seleccione **"AdministratorAccess"**.
4. Haga clic en **"Create group"**.

#### Parte 4: Crear Usuarios de IAM Adicionales

**Crear el Usuario Desarrollador 1:**

1. Haga clic en **"Users"** → **"Create user"**.
2. **User name**: "desarrollador-1".
3. Habilite el **acceso a la consola**.
4. Establezca la contraseña (personalizada o autogenerada).
5. Haga clic en **"Next"**.
6. **Add user to groups**: Seleccione el grupo **"Desarrolladores"**.
7. Haga clic en **"Next"** → **"Create user"**.

**Crear el Usuario Desarrollador 2:**

1. Repita los pasos anteriores.
2. **User name**: "desarrollador-2".
3. Agregue al grupo **"Desarrolladores"**.
4. Cree el usuario.

#### Parte 5: Crear un Rol de IAM para EC2

1. Haga clic en **"Roles"** → **"Create role"**.
2. **Trusted entity type**: "AWS service".
3. **Use case**: Seleccione **"EC2"**.
4. Haga clic en **"Next"**.
5. **Attach permissions**:
   - Busque y seleccione **"AmazonS3ReadOnlyAccess"**.
6. Haga clic en **"Next"**.
7. **Role name**: "Rol-Lectura-S3-para-EC2".
8. **Description**: "Permite que las instancias EC2 lean de S3".
9. Haga clic en **"Create role"**.

#### Parte 6: Probar las Políticas de IAM

1. **Cierre la sesión** de la cuenta raíz.
2. **Inicie sesión** como "desarrollador-1" usando:
   - La URL de inicio de sesión de la consola (guardada anteriormente).
   - Nombre de usuario: desarrollador-1.
   - Contraseña: (según se haya establecido).
3. Intente acceder al servicio **S3** (debería funcionar - acceso total).
4. Intente crear un bucket de **S3** (debería funcionar).
5. Intente acceder al servicio **IAM** (debería ser denegado - sin permiso).
6. Intente ver las instancias **EC2** (debería funcionar - solo lectura).
7. Intente lanzar una instancia **EC2** (debería ser denegado - solo lectura).
8. **Cierre la sesión**.

### Resultados Esperados

- Cuenta raíz asegurada con **MFA**.
- Usuario administrador de **IAM** creado con permisos totales.
- Dos grupos de **IAM** creados (Desarrolladores, Administradores).
- Dos usuarios desarrolladores creados y añadidos al grupo Desarrolladores.
- Rol de **IAM** creado para que **EC2** acceda a **S3**.
- Pruebas exitosas de los límites de permisos y restricciones.

### Verificación

1. El **Panel de IAM** muestra:
   - **MFA** habilitado para la raíz.
   - Múltiples usuarios creados.
   - Grupos con políticas adjuntas.
   - Rol creado.
2. La **prueba de inicio de sesión** como desarrollador-1 confirma los límites de los permisos.

### Resolución de Problemas

**Problema:** No se puede iniciar sesión como usuario de **IAM**.
**Solución:** Use la URL de inicio de sesión específica de la cuenta, no la página de inicio de sesión raíz.

**Problema:** Falla la configuración de **MFA**.
**Solución:** Asegúrese de que la hora del teléfono esté sincronizada; intente escanear el código QR nuevamente.

**Problema:** Errores de permiso denegado.
**Solución:** Verifique que el usuario esté en el grupo correcto; revise las políticas adjuntas al grupo.

### Limpieza

> **Nota:** Conserve el usuario-admin para futuros laboratorios. Puede eliminar los usuarios y grupos de desarrolladores si lo desea.

**Para eliminar usuarios:**
1. Seleccione el usuario → **"Delete"** → Confirme.

**Para eliminar grupos:**
1. Primero elimine a todos los usuarios del grupo.
2. Seleccione el grupo → **"Delete"** → Confirme.

**Para eliminar el rol:**
1. Seleccione el rol → **"Delete"** → Confirme.

---

## Laboratorio 3: Lanzar y Configurar una Instancia EC2

**Duración:** 45 minutos
**Costo:** Gratis (**t2.micro**/**t3.micro** en el **Free Tier**)
**Dificultad:** Intermedio

### Objetivo

Lanzar un servidor web en **EC2**, conectarse mediante **SSH**, crear una **AMI** y realizar **snapshots**.

### Requisitos Previos

- Cuenta de **AWS**.
- Usuario de **IAM** con permisos de **EC2**.
- Conocimientos básicos de línea de comandos.

### Instrucciones Paso a Paso

#### Parte 1: Lanzar una Instancia EC2

1. Navegue al servicio **EC2**.
2. Seleccione la región: **us-east-1** (o su región preferida).
3. Haga clic en **"Launch instance"**.
4. **Name**: "MiServidorWeb".
5. **Application and OS Images (AMI)**:
   - Seleccione **"Amazon Linux 2023 AMI"**.
   - Verifique la etiqueta "Free tier eligible".
6. **Instance type**:
   - Seleccione **"t2.micro"** o **"t3.micro"** (Elegible para el **Free Tier**).
7. **Key pair**:
   - Haga clic en **"Create new key pair"**.
   - **Key pair name**: "mi-par-de-claves".
   - **Key pair type**: RSA.
   - **Private key file format**:
     - **.pem** (para **Mac**/**Linux**/**Windows OpenSSH**).
     - **.ppk** (para **Windows PuTTY**).
   - Haga clic en **"Create key pair"**.
   - **Guarde el archivo de forma segura** (no podrá descargarlo de nuevo).

#### Parte 2: Configurar los Ajustes de Red

1. **Network settings**:
   - Haga clic en **"Edit"**.
   - **VPC**: Default **VPC**.
   - **Subnet**: Sin preferencia.
   - **Auto-assign public IP**: Enable.
2. **Firewall (Security groups)**:
   - Seleccione **"Create security group"**.
   - **Security group name**: "web-server-sg".
   - **Description**: "Permitir SSH y HTTP".
   - **Inbound security group rules**:
     - **Rule 1**:
       - Type: SSH.
       - Protocol: TCP.
       - Port: 22.
       - Source: My IP.
     - Haga clic en **"Add security group rule"**.
     - **Rule 2**:
       - Type: HTTP.
       - Protocol: TCP.
       - Port: 80.
       - Source: 0.0.0.0/0 (cualquier lugar).

#### Parte 3: Configurar el Almacenamiento y los Datos de Usuario

1. **Configure storage**:
   - **Size**: 8 GiB (predeterminado).
   - **Volume type**: gp3 (predeterminado).
   - Mantenga los demás valores predeterminados.
2. **Expanda "Advanced details"**.
3. Desplácese hasta **"User data"**.
4. Pegue el siguiente script:

```bash
#!/bin/bash
yum update -y
yum install -y httpd
systemctl start httpd
systemctl enable httpd
echo "<h1>Hola desde AWS EC2</h1>" > /var/www/html/index.html
```

5. Haga clic en **"Launch instance"**.
6. Espere el mensaje **"Successfully initiated launch"**.
7. Haga clic en **"View all instances"**.

#### Parte 4: Conectarse a la Instancia

**Espere a que la instancia esté lista:**
- **Instance state**: Running.
- **Status check**: 2/2 checks passed (puede tardar 2-3 minutos).

**Obtener información de conexión:**
1. Seleccione su instancia.
2. Copie la **"Public IPv4 address"**.
3. Pruebe el servidor web: Abra el navegador, navegue a `http://[public-ip]`.
4. Debería ver: **"Hola desde AWS EC2"**.

**Conectarse vía SSH (Mac/Linux/Windows OpenSSH):**

1. Abra la terminal.
2. Navegue al directorio con el par de claves:
   ```bash
   cd ~/Downloads
   ```
3. Establezca los permisos correctos:
   ```bash
   chmod 400 mi-par-de-claves.pem
   ```
4. Conéctese a la instancia:
   ```bash
   ssh -i mi-par-de-claves.pem ec2-user@[public-ip-address]
   ```
5. Escriba **"yes"** para aceptar la huella digital.
6. ¡Ya está conectado!

**Conectarse vía SSH (Windows PuTTY):**

1. Abra **PuTTY**.
2. **Host Name**: ec2-user@[public-ip].
3. **Port**: 22.
4. **Connection → SSH → Auth**:
   - Explore y seleccione el archivo .ppk.
5. Haga clic en **"Open"**.
6. Acepte la alerta de seguridad.
7. ¡Está conectado!

**Verificar el servidor web:**
```bash
sudo systemctl status httpd
```

#### Parte 5: Crear una AMI (Amazon Machine Image)

1. En la consola de **EC2**, seleccione su instancia.
2. Haga clic en **"Actions"** → **"Image and templates"** → **"Create image"**.
3. **Image name**: "MiServidorWeb-AMI".
4. **Image description**: "Servidor web con Apache instalado".
5. Mantenga los demás valores predeterminados.
6. Haga clic en **"Create image"**.
7. Vaya a **"AMIs"** en el menú de navegación izquierdo.
8. Espere a que el **Status** sea "Available" (tarda 2-5 minutos).

> **Nota:** Ahora puede lanzar nuevas instancias desde esta **AMI** con Apache preinstalado.

#### Parte 6: Crear un Snapshot de EBS

1. Vaya a **"Volumes"** en el menú izquierdo de **EC2**.
2. Seleccione el volumen adjunto a su instancia (verifique "Attachment information").
3. Haga clic en **"Actions"** → **"Create snapshot"**.
4. **Description**: "WebServer-backup".
5. **Tags**:
   - Key: Name.
   - Value: WebServer-Snapshot.
6. Haga clic en **"Create snapshot"**.
7. Vaya a **"Snapshots"** para ver el estado.
8. Espere a que el **Status** sea "Completed".

### Resultados Esperados

- Instancia **EC2** ejecutando **Amazon Linux 2023**.
- Servidor web Apache instalado y accesible vía HTTP.
- Conexión exitosa vía **SSH**.
- **AMI** creada a partir de la instancia en ejecución.
- **Snapshot** de **EBS** creado para respaldo.

### Verificación

1. **Servidor web accesible**: Visite `http://[public-ip]` → Vea "Hola desde AWS EC2".
2. **La conexión SSH funciona**: Capaz de conectarse y ejecutar comandos.
3. **AMI creada**: Aparece en la lista de **AMIs** con estado "Available".
4. **Snapshot creado**: Aparece en la lista de **Snapshots** con estado "Completed".

### Resolución de Problemas

**Problema:** No se puede acceder al servidor web (tiempo de espera agotado).
**Solución:**
- Verifique que el grupo de seguridad permita HTTP (puerto 80) desde 0.0.0.0/0.
- Asegúrese de que la instancia esté en estado "running".
- Verifique que las comprobaciones de estado hayan pasado.
- Verifique que esté usando HTTP, no HTTPS.

**Problema:** Conexión SSH rechazada.
**Solución:**
- Verifique que el grupo de seguridad permita SSH (puerto 22) desde su IP.
- Verifique que esté usando el archivo de par de claves correcto.
- Asegúrese de usar "ec2-user" como nombre de usuario.
- Verifique que el archivo de clave tenga los permisos correctos (400).

**Problema:** Permiso denegado (publickey).
**Solución:**
- Verifique que esté usando el archivo .pem correcto.
- Verifique los permisos del archivo: `chmod 400 mi-par-de-claves.pem`.
- Asegúrese de usar el nombre de usuario correcto (ec2-user para **Amazon Linux**).

**Problema:** Falla la creación de la **AMI**.
**Solución:**
- Asegúrese de que la instancia esté en estado "running" o "stopped".
- Verifique que tenga suficiente cuota de **snapshots** de **EBS**.

### Limpieza (¡Importante!)

> **Crítico:** Siempre realice la limpieza para evitar cargos después de que expire el **Free Tier**.

**Elimine los recursos en este orden:**

1. **Terminar la instancia**:
   - Seleccione la instancia.
   - **"Instance state"** → **"Terminate instance"**.
   - Confirme la terminación.
   - Espere al estado: "Terminated".

2. **Eliminar el snapshot**:
   - Vaya a **"Snapshots"**.
   - Seleccione su **snapshot**.
   - **"Actions"** → **"Delete snapshot"**.
   - Confirme la eliminación.

3. **Anular el registro de la AMI**:
   - Vaya a **"AMIs"**.
   - Seleccione su **AMI**.
   - **"Actions"** → **"Deregister AMI"**.
   - Confirme la anulación.

4. **Eliminar el snapshot de la AMI**:
   - Vaya a **"Snapshots"**.
   - Busque el **snapshot** creado por la **AMI** (verifique la descripción).
   - **"Actions"** → **"Delete snapshot"**.
   - Confirme la eliminación.

5. **Eliminar el par de claves (opcional)**:
   - Vaya a **"Key Pairs"**.
   - Seleccione el par de claves.
   - **"Actions"** → **"Delete"**.
   - Confirme la eliminación.

---

## Laboratorio 4: Almacenamiento en Amazon S3 y Alojamiento de Sitios Web

**Duración:** 40 minutos
**Costo:** Gratis (dentro de los 5 GB del **Free Tier**)
**Dificultad:** Intermedio

### Objetivo

Dominar las características de almacenamiento de **S3**, incluyendo la creación de buckets, alojamiento de sitios web estáticos, versionado, políticas de ciclo de vida y cifrado.

### Requisitos Previos

- Cuenta de **AWS**.
- Conocimientos básicos de HTML.
- Editor de texto.

### Instrucciones Paso a Paso

#### Parte 1: Crear un Bucket de S3

1. Navegue al servicio **S3**.
2. Haga clic en **"Create bucket"**.
3. **Bucket name**: "mi-sitio-web-[sunombre]-[numeros-aleatorios]".
   - Debe ser globalmente único.
   - Ejemplo: "mi-sitio-web-juan-12345".
   - Solo letras minúsculas, números y guiones.
4. **AWS Region**: Seleccione su región preferida (se recomienda **us-east-1**).
5. **Object Ownership**: ACLs disabled (recomendado).
6. **Ajustes de Block Public Access**:
   - **Desmarque** "Block all public access".
   - **Marque** la casilla de confirmación (necesario para el alojamiento de sitios web).
7. **Bucket Versioning**: Enable.
8. **Tags**:
   - Key: Project.
   - Value: Learning.
9. **Default encryption**: Server-side encryption with Amazon S3 managed keys (SSE-S3).
10. Haga clic en **"Create bucket"**.

#### Parte 2: Cargar Archivos HTML

**Crear el archivo index.html:**

1. Abra el editor de texto.
2. Cree un archivo llamado `index.html`.
3. Pegue el siguiente contenido:

```html
<!DOCTYPE html>
<html>
<head>
    <title>Mi Sitio Web en AWS</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            max-width: 800px;
            margin: 50px auto;
            padding: 20px;
        }
        h1 { color: #FF9900; }
    </style>
</head>
<body>
    <h1>Bienvenido a mi Sitio Web en S3</h1>
    <p>¡Este sitio web está alojado en Amazon S3!</p>
    <p>S3 proporciona almacenamiento de objetos duradero y escalable.</p>
</body>
</html>
```

4. Guarde el archivo.

**Crear el archivo error.html:**

1. Cree un archivo llamado `error.html`.
2. Pegue el siguiente contenido:

```html
<!DOCTYPE html>
<html>
<head>
    <title>Error</title>
</head>
<body>
    <h1>404 - Página No Encontrada</h1>
    <p>La página solicitada no existe.</p>
</body>
</html>
```

3. Guarde el archivo.

**Cargar archivos a S3:**

1. Haga clic en el nombre de su bucket.
2. Haga clic en **"Upload"**.
3. Haga clic en **"Add files"**.
S3!</p>
    <p>S3 provides durable, scalable object storage.</p>
</body>
</html>
```

4. Save file

**Create error.html file:**

1. Create file named `error.html`
2. Paste the following content:

```html
<!DOCTYPE html>
<html>
<head>
    <title>Error</title>
</head>
<body>
    <h1>404 - Página No Encontrada</h1>
    <p>La página solicitada no existe.</p>
</body>
</html>
```

3. Guarde el archivo.

**Cargar archivos a S3:**

1. Haga clic en el nombre de su bucket.
2. Haga clic en **"Upload"**.
3. Haga clic en **"Add files"**.
4. Seleccione `index.html` y `error.html`.
5. Haga clic en **"Upload"**.
6. Espere a que se complete la carga.
7. Haga clic en **"Close"**.

#### Parte 3: Habilitar el Alojamiento de Sitios Web Estáticos

1. En su bucket, vaya a la pestaña **"Properties"**.
2. Desplácese hasta **"Static website hosting"**.
3. Haga clic en **"Edit"**.
4. **Static website hosting**: Enable.
5. **Hosting type**: "Host a static website".
6. **Index document**: index.html.
7. **Error document**: error.html.
8. Haga clic en **"Save changes"**.
9. Desplácese de nuevo hasta "Static website hosting".
10. **Copie la URL del "Bucket website endpoint"** (guárdela para más tarde).

#### Parte 4: Configurar la Política del Bucket para Acceso Público

1. Vaya a la pestaña **"Permissions"**.
2. Desplácese hasta **"Bucket policy"**.
3. Haga clic en **"Edit"**.
4. Pegue la siguiente política (reemplace `YOUR-BUCKET-NAME` con el nombre real de su bucket):

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "PublicReadGetObject",
      "Effect": "Allow",
      "Principal": "*",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::YOUR-BUCKET-NAME/*"
    }
  ]
}
```

5. Haga clic en **"Save changes"**.
6. **Probar el sitio web**: Abra la URL del endpoint del sitio web del bucket en su navegador.
7. ¡Debería ver su página "Bienvenido a mi Sitio Web en S3"!

#### Parte 5: Probar el Versionado

1. Edite `index.html` localmente:
   - Cambie el encabezado a "Bienvenido a mi Sitio Web en S3 Actualizado".
   - Añada una línea: `<p>¡Esta es la versión 2!</p>`.
   - Guarde el archivo.
2. Cargue la nueva versión en **S3**:
   - Vaya al bucket → **"Upload"**.
   - Seleccione el archivo `index.html` modificado.
   - Haga clic en **"Upload"**.
3. Ver las versiones:
   - En el bucket, seleccione `index.html`.
   - Haga clic en la pestaña **"Versions"**.
   - Verá múltiples versiones listadas.
4. Haga clic en la versión anterior para verla o descargarla.
5. Para restaurar una versión anterior:
   - Seleccione la versión anterior.
   - Haga clic en **"Actions"** → **"Download"**.
   - Vuelva a cargarla como una nueva versión.

#### Parte 6: Crear una Política de Ciclo de Vida

1. Vaya a la pestaña **"Management"**.
2. Haga clic en **"Create lifecycle rule"**.
3. **Lifecycle rule name**: "Archivar-Archivos-Antiguos".
4. **Rule scope**: Apply to all objects in the bucket.
5. **Lifecycle rule actions** (marque estas):
   - Transition current versions of objects between storage classes.
   - Expire current versions of objects.
6. **Transition current versions**:
   - **Days after object creation**: 30.
   - **Storage class**: **Standard-IA**.
   - Haga clic en **"Add transition"**.
   - **Days**: 90.
   - **Storage class**: **Glacier Flexible Retrieval**.
7. **Expire current versions of objects**:
   - **Days after object creation**: 365.
8. **Acepte la advertencia** sobre los costos.
9. Haga clic en **"Create rule"**.

#### Parte 7: Habilitar el Cifrado en el Lado del Servidor

1. Vaya a la pestaña **"Properties"**.
2. Desplácese hasta **"Default encryption"**.
3. Haga clic en **"Edit"**.
4. **Encryption type**: Server-side encryption with Amazon S3 managed keys (SSE-S3).
5. **Bucket Key**: Enabled (reduce los costos de cifrado).
6. Haga clic en **"Save changes"**.

### Resultados Esperados

- Bucket de **S3** creado con un nombre único.
- Alojamiento de sitio web estático habilitado y accesible.
- Archivos HTML cargados y visibles vía HTTP.
- Versionado habilitado y probado.
- Política de ciclo de vida creada para el archivado automático.
- Cifrado habilitado para mayor seguridad.

### Verificación

1. **Sitio web accesible**: Visite la URL del endpoint del bucket → Vea su sitio web.
2. **El versionado funciona**: Múltiples versiones visibles en la pestaña **Versions**.
3. **Regla de ciclo de vida creada**: Visible en la pestaña **Management**.
4. **Cifrado habilitado**: Se muestra en la pestaña **Properties**.

### Resolución de Problemas

**Problema:** Error 403 Forbidden al acceder al sitio web.
**Solución:**
- Verifique que la política del bucket permita la lectura pública (s3:GetObject).
- Verifique que "Block all public access" esté desactivado (OFF).
- Asegúrese de que la política del bucket tenga el nombre de bucket correcto.
- Verifique que el ARN incluya /* al final.

**Problema:** Error 404 Not Found.
**Solución:**
- Asegúrese de que `index.html` esté cargado en la raíz del bucket.
- Verifique que el nombre del archivo sea exactamente "index.html" (distingue entre mayúsculas y minúsculas).
- Verifique que el alojamiento de sitios web estáticos esté habilitado.

**Problema:** No se pueden cargar archivos.
**Solución:**
- Verifique que tenga permisos s3:PutObject.
- Verifique que el bucket exista y esté en la región correcta.
- Intente cargar archivos más pequeños primero.

### Limpieza

> **Importante:** Elimine todos los objetos antes de eliminar el bucket.

1. **Vaciar el bucket**:
   - Seleccione su bucket.
   - Haga clic en **"Empty"**.
   - Escriba **"permanently delete"**.
   - Haga clic en **"Empty"**.
   - Espere a que finalice.

2. **Eliminar el bucket**:
   - Seleccione su bucket.
   - Haga clic en **"Delete"**.
   - Escriba el nombre del bucket para confirmar.
   - Haga clic en **"Delete bucket"**.

---

## Laboratorio 5: VPC, Subredes y Configuración de Red

**Duración:** 60 minutos
**Costo:** Gratis (se excluye el **NAT Gateway** para evitar cargos)
**Dificultad:** Avanzado

### Objetivo

Construir una **VPC** personalizada con subredes públicas y privadas, configurar el enrutamiento, grupos de seguridad y **NACLs**.

### Requisitos Previos

- Comprensión de los conceptos básicos de redes (direcciones IP, subredes, CIDR).
- Haber completado el Laboratorio 3 (se requieren conocimientos de **EC2**).

### Instrucciones Paso a Paso

#### Parte 1: Crear la VPC

1. Navegue al servicio **VPC**.
2. Haga clic en **"Create VPC"**.
3. **Resources to create**: "VPC and more" (crea la **VPC** con subredes automáticamente).
4. **Name tag auto-generation**: "MiVPC".
5. **IPv4 CIDR block**: 10.0.0.0/16.
   - Proporciona 65,536 direcciones IP.
6. **IPv6 CIDR block**: No IPv6 CIDR block.
7. **Tenancy**: Default.
8. **Number of Availability Zones**: 2.
9. **Number of public subnets**: 2.
10. **Number of private subnets**: 2.
11. **NAT gateways**: None (para mantenerse en el **Free Tier**).
    - **Importante:** El **NAT Gateway** cuesta $0.045/hora.
12. **VPC endpoints**: None.
13. **DNS options**:
    - Enable DNS hostnames: Yes.
    - Enable DNS resolution: Yes.
14. Haga clic en **"Create VPC"**.
15. Espere a la creación (tarda 1-2 minutos).
16. Haga clic en **"View VPC"**.

#### Parte 2: Revisar los Componentes de la VPC

**Revisar la VPC:**
1. Vaya a **"Your VPCs"**.
2. Verifique que la **VPC** se haya creado con CIDR 10.0.0.0/16.
3. Anote el ID de la **VPC** (comienza con vpc-).

**Revisar las Subredes:**
1. Vaya a **"Subnets"**.
2. Debería ver 4 subredes:
   - **Public subnet 1**: 10.0.0.0/20 (AZ a) - 4096 IPs.
   - **Public subnet 2**: 10.0.16.0/20 (AZ b) - 4096 IPs.
   - **Private subnet 1**: 10.0.128.0/20 (AZ a) - 4096 IPs.
   - **Private subnet 2**: 10.0.144.0/20 (AZ b) - 4096 IPs.

**Revisar el Internet Gateway:**
1. Vaya a **"Internet Gateways"**.
2. Verifique que el **IGW** se haya creado y adjuntado a su **VPC**.
3. Anote el ID del **IGW** (comienza con igw-).

**Revisar las Tablas de Enrutamiento:**
1. Vaya a **"Route Tables"**.
2. Debería ver:
   - **Public route table**:
     - Local route: 10.0.0.0/16 → local.
     - Internet route: 0.0.0.0/0 → igw-xxx.
     - Asociada con las subredes públicas.
   - **Private route tables**:
     - Solo ruta local: 10.0.0.0/16 → local.
     - Asociadas con las subredes privadas.

#### Parte 3: Crear Grupos de Seguridad

**Crear el Grupo de Seguridad para el Servidor Web:**

1. Vaya a **"Security Groups"**.
2. Haga clic en **"Create security group"**.
3. **Security group name**: "WebServer-SG".
4. **Description**: "Permitir HTTP y SSH".
5. **VPC**: Seleccione su **VPC** (MiVPC).
6. **Inbound rules**:
   - Haga clic en **"Add rule"**.
     - Type: SSH.
     - Source: My IP.
   - Haga clic en **"Add rule"**.
     - Type: HTTP.
     - Source: 0.0.0.0/0 (cualquier lugar IPv4).
7. **Outbound rules**: Deje el valor predeterminado (permite todo el tráfico de salida).
8. **Tags**:
   - Key: Name.
   - Value: WebServer-SG.
9. Haga clic en **"Create security group"**.

**Crear el Grupo de Seguridad para la Base de Datos:**

1. Haga clic en **"Create security group"** de nuevo.
2. **Security group name**: "Database-SG".
3. **Description**: "Permitir MySQL desde el WebServer".
4. **VPC**: Seleccione su **VPC** (MiVPC).
5. **Inbound rules**:
   - Haga clic en **"Add rule"**.
     - Type: MySQL/Aurora.
     - Port: 3306.
     - Source: Custom.
     - Busque y seleccione "WebServer-SG".
6. **Outbound rules**: Deje el valor predeterminado.
7. Haga clic en **"Create security group"**.

> **Explicación:** Database-SG solo permite conexiones MySQL desde instancias con WebServer-SG, implementando el principio de mínimo privilegio.

#### Parte 4: Lanzar una Instancia EC2 en la VPC Personalizada

1. Vaya al servicio **EC2**.
2. Haga clic en **"Launch instance"**.
3. **Name**: "VPC-Test-Instance".
4. **AMI**: **Amazon Linux 2023 AMI** (**Free Tier**).
5. **Instance type**: **t2.micro**.
6. **Key pair**: Use uno existente o cree uno nuevo.
7. **Network settings**:
   - Haga clic en **"Edit"**.
   - **VPC**: Seleccione MiVPC.
   - **Subnet**: Seleccione una subred pública (10.0.0.0/20 o 10.0.16.0/20).
   - **Auto-assign public IP**: Enable.
   - **Firewall**: Select existing security group.
   - Seleccione **WebServer-SG**.
8. Mantenga los demás valores predeterminados.
9. Haga clic en **"Launch instance"**.
10. Espere a que la instancia alcance el estado "Running".
11. **Verificación**: La instancia tiene una IP pública y puede conectarse a ella vía **SSH**.

#### Parte 5: Crear una ACL de Red (NACL)

1. Vaya a **"Network ACLs"** en **VPC**.
2. Haga clic en **"Create network ACL"**.
3. **Name**: "Custom-NACL".
4. **VPC**: Seleccione MiVPC.
5. Haga clic en **"Create network ACL"**.

**Configurar las Reglas de Entrada (Inbound):**

1. Seleccione su **NACL**.
2. Vaya a la pestaña **"Inbound rules"**.
3. Haga clic en **"Edit inbound rules"**.
4. Haga clic en **"Add new rule"** y añada:
   - **Rule 100**:
     - Type: HTTP (80).
     - Source: 0.0.0.0/0.
     - Allow.
   - **Rule 110**:
     - Type: SSH (22).
     - Source: 0.0.0.0/0.
     - Allow.
   - **Rule 120**:
     - Type: Custom TCP.
     - Port range: 1024-65535 (puertos efímeros).
     - Source: 0.0.0.0/0.
     - Allow.
   - **Rule \* (predeterminada)**: All traffic, Deny.
5. Haga clic en **"Save changes"**.

**Configurar las Reglas de Salida (Outbound):**

1. Vaya a la pestaña **"Outbound rules"**.
2. Haga clic en **"Edit outbound rules"**.
3. Añada las reglas:
   - **Rule 100**:
     - Type: HTTP (80).
     - Destination: 0.0.0.0/0.
     - Allow.
   - **Rule 110**:
     - Type: HTTPS (443).
     - Destination: 0.0.0.0/0.
     - Allow.
   - **Rule 120**:
     - Type: Custom TCP.
     - Port range: 1024-65535.
     - Destination: 0.0.0.0/0.
     - Allow.
4. Haga clic en **"Save changes"**.

**Asociar la NACL con una Subred (Opcional):**

1. Vaya a la pestaña **"Subnet associations"**.
2. Haga clic en **"Edit subnet associations"**.
3. Seleccione una subred pública.
4. Haga clic en **"Save changes"**.

> **Nota:** La **NACL** predeterminada permite todo el tráfico. Las **NACLs** personalizadas deniegan todo el tráfico por defecto.

### Resultados Esperados

- **VPC** personalizada creada con CIDR 10.0.0.0/16.
- 2 subredes públicas y 2 subredes privadas en 2 **AZs**.
- **Internet Gateway** adjunto y rutas configuradas.
- Grupos de seguridad creados para el servidor web y la base de datos.
- Instancia **EC2** lanzada en una subred pública.
- **NACL** personalizada creada y configurada.

### Verificación

1. **La VPC existe**: Aparece en "Your VPCs" con el CIDR correcto.
2. **Subredes creadas**: 4 subredes visibles con los bloques CIDR correctos.
3. **El enrutamiento funciona**: La instancia **EC2** en la subred pública tiene acceso a internet.
4. **Los grupos de seguridad funcionan**: Se puede conectar vía **SSH** a la instancia, HTTP accesible.
5. **NACLs configuradas**: Reglas visibles en la **NACL**.

### Resolución de Problemas

**Problema:** No se puede crear la **VPC**.
**Solución:**
- Verifique que no haya excedido el límite de **VPCs** (5 por región por defecto).
- Verifique que el bloque CIDR no se superponga con **VPCs** existentes.

**Problema:** La instancia **EC2** no tiene acceso a internet.
**Solución:**
- Verifique que la instancia esté en una subred pública.
- Verifique que la tabla de enrutamiento tenga una ruta al **IGW** (0.0.0.0/0 → igw-xxx).
- Asegúrese de que la asignación automática de IP pública esté habilitada.
- Verifique que la **NACL** permita el tráfico.

**Problema:** No se puede conectar vía **SSH** a la instancia.
**Solución:**
- Verifique que el grupo de seguridad permita SSH desde su IP.
- Verifique que la **NACL** permita SSH y los puertos efímeros.
- Asegúrese de que la instancia tenga una IP pública.
- Verifique la configuración de la tabla de enrutamiento.

### Limpieza

**Elimine los recursos en orden:**

1. **Terminar la instancia EC2**:
   - Vaya a **EC2** → **Instances**.
   - Seleccione la instancia → Terminate.

2. **Eliminar los grupos de seguridad personalizados**:
   - Vaya a **VPC** → **Security Groups**.
   - Seleccione los SGs personalizados → Delete.
   - Nota: El SG predeterminado (default) no se puede eliminar.

3. **Eliminar las NACLs personalizadas**:
   - Vaya a **VPC** → **Network ACLs**.
   - Desasóciela de las subredes primero.
   - Seleccione la **NACL** personalizada → Delete.
   - Nota: La **NACL** predeterminada no se puede eliminar.

4. **Eliminar la VPC**:
   - Vaya a **VPC** → **Your VPCs**.
   - Seleccione su **VPC** → Delete **VPC**.
   - Esto eliminará:
     - Subredes.
     - Tablas de enrutamiento (excepto la predeterminada).
     - **Internet Gateway**.
     - La **VPC** misma.
   - Confirme la eliminación.

---

## Laboratorio 6: Amazon RDS Database

**Duración:** 30 minutos
**Costo:** Gratis (**db.t3.micro** o **db.t4g.micro** en el **Free Tier**)
**Dificultad:** Intermedio

### Objetivo

Lanzar una base de datos MySQL gestionada utilizando **Amazon RDS**.

### Requisitos Previos

- Comprensión básica de las bases de datos relacionales.
- Haber completado el Laboratorio 3 (conocimientos de **EC2**) y el Laboratorio 5 (conocimientos de **VPC**).

### Instrucciones Paso a Paso

#### Parte 1: Crear la Base de Datos RDS

1. Navegue al servicio **RDS**.
2. Haga clic en **"Create database"**.
3. **Database creation method**: Standard create.
4. **Engine options**:
   - Engine type: MySQL.
   - Version: MySQL 8.0.xx (la más reciente).
5. **Templates**: **Free tier**.
   - **Importante:** Esto configura automáticamente las opciones elegibles para la capa gratuita.
6. **Settings**:
   - **DB instance identifier**: "mibase-de-datos".
   - **Master username**: admin.
   - **Credentials management**: Self managed.
   - **Master password**: Cree una contraseña segura (mínimo 8 caracteres).
   - **Confirm password**: Vuelva a introducir la contraseña.
   - **¡Guarde la contraseña de forma segura!**

#### Parte 2: Configurar la Instancia

1. **DB instance class**:
   - Burstable classes (incluye las clases t).
   - **db.t3.micro** o **db.t4g.micro** (Elegibles para el **Free Tier**).
2. **Storage**:
   - Storage type: General Purpose SSD (gp3).
   - Allocated storage: 20 GiB.
   - **Desmarque** "Enable storage autoscaling" (para controlar los costos).
3. **Storage autoscaling**: Disabled.

#### Parte 3: Configurar la Conectividad

1. **Compute resource**:
   - Don't connect to an **EC2** compute resource.
2. **Network type**: IPv4.
3. **Virtual private cloud (VPC)**:
   - Seleccione la **VPC** predeterminada (o su **VPC** personalizada si tiene una).
4. **DB subnet group**: Default.
5. **Public access**: **No** (recomendado por seguridad).
   - La base de datos no tendrá IP pública.
   - Solo será accesible desde **EC2** en la misma **VPC**.
6. **VPC security group**:
   - Choose existing.
   - Create new.
   - **Name**: "rds-mysql-sg".
7. **Availability Zone**: No preference.
8. **Database port**: 3306 (predeterminado).

#### Parte 4: Configuración Adicional

1. Expanda **"Additional configuration"**.
2. **Database options**:
   - **Initial database name**: "mibase".
   - Esto crea una base de datos automáticamente.
3. **Backup**:
   - **Desmarque** "Enable automated backups" (para mantenerse en el **Free Tier**).
   - El **Free Tier** permite respaldos, pero por seguridad desactívelos.
4. **Encryption**:
   - **Desmarque** "Enable encryption" (opcional, por simplicidad).
   - En producción, siempre habilite el cifrado.
5. **Monitoring**:
   - **Desmarque** "Enable Enhanced monitoring" (para evitar cargos).
6. **Maintenance**:
   - Mantenga los valores predeterminados.
7. Haga clic en **"Create database"**.
8. Espere 5-10 minutos para la creación de la base de datos.
9. El **Status** cambiará: Creating → Backing up → Available.

#### Parte 5: Revisar los Detalles de la Base de Datos

1. Una vez que el estado sea **"Available"**, haga clic en el nombre de la base de datos.
2. Pestaña **"Connectivity & security"**:
   - Anote el **Endpoint** (ej: mibase-de-datos.xxxxx.us-east-1.rds.amazonaws.com).
   - Anote el **Port**: 3306.
   - **Security group**: Haga clic para ver las reglas.
3. Pestaña **"Configuration"**:
   - Verifique la clase de instancia de BD, el almacenamiento y la versión.

#### Parte 6: Conectarse a RDS (Requiere EC2 en la misma VPC)

**Lanzar una instancia EC2 (si no tiene una):**

1. Vaya a **EC2** → Launch instance.
2. Use la misma **VPC** que **RDS**.
3. Seleccione una subred pública.
4. Use **Amazon Linux 2023 AMI**.
5. Lance la instancia y conéctese vía **SSH**.

**Instalar el cliente MySQL en EC2:**

```bash
sudo yum update -y
sudo yum install -y mariadb105
```

**Actualizar el Grupo de Seguridad de RDS:**

1. Vaya a **VPC** → **Security Groups**.
2. Seleccione el grupo de seguridad de **RDS** (rds-mysql-sg).
3. Edite las reglas de entrada (**inbound rules**).
4. Añada la regla:
   - Type: MySQL/Aurora.
   - Port: 3306.
   - Source: Grupo de seguridad de su instancia **EC2**.
5. Guarde las reglas.

**Conectarse a RDS desde EC2:**

```bash
mysql -h mibase-de-datos.xxxxx.us-east-1.rds.amazonaws.com -u admin -p
```

Reemplace con su endpoint real de **RDS**.

Introduzca la contraseña cuando se le solicite.

**Probar la base de datos:**

```sql
SHOW DATABASES;
USE mibase;
CREATE TABLE usuarios (id INT, nombre VARCHAR(50));
INSERT INTO usuarios VALUES (1, 'Juan Perez');
INSERT INTO usuarios VALUES (2, 'Maria Garcia');
SELECT * FROM usuarios;
```

Resultado esperado:
```
+------+--------------+
| id   | nombre       |
+------+--------------+
|    1 | Juan Perez   |
|    2 | Maria Garcia |
+------+--------------+
```

**Salir de MySQL:**
```sql
exit;
```

### Resultados Esperados

- Base de datos **RDS** MySQL creada y funcionando.
- Base de datos accesible desde una instancia **EC2** en la misma **VPC**.
- Conexión exitosa y ejecución de comandos SQL.
- Tabla de prueba creada con datos de ejemplo.

### Verificación

1. **RDS muestra el estado "Available"**.
2. **Se puede conectar desde EC2** usando el cliente MySQL.
3. **Los comandos SQL se ejecutan correctamente**.
4. **Sin acceso público** (configuración más segura).

### Resolución de Problemas

**Problema:** No se puede conectar a **RDS** desde **EC2**.
**Solución:**
- Verifique que ambos estén en la misma **VPC**.
- Verifique que el grupo de seguridad de **RDS** permita MySQL (3306) desde el grupo de seguridad de **EC2**.
- Asegúrese de usar el endpoint y las credenciales correctas.
- Verifique que el estado de **RDS** sea "Available".

**Problema:** Error de acceso denegado.
**Solución:**
- Verifique el nombre de usuario (admin) y la contraseña.
- Asegúrese de que la contraseña se haya introducido correctamente (distingue mayúsculas de minúsculas).
- Verifique que el usuario exista en la base de datos.

**Problema:** Tiempo de espera de conexión agotado.
**Solución:**
- Verifique las reglas del grupo de seguridad.
- Verifique que **EC2** y **RDS** estén en la misma **VPC**.
- Asegúrese de que **RDS** no sea accesible públicamente (no se puede conectar desde fuera de la **VPC**).
- Verifique las **NACLs**.

### Limpieza

> **Crítico:** Las instancias de **RDS** incurren en cargos si se ejecutan más allá de las horas del **Free Tier** (750 horas/mes).

1. Vaya a la consola de **RDS**.
2. Seleccione su base de datos.
3. Haga clic en **"Actions"** → **"Delete"**.
4. Opciones de eliminación:
   - **Desmarque** "Create final snapshot" (para fines del laboratorio).
   - **Desmarque** "Retain automated backups".
   - **Marque** "I acknowledge that upon instance deletion...".
5. Escriba **"delete me"** para confirmar.
6. Haga clic en **"Delete"**.
7. La eliminación tarda 2-5 minutos.
8. Verifique que la base de datos se haya eliminado de la lista.

---

## Laboratorio 7: Monitoreo y Alarmas con CloudWatch

**Duración:** 25 minutos
**Costo:** Gratis (dentro de los límites del **Free Tier**)
**Dificultad:** Principiante

### Objetivo

Monitorear instancias **EC2** utilizando métricas de **CloudWatch** y crear alarmas para notificaciones.

### Requisitos Previos

- Instancia **EC2** en ejecución (del Laboratorio 3 o una nueva).
- Dirección de correo electrónico para notificaciones.

### Instrucciones Paso a Paso

#### Parte 1: Lanzar una Instancia EC2 (si es necesario)

1. Lance una instancia **EC2** **t2.micro** si no tiene una.
2. Espere a que la instancia alcance el estado "Running".
3. Anote el ID de la instancia.

#### Parte 2: Explorar las Métricas de CloudWatch

1. Navegue al servicio **CloudWatch**.
2. Haga clic en **"All metrics"** en el menú izquierdo.
3. Haga clic en **"EC2"**.
4. Haga clic en **"Per-Instance Metrics"**.
5. Busque el ID de su instancia.
6. Seleccione las métricas:
   - **CPUUtilization**
   - **NetworkIn**
   - **NetworkOut**
7. Observe las métricas graficadas.
8. Cambie el rango de tiempo (1 hora, 3 horas, 1 día).
9. Cambie el período (1 minuto, 5 minutos).

#### Parte 3: Crear una Alarma de CloudWatch

1. Seleccione la métrica **"CPUUtilization"** (casilla de verificación).
2. Haga clic en **"Actions"** → **"Create alarm"**.
3. **Metric and conditions**:
   - Metric name: CPUUtilization.
   - Statistic: Average.
   - Period: 5 minutes.
4. **Conditions**:
   - Threshold type: Static.
   - Whenever CPUUtilization is: Greater.
   - than: **70**.
5. Haga clic en **"Next"**.

#### Parte 4: Configurar la Notificación de SNS

1. **Alarm state trigger**: In alarm.
2. **SNS topic**:
   - Create new topic.
   - **Topic name**: "Alertas-EC2".
   - **Email endpoints**: Ingrese su dirección de correo electrónico.
3. Haga clic en **"Create topic"**.
4. Haga clic en **"Next"**.
5. **Alarm name**: "Alerta-CPU-Alta".
6. **Alarm description**: "Alerta cuando la CPU de EC2 supere el 70%".
7. Haga clic en **"Next"**.
8. Revise los ajustes.
9. Haga clic en **"Create alarm"**.
10. **Revise su correo electrónico** y haga clic en el enlace de confirmación en el correo de suscripción de **SNS**.

#### Parte 5: Probar la Alarma (Opcional)

> **Advertencia:** Esto estresará su CPU. Solo hágalo si desea probarla.

**Conectarse vía SSH a la instancia EC2:**

```bash
ssh -i su-clave.pem ec2-user@[public-ip]
```

**Instalar la herramienta stress:**

```bash
sudo yum install -y stress
```

**Ejecutar la prueba de estrés de CPU:**

```bash
stress --cpu 2 --timeout 300
```

Esto se ejecutará durante 5 minutos (300 segundos).

**Monitorear la alarma:**

1. Vaya a **CloudWatch** → **Alarms**.
2. Espere 5-10 minutos.
3. El estado de la alarma cambiará: OK → In alarm.
4. Recibirá una notificación por correo electrónico.
5. Una vez que la prueba de estrés finalice, la alarma volverá al estado OK.

#### Parte 6: Ver los Logs de CloudWatch

1. En **CloudWatch**, vaya a **"Logs"** → **"Log groups"**.
2. Haga clic en **"Create log group"**.
3. **Log group name**: "/aws/mi-aplicacion".
4. Haga clic en **"Create"**.
5. Haga clic en el grupo de logs para explorar.
6. Nota: Aún no hay logs (es necesario configurar la aplicación para que los envíe).

**Explorar grupos de logs existentes:**
- Busque /aws/lambda/, /aws/rds/, etc.
- Haga clic en el grupo de logs → Log streams → Ver logs.

### Resultados Esperados

- Métricas de **CloudWatch** visibles para la instancia **EC2**.
- Alarma de CPU creada con un umbral del 70%.
- Tema de **SNS** creado y suscripción por correo confirmada.
- Notificación por correo recibida cuando se activa la alarma (si se probó).
- Grupo de logs creado.

### Verificación

1. **Alarma visible** en **CloudWatch** → **Alarms**.
2. **Suscripción de SNS confirmada** (revise su correo).
3. **Métricas mostrándose** en los gráficos.
4. **La alarma se activa correctamente** (si se realizó la prueba de estrés).

### Resolución de Problemas

**Problema:** No se muestran métricas para **EC2**.
**Solución:**
- Espere 5-10 minutos después del lanzamiento de la instancia.
- Verifique que la instancia esté funcionando.
- Verifique que se haya seleccionado la región correcta.
- Actualice la página.

**Problema:** No se recibió la notificación por correo electrónico.
**Solución:**
- Revise la carpeta de spam/correo no deseado.
- Verifique que la dirección de correo se haya introducido correctamente.
- Reenvíe la confirmación desde la consola de **SNS**.
- Verifique el estado de la suscripción de **SNS** (debe ser "Confirmed").

**Problema:** La alarma no se activa.
**Solución:**
- Verifique que la CPU esté superando realmente el 70%.
- Verifique la configuración de la alarma y el umbral.
- Espere el período de evaluación (5 minutos).
- Revise el historial de la alarma en los detalles.

### Limpieza

1. **Eliminar la alarma de CloudWatch**:
   - Vaya a **CloudWatch** → **Alarms**.
   - Seleccione la alarma → **Actions** → **Delete**.
   - Confirme la eliminación.

2. **Eliminar el tema de SNS**:
   - Vaya a **SNS** → **Topics**.
   - Seleccione el tema → **Delete**.
   - Escriba **"delete me"**.
   - Confirme la eliminación.

3. **Eliminar el grupo de logs**:
   - Vaya a **CloudWatch** → **Log groups**.
   - Seleccione el grupo de logs → **Actions** → **Delete**.
   - Confirme la eliminación.

4. **Terminar la instancia EC2**:
   - Vaya a **EC2** → **Instances**.
   - Seleccione la instancia → **Instance state** → **Terminate**.

---

## Laboratorio 8: Herramientas de Gestión de Costos de AWS

**Duración:** 30 minutos
**Costo:** Gratis
**Dificultad:** Principiante

### Objetivo

Explorar las herramientas de facturación y gestión de costos de **AWS**, incluyendo el **Pricing Calculator**, **Cost Explorer**, **Budgets** y **Trusted Advisor**.

### Requisitos Previos

- Cuenta de **AWS** con algo de uso (aunque sea mínimo).
- Acceso a facturación habilitado.

### Instrucciones Paso a Paso

#### Parte 1: AWS Pricing Calculator

1. Abra el navegador y visite [https://calculator.aws](https://calculator.aws)
2. Haga clic en **"Create estimate"**.
3. **Añadir EC2**:
   - Busque "EC2".
   - Haga clic en **"Configure"**.
   - **Region**: Seleccione **us-east-1**.
   - **Quick estimate**:
     - Number of instances: 10.
     - Instance type: **t3.medium**.
   - **Pricing model**: On-Demand.
   - Revise la estimación de costo mensual.
   - Haga clic en **"Add to my estimate"**.

4. **Añadir S3**:
   - Busque "S3".
   - Haga clic en **"Configure"**.
   - **S3 Standard storage**: 1000 GB.
   - **PUT/COPY/POST requests**: 100,000.
   - **GET requests**: 1,000,000.
   - Revise el costo.
   - Haga clic en **"Add to my estimate"**.

5. **Añadir RDS**:
   - Busque "RDS".
   - Haga clic en **"Configure"**.
   - **Database engine**: MySQL.
   - **Instance type**: **db.t3.medium**.
   - **Deployment**: Single-AZ.
   - **Storage**: 100 GB.
   - **Pricing model**: On-Demand.
   - Haga clic en **"Add to my estimate"**.

6. **Revisar el total**:
   - Vea el costo mensual estimado.
   - Compare diferentes modelos de precios:
     - On-Demand.
     - **Reserved Instances** (1 año, 3 años).
     - **Savings Plans**.
   - Note el ahorro potencial.

7. **Exportar la estimación**:
   - Haga clic en **"Export"** → **"PDF"** o **"CSV"**.
   - Guarde para referencia.

8. **Compartir la estimación**:
   - Haga clic en **"Share"**.
   - Copie el enlace para compartir.
   - Puede enviarlo a colegas o guardarlo para más tarde.

#### Parte 2: AWS Cost Explorer

> **Nota:** El **Cost Explorer** tarda 24 horas en mostrar datos en cuentas nuevas.

1. Vaya a la consola de **Billing and Cost Management**.
2. Haga clic en **"Cost Explorer"** en el menú izquierdo.
3. Haga clic en **"Launch Cost Explorer"** (si es la primera vez).
4. Espere a la inicialización (si la cuenta es nueva, los datos aparecerán en 24 horas).

**Explorar los costos (si hay datos disponibles):**

5. **Ver costos mensuales**:
   - La vista predeterminada muestra los últimos 6 meses.
   - Vea los costos por servicio.
   - Identifique los servicios principales.

6. **Filtrar por servicio**:
   - Haga clic en el desplegable de filtros.
   - Seleccione un servicio específico (**EC2**, **S3**, etc.).
   - Vea los costos específicos del servicio.

7. **Agrupar por (Group by)**:
   - Servicio.
   - Región.
   - Etiqueta (**Tag**).
   - Tipo de instancia.

8. **Ver previsión (Forecast)**:
   - Vea los costos proyectados para el próximo mes.
   - Basado en los patrones de uso actuales.

9. **Crear un informe personalizado**:
   - Seleccione el rango de fechas.
   - Elija agrupaciones y filtros.
   - Haga clic en **"Save to report library"**.
   - Nombre: "Desglose Mensual por Servicio".
   - Guarde el informe para uso futuro.

10. **Descargar CSV**:
    - Haga clic en **"Download CSV"**.
    - Ábralo en una hoja de cálculo para analizarlo.

#### Parte 3: Revisar las Facturas

1. Vaya a **"Bills"** en la consola de **Billing**.
2. **Ver los cargos del mes actual**:
   - Expanda los servicios para ver el desglose.
   - Vea los cargos por región.
   - Revise los costos de transferencia de datos.
3. **Revisar el uso del Free Tier**:
   - Haga clic en **"Free Tier"** en el menú izquierdo.
   - Vea el uso del mes actual frente a los límites del **Free Tier**.
   - **Importante:** Monitoree esto para evitar cargos.
   - Vea las advertencias para los servicios que se acercan a los límites.
4. **Descargar la factura**:
   - Haga clic en **"Download CSV"**.
   - Guarde para sus registros.

#### Parte 4: AWS Budgets

1. Vaya a **"Budgets"** en la consola de **Billing**.
2. Revise el presupuesto creado en el Laboratorio 1 (si lo hizo).
3. **Crear un presupuesto adicional**:
   - Haga clic en **"Create budget"**.
   - **Budget type**: Usage budget.
   - **Service**: **Amazon Elastic Compute Cloud**.
   - Haga clic en **"Next"**.

4. **Establecer los detalles del presupuesto**:
   - **Budget name**: "Presupuesto-Uso-EC2".
   - **Period**: Monthly.
   - **Usage type**: Running Hours.
   - **Unit**: Hrs.
   - **Amount**: 750 (Límite del **Free Tier**).
   - Haga clic en **"Next"**.

5. **Configurar la alerta**:
   - **Threshold**: 80% del monto presupuestado.
   - **Email recipients**: Su correo electrónico.
   - Haga clic en **"Add alert threshold"**.
   - **Second threshold**: 100%.
   - Haga clic en **"Next"**.

6. Revise y haga clic en **"Create budget"**.

7. **Ver presupuestos**:
   - Vea todos los presupuestos en el panel de control.
   - Monitoree el uso actual frente al presupuesto.
   - Vea el historial de alertas.

#### Parte 5: AWS Trusted Advisor

1. Navegue al servicio **Trusted Advisor**.
2. **Resumen del panel (Dashboard)**:
   - Vea las comprobaciones por categoría:
     - **Cost Optimization** (Optimización de Costos).
     - **Performance** (Rendimiento).
     - **Security** (Seguridad).
     - **Fault Tolerance** (Tolerancia a Fallos).
     - **Service Limits** (Límites de Servicio).

3. **Revisar las 7 comprobaciones principales** (disponibles en el soporte **Basic**):
   - **S3 Bucket Permissions**:
     - Busca buckets accesibles públicamente.
     - Haga clic para ver detalles.
   - **Security Groups - Specific Ports Unrestricted**:
     - Identifica reglas demasiado permisivas.
   - **IAM Use**:
     - Verifica si está utilizando **IAM**.
   - **MFA on Root Account**:
     - Verifica si **MFA** está habilitado.
   - **EBS Public Snapshots**:
     - Busca **snapshots** públicos.
   - **RDS Public Snapshots**:
     - Busca **snapshots** públicos.
   - **Service Limits**:
     - Muestra el uso frente a los límites.

4. **Haga clic en cada comprobación**:
   - Vea detalles y recomendaciones.
   - Tome medidas sobre las advertencias.
   - Verde = bien, Amarillo = investigar, Rojo = acción necesaria.

5. **Actualizar comprobaciones**:
   - Haga clic en **"Refresh all"**.
   - Las comprobaciones se actualizan (puede tardar unos minutos).

> **Nota:** Las funciones completas de **Trusted Advisor** requieren un plan de soporte **Business** o **Enterprise**.

### Resultados Esperados

- Estimación de costos creada en el **Pricing Calculator**.
- Exploración del **Cost Explorer** (si hay datos disponibles).
- Revisión de la facturación actual y el uso del **Free Tier**.
- Presupuesto de uso creado para **EC2**.
- Revisión de las recomendaciones de **Trusted Advisor**.

### Verificación

1. **Estimación de precios creada** y compartible.
2. **Cost Explorer iniciado** (los datos pueden tardar 24 horas).
3. **Factura actual visible** con el desglose por servicio.
4. **Presupuestos configurados** con alertas por correo.
5. **Comprobaciones de Trusted Advisor revisadas**.

### Resolución de Problemas

**Problema:** No se puede acceder a la información de facturación.
**Solución:**
- Habilite el acceso de **IAM** a la facturación en la configuración de la cuenta.
- Inicie sesión como usuario raíz o usuario de **IAM** con permisos de facturación.

**Problema:** El **Cost Explorer** no muestra datos.
**Solución:**
- Espere 24 horas después de la creación de la cuenta.
- Asegúrese de tener algo de uso (lance servicios).
- Actualice la página.

**Problema:** **Trusted Advisor** muestra pocas comprobaciones.
**Solución:**
- El soporte **Basic** solo incluye 7 comprobaciones principales.
- Actualice a **Business**/**Enterprise** para obtener todas las comprobaciones.
- Este es el comportamiento esperado.

### Limpieza

> **Nota:** Mantenga activos los presupuestos y las comprobaciones de **Trusted Advisor** para una protección continua. No se necesita limpieza.

---

## Laboratorio 9: Función Serverless Lambda

**Duración:** 25 minutos
**Costo:** Gratis (1 millón de solicitudes/mes en el **Free Tier**)
**Dificultad:** Intermedio

### Objetivo

Crear una función **Lambda** serverless con un activador (**trigger**) de **API Gateway**.

### Requisitos Previos

- Conocimientos básicos de programación (Python es útil pero no obligatorio).
- Comprensión de los conceptos de API.

### Instrucciones Paso a Paso

#### Parte 1: Crear la Función Lambda

1. Navegue al servicio **Lambda**.
2. Haga clic en **"Create function"**.
3. **Function option**: Author from scratch.
4. **Function name**: "FuncionHolaMundo".
5. **Runtime**: Python 3.12 (o el más reciente disponible).
6. **Architecture**: x86_64.
7. **Permissions**:
   - Execution role: Create a new role with basic Lambda permissions.
   - Role name: (autogenerado).
8. Haga clic en **"Create function"**.
9. Espere a la creación de la función.

#### Parte 2: Escribir el Código de la Función

1. En la sección **Code source**:
2. Elimine el código existente en `lambda_function.py`.
3. Pegue el siguiente código:

```python
import json

def lambda_handler(event, context):
    # Obtener el nombre del evento, por defecto 'Mundo'
    name = event.get('name', 'Mundo')

    # Crear la respuesta
    message = f'¡Hola, {name}!'

    return {
        'statusCode': 200,
        'body': json.dumps(message),
        'headers': {
            'Content-Type': 'application/json'
        }
    }
```

4. Haga clic en **"Deploy"** (¡importante!).
5. Espere al mensaje "Successfully deployed".

#### Parte 3: Probar la Función

1. Haga clic en el botón **"Test"**.
2. **Configurar el evento de prueba**:
   - **Event name**: "EventoPrueba".
   - **Event JSON**:
   ```json
   {
     "name": "Estudiante AWS"
   }
   ```
3. Haga clic en **"Save"**.
4. Haga clic en **"Test"** de nuevo.
5. **Ver los resultados de la ejecución**:
   - **Status**: Succeeded.
   - **Response**:
   ```json
   {
     "statusCode": 200,
     "body": "\"¡Hola, Estudiante AWS!\"",
     "headers": {
       "Content-Type": "application/json"
     }
   }
   ```
6. Vea los **logs** en la salida.
7. Note el tiempo de ejecución y la memoria utilizada.

#### Parte 4: Ver los Logs de CloudWatch

1. Haga clic en la pestaña **"Monitor"**.
2. Haga clic en **"View CloudWatch logs"**.
3. Haga clic en el flujo de logs más reciente.
4. Vea los detalles del log:
   - START RequestId.
   - Salida de la función.
   - END RequestId.
   - REPORT (duración, memoria).

#### Parte 5: Configurar el Activador de API Gateway

1. Vuelva a la **función Lambda** (pestaña Code).
2. Haga clic en **"Add trigger"**.
3. **Select a trigger**: **API Gateway**.
4. **API type**: HTTP API.
5. **Security**: Open.
   - **Advertencia:** Esto hace que la API sea accesible públicamente.
   - Para producción, use autenticación.
6. Haga clic en **"Add"**.
7. Espere a la creación del activador.

#### Parte 6: Probar el Endpoint de la API

1. En **Configuration** → **Triggers**, haga clic en **API Gateway**.
2. **Copie la URL del endpoint de la API**.
   - Ejemplo: `https://abc123.execute-api.us-east-1.amazonaws.com/default/FuncionHolaMundo`.
3. **Probar en el navegador**:
   - Pegue la URL en el navegador.
   - Añada un parámetro de consulta: `?name=SuNombre`.
   - URL completa: `https://abc123.execute-api.us-east-1.amazonaws.com/default/FuncionHolaMundo?name=Juan`.
   - **Resultado:** Debería ver: `"¡Hola, Juan!"`.

4. **Probar con curl (terminal)**:
   ```bash
   curl "https://su-url-de-api.execute-api.us-east-1.amazonaws.com/default/FuncionHolaMundo?name=Juan"
   ```

5. **Probar con diferentes nombres**:
   - Pruebe `?name=AWS`.
   - Pruebe sin parámetros (debería devolver "¡Hola, Mundo!").

#### Parte 7: Modificar la Función

1. Vuelva a la pestaña **Code**.
2. Modifique el código para añadir más funcionalidad:

```python
import json
from datetime import datetime

def lambda_handler(event, context):
    # Obtener el nombre del evento o de los parámetros de consulta
    name = event.get('name')
    if not name and 'queryStringParameters' in event:
        name = event['queryStringParameters'].get('name', 'Mundo')
    else:
        name = name or 'Mundo'

    # Obtener la hora actual
    current_time = datetime.now().strftime('%Y-%m-%d %H:%M:%S')

    # Crear la respuesta
    message = {
        'saludo': f'¡Hola, {name}!',
        'timestamp': current_time,
        'requestId': context.request_id
    }

    return {
        'statusCode': 200,
        'body': json.dumps(message),
        'headers': {
            'Content-Type': 'application/json'
        }
    }
```

3. Haga clic en **"Deploy"**.
4. **Pruebe de nuevo** con el endpoint de la API.
5. Ahora la respuesta incluye la marca de tiempo (**timestamp**) y el ID de la solicitud.

### Resultados Esperados

- Función **Lambda** creada y desplegada.
- La función se ejecuta correctamente con eventos de prueba.
- Los logs de **CloudWatch** capturan la salida de la función.
- Activador de **API Gateway** configurado.
- Función accesible vía endpoint público HTTPS.
- Función modificada con funcionalidad mejorada.

### Verificación

1. **El evento de prueba se ejecuta con éxito**.
2. **El endpoint de la API devuelve la respuesta correcta**.
3. **Los logs de CloudWatch muestran los detalles de la ejecución**.
4. **Diferentes entradas producen diferentes salidas**.

### Resolución de Problemas

**Problema:** La función falla con un error de sintaxis.
**Solución:**
- Verifique la indentación de Python (use espacios, no pestañas).
- Verifique que todas las comillas y paréntesis coincidan.
- Revise el error en los logs de **CloudWatch**.

**Problema:** La API devuelve "Internal Server Error".
**Solución:**
- Revise los logs de **CloudWatch** para ver los detalles del error.
- Verifique que la función se haya desplegado tras los cambios de código.
- Asegúrese de que el formato JSON sea correcto en la respuesta.

**Problema:** No se puede acceder al endpoint de la API.
**Solución:**
- Verifique que se haya añadido el activador de **API Gateway**.
- Verifique que la seguridad esté establecida en "Open".
- Asegúrese de usar el método HTTP correcto (GET).
- Intente en un navegador diferente o en modo incógnito.

**Problema:** Los parámetros de consulta no funcionan.
**Solución:**
- Use el código modificado que verifica `queryStringParameters`.
- Formatee la URL correctamente: `?name=Valor`.
- Verifique los ajustes de integración de **API Gateway**.

### Limpieza

1. **Eliminar la función Lambda**:
   - Seleccione la función.
   - **Actions** → **Delete**.
   - Escriba "delete".
   - Confirme la eliminación.

2. **Eliminar el API Gateway**:
   - Vaya al servicio **API Gateway**.
   - Seleccione su API.
   - **Actions** → **Delete**.
   - Confirme la eliminación.

> **Nota:** Los logs de **CloudWatch** persisten tras eliminar la función. Elimine el grupo de logs si lo desea:
> - **CloudWatch** → **Log groups** → Seleccione `/aws/lambda/FuncionHolaMundo` → **Delete**.

---

## Laboratorio 10: Infraestructura como Código con CloudFormation

**Duración:** 20 minutos
**Costo:** Gratis (los recursos creados son elegibles para el **Free Tier**)
**Dificultad:** Intermedio

### Objetivo

Desplegar infraestructura de **AWS** utilizando plantillas de **CloudFormation** (Infraestructura como Código).

### Requisitos Previos

- Comprensión de YAML o JSON.
- Editor de texto.
- Familiaridad con **S3** (del Laboratorio 4).

### Instrucciones Paso a Paso

#### Parte 1: Crear la Plantilla de CloudFormation

1. Abra el editor de texto.
2. Cree un archivo llamado `simple-stack.yaml`.
3. Pegue la siguiente plantilla:

```yaml
AWSTemplateFormatVersion: '2010-09-09'
Description: Pila simple de bucket S3 para aprender CloudFormation

Resources:
  MyS3Bucket:
    Type: AWS::S3::Bucket
    Properties:
      BucketName: !Sub 'cf-bucket-${AWS::AccountId}'
      VersioningConfiguration:
        Status: Enabled
      Tags:
        - Key: Environment
          Value: Learning
        - Key: ManagedBy
          Value: CloudFormation

Outputs:
  BucketName:
    Description: Nombre del bucket S3
    Value: !Ref MyS3Bucket
  BucketArn:
    Description: ARN del bucket S3
    Value: !GetAtt MyS3Bucket.Arn
```

4. **Guarde el archivo** en su computadora.

> **Explicación:**
> - **Resources**: Define el bucket de **S3** con versionado.
> - **!Sub**: Sustituye el ID de la cuenta para que el nombre del bucket sea único.
> - **Outputs**: Devuelve el nombre del bucket y el ARN tras la creación.

#### Parte 2: Crear la Pila (Stack) de CloudFormation

1. Navegue al servicio **CloudFormation**.
2. Haga clic en **"Create stack"** → **"With new resources (standard)"**.
3. **Prepare template**: Template is ready.
4. **Template source**: Upload a template file.
5. Haga clic en **"Choose file"** y seleccione `simple-stack.yaml`.
6. Haga clic en **"Next"**.
7. **Stack name**: "MiPrimeraPila".
8. Haga clic en **"Next"**.
9. **Configure stack options**:
   - **Tags (opcional)**:
     - Key: Project.
     - Value: CloudFormation-Lab.
10. Haga clic en **"Next"**.
11. **Review**:
    - Verifique todos los ajustes.
    - Revise la plantilla en la vista JSON/YAML.
12. Haga clic en **"Submit"**.

#### Parte 3: Monitorear la Creación de la Pila

1. **Stack status**: CREATE_IN_PROGRESS.
2. Haga clic en la pestaña **"Events"**.
   - Observe los eventos de creación en tiempo real.
   - Vea cómo se crea cada recurso.
3. Haga clic en la pestaña **"Resources"**.
   - Vea el ID lógico y el ID físico.
   - Vea cómo se crea el bucket de **S3**.
4. Espere al **Status**: CREATE_COMPLETE (tarda 1-2 minutos).
5. Haga clic en la pestaña **"Outputs"**.
   - Vea el BucketName y el BucketArn.
   - Copie el nombre del bucket.

#### Parte 4: Verificar la Creación del Recurso

1. Abra una nueva pestaña → Navegue al servicio **S3**.
2. **Verificar que el bucket existe**:
   - Busque el bucket: `cf-bucket-[su-id-de-cuenta]`.
   - Haga clic en el bucket.
   - Verifique que el versionado esté habilitado (Properties → Bucket Versioning).
   - Revise las etiquetas (Properties → Tags).
3. **Nota:** Este bucket fue creado íntegramente por la plantilla de **CloudFormation**.

#### Parte 5: Actualizar la Pila

**Crear la plantilla actualizada:**

1. Abra `simple-stack.yaml`.
2. Añada la configuración de cifrado:

```yaml
AWSTemplateFormatVersion: '2010-09-09'
Description: Pila simple de bucket S3 para aprender CloudFormation

Resources:
  MyS3Bucket:
    Type: AWS::S3::Bucket
    Properties:
      BucketName: !Sub 'cf-bucket-${AWS::AccountId}'
      VersioningConfiguration:
        Status: Enabled
      BucketEncryption:
        ServerSideEncryptionConfiguration:
          - ServerSideEncryptionByDefault:
              SSEAlgorithm: AES256
      PublicAccessBlockConfiguration:
        BlockPublicAcls: true
        BlockPublicPolicy: true
        IgnorePublicAcls: true
        RestrictPublicBuckets: true
      Tags:
        - Key: Environment
          Value: Learning
        - Key: ManagedBy
          Value: CloudFormation

Outputs:
  BucketName:
    Description: Nombre del bucket S3
    Value: !Ref MyS3Bucket
  BucketArn:
    Description: ARN del bucket S3
    Value: !GetAtt MyS3Bucket.Arn
```

3. Guarde el archivo.

**Actualizar la pila:**

1. Vuelva a la consola de **CloudFormation**.
2. Seleccione **MiPrimeraPila**.
3. Haga clic en **"Update"**.
4. **Replace current template**: Upload a template file.
5. Elija el archivo `simple-stack.yaml` actualizado.
6. Haga clic en **"Next"**.
7. **Parameters**: (nada que cambiar).
8. Haga clic en **"Next"**.
9. **Review change set**:
   - **CloudFormation** muestra qué cambiará.
   - Añadido: Configuración de cifrado.
   - Añadido: Bloqueo de acceso público.
   - **Importante:** Se muestra ANTES de realizar los cambios.
10. Haga clic en **"Next"**.
11. Revise y haga clic en **"Submit"**.
12. **Observar la actualización**:
    - Status: UPDATE_IN_PROGRESS.
    - Los eventos muestran las modificaciones.
    - Status: UPDATE_COMPLETE.

**Verificar la actualización:**

1. Vaya a **S3** → Su bucket.
2. Properties → Default encryption → Verifique que esté habilitado.
3. Properties → Block public access → Verifique que todos estén habilitados.

#### Parte 6: Ver la Plantilla de la Pila

1. En **CloudFormation**, seleccione la pila.
2. Haga clic en la pestaña **"Template"**.
3. **Ver en el Diseñador**:
   - Haga clic en **"View in Application Composer"** o **"View in Designer"**.
   - Vea la representación visual de los recursos.
   - Muestra las relaciones entre los recursos.
4. **Descargar la plantilla**:
   - Ver en formato JSON o YAML.
   - Haga clic en "Copy to clipboard" si es necesario.

### Resultados Esperados

- Pila de **CloudFormation** creada con éxito.
- Bucket de **S3** desplegado con versionado habilitado.
- Pila actualizada para añadir cifrado.
- Plantilla visualizable en el diseñador.
- Infraestructura definida como código (repetible, control de versiones).

### Verificación

1. **La pila muestra el estado CREATE_COMPLETE**.
2. **El bucket de S3 existe** con la configuración correcta.
3. **Los Outputs muestran** el nombre del bucket y el ARN.
4. **Actualización exitosa** con el cifrado habilitado.
5. **Todos los recursos etiquetados** con información de **CloudFormation**.

### Resolución de Problemas

**Problema:** La creación de la pila falla con "Bucket already exists".
**Solución:**
- Los nombres de los buckets deben ser globalmente únicos.
- Cambie el nombre del bucket en la plantilla.
- O elimine el bucket existente primero.

**Problema:** Error de validación de la plantilla.
**Solución:**
- Verifique la sintaxis YAML (la indentación es crítica).
- Verifique que todas las claves estén escritas correctamente.
- Use un validador de YAML en línea.
- Consulte la documentación de **CloudFormation** para las propiedades de los recursos.

**Problema:** La actualización falla.
**Solución:**
- Revise el conjunto de cambios (**change set**) antes de confirmar.
- Algunas propiedades no se pueden actualizar (requieren reemplazo).
- Revise la pestaña **Events** para ver el error específico.
- Puede ser necesario eliminar y recrear la pila.

**Problema:** No se puede eliminar la pila.
**Solución:**
- Asegúrese de que el bucket de **S3** esté vacío primero.
- **CloudFormation** no puede eliminar buckets que no estén vacíos.
- Vacíe el bucket manualmente y reintente la eliminación.

### Limpieza

> **Importante:** **CloudFormation** facilita la limpieza: elimina todos los recursos automáticamente.

1. Vaya a la consola de **CloudFormation**.
2. Seleccione **MiPrimeraPila**.
3. Haga clic en **"Delete"**.
4. **Confirme la eliminación**.
5. **Monitorear la eliminación**:
   - Status: DELETE_IN_PROGRESS.
   - Los eventos muestran cómo se eliminan los recursos.
   - Bucket de **S3** eliminado (si estaba vacío).
   - Status: DELETE_COMPLETE (o la pila desaparece).
6. **Verificar en S3**:
   - Vaya a la consola de **S3**.
   - El bucket debería haber desaparecido.

> **Nota:** Si la eliminación falla, suele ser porque el bucket de **S3** no está vacío. Vacíe el bucket manualmente y reintente.

---

## Laboratorio 11: Auto Scaling y Load Balancing

**Duración:** 45 minutos
**Costo:** Gratis (dentro de los límites del **Free Tier**)
**Dificultad:** Avanzado

### Objetivos de Aprendizaje

Al final de este laboratorio, usted podrá:

1. Crear una Plantilla de Lanzamiento (**Launch Template**) para instancias **EC2**.
2. Configurar un **Application Load Balancer** (**ALB**).
3. Configurar un **Auto Scaling Group** con políticas de escalado.
4. Comprender el seguimiento de objetivos (**target tracking**) y el escalado por pasos (**step scaling**).
5. Probar los comportamientos de escalado horizontal automático (hacia afuera y hacia adentro).
6. Monitorear las actividades de **Auto Scaling** en **CloudWatch**.

### Por Qué es Importante Este Laboratorio

**Escenario del Mundo Real:** Un sitio web de comercio electrónico experimenta 10 veces más tráfico durante las ventas del Black Friday. El **Auto Scaling** añade servidores automáticamente durante las horas pico y los elimina cuando el tráfico disminuye, optimizando tanto el rendimiento como el costo.

**Relevancia para el Examen:** El **Auto Scaling** y **ELB** son temas muy evaluados. Debe conocer:
- Tipos de equilibradores de carga (**ALB**, **NLB**, **CLB**).
- Componentes de **Auto Scaling** (plantillas de lanzamiento, grupos, políticas).
- Políticas de escalado (seguimiento de objetivos, pasos, programado).
- Comprobaciones de estado (**health checks**) y alta disponibilidad.

### Requisitos Previos

- Haber completado el Laboratorio 3 (**EC2**) y el Laboratorio 5 (**VPC**).
- Comprensión de los conceptos de equilibrio de carga.
- Conocimientos básicos de servidores web.
12. Haga clic en **"Create launch template"**.
13. Verá el mensaje de éxito: "Successfully created Plantilla-ServidorWeb".
14. Haga clic en **"View launch template"** para verificar.

**Validación:**
- La plantilla aparece en la lista con la versión 1.
- Toda la configuración es visible en los detalles de la plantilla.

#### Parte 2: Crear el Application Load Balancer

1. En la consola de **EC2**, desplácese hacia abajo en el menú izquierdo hasta **"Load Balancers"**.
2. Haga clic en **"Create load balancer"**.
3. **Load balancer types**: Seleccione **"Application Load Balancer"**.
4. Haga clic en **"Create"**.

5. **Basic configuration**:
   - **Name**: "WebApp-ALB".
   - **Scheme**: Internet-facing.
   - **IP address type**: IPv4.

6. **Network mapping**:
   - **VPC**: Default VPC.
   - **Mappings**: Seleccione al menos 2 **Availability Zones**.
     - Marque las casillas para **us-east-1a**, **us-east-1b** (o las **AZs** de su región).
     - Seleccione subredes públicas para cada una.

7. **Security groups**:
   - Haga clic en **"Create new security group"** (se abre una nueva pestaña).
   - **Name**: "ALB-SG".
   - **Description**: "Permitir HTTP desde internet".
   - **VPC**: Default.
   - **Inbound rules**:
     - Type: HTTP, Port: 80, Source: 0.0.0.0/0.
   - **Outbound rules**: Deje el valor predeterminado (todo el tráfico).
   - Haga clic en **"Create security group"**.
   - Vuelva a la pestaña del **ALB**, actualice la lista de grupos de seguridad.
   - Seleccione **"ALB-SG"**.
   - **Elimine el grupo de seguridad predeterminado**.

8. **Listeners and routing**:
   - Protocolo: HTTP, Puerto: 80 (predeterminado).
   - **Default action**: Create target group.
     - Haga clic en **"Create target group"** (se abre una nueva pestaña).

#### Parte 3: Crear el Target Group

1. **Target type**: Instances.
2. **Target group name**: "WebApp-TG".
3. **Protocol**: HTTP, Port: 80.
4. **VPC**: Default VPC.

5. **Health checks**:
   - **Protocol**: HTTP.
   - **Path**: / (ruta raíz).
   - **Advanced health check settings**:
     - Healthy threshold: 2.
     - Unhealthy threshold: 2.
     - Timeout: 5 segundos.
     - Interval: 30 segundos.
     - Success codes: 200.
   - **Por qué estos ajustes**: Comprobaciones de estado rápidas (intervalo de 30s) con una conmutación por error veloz (2 comprobaciones fallidas = **unhealthy**).

6. Haga clic en **"Next"**.
7. **Register targets**: Omita este paso (el **Auto Scaling** registrará las instancias automáticamente).
8. Haga clic en **"Create target group"**.
9. Vuelva a la pestaña del **ALB**, actualice los grupos de destino.
10. Seleccione **"WebApp-TG"** del menú desplegable.

11. **Tags (opcional)**:
    - Key: Project, Value: Laboratorio-AutoScaling.

12. **Revise** todos los ajustes.
13. Haga clic en **"Create load balancer"**.
14. Espere 2-3 minutos hasta que el **State** sea **Active**.

**Validación:**
- El **ALB** muestra el estado "Active".
- Copie el nombre DNS (ej: WebApp-ALB-1234567890.us-east-1.elb.amazonaws.com).
- Intente acceder en el navegador (mostrará un error 503; no hay destinos todavía, esto es esperado).

#### Parte 4: Crear el Auto Scaling Group

1. En el menú izquierdo de **EC2**, haga clic en **"Auto Scaling Groups"**.
2. Haga clic en **"Create Auto Scaling group"**.

3. **Paso 1: Elegir la plantilla de lanzamiento**
   - **Name**: "WebApp-ASG".
   - **Launch template**: Seleccione "Plantilla-ServidorWeb".
   - **Version**: Latest (1).
   - Haga clic en **"Next"**.

4. **Paso 2: Elegir opciones de lanzamiento de instancias**
   - **VPC**: Default VPC.
   - **Availability Zones and subnets**: Seleccione 2 o más **AZs**.
     - Elija subredes públicas en cada **AZ**.
   - Haga clic en **"Next"**.

5. **Paso 3: Configurar opciones avanzadas**
   - **Load balancing**: Attach to an existing load balancer.
   - **Choose from your load balancer target groups**.
   - Seleccione **"WebApp-TG"**.
   - **Health checks**:
     - Marque **"Turn on Elastic Load Balancing health checks"**.
     - Health check grace period: 300 segundos.
     - **Por qué:** Da tiempo a las instancias para iniciarse completamente antes de las comprobaciones de estado.
   - **Monitoring**:
     - Marque **"Enable group metrics collection within CloudWatch"**.
   - Haga clic en **"Next"**.

6. **Paso 4: Configurar el tamaño del grupo y el escalado**
   - **Group size**:
     - Desired capacity: 2.
     - Minimum capacity: 1.
     - Maximum capacity: 4.
   - **Scaling policies**:
     - Seleccione **"Target tracking scaling policy"**.
     - **Scaling policy name**: "Seguimiento-Objetivo-CPU".
     - **Metric type**: Average CPU utilization.
     - **Target value**: 50.
     - **Instance warmup**: 300 segundos.

#### Parte 5: Probar el Equilibrio de Carga (Load Balancing)

1. **Copie el nombre DNS del ALB** desde la página de **Load Balancers**.
2. **Abra en el navegador**: `http://WebApp-ALB-1234567890.us-east-1.elb.amazonaws.com`.
3. Debería ver: "¡Auto Scaling está Funcionando!" con un ID de instancia.
4. **Refresque la página varias veces** (F5).
5. Note que el ID de la instancia cambia entre refrescos.
   - **Qué está pasando:** El **ALB** distribuye las solicitudes entre las instancias (**round-robin** por defecto).

6. **Probar desde la línea de comandos (opcional)**:
```bash
for i in {1..10}; do curl http://su-nombre-dns-de-alb.elb.amazonaws.com | grep "ID de la Instancia"; done
```
   - Muestra la distribución entre las instancias.

**Comportamiento Esperado:**
- Las solicitudes alternan entre 2 IDs de instancia diferentes.
- Ambas instancias sirven tráfico.
- El tiempo de respuesta es rápido (<100ms).

#### Parte 6: Probar el Auto Scaling (Escalado Horizontal - Scale Out)

> **Advertencia:** Esto generará carga de CPU. Monitoree de cerca y detenga si es necesario.

1. **Conectarse vía SSH a una instancia**:
   - Vaya a **EC2** → **Instances**.
   - Busque instancias etiquetadas como "ServidorWeb-AutoScaled".
   - Conéctese vía **SSH**:
   ```bash
   ssh -i su-clave.pem ec2-user@[ip-publica]
   ```

2. **Instalar la herramienta stress**:
```bash
sudo yum install -y stress
```

3. **Generar carga de CPU**:
```bash
stress --cpu 2 --timeout 600
```
   - Se ejecuta durante 10 minutos (600 segundos).
   - Lleva la CPU a ~100%.

4. **Monitorear el Auto Scaling**:
   - Vaya a **Auto Scaling Groups** → **WebApp-ASG**.
   - Haga clic en la pestaña **"Activity"**.
   - Observe nuevas actividades de escalado.
   - **Tiempo de escalado:** Típicamente 5-10 minutos.
     - 5 minutos de CPU alta (evaluación de **CloudWatch**).
     - 2-3 minutos para lanzar la nueva instancia.
     - 5 minutos de período de calentamiento (**warmup**).

5. **Ver métricas de CloudWatch**:
   - Vaya a **CloudWatch** → **Metrics** → **EC2** → **By Auto Scaling Group**.
   - Seleccione **CPUUtilization** para **WebApp-ASG**.
   - **Ajustes del gráfico:** período de 1 minuto.
   - Verá el pico de CPU por encima del objetivo del 50%.

6. **Verificar el escalado horizontal**:
   - La capacidad del **Auto Scaling Group** aumenta: 2 → 3 o 3 → 4.
   - El historial de actividad muestra: "Launching a new EC2 instance".
   - Aparece una nueva instancia en la lista de **Instances**.
   - El **Target Group** muestra 3-4 destinos **healthy**.

**Lo que Debería Ver:**
- La métrica de CPU cruza el umbral del 50%.
- Tras ~5 minutos, se activa la actividad de escalado.
- Se lanza una nueva instancia automáticamente.
- La capacidad total aumenta.
- La carga se distribuye entre más instancias.

#### Parte 7: Probar el Auto Scaling (Escalado hacia adentro - Scale In)

1. **Detener la prueba de estrés**: Presione Ctrl+C en la sesión **SSH** (o espere al tiempo de espera).
2. **Salir de SSH**: Escriba `exit`.

3. **Monitorear la disminución de CPU**:
   - **CloudWatch** muestra la caída de la CPU por debajo del 50%.
   - Espere 15-20 minutos para el escalado hacia adentro.
   - **Por qué tarda más:** **AWS** espera de forma conservadora antes de eliminar capacidad.

4. **Ver la Actividad de Auto Scaling**:
   - La pestaña **Activity** muestra: "Terminating EC2 instance".
   - La capacidad disminuye de nuevo a 2 (capacidad deseada).
   - La instancia extra se termina automáticamente.

**Cronología Esperada:**
- Caída de CPU: Inmediata.
- Evaluación de escalado hacia adentro: 15 minutos (**cooldown** predeterminado).
- Terminación de la instancia: 2-3 minutos.
- Tiempo total para escalar hacia adentro: ~20 minutos.

### Resultados Esperados

- Plantilla de lanzamiento creada con script de **user data**.
- **Application Load Balancer** distribuyendo tráfico entre **AZs**.
- **Target group** con comprobaciones de estado configuradas.
- **Auto Scaling Group** manteniendo 2 instancias normalmente.
- Escalado horizontal automático cuando la CPU supera el 50%.
- Escalado hacia adentro automático cuando la CPU vuelve a la normalidad.
- Todas las instancias sirviendo tráfico a través del **ALB**.

### Lista de Verificación

- [ ] La plantilla de lanzamiento aparece en la lista de plantillas de **EC2**.
- [ ] El estado del **ALB** es "Active".
- [ ] El **Target group** muestra todas las instancias como "healthy".
- [ ] Al acceder a la URL del **ALB** se muestra la página web.
- [ ] Al refrescar se muestran diferentes IDs de instancia (equilibrio de carga).
- [ ] El **Auto Scaling Group** mantiene la capacidad deseada.
- [ ] El escalado horizontal ocurrió durante la prueba de estrés.
- [ ] El escalado hacia adentro ocurrió tras normalizarse la CPU.

### Consejos del Mundo Real

**Mejores Prácticas para Plantillas de Lanzamiento:**
- Use versiones de las plantillas para tener capacidad de reversión (**rollback**).
- Use la última **AMI** de **Amazon Linux** para parches de seguridad.
- Incluya agentes de monitoreo en el **user data**.
- Pruebe los scripts de **user data** antes de usarlos en plantillas.

**Configuración del Load Balancer:**
- Use siempre al menos 2 **AZs** para alta disponibilidad.
- Configure rutas de comprobación de estado adecuadas (no solo `/`).
- Establezca valores de tiempo de espera razonables (5-10 segundos).
- Use HTTPS en producción (requiere certificado SSL).
- Habilite los registros de acceso para resolución de problemas.

**Ajuste de Auto Scaling:**
- **Escalado conservador:** Umbrales más bajos (40% CPU) con períodos de enfriamiento más largos.
- **Escalado agresivo:** Umbrales más altos (70% CPU) con períodos de enfriamiento más cortos.
- **Recomendación para producción:** Comience de forma conservadora y ajuste según las métricas.
- **Optimización de costos:** Use escalado programado para patrones predecibles.

**Métricas Comunes de Escalado:**
- Utilización de CPU: La más común, buena para aplicaciones limitadas por cómputo.
- Recuento de solicitudes por destino: Buena para aplicaciones web con solicitudes uniformes.
- Rendimiento de red: Para aplicaciones intensivas en red.
- Métricas personalizadas de **CloudWatch**: Específicas de la aplicación (longitud de cola, etc.).

### Resolución de Problemas

**Problema:** Las instancias se lanzan pero permanecen "unhealthy" en el **target group**.

**Solución:**
- Verifique que el grupo de seguridad permita HTTP (puerto 80) desde el **ALB**.
- Verifique que la ruta de comprobación de estado sea correcta (`/`).
- Asegúrese de que el servidor web se haya iniciado (revise los logs de **user data**: `/var/log/cloud-init-output.log`).
- Aumente el período de gracia de la comprobación de estado a 400-500 segundos.
- Conéctese vía **SSH** a la instancia y pruebe: `curl localhost`.

**Problema:** El **Auto Scaling** no escala horizontalmente a pesar de la CPU alta.

**Solución:**
- Verifique que la métrica de CPU se esté publicando en **CloudWatch** (**EC2** → **Instances** → **Monitoring**).
- Compruebe si el **Auto Scaling Group** ya está en la capacidad máxima (4 instancias).
- Espere el período de evaluación completo (5 minutos de CPU alta).
- Revise la configuración de la política de escalado y los unbrales.
- Revise las alarmas de **CloudWatch** para la política de escalado.

**Problema:** No se puede acceder a la URL del load balancer (tiempo de espera agotado).

**Solución:**
- Verifique que el grupo de seguridad del **ALB** permita HTTP desde 0.0.0.0/0.
- Asegúrese de que el **ALB** esté en subredes públicas con una puerta de enlace de internet (**internet gateway**).
- Verifique que el estado del **ALB** sea "Active" y no "Provisioning".
- Verifique que al menos un destino sea **healthy**.
- Compruebe que las tablas de enrutamiento tengan una ruta a la puerta de enlace de internet.

**Problema:** El escalado hacia adentro nunca ocurre.

**Solución:**
- La protección de escalado hacia adentro predeterminada podría estar habilitada (revise los ajustes del **ASG**).
- Espere más tiempo (el escalado hacia adentro tarda 15-20 minutos).
- Verifique que la CPU realmente haya bajado del umbral.
- Revise los ajustes de protección de instancia en instancias individuales.
- Revise las políticas de escalado hacia adentro (pueden tener umbrales diferentes).

**Problema:** La página web no muestra el ID de la instancia.

**Solución:**
- El script de **user data** podría haber fallado.
- SSH a la instancia: `sudo cat /var/log/cloud-init-output.log`.
- Busque errores en el script.
- Verifique que **httpd** esté en ejecución: `sudo systemctl status httpd`.
- Pruebe el archivo HTML: `cat /var/www/html/index.html`.

### Limpieza

> **Crítico:** Los **Load Balancers** y las instancias **EC2** en ejecución incurren en cargos. Limpie inmediatamente después del laboratorio.

**Orden de Limpieza (Importante - siga la secuencia):**

1. **Eliminar el Auto Scaling Group**:
   - Vaya a **Auto Scaling Groups**.
   - Seleccione "WebApp-ASG".
   - **Actions** → **Delete**.
   - Escriba "delete" para confirmar.
   - **Esto termina todas las instancias del grupo**.
   - Espere a que las instancias terminen (2-3 minutos).

2. **Verificar que las instancias terminaron**:
   - Vaya a **EC2** → **Instances**.
   - Asegúrese de que todas las instancias "ServidorWeb-AutoScaled" muestren "Terminated".

3. **Eliminar el Load Balancer**:
   - Vaya a **Load Balancers**.
   - Seleccione "WebApp-ALB".
   - **Actions** → **Delete load balancer**.
   - Escriba "confirm" para eliminar.
   - Espere a la eliminación (1-2 minutos).

4. **Eliminar el Target Group**:
   - Vaya a **Target Groups**.
   - Seleccione "WebApp-TG".
   - **Actions** → **Delete**.
   - Confirme la eliminación.

5. **Eliminar la Plantilla de Lanzamiento**:
   - Vaya a **Launch Templates**.
   - Seleccione "Plantilla-ServidorWeb".
   - **Actions** → **Delete template**.
   - Confirme la eliminación.

6. **Eliminar los Grupos de Seguridad**:
   - Vaya a **Security Groups**.
   - Seleccione "ALB-SG" → **Actions** → **Delete security groups**.
   - Seleccione "ALB-WebServer-SG" → **Actions** → **Delete security groups**.
   - **Note:** Puede ser necesario esperar si existen dependencias.

7. **Verificar la limpieza**:
   - No hay instancias **EC2** en ejecución o pendientes de este laboratorio.
   - No hay equilibradores de carga en la lista.
   - No hay grupos de **Auto Scaling** en la lista.
   - El **Target group** ha sido eliminado.

**Advertencia de Costo:**
- **Load Balancers**: $0.0225/hora (~$16/mes) - NO es elegible para el **Free Tier**.
- Instancias **EC2**: Gratis si está dentro de las 750 horas/mes en **t2.micro**.
- **Dejar el ALB funcionando toda la noche = ~$0.54 desperdiciados**.

**Comandos de Verificación (opcional):**
```bash
aws elbv2 describe-load-balancers --region us-east-1
aws autoscaling describe-auto-scaling-groups --region us-east-1
aws ec2 describe-instances --filters "Name=instance-state-name,Values=running" --region us-east-1
```

### Verificación de Conocimientos Post-Laboratorio

**Pregunta 1:** ¿Cuál es la diferencia entre capacidad deseada, mínima y máxima?

<details>
<summary>Haga clic para ver la respuesta</summary>

**Respuesta:**
- **Capacidad deseada (Desired capacity):** El número objetivo actual de instancias que el **Auto Scaling** mantiene.
- **Capacidad mínima (Minimum capacity):** El número más bajo de instancias (nunca baja de aquí).
- **Capacidad máxima (Maximum capacity):** El número más alto de instancias (nunca supera este límite).

Ejemplo: Min=1, Desired=2, Max=4
- Operación normal: 2 instancias en ejecución.
- Durante escalado horizontal: Puede añadir hasta 2 más (total 4).
- Durante escalado hacia adentro: Puede eliminar 1 (el mínimo es 1).
- La capacidad deseada cambia dinámicamente; min/max son los límites.

</details>

**Pregunta 2:** ¿Por qué usar el seguimiento de objetivos en lugar del escalado por pasos?

<details>
<summary>Haga clic para ver la respuesta</summary>

**Respuesta:**
- **Seguimiento de objetivos (Target tracking):** Más sencillo de configurar, calcula automáticamente los ajustes de escalado para mantener el objetivo (como un termostato). Es lo mejor para la mayoría de los casos de uso.
- **Escalado por pasos (Step scaling):** Más control, define cantidades de escalado específicas para diferentes rangos de umbrales. Se usa para patrones de escalado complejos.
- **Consejo para el examen:** El seguimiento de objetivos es lo recomendado por **AWS** para la mayoría de los escenarios y es la opción predeterminada.

</details>

**Pregunta 3:** ¿Qué sucede si una instancia falla las comprobaciones de estado?

<details>
<summary>Haga clic para ver la respuesta</summary>

**Respuesta:**
1. El **target group** marca la instancia como "unhealthy" tras 2 comprobaciones fallidas (configurable).
2. El equilibrador de carga deja de enviar tráfico a esa instancia.
3. El **Auto Scaling** detecta la instancia **unhealthy**.
4. Tras el período de gracia, el **Auto Scaling** termina la instancia **unhealthy**.
5. El **Auto Scaling** lanza una instancia de reemplazo para mantener la capacidad deseada.
6. La nueva instancia pasa por las comprobaciones de estado antes de recibir tráfico.

¡Esto proporciona una infraestructura que se repara sola!

</details>

**Pregunta 4:** ¿Puede el Auto Scaling funcionar sin un equilibrador de carga?

<details>
<summary>Haga clic para ver la respuesta</summary>

**Respuesta:** ¡Sí! El **Auto Scaling** funciona independientemente de los equilibradores de carga.

**Sin ELB:**
- Las instancias se siguen lanzando/terminando según las políticas.
- Casos de uso: Procesamiento por lotes, nodos de trabajo (**workers**), tareas en segundo plano.
- Comprobaciones de estado basadas solo en el estado de **EC2**.

**Con ELB:**
- Mejor para aplicaciones web que sirven tráfico de usuarios.
- Comprobaciones de estado tanto de **ELB** como de **EC2**.
- Tráfico distribuido automáticamente.
- Arquitectura más resiliente.

**Consejo para el examen:** Sepa que el **Auto Scaling** y el **ELB** son servicios separados que funcionan bien juntos pero no son obligatorios el uno para el otro.

</details>

**Pregunta 5:** ¿Cuál es el propósito del período de calentamiento (warmup)?

<details>
<summary>Haga clic para ver la respuesta</summary>

**Respuesta:** El período de calentamiento (300 segundos recomendados) evita que las nuevas instancias sean evaluadas para el escalado antes de estar listas.

**Sin warmup:**
- Se lanza una nueva instancia (la CPU está baja mientras se inicia).
- El promedio de CPU del grupo cae.
- Podría activar un escalado hacia adentro prematuro.
- Inestabilidad y fluctuaciones innecesarias.

**Con warmup:**
- Se lanza la nueva instancia.
- Las métricas se ignoran durante 300 segundos.
- La instancia tiene tiempo de inicializarse y recibir tráfico.
- Comportamiento de escalado estable.

</details>

### Conclusiones Clave

- **Auto Scaling proporciona elasticidad**: la capacidad coincide con la demanda automáticamente.
- **ELB distribuye el tráfico**: sin puntos únicos de fallo.
- **El seguimiento de objetivos es lo más simple**: establezca el objetivo, **AWS** se encarga del resto.
- **Las comprobaciones de estado son críticas**: determinan qué instancias reciben tráfico.
- **Despliegue Multi-AZ**: proporciona alta disponibilidad.
- **Plantillas de lanzamiento**: defina la configuración de la instancia una vez, reutilícela muchas veces.
- **Los períodos de enfriamiento (cooldown) evitan fluctuaciones**: evitan ciclos rápidos de escalado.
- **Optimización de costos**: pague solo por lo que necesita, cuando lo necesita.

---

## Laboratorio 12: Práctica con DynamoDB

**Duración:** 35 minutos
**Costo:** Gratis (25 GB de almacenamiento siempre gratis)
**Dificultad:** Intermedio

### Objetivos de Aprendizaje

Al final de este laboratorio, usted podrá:

1. Crear una tabla de **DynamoDB** con claves de partición y de ordenación.
2. Comprender los tipos de datos y atributos de **DynamoDB**.
3. Añadir, consultar y escanear elementos utilizando la consola de **AWS**.
4. Crear y usar Índices Secundarios Globales (**GSI**).
5. Configurar el auto-scaling de **DynamoDB**.
6. Comprender los modos de capacidad de lectura/escritura.
7. Exportar datos de la tabla y habilitar la recuperación en un punto en el tiempo (**PITR**).

### Por Qué es Importante Este Laboratorio

**Escenario del Mundo Real:** Una empresa de juegos móviles utiliza **DynamoDB** para almacenar perfiles de jugadores, puntuaciones de juegos y tablas de clasificación en tiempo real. **DynamoDB** maneja millones de solicitudes por día con latencia de milisegundos de un solo dígito, escalando automáticamente sin gestión de servidores.

**Relevancia para el Examen:** **DynamoDB** es un servicio clave de **AWS**. Debe conocer:
- NoSQL frente a bases de datos relacionales.
- Claves primarias (clave de partición, clave de ordenación).
- Índices (**LSI**, **GSI**).
- Modos de capacidad (bajo demanda frente a provisionada).
- **DynamoDB Accelerator** (**DAX**) para almacenamiento en caché.

### Requisitos Previos

- Comprensión básica de las bases de datos (SQL o NoSQL).
- Conocimiento de estructuras de datos (pares clave-valor).
- Haber completado el Laboratorio 1 (se recomiendan alertas de facturación).

### Instrucciones Paso a Paso

#### Parte 1: Crear la Tabla de DynamoDB

1. Navegue al servicio **DynamoDB**.
2. Haga clic en **"Create table"**.

3. **Table details**:
   - **Table name**: "PuntuacionesJuego".
   - **Partition key**: "IDUsuario" (String).
   - **Sort key**: "TituloJuego" (String).
   - **Por qué estas claves:**
     - La clave de partición distribuye los datos entre las particiones.
     - La clave de ordenación organiza los elementos dentro de cada partición.
     - Juntas forman una clave compuesta única.
     - Ejemplo: Usuario123 + Minecraft, Usuario123 + Fortnite.

4. **Table settings**:
   - Seleccione **"Customize settings"** (no la configuración predeterminada).

5. **Table class**:
   - Seleccione **"DynamoDB Standard"**.
   - (**Standard-IA** es para datos de acceso poco frecuente).

6. **Read/write capacity settings**:
   - **Capacity mode**: On-demand.
   - **Por qué bajo demanda:** Escala automáticamente, sin planificación de capacidad, paga por solicitud.
   - **Alternativa:** Modo provisionado (Free Tier: 25 WCU + 25 RCU).
   - **Lo mejor para el laboratorio:** Bajo demanda (más simple, menos probabilidad de limitación o **throttling**).

7. **Secondary indexes**:
   - Omita por ahora (se añadirán más tarde en el laboratorio).

8. **Encryption at rest**:
   - **Encryption type**: Owned by Amazon DynamoDB.
   - (Predeterminado, sin costo adicional).

9. **Tags (opcional)**:
   - Key: Project, Value: Laboratorio-DynamoDB.

10. Haga clic en **"Create table"**.
11. Espere 10-20 segundos hasta que el estado sea: "Active".

**Validación:**
- La tabla aparece en la lista de tablas.
- El estado muestra "Active".
- Los detalles de la tabla muestran las claves de partición y de ordenación.

#### Parte 2: Añadir Elementos a la Tabla

1. Haga clic en el nombre de la tabla **"PuntuacionesJuego"**.
2. Haga clic en el botón **"Explore table items"**.
3. Haga clic en **"Create item"**.

**Elemento 1:**
4. **Añadir atributos**:
   - IDUsuario (String): "Usuario001".
   - TituloJuego (String): "Minecraft".
5. Haga clic en **"Add new attribute"** → **Number**.
   - Attribute name: "Puntuacion".
   - Value: 1250.
6. Haga clic en **"Add new attribute"** → **Number**.
   - Attribute name: "Nivel".
   - Value: 15.
7. Haga clic en **"Add new attribute"** → **String**.
   - Attribute name: "NombreJugador".
   - Value: "Steve".
8. Haga clic en **"Add new attribute"** → **Number**.
   - Attribute name: "Timestamp".
   - Value: 1699564800 (Unix timestamp).
9. Haga clic en **"Create item"**.

**Elemento 2:**
10. Haga clic en **"Create item"** de nuevo.
11. Añada los atributos:
    - IDUsuario: "Usuario001".
    - TituloJuego: "Fortnite".
    - Puntuacion: 2400.
    - Nivel: 22.
    - NombreJugador: "Steve".
    - Timestamp: 1699651200.

**Elemento 3:**
12. Cree el tercer elemento:
    - IDUsuario: "Usuario002".
    - TituloJuego: "Minecraft".
    - Puntuacion: 980.
    - Nivel: 12.
    - NombreJugador: "Alex".
    - Timestamp: 1699737600.

**Elemento 4:**
13. Cree el cuarto elemento:
    - IDUsuario: "Usuario002".
    - TituloJuego: "Fortnite".
    - Puntuacion: 3100.
    - Nivel: 28.
    - NombreJugador: "Alex".
    - Timestamp: 1699824000.

**Elemento 5:**
14. Cree el quinto elemento:
    - IDUsuario: "Usuario003".
    - TituloJuego: "Minecraft".
    - Puntuacion: 1800.
    - Nivel: 18.
    - NombreJugador: "Herobrine".
    - Timestamp: 1699910400.

**Lo que Debería Ver:**
- Los elementos aparecen en la tabla inmediatamente.
- Cada elemento tiene IDUsuario + TituloJuego (claves) además de atributos adicionales.
- Los elementos pueden tener diferentes atributos (sin esquema o **schema-less**).
- El escaneo (**Scan**) muestra todos los elementos.

#### Parte 3: Consultar Elementos (Query)

> **Query vs. Scan:** La consulta (**Query**) es eficiente (usa claves), el escaneo (**Scan**) lee toda la tabla (lento, costoso).

1. Haga clic en **"Query"** (vista predeterminada tras añadir elementos).
2. **Query items where**:
   - Partition key: IDUsuario.
   - **Ingrese:** "Usuario001".
3. Haga clic en **"Run"**.

**Resultados:**
- Muestra 2 elementos: Minecraft y Fortnite para Usuario001.
- Ordenados por TituloJuego (clave de ordenación).
- Consulta rápida utilizando la clave primaria.

4. **Añadir condición de clave de ordenación**:
   - Partition key: "Usuario001".
   - **Sort key condition**: TituloJuego = "Minecraft".
5. Haga clic en **"Run"**.

**Resultados:**
- Muestra solo 1 elemento: la puntuación de Minecraft de Usuario001.
- Consulta aún más específica.

6. **Intentar otra consulta**:
   - Partition key: "Usuario002".
   - Deje la clave de ordenación vacía.
7. Haga clic en **"Run"**.

**Resultados:**
- Muestra ambos juegos para Usuario002.

**Uso en el Mundo Real:** Consultar la puntuación de un juego específico de un jugador o todos los juegos de un jugador.

#### Parte 4: Escanear Elementos (Scan)

1. Haga clic en la pestaña **"Scan"** (junto a Query).
2. Haga clic en **"Run"**.

**Resultados:**
- Muestra los 5 elementos de la tabla.
- No se aplica ningún filtro.
- **Advertencia:** Los escaneos son costosos para tablas grandes (leen todos los datos).

3. **Añadir un filtro de escaneo**:
   - Haga clic en **"Filters"**.
   - **Add filter**:
     - Attribute name: Puntuacion.
     - Condition: Greater than or equal to.
     - Value: 2000.
   - Haga clic en **"Run"**.

**Resultados:**
- Muestra solo 2 elementos: Usuario001 Fortnite (2400), Usuario002 Fortnite (3100).
- Filtrados tras escanear toda la tabla.
- **Nota:** Sigue escaneando todos los elementos y luego filtra (no es tan eficiente como una consulta).

**Mejor Práctica:** Use consultas con claves siempre que sea posible; reserve los escaneos para análisis.

#### Parte 5: Crear un Índice Secundario Global (GSI)

**Problema:** ¿Qué pasa si queremos encontrar a todos los jugadores con Puntuacion > 2000 de forma eficiente?
**Solución:** Crear un **GSI** con Puntuacion como clave de partición.

1. Vaya a la pestaña **"Indexes"**.
2. Haga clic en **"Create index"**.

3. **Index details**:
   - **Partition key**: Puntuacion (Number).
   - **Sort key**: Timestamp (Number) (opcional, pero útil para ordenar).
   - **Index name**: "IndicePuntuacion".
   - **Attribute projections**: All.
     - Proyecta todos los atributos de la tabla en el índice.
     - Alternativa: Solo claves (más pequeño, más barato) o Include (especificar atributos).

4. Haga clic en **"Create index"**.
5. Espere 20-30 segundos hasta que el estado sea: "Active".

**Validación:**
- El índice aparece en la pestaña **Indexes**.
- Status: Active.
- Ahora puede consultar por Puntuacion de forma eficiente.

#### Parte 6: Consultar Usando el GSI

1. Vuelva a **"Explore table items"**.
2. Haga clic en la pestaña **"Query"**.
3. **Index**: Seleccione "IndicePuntuacion" del menú desplegable.
4. **Query items**:
   - Partition key (Puntuacion): 1250.
5. Haga clic en **"Run"**.

**Resultados:**
- Muestra la puntuación de Minecraft de Usuario001.
- Consultado por Puntuacion en lugar de IDUsuario.

6. **Intentar una consulta de rango en el GSI**:
   - Lamentablemente, la consulta (**Query**) de **DynamoDB** requiere el valor exacto de la clave de partición.
   - Para consultas de rango en Puntuacion, se debe usar **Scan** con filtro (una limitación de **DynamoDB**).
   - **Enfoque alternativo:** Crear elementos con rangos de puntuación como claves de partición (avanzado).

**Uso en el Mundo Real:**
- **GSI** para consultar por atributos que no son clave.
- Patrón común: IDUsuario como clave de partición, **GSI** en Email para búsquedas de inicio de sesión.
- Hasta 20 **GSIs** por tabla.

#### Parte 7: Actualizar Elemento

1. En la vista "Explore table items", seleccione un elemento (haga clic en el botón de opción).
2. Haga clic en **"Actions"** → **"Edit item"**.
3. Cambie Puntuacion de 1250 a 1300.
4. Haga clic en **"Add new attribute"** → **String**.
   - Attribute name: "Logro".
   - Value: "Maestro Constructor".
5. Haga clic en **"Save changes"**.

**Lo que Debería Ver:**
- El elemento se actualiza inmediatamente.
- Se añade el nuevo atributo.
- Los otros elementos no se ven afectados (flexibilidad **schema-less**).

#### Parte 8: Eliminar Elemento

1. Seleccione un elemento (casilla de verificación).
2. Haga clic en **"Actions"** → **"Delete items"**.
3. Confirme la eliminación.
4. El elemento se elimina inmediatamente.

**Restaurar elemento (práctica de añadir):**
- Haga clic en "Create item" y vuelva a añadir el elemento eliminado.

#### Parte 9: Configurar los Ajustes de la Tabla

1. Vaya a la pestaña **"Additional settings"**.

**Recuperación en un punto en el tiempo (PITR):**
2. Desplácese hasta la sección **"Point-in-time recovery"**.
3. Haga clic en **"Edit"**.
4. Seleccione **"Turn on"**.
5. Haga clic en **"Save changes"**.
   - **Qué hace:** Copias de seguridad continuas, permite restaurar a cualquier punto en los últimos 35 días.
   - **Costo:** Cargo adicional basado en el tamaño de la tabla.
   - **Consejo para el examen:** **PITR** protege contra eliminaciones o actualizaciones accidentales.

**Time to Live (TTL):**
6. Scroll to **"Time to Live (TTL)"** section
7. Click **"Edit"**
8. **Turn on** TTL
9. **TTL attribute:** "ExpiresAt"
10. Click **"Save changes"**
    - **What it does:** Automatically deletes items after expiration time
    - **Use case:** Session data, temporary records
    - **Cost:** Free (deletion doesn't consume write capacity)

**Note:** We didn't add ExpiresAt to our items, so TTL won't affect them

#### Parte 10: Exportar Datos de la Tabla

1. Vaya a la pestaña **"Exports and streams"**.
2. Haga clic en **"Export to S3"**.
3. **Destination S3 bucket**:
   - Haga clic en **"Browse S3"**.
   - Seleccione un bucket existente o cree uno nuevo: "dynamodb-exports-[sunombre]".
4. **Export format**: DynamoDB JSON.
5. Haga clic en **"Export"**.
6. Estado de la exportación: "In progress" → "Completed" (2-3 minutos).

7. **Ver los datos exportados**:
   - Vaya a **S3** → Su bucket de exportación.
   - Navegue por las carpetas hasta encontrar el archivo de datos.
   - Descargue y vea la exportación JSON.

**Casos de uso:**
- Análisis de datos con **Athena**.
- Respaldo y archivo.
- Migración de datos.
- Requisitos de cumplimiento (**compliance**).

### Resultados Esperados

- Tabla de **DynamoDB** creada con una clave primaria compuesta.
- Múltiples elementos añadidos con varios atributos.
- Consulta exitosa de elementos utilizando la clave de partición.
- Escaneo de la tabla con filtros aplicados.
- Creación de un Índice Secundario Global (**GSI**) para consultas alternativas.
- Elementos actualizados y eliminados.
- Recuperación en un punto en el tiempo (**PITR**) y **TTL** configurados.
- Datos de la tabla exportados a **S3**.

### Lista de Verificación

- [ ] La tabla "PuntuacionesJuego" muestra el estado "Active".
- [ ] Hay al menos 5 elementos en la tabla.
- [ ] La consulta por IDUsuario devuelve los elementos correctos.
- [ ] El escaneo con filtro muestra los resultados esperados.
- [ ] El Índice Secundario Global "IndicePuntuacion" está activo.
- [ ] Se puede realizar una consulta utilizando el **GSI**.
- [ ] La recuperación en un punto en el tiempo está habilitada.
- [ ] Exportación a **S3** realizada con éxito.

### Consejos del Mundo Real

**Diseño de la Clave de Partición:**
- Alta cardinalidad (muchos valores únicos) para una distribución uniforme.
- Evite las "particiones calientes" (**hot partitions**) donde una sola clave recibe la mayor parte del tráfico.
- Mal ejemplo: La fecha como clave de partición (todos los datos de hoy en una sola partición).
- Buen ejemplo: IDCliente (distribuido entre los clientes).

**Cuándo usar DynamoDB:**
- **Sí:** Aplicaciones a gran escala, juegos, IoT, backends móviles, pujas en tiempo real.
- **Sí:** Aplicaciones sin servidor o **Serverless** (combina bien con **Lambda**).
- **Sí:** Patrones de acceso de clave-valor.
- **No:** Uniones (**JOINs**) complejas (use **RDS** en su lugar).
- **No:** Consultas ad-hoc (use **Athena** + **S3** o **RDS**).
- **No:** Transacciones ACID entre tablas (use **RDS**).

**Selección del Modo de Capacidad:**
- **Bajo demanda (On-demand):** Cargas de trabajo impredecibles, aplicaciones nuevas, pago por solicitud.
- **Provisionado:** Tráfico predecible, estado estable, optimización de costos (hasta un 60% de ahorro).
- **Cambio de modo:** Se puede cambiar de modo una vez cada 24 horas.

**Optimización de Costos:**
- Use bajo demanda para desarrollo y provisionado para producción.
- Habilite el **auto-scaling** para el modo provisionado.
- Use **S3** + **Athena** para análisis en lugar de escaneos (**scans**).
- Archive datos antiguos en **S3** utilizando **TTL** + **streams**.
- Clase **Standard-IA** para datos de acceso poco frecuente.

### Resolución de Problemas

**Problema:** "Validation Exception" al crear un elemento.

**Solución:**
- Verifique que se proporcionen los valores de la clave de partición y la clave de ordenación.
- Compruebe que los nombres de los atributos no tengan errores tipográficos.
- Asegúrese de que los tipos de datos coincidan (**String** frente a **Number**).
- Las claves de partición y de ordenación son campos obligatorios.

**Problema:** La consulta no devuelve resultados.

**Solución:**
- Verifique el valor exacto de la clave de partición (distingue entre mayúsculas y minúsculas).
- Compruebe que está utilizando el índice correcto (tabla frente a **GSI**).
- Confirme que existen elementos con esa clave de partición.
- Intente un escaneo para ver todos los elementos primero.

**Problema:** No se puede crear un **GSI** - límite excedido.

**Solución:**
- El **Free Tier** permite hasta 20 **GSIs** por tabla.
- Elimine índices no utilizados antes de crear nuevos.
- Considere si realmente necesita un **GSI** (un escaneo podría ser aceptable).

**Problema:** La exportación a **S3** falla.

**Solución:**
- Verifique que el bucket de **S3** exista y esté en la misma región.
- Compruebe que **DynamoDB** tenga permisos para escribir en el bucket.
- Asegúrese de que el nombre del bucket sea único a nivel mundial.
- Revise los detalles del estado de exportación para ver el error específico.

**Problema:** Altos costos de lectura/escritura.

**Solución:**
- Busque operaciones de escaneo (use consultas en su lugar).
- Revise los precios de bajo demanda frente a los provisionados.
- Implemente una capa de almacenamiento en caché (**DAX** o **ElastiCache**).
- Use lecturas eventualmente consistentes (un 50% más baras).

### Limpieza

> **Buenas Noticias:** **DynamoDB** solo cobra por el almacenamiento y las solicitudes. Con 25 GB siempre gratis, las tablas pequeñas son esencialmente gratuitas.

**Opción 1: Mantener la Tabla (Recomendado para el Aprendizaje)**
- Costo mínimo con 5 elementos (<1 KB de almacenamiento).
- Bueno para practicar consultas.
- Permite experimentar más.

**Opción 2: Eliminar la Tabla**

1. Vaya a **DynamoDB** → **Tables**.
2. Seleccione la tabla **"PuntuacionesJuego"**.
3. Haga clic en **"Delete"**.
4. **Eliminar todas las alarmas de CloudWatch para esta tabla:** Marque la casilla.
5. **Crear una copia de seguridad antes de eliminar:** Desmarque (para fines del laboratorio).
6. Escriba **"delete"** para confirmar.
7. Haga clic en **"Delete table"**.

8. **Eliminar el bucket de exportación de S3 (si se creó):**
   - Vaya a **S3**.
   - Seleccione el bucket de exportación.
   - Haga clic en **"Empty"** → Escriba "permanently delete" → **Empty**.
   - Haga clic en **"Delete"** → Escriba el nombre del bucket → **Delete**.

**Validación:**
- La tabla ya no aparece en la lista de tablas de **DynamoDB**.
- El bucket de exportación se eliminó de **S3**.
- No hay cargos continuos.

**Nota de Costo:**
- Modo bajo demanda: $0 sin tráfico.
- 5 elementos < 1 KB: ~$0.00025/mes por almacenamiento.
- Efectivamente gratis mantenerla para practicar.

### Verificación de Conocimientos Post-Laboratorio

**Pregunta 1:** ¿Cuál es la diferencia entre la clave de partición y la clave de ordenación?

<details>
<summary>Haga clic para ver la respuesta</summary>

**Respuesta:**
- **Clave de partición (Hash key):** Obligatoria, determina qué partición almacena el elemento, debe ser única si se usa sola.
- **Clave de ordenación (Range key):** Opcional, ordena los elementos dentro de una partición, permite consultas de rango.
- **Juntas:** Forman una clave primaria compuesta (partición + ordenación); la clave de partición agrupa los elementos y la clave de ordenación los organiza dentro del grupo.

Ejemplo: IDUsuario (partición) + Timestamp (ordenación) permite consultar todas las acciones de un usuario, ordenadas por tiempo.

</details>

**Pregunta 2:** ¿Cuándo se debe usar un Índice Secundario Global (GSI)?

<details>
<summary>Haga clic para ver la respuesta</summary>

**Respuesta:**
Use un **GSI** cuando necesite realizar consultas por atributos distintos a la clave primaria.

**Escenarios de ejemplo:**
- La tabla tiene IDUsuario como clave de partición, necesita consultar por Email → Cree un **GSI** con Email como clave de partición.
- La tabla tiene IDPedido como clave de partición, necesita encontrar todos los pedidos de un IDCliente → **GSI** en IDCliente.
- Necesita un orden de clasificación diferente → **GSI** con una clave de ordenación diferente.

**Limitaciones:**
- Eventualmente consistente (ligero retraso).
- Consume capacidad de escritura adicional.
- Cuesta almacenamiento extra (proyecta atributos).
- No se puede cambiar después de la creación (se debe eliminar y volver a crear).

</details>

**Pregunta 3:** ¿Cuál es la diferencia entre Query y Scan?

<details>
<summary>Haga clic para ver la respuesta</summary>

**Respuesta:**
- **Query (Consulta):** Eficiente, utiliza la clave de partición (y opcionalmente la de ordenación), devuelve solo los elementos coincidentes, baja latencia, costo predecible.
- **Scan (Escaneo):** Ineficiente, lee toda la tabla, filtra después de leer, alta latencia para tablas grandes, costoso.

**Ejemplo:**
- Consulta para "IDUsuario=Usuario001": Lee solo los elementos de Usuario001.
- Escaneo con filtro "IDUsuario=Usuario001": Lee TODOS los elementos y luego filtra.

**Consejo para el examen:** Prefiera siempre **Query** sobre **Scan**. Use **Scan** solo para análisis u operaciones únicas.

</details>

**Pregunta 4:** ¿Qué es DynamoDB Accelerator (DAX)?

<details>
<summary>Haga clic para ver la respuesta</summary>

**Respuesta:**
**DAX** es una caché en memoria para **DynamoDB** que proporciona una latencia de microsegundos.

**Características:**
- Caché totalmente gestionada y de alta disponibilidad.
- Reduce la latencia de lectura de milisegundos a microsegundos.
- Sin cambios en el código de la aplicación (compatible directamente).
- Soporta lecturas eventualmente consistentes y fuertemente consistentes.

**Casos de uso:**
- Cargas de trabajo con muchas lecturas.
- Tablas de clasificación de juegos.
- Pujas en tiempo real.
- Aplicaciones que requieren una respuesta <1ms.

**Nota:** No incluido en el **Free Tier**, se aplican cargos.

</details>

**Pregunta 5:** ¿Cómo funciona el precio de DynamoDB?

<details>
<summary>Haga clic para ver la respuesta</summary>

**Respuesta:**
**Modo bajo demanda (On-Demand):**
- Pago por solicitud: $1.25 por millón de solicitudes de escritura, $0.25 por millón de solicitudes de lectura.
- Almacenamiento: $0.25 por GB/mes.
- Ideal para cargas de trabajo impredecibles.

**Modo provisionado:**
- Reserva de capacidad: $0.00065 por WCU-hora, $0.00013 por RCU-hora.
- **Auto-scaling** disponible.
- Ideal para cargas de trabajo predecibles y estables.
- **Free Tier:** 25 WCU + 25 RCU + 25 GB de almacenamiento.

**Costos adicionales:**
- Copias de seguridad, restauraciones, tablas globales, **streams**.
- Transferencia de datos hacia afuera.

**Consejo para el examen:** Conozca la diferencia entre los modos de capacidad y cuándo usar cada uno.

</details>

**Pregunta 6:** ¿Puede DynamoDB manejar datos relacionales como las bases de datos SQL?

<details>
<summary>Haga clic para ver la respuesta</summary>

**Respuesta:**
**DynamoDB** es NoSQL; no está diseñado para patrones relacionales como los **JOINs**.

**Lo que DynamoDB NO hace bien:**
- Uniones de múltiples tablas (**multi-table joins**).
- Agregaciones complejas.
- Consultas ad-hoc.
- Restricciones de integridad referencial.

**Lo que DynamoDB SÍ hace bien:**
- Búsquedas de clave-valor.
- Patrones de diseño de una sola tabla.
- Aplicaciones a gran escala.
- Requisitos de baja latencia.

**Mejor Práctica:**
- Desnormalice los datos (duplique la información entre los elementos).
- Diseño de una sola tabla (**single-table design**) (patrón avanzado).
- Use **RDS/Aurora** para necesidades relacionales complejas.

**Consejo para el examen:** Sepa cuándo usar **DynamoDB** frente a **RDS**.

</details>

### Conclusiones Clave

- **DynamoDB es NoSQL**: servicio gestionado, escalable y sin esquema.
- **La clave primaria es crítica**: determina la distribución de datos y los patrones de acceso.
- **Query > Scan**: diseñe siempre para patrones de acceso de consulta.
- **Los GSIs permiten flexibilidad**: consulta por atributos que no son clave.
- **25 GB de almacenamiento siempre gratis**: excelente para el aprendizaje y aplicaciones pequeñas.
- **Modo bajo demanda**: el más sencillo para principiantes, sin planificación de capacidad.
- **Recuperación en un punto en el tiempo**: protección contra errores.
- **Uso con Lambda**: perfecto para arquitecturas sin servidor (**serverless**).
- **No es un reemplazo de RDS**: elija la base de datos adecuada para su caso de uso.

### Recursos Adicionales

- [Guía del desarrollador de DynamoDB](https://docs.aws.amazon.com/dynamodb/)
- [Mejores prácticas para DynamoDB](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/best-practices.html)
- [Modelado de datos en DynamoDB](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/data-modeling.html)
- [Sesiones de DynamoDB en AWS re:Invent](https://www.youtube.com/results?search_query=aws+reinvent+dynamodb)

---

---

## Preguntas Frecuentes de Resolución de Problemas (FAQ)

Esta sección aborda problemas comunes encontrados en todos los laboratorios.

### Problemas Generales de la Consola de AWS

**P: No puedo encontrar un servicio en la consola de AWS**

**R:**
- Use la barra de búsqueda en la parte superior (escriba el nombre del servicio).
- Compruebe si se encuentra en la región correcta (algunos servicios son específicos de una región).
- Verifique que su usuario de **IAM** tenga permisos para acceder a ese servicio.
- Algunos servicios tienen nombres diferentes (ej., "Billing and Cost Management" frente a "Billing").

**P: La consola de AWS es muy lenta o agota el tiempo de espera**

**R:**
- Borre la caché y las cookies del navegador.
- Pruebe el modo de incógnito o navegación privada.
- Cambie a un navegador diferente (Chrome, Firefox, Edge).
- Verifique su conexión a internet (se recomienda un mínimo de 5 Mbps).
- Pruebe una región diferente (algunas regiones pueden tener problemas de conectividad).
- Consulte el **AWS Service Health Dashboard**: https://status.aws.amazon.com

**P: Recibo errores de "No está autorizado para realizar esta operación"**

**R:**
- Verifique que el usuario de **IAM** tenga los permisos adecuados (políticas adjuntas).
- Si utiliza un usuario de **IAM**, asegúrese de que la cuenta raíz habilitó el acceso de facturación de **IAM** (para operaciones de facturación).
- Espere de 5 a 10 minutos después de crear el usuario de **IAM** (retraso en la propagación de políticas).
- Intente cerrar la sesión y volver a entrar.
- Verifique que se encuentra en la cuenta correcta (compruebe el ID de la cuenta).

**P: Los recursos que creé no aparecen en la consola**

**R:**
- **Lo más común:** Región incorrecta seleccionada (compruebe el menú desplegable de regiones en la parte superior derecha).
- Espere de 30 a 60 segundos y actualice (consistencia eventual).
- Verifique los filtros aplicados en la consola (borre todos los filtros).
- Verifique que el recurso se haya creado realmente (busque mensajes de error).
- Consulte **CloudTrail** para ver los eventos de creación.

### Problemas de Facturación y Costos

**P: Se me está cobrando aunque estoy usando el Free Tier**

**R:**
- Consulte el **Free Tier Dashboard** para ver el uso frente a los límites.
- Verifique que los tipos de instancia sean elegibles para el **Free Tier** (**t2.micro**, **t3.micro**).
- Compruebe si hay recursos en múltiples regiones (el **Free Tier** es por cuenta, no por región).
- Busque servicios que no sean del **Free Tier** (**NAT Gateway**, **Load Balancers**).
- Cargos por transferencia de datos (hacia internet).
- Almacenamiento **EBS** que supere los 30 GB.
- **RDS Multi-AZ** (no es elegible para el **Free Tier**).

**P: Mi alarma de facturación no funciona**

**R:**
- Verifique que la alarma de facturación se haya creado únicamente en la región **us-east-1**.
- Compruebe que la suscripción de **SNS** esté confirmada (busque el correo de confirmación).
- Espere 24 horas para recibir los datos iniciales (las métricas de facturación tardan en aparecer).
- Asegúrese de que los cargos realmente superen el umbral.
- Verifique que el estado de la alarma sea "OK" y no "Insufficient data".

**P: No puedo acceder al panel de facturación**

**R:**
- Debe haber iniciado sesión como usuario raíz (root) O
- Ser un usuario de **IAM** con permisos de facturación Y tener habilitado el acceso de facturación de **IAM** por el usuario raíz.
- Vaya a Configuración de la cuenta → Habilite "Activar acceso de IAM" para la facturación.

**P: Cargos inesperados después de que terminó el Free Tier**

**R:**
- El **Free Tier** caduca 12 meses después de la creación de la cuenta (compruebe la fecha de inicio).
- Algunos servicios son "siempre gratuitos" (**Lambda** 1 millón de solicitudes, **DynamoDB** 25 GB).
- Configure alertas de facturación para el monitoreo posterior al **Free Tier**.
- Revise la factura en detalle para identificar los servicios costosos.
- Considere apagar los recursos no esenciales.

### Problemas de EC2

**P: No puedo conectarme a la instancia EC2 a través de SSH**

**R:**
1. **Tiempo de espera de la conexión (Connection timeout):**
   - El grupo de seguridad (**Security group**) permite **SSH** (puerto 22) desde su IP.
   - La instancia está en una subred pública con una IP pública.
   - La tabla de enrutamiento (**Route table**) tiene una ruta al **Internet Gateway** (0.0.0.0/0 → **igw-xxx**).
   - La **ACL** de red permite el tráfico **SSH**.
   - La instancia está en estado "running".

2. **Permiso denegado (publickey):**
   - Está utilizando el archivo de clave **.pem** correcto.
   - El archivo de clave tiene los permisos adecuados: `chmod 400 key.pem`.
   - Está utilizando el nombre de usuario correcto (**ec2-user** para Amazon Linux, **ubuntu** para Ubuntu).
   - El par de claves (**Key pair**) coincide con la instancia.

3. **Fallo en la verificación de la clave del host:**
   - Escriba "yes" para aceptar la huella digital.
   - O use: `ssh -o StrictHostKeyChecking=no -i key.pem ec2-user@IP`.

**P: Las comprobaciones de estado de la instancia (status checks) fallan**

**R:**
- **1/2 comprobaciones superadas:** Falló la legibilidad del sistema (problema de hardware de AWS: detenga e inicie la instancia).
- **0/2 comprobaciones superadas:** Fallan tanto las comprobaciones del sistema como las de la instancia (consulte los registros, script de **user data** defectuoso).
- Espere 2-3 minutos después del lanzamiento (las comprobaciones llevan tiempo).
- Vea el **System Log** y la captura de pantalla de la instancia en el menú **Actions**.

**P: El script de user data no se ejecutó**

**R:**
- Conéctese por **SSH** a la instancia: `cat /var/log/cloud-init-output.log`.
- Busque errores en la ejecución del script.
- Verifique la sintaxis (los scripts de bash necesitan `#!/bin/bash`).
- Asegúrese de que el script tenga los permisos adecuados.
- El **user data** se ejecuta solo en el primer arranque (a menos que se configure lo contrario).

**P: No puedo acceder al servidor web en EC2**

**R:**
- El grupo de seguridad permite **HTTP** (puerto 80) desde 0.0.0.0/0.
- El servidor web está realmente en ejecución: `sudo systemctl status httpd` o `nginx`.
- Verifique desde adentro: `curl localhost` (debería funcionar).
- Asegúrese de usar **HTTP** y no **HTTPS** (**http://** en lugar de **https://**).
- Verifique el firewall en la instancia: `sudo iptables -L`.

**P: El tipo de instancia no está disponible / error de capacidad**

**R:**
- Pruebe con una zona de disponibilidad diferente dentro de la misma región.
- Pruebe con un tipo de instancia ligeramente diferente (**t2.micro** frente a **t3.micro**).
- Espere 30 minutos y vuelva a intentarlo (la capacidad fluctúa).
- Considere una región diferente.
- Para problemas persistentes, contacte al soporte de AWS.

### Problemas de S3

**P: Error 403 Forbidden al acceder al sitio web de S3**

**R:**
- La política del bucket permite la lectura pública (`s3:GetObject` para Principal: *).
- "Block all public access" está desactivado (**OFF**).
- El alojamiento de sitios web estáticos está habilitado.
- Los objetos se han cargado realmente en el bucket.
- El **ARN** de la política del bucket incluye `/*` al final (`arn:aws:s3:::nombre-del-bucket/*`).
- Está usando la URL del punto de enlace (**endpoint**) del sitio web del bucket correcta (no la URL regular de S3).

**P: Error 404 Not Found en el sitio web de S3**

**R:**
- El archivo **index.html** existe en la raíz del bucket (distingue entre mayúsculas y minúsculas).
- El nombre del archivo es exactamente "index.html" (no Index.html ni index.HTML).
- El alojamiento de sitios web estáticos está habilitado.
- Verifique si está usando el punto de enlace correcto (el de sitio web, no el punto de enlace **REST**).
- Borre la caché del navegador.

**P: No se puede crear el bucket: el nombre ya existe**

**R:**
- Los nombres de los buckets de **S3** son únicos a nivel mundial en todas las cuentas de AWS.
- Pruebe con un nombre diferente con números aleatorios: `mi-bucket-12345678`.
- Los nombres de los buckets deben cumplir con DNS (minúsculas, sin guiones bajos).
- Es posible que otra persona tenga ese nombre (incluso si se eliminó hace menos de 24 horas).

**P: No se puede eliminar el bucket: "Bucket not empty"**

**R:**
1. Vaya al bucket.
2. Haga clic en el botón **"Empty"**.
3. Escriba "permanently delete".
4. Espere a que se complete el vaciado.
5. Luego haga clic en **"Delete bucket"**.
- Alternativa: Habilite el control de versiones → Elimine todas las versiones → Luego elimine el bucket.
- Verifique si hay cargas multiparte incompletas.

**P: Las cargas en S3 son muy lentas**

**R:**
- Verifique la velocidad de su conexión a internet.
- Intente cargar archivos más pequeños primero.
- Use la carga multiparte (**multipart upload**) para archivos de más de 100 MB.
- Considere usar la **AWS CLI** o los **SDKs** (más rápido que la consola).
- Compruebe si las extensiones del navegador están interfiriendo.

### Problemas de VPC y Redes

**P: La instancia EC2 no tiene acceso a internet**

**R:**
- La instancia está en una subred pública (verifique la configuración de la subred).
- La instancia tiene una IP pública o una **Elastic IP**.
- La tabla de enrutamiento de la subred tiene una ruta al **Internet Gateway** (0.0.0.0/0 → **igw-xxx**).
- El grupo de seguridad permite el tráfico de salida (por defecto permite todo).
- La **ACL** de red permite el tráfico de salida (por defecto permite todo).
- La resolución **DNS** está habilitada para la **VPC**.

**P: No se puede conectar por SSH entre instancias EC2 en la misma VPC**

**R:**
- Los grupos de seguridad permiten el tráfico entre las instancias.
- Use IPs privadas (no IPs públicas) para la comunicación dentro de la misma **VPC**.
- Ambas instancias están en subredes con el enrutamiento adecuado.
- Las **ACLs** de red permiten el tráfico (si se usan **NACLs** personalizadas).

**P: La creación de la VPC falla**

**R:**
- Verifique que no se haya superado el límite de **VPCs** (5 por región por defecto).
- El bloque **CIDR** no se superpone con las **VPCs** existentes (si se planea un **VPC peering**).
- El bloque **CIDR** es válido (de /16 a /28 para la **VPC**).
- Pruebe en una región diferente si los problemas persisten.

**P: Los cambios en el grupo de seguridad no surten efecto**

**R:**
- Espere de 30 a 60 segundos (consistencia eventual).
- Actualice la página de la consola.
- Verifique que el grupo de seguridad correcto esté adjunto a la instancia.
- Verifique la sintaxis de las reglas (rangos de puertos, protocolos, orígenes).
- Los grupos de seguridad tienen estado (**stateful**) (no necesitan reglas de salida para las respuestas).

### Problemas de RDS

**P: No puedo conectarme a RDS desde EC2**

**R:**
- **RDS** y **EC2** están en la misma **VPC**.
- El grupo de seguridad de **RDS** permite el puerto de **MySQL/PostgreSQL** desde el grupo de seguridad de **EC2**.
- Está usando el nombre de host del punto de enlace de **RDS** (no la dirección IP).
- El puerto es correcto (3306 para **MySQL**, 5432 para **PostgreSQL**).
- El estado de **RDS** es "Available".
- Las credenciales son correctas (distingue entre mayúsculas y minúsculas).
- No está intentando conectarse desde el internet público (Acceso público de **RDS** = No).

**P: La creación de RDS es lenta**

**R:**
- Normal: **RDS** tarda de 5 a 15 minutos en crearse.
- **Multi-AZ** tarda más (de 15 a 20 minutos).
- Observe el estado: Creating → Backing up → Available.
- Si se queda atascado por más de 30 minutos, consulte **CloudTrail** para ver errores.

**P: RDS cuesta más de lo esperado**

**R:**
- Verifique la clase de instancia (**db.t3.micro** es del **Free Tier**).
- **Multi-AZ** duplica los costos (no es elegible para el **Free Tier**).
- Almacenamiento de respaldo superior a 20 GB.
- Los **IOPS** provisionados tienen un costo extra.
- Se habilitó el monitoreo mejorado (**Enhanced Monitoring**) ($).
- Verifique que se usó la plantilla "**Free Tier**" durante la creación.

### Problemas de IAM

**P: El usuario de IAM no puede iniciar sesión**

**R:**
- Está utilizando la URL de inicio de sesión de usuario de **IAM** (no la página de inicio de sesión raíz).
- Formato de la URL de inicio de sesión: `https://id-de-la-cuenta.signin.aws.amazon.com/console`.
- O: `https://alias-de-la-cuenta.signin.aws.amazon.com/console`.
- El nombre de usuario y la contraseña son correctos (distingue entre mayúsculas y minúsculas).
- El usuario tiene habilitado el acceso a la consola (no solo el acceso programático).
- La cuenta no está bloqueada después de múltiples intentos fallidos (espere 15 minutos).

**P: El usuario de IAM no tiene permisos a pesar de tener una política adjunta**

**R:**
- Espere de 5 a 10 minutos para la propagación de la política.
- Verifique que la política esté adjunta al usuario O al grupo al que pertenece el usuario.
- La sintaxis **JSON** de la política es correcta (use el validador de políticas).
- No hay declaraciones **Deny** explícitas (Deny anula a Allow).
- Verifique si las **SCPs** (**Service Control Policies**) están limitando el acceso (**AWS Organizations**).

**P: No puedo habilitar MFA**

**R:**
- La hora del teléfono está sincronizada con la hora de internet.
- Escaneó el código QR con éxito con la aplicación de autenticación.
- Ingrese dos códigos consecutivos (no el mismo código dos veces).
- Ingrese los códigos rápidamente (caducan cada 30 segundos).
- Pruebe con una aplicación de autenticación diferente si los problemas persisten.

### Problemas de Lambda

**P: La función Lambda falla por tiempo de espera (timeout)**

**R:**
- Aumente la configuración del tiempo de espera (por defecto 3 segundos, máximo 15 minutos).
- Verifique que la función realmente se complete dentro del tiempo de espera.
- Busque bucles infinitos u operaciones de bloqueo.
- Revise los registros de **CloudWatch** para ver el tiempo de ejecución.

**P: La función Lambda falla con "Permission denied"**

**R:**
- El rol de ejecución de **Lambda** tiene los permisos necesarios.
- Si accede a **S3**: el rol necesita `s3:GetObject`, `s3:PutObject`.
- Si accede a **DynamoDB**: el rol necesita los permisos adecuados de **DynamoDB**.
- Consulte **CloudWatch Logs** para ver errores de permisos específicos.

**P: API Gateway devuelve "Internal Server Error"**

**R:**
- Consulte los registros de **CloudWatch** de la función **Lambda** para ver el error real.
- La función devuelve el formato de respuesta adecuado (**statusCode**, **body**, **headers**).
- La integración está configurada correctamente (se recomienda la integración de proxy de **Lambda**).
- La **API** está desplegada (debe volver a desplegar después de realizar cambios).

**P: La función Lambda no puede conectarse a RDS/internet**

**R:**
- Si **Lambda** está en una **VPC**: la **VPC** necesita un **NAT Gateway** para el acceso a internet.
- Los grupos de seguridad permiten el tráfico entre **Lambda** y **RDS**.
- El rol de ejecución de **Lambda** tiene el permiso `ec2:CreateNetworkInterface`.
- Se aumentó el tiempo de espera (la **VPC** añade latencia).

### Problemas de CloudFormation

**P: Falló la creación de la pila (stack) - se inició la reversión (rollback)**

**R:**
- Consulte la pestaña **"Events"** para ver el mensaje de error específico.
- Común: Conflicto de nombre de recurso (ya existe).
- Común: Permisos insuficientes.
- Común: Valores de parámetros inválidos.
- Corrija la plantilla y cree una nueva pila (o actualícela si es compatible).

**P: La pila se queda en CREATE_IN_PROGRESS**

**R:**
- Consulte los eventos (**Events**) para ver la última acción completada.
- Algunos recursos tardan tiempo (**RDS** 10-15 min, **NAT Gateway** 3-5 min).
- Si realmente está atascada por más de 30 min, elimine la pila y vuelva a crearla.
- Verifique los límites del servicio (podría estar al límite de su capacidad).

**P: No se puede eliminar la pila - dependencias de recursos**

**R:**
- Vacíe los buckets de **S3** antes de eliminar la pila.
- Elimine las **ENIs** (**Elastic Network Interfaces**) adjuntas a **Lambda/RDS**.
- Elimine las dependencias manualmente y luego vuelva a intentarlo.
- Verifique la política de recursos "**Retain**" (algunos recursos están protegidos).

### Problemas de Auto Scaling y Load Balancer

**P: Auto Scaling no lanza instancias**

**R:**
- Consulte la pestaña **Activity** del grupo de **Auto Scaling** para ver errores.
- Verifique que la plantilla de lanzamiento sea válida (la **AMI** existe, el tipo de instancia está disponible).
- No se ha alcanzado el límite de capacidad máxima.
- La subred tiene direcciones IP disponibles.
- No se han superado los límites del servicio (límite de instancias **EC2**).

**P: El Load Balancer muestra todos los destinos como no saludables (unhealthy)**

**R:**
- La ruta de comprobación de estado (**health check path**) es correcta (debe devolver un estado 200).
- El grupo de seguridad permite el tráfico de comprobación de estado desde el balanceador de carga.
- La aplicación está realmente en ejecución en las instancias.
- El intervalo/umbral de comprobación de estado es adecuado (aumente el período de gracia).
- Verifique la configuración de comprobación de estado del grupo de destino (**target group**).

**P: El Load Balancer devuelve 503 Service Unavailable**

**R:**
- No hay destinos saludables disponibles.
- Todos los destinos están fallando las comprobaciones de estado.
- El grupo de destino no tiene destinos registrados.
- Las instancias están en las subredes correctas.
- Espere a que las instancias superen las comprobaciones de estado (2 éxitos consecutivos).

### Problemas de DynamoDB

**P: La consulta no devuelve resultados**

**R:**
- El valor de la clave de partición coincide exactamente (distingue entre mayúsculas y minúsculas).
- Está usando el índice correcto (tabla base frente a **GSI**).
- Realmente existen elementos con esa clave.
- Intente un **Scan** para verificar los elementos en la tabla.

**P: Errores de limitación de DynamoDB (ProvisionedThroughputExceededException)**

**R:**
- Cambie al modo de capacidad bajo demanda (**On-Demand**).
- O aumente la capacidad provisionada (**WCU/RCU**).
- Habilite el **Auto Scaling** para el modo provisionado.
- Implemente un retroceso exponencial (**exponential backoff**) en la aplicación.
- Verifique si hay una partición caliente (una sola clave recibe todo el tráfico).

**P: No se puede crear un GSI - límite excedido**

**R:**
- Máximo 20 **GSIs** por tabla.
- Elimine los índices no utilizados.
- Considere si un **Scan** con filtro es aceptable.

### Mensajes de Error Comunes Decodificados

**"InvalidParameterValue":**
- Un parámetro que proporcionó tiene un valor inválido.
- Consulte la documentación de AWS para ver los valores válidos.
- Común: Zona de disponibilidad incorrecta, bloque **CIDR** inválido, tipo de instancia incorrecto.

**"UnauthorizedOperation":**
- El usuario/rol de **IAM** carece del permiso requerido.
- Añada la política necesaria al usuario/rol.
- Verifique si hay errores tipográficos en los nombres de las acciones.

**"ResourceNotFoundException":**
- El recurso al que intenta acceder no existe.
- Verifique que el ID del recurso sea correcto.
- Compruebe la región correcta.
- Es posible que el recurso haya sido eliminado.

**"LimitExceeded":**
- Alcanzó un límite de servicio de AWS (instancias **EC2**, **VPCs**, grupos de seguridad).
- Solicite un aumento de límite a través de la consola de **Service Quotas**.
- O limpie los recursos no utilizados.

**"DependencyViolation":**
- Intenta eliminar un recurso con dependencias.
- Ejemplo: No se puede eliminar un grupo de seguridad adjunto a una instancia en ejecución.
- Elimine las dependencias primero.

### Obtener Ayuda

**Cuándo pedir ayuda:**
- Ha intentado todos los pasos de resolución de problemas.
- El problema persiste por más de 30 minutos.
- Preocupación por la facturación (cargos inesperados).
- Se sospecha de una interrupción del servicio.

**Dónde obtener ayuda:**
1. **Documentación de AWS:** La más completa: https://docs.aws.amazon.com
2. **AWS re:Post:** Foro de la comunidad: https://repost.aws
3. **Soporte de AWS:** Si tiene un plan de soporte de pago.
4. **Stack Overflow:** Etiquete las preguntas con [amazon-web-services].
5. **AWS Service Health Dashboard:** Compruebe si hay interrupciones: https://status.aws.amazon.com

**Información a proporcionar al pedir ayuda:**
- Nombre del servicio de AWS.
- Región.
- Mensaje de error exacto.
- Pasos realizados hasta ahora.
- Capturas de pantalla (oculte la información sensible).
- IDs de recursos.
- Cronología (cuándo comenzó el problema).

**Qué NO compartir:**
- Claves de acceso (**Access Keys**) o claves secretas (**Secret Keys**) de AWS.
- Contraseñas.
- **ARNs** completos con IDs de cuenta (pueden estar parcialmente ocultos).
- Información de tarjetas de crédito.

---

## Recomendaciones de Práctica Adicionales

### Explore más Servicios de AWS

1. **DynamoDB**
   - Cree una tabla NoSQL.
   - Añada elementos usando la consola.
   - Consulte y escanee los datos.
   - Explore los índices.

2. **CloudTrail**
   - Habilite un seguimiento (**trail**).
   - Vea el historial de llamadas a la **API**.
   - Busque eventos específicos.
   - Entienda el registro de auditoría.

3. **AWS Config**
   - Configure **Config**.
   - Rastree las configuraciones de los recursos.
   - Vea la línea de tiempo de configuración.
   - Cree reglas de cumplimiento.

4. **Auto Scaling**
   - Cree una plantilla de lanzamiento.
   - Configure un grupo de **Auto Scaling**.
   - Configure políticas de escalado.
   - Pruebe el escalado horizontal de salida y entrada (**scale-out/scale-in**).

5. **Elastic Beanstalk**
   - Despliegue una aplicación de ejemplo.
   - Explore el entorno gestionado.
   - Vea los registros y el monitoreo.
   - Actualice la aplicación.

### Práctica con la AWS CLI

**Instale la AWS CLI:**

1. Siga las instrucciones: [https://aws.amazon.com/cli/](https://aws.amazon.com/cli/)
2. Configure las credenciales: `aws configure`
3. Ejecute comandos básicos:
   ```bash
   aws s3 ls
   aws ec2 describe-instances
   aws iam list-users
   ```

### Exploración Multi-Región

1. **Comparar regiones:**
   - Observe las diferencias en la disponibilidad de servicios.
   - Compruebe las variaciones de precios.
   - Pruebe la latencia desde su ubicación.

2. **Practicar la recuperación de desastres (Disaster Recovery):**
   - Cree recursos en múltiples regiones.
   - Entienda la replicación entre regiones (**cross-region replication**).
   - Aprenda sobre servicios globales frente a regionales.

### Gestión de Costos

1. **Monitoree el Free Tier Dashboard diariamente.**
2. **Configure múltiples presupuestos (Budgets)** para diferentes servicios.
3. **Revise el Cost Explorer semanalmente.**
4. **Practique el uso de la Calculadora de Precios (Pricing Calculator)** para diferentes escenarios.
5. **Entienda el ciclo de facturación** y los métodos de pago.

### Hábitos de Documentación

1. **Tome capturas de pantalla** de cada paso.
2. **Cree diagramas** de las arquitecturas construidas.
3. **Escriba notas** sobre las lecciones aprendidas.
4. **Documente errores** y soluciones.
5. **Construya su propia hoja de trucos (Cheat Sheet).**

### Laboratorios Avanzados (Después de lo básico)

1. **VPC Peering**
2. **Load Balancer con Auto Scaling**
3. **Despliegue de RDS Multi-AZ**
4. **CloudFront con origen en S3**
5. **Lambda con SQS y DynamoDB**
6. **CodePipeline para CI/CD**

---

## Recordatorios Finales Importantes

### Limpie Siempre los Recursos

> **Crítico:** Dejar recursos en ejecución puede generar cargos inesperados después de que expire el **Free Tier**.

**Lista de verificación diaria:**
- [ ] Terminar todas las instancias **EC2**.
- [ ] Eliminar las bases de datos **RDS**.
- [ ] Vaciar y eliminar los buckets de **S3**.
- [ ] Eliminar las pilas de **CloudFormation**.
- [ ] Eliminar los volúmenes **EBS** y las instantáneas (**snapshots**) no utilizados.
- [ ] Revisar el panel de facturación.

### Monitorear los Costos

- Compruebe el **uso del Free Tier** a diario.
- Revise las **alertas de facturación**.
- Configure **presupuestos** para cada servicio.
- Descargue las **facturas mensuales** para sus registros.
- Use el **Cost Explorer** para rastrear tendencias.

### Mejores Prácticas de Seguridad

- **Nunca comparta** las credenciales de la cuenta raíz (root).
- **Habilite MFA** en todas las cuentas.
- **Use roles de IAM** en lugar de claves de acceso siempre que sea posible.
- **Siga el principio de privilegio mínimo.**
- **Rote regularmente** las credenciales.
- **Revise** las reglas de los grupos de seguridad con frecuencia.

### Aprender Más

- **Documentación de AWS:** [https://docs.aws.amazon.com](https://docs.aws.amazon.com)
- **AWS Skill Builder:** Cursos de formación gratuitos.
- **AWS Workshops:** [https://workshops.aws](https://workshops.aws)
- **YouTube de AWS:** Tutoriales y demostraciones oficiales.
- **AWS re:Post:** Foro comunitario de preguntas y respuestas.

---

## ¡Felicitaciones!

¡Ha completado los 10 laboratorios prácticos! Ahora tiene experiencia práctica con:

- Gestión de facturación y costos.
- Seguridad de **IAM**.
- Instancias de cómputo **EC2**.
- Almacenamiento de objetos **S3**.
- Redes de **VPC**.
- Bases de datos gestionadas de **RDS**.
- Monitoreo de **CloudWatch**.
- Funciones sin servidor de **Lambda**.
- Infraestructura como código con **CloudFormation**.

Este conocimiento práctico le ayudará significativamente en el examen de **AWS Cloud Practitioner** y en el uso de **AWS** en el mundo real.

**Próximos pasos:**
- Revise las áreas débiles.
- Realice exámenes de práctica.
- Estudie la teoría junto con la experiencia práctica.
- Programe su examen de certificación.

¡Mucho éxito en su camino como **AWS Cloud Practitioner**!

---

[← Volver al Plan de Estudio](06-study-plan.md) | [Volver al Inicio](README.md) | [Siguiente: Comparación de Servicios →](08-service-comparisons.md)
