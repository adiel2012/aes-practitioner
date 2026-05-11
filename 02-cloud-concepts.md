# Dominio 1: Conceptos de la Nube (24%)

[← Anterior: Introducción](01-introduction.md) | [Volver al Inicio](README.md) | [Siguiente: Seguridad y Cumplimiento →](03-security-compliance.md)

## ¿Qué es Cloud Computing?

### Definición

**Cloud Computing** es la **entrega bajo demanda** (**on-demand delivery**) de recursos de TI a través de Internet con **precios de pago por uso** (**pay-as-you-go pricing**). En lugar de comprar, poseer y mantener centros de datos y servidores físicos, puedes acceder a servicios tecnológicos según los necesites.

### Seis Ventajas de Cloud Computing

#### 1. Cambiar gastos de capital por gastos variables (Trade Capital Expense for Variable Expense)

**Concepto Clave:**
- Paga solo por lo que consumes
- Sin costes de infraestructura iniciales
- Menor **Total Cost of Ownership (TCO)**
- Convertir costes fijos en costes variables

**Escenario del Mundo Real:**
Una startup tradicional necesita comprar $500,000 en servidores por adelantado, aunque inicialmente solo use el 20% de esa capacidad. Si el negocio falla, es una pérdida total. Con AWS, la misma startup puede comenzar con $500 al mes en costes de nube, escalando solo a medida que crece. Evitan el riesgo financiero de grandes inversiones iniciales.

**Ejemplo:**
Netflix migró de centros de datos locales a AWS, eliminando miles de millones en **Capital Expenditure**. En lugar de comprar servidores para la demanda máxima (viernes por la noche), pagan exactamente por lo que usan hora tras hora. Durante las horas de menor actividad (mañanas de días laborables), sus costes disminuyen automáticamente.

**Contexto del Examen:**
Busca preguntas sobre startups con presupuestos limitados, empresas que quieren evitar grandes costes iniciales o escenarios que pregunten sobre la conversión de **CapEx** a **OpEx**.

#### 2. Beneficiarse de economías de escala masivas (Benefit from Massive Economies of Scale)

**Concepto Clave:**
- AWS logra mayores economías de escala
- Precios de pago por uso más bajos
- Los precios disminuyen con el tiempo
- La infraestructura compartida reduce los costes

**Escenario del Mundo Real:**
AWS sirve a millones de clientes en todo el mundo. Pueden negociar mejores tratos con los fabricantes de hardware, obtener descuentos por volumen en energía y refrigeración, y distribuir los costes fijos en una base masiva de clientes. Estos ahorros se pasan a los clientes a través de reducciones de precios regulares.

**Ejemplo:**
Entre 2006 y 2023, AWS ha reducido los precios más de 100 veces en sus servicios. Una empresa que ejecuta una base de datos en **RDS** hoy paga significativamente menos por GB de lo que pagaba hace cinco años por el mismo servicio, sin necesidad de realizar ninguna acción por su parte.

**Contexto del Examen:**
Las preguntas pueden preguntar por qué los proveedores de nube pueden ofrecer precios más bajos que las empresas individuales que ejecutan sus propios centros de datos, o por qué AWS reduce los precios regularmente.

#### 3. Dejar de adivinar la capacidad (Stop Guessing Capacity)

**Concepto Clave:**
- Escala hacia arriba o hacia abajo según la demanda
- Sin exceso o falta de aprovisionamiento
- Recursos elásticos
- Capacidades de **Auto Scaling**

**Escenario del Mundo Real:**
Una empresa de comercio electrónico que se prepara para el Black Friday tradicionalmente tenía que adivinar la demanda y comprar servidores con meses de antelación. Si adivinaban demasiado bajo, el sitio colapsaba y perdían ventas. Si adivinaban demasiado alto, desperdiciaban dinero en servidores inactivos. Con **AWS Auto Scaling**, la capacidad se ajusta automáticamente según el tráfico real.

**Ejemplo:**
Airbnb experimenta picos de tráfico masivos durante grandes eventos (Olimpiadas, Nochevieja). Usando **AWS Auto Scaling**, escalan automáticamente de 100 instancias **EC2** a 5,000 instancias durante la demanda máxima, y luego vuelven a la línea base después. Pagan solo por lo que realmente necesitan en cada momento.

**Contexto del Examen:**
Busca escenarios que involucren cargas de trabajo impredecibles, negocios estacionales o empresas preocupadas por el sobre o subaprovisionamiento de infraestructura.

#### 4. Aumentar la velocidad y la agilidad (Increase Speed and Agility)

**Concepto Clave:**
- Recursos disponibles en minutos
- Experimentación e innovación más rápidas
- Reducción del **Time to Market**
- Despliegue rápido de nuevas funciones

**Escenario del Mundo Real:**
En un entorno tradicional, un desarrollador que solicita un nuevo servidor podría esperar semanas para la adquisición, entrega y configuración. Con AWS, el mismo desarrollador puede aprovisionar recursos idénticos en menos de 5 minutos a través de la consola o una sola llamada a la API.

**Ejemplo:**
Una empresa de software quiere probar una nueva idea de función. Localmente, tendrían que justificar el coste, enviar un ticket, esperar la aprobación y esperar semanas por los recursos. Con AWS, un desarrollador puede levantar un entorno de prueba en minutos, experimentar durante unos días a un coste de menos de $50 y luego eliminarlo si la idea no funciona. Esto permite una cultura de "fail fast".

**Contexto del Examen:**
Las preguntas sobre innovación rápida, cargas de trabajo experimentales, reducción del tiempo de comercialización o productividad del desarrollador a menudo se relacionan con esta ventaja.

#### 5. Dejar de gastar dinero en la ejecución y el mantenimiento de centros de datos (Stop Spending Money Running and Maintaining Data Centers)

**Concepto Clave:**
- Enfocarse en los diferenciadores del negocio
- AWS gestiona la infraestructura
- Reducción de la carga operativa
- Eliminar el "**undifferentiated heavy lifting**"

**Escenario del Mundo Real:**
La ventaja competitiva de una empresa minorista es comprender las preferencias del cliente y gestionar el inventario, no ejecutar centros de datos. Al mudarse a AWS, pueden redirigir el tiempo, el dinero y el talento que antes gastaban en montar servidores, gestionar sistemas HVAC y parchear firmware hacia actividades que mejoren directamente la experiencia del cliente.

**Ejemplo:**
GE Oil and Gas movió 500 aplicaciones a AWS, lo que les permitió redirigir al personal de TI del mantenimiento de la infraestructura a la creación de herramientas analíticas que ayudan a los clientes a predecir fallos en los equipos. Este cambio de "mantener las luces encendidas" a la innovación se convirtió en un diferenciador competitivo.

**Contexto del Examen:**
Busca preguntas sobre enfocarse en el negocio principal, reducir los gastos operativos generales o permitir que los equipos de TI trabajen en proyectos estratégicos en lugar del mantenimiento rutinario.

#### 6. Globalizarse en minutos (Go Global in Minutes)

**Concepto Clave:**
- Desplegar aplicaciones globalmente
- Baja latencia para usuarios en todo el mundo
- Múltiples **AWS Regions** disponibles
- Fácil expansión geográfica

**Escenario del Mundo Real:**
Una empresa con sede en EE. UU. quiere expandirse a Europa y Asia. Construir centros de datos en esas regiones tomaría años y costaría millones. Con AWS, pueden desplegar su aplicación en regiones europeas y asiáticas en unas pocas horas, proporcionando instantáneamente acceso de baja latencia a los usuarios en esos mercados.

**Ejemplo:**
Samsung desplegó su plataforma SmartThings en 17 regiones de AWS a nivel mundial, lo que les permitió atender a los clientes de IoT con baja latencia en todo el mundo. Lo que habría requerido miles de millones en inversión en infraestructura y años de construcción se logró en meses.

**Contexto del Examen:**
Las preguntas sobre expansión global, reducción de la latencia para usuarios internacionales o recuperación ante desastres en regiones geográficas se relacionan con esta ventaja.

> **Punto Clave:** Estas seis ventajas se prueban con frecuencia en el examen. Comprende cada una y sé capaz de identificarlas en escenarios. El examen a menudo presenta un escenario de negocio y pregunta qué ventaja se aplica.

> **Consejo para el Examen:** Formato de pregunta común: "Una empresa quiere [escenario]. ¿Qué ventaja de **Cloud Computing** representa esto?". Practica mapeando escenarios a ventajas.

## Modelos de Cómputo en la Nube (Cloud Computing Models)

### Infraestructura como Servicio (Infrastructure as a Service - IaaS)

**Definición:** Bloques de construcción básicos para la TI en la nube.

**Características:**
- Máximo nivel de flexibilidad y control.
- Tú gestionas: **OS**, aplicaciones, datos.
- AWS gestiona: Hardware, redes, instalaciones.

**Ejemplos de AWS:**
- **Amazon EC2** (servidores virtuales)
- **Amazon S3** (almacenamiento de objetos)
- **Amazon VPC** (redes virtuales)
- **Amazon EBS** (almacenamiento de bloques)

**Casos de Uso:**
- Migración de aplicaciones existentes.
- Desarrollo de aplicaciones personalizadas.
- Cuando necesitas control total sobre los recursos.
- Cargas de trabajo de cómputo de alto rendimiento.
- Aplicaciones complejas de múltiples niveles.

**Ejemplos de Empresas Reales:**

1. **Netflix (EC2, S3)**
   - Utiliza miles de instancias EC2 para la codificación de vídeo.
   - Almacena petabytes de contenido de vídeo en S3.
   - Mantiene el control total sobre su arquitectura de microservicios.
   - Por qué **IaaS**: Necesita un control detallado sobre la optimización del rendimiento.

2. **Dropbox (S3)**
   - Inicialmente construyó toda su infraestructura de almacenamiento en S3.
   - Necesitaba una infraestructura de almacenamiento a nivel de bloque.
   - Eventualmente se movió a un enfoque híbrido pero aún usa S3.
   - Por qué **IaaS**: Requería infraestructura de almacenamiento sin construir centros de datos.

3. **Zillow (EC2, EBS)**
   - Ejecuta el procesamiento de datos de propiedades en EC2.
   - Usa EBS para el almacenamiento de bases de datos con requisitos específicos de IOPS.
   - Necesita control sobre los tipos de instancia y configuraciones.
   - Por qué **IaaS**: El procesamiento de datos complejo requiere flexibilidad de infraestructura.

**Cuándo Elegir IaaS:**
- Necesitas acceso a nivel de sistema operativo.
- Ejecutas aplicaciones heredadas.
- Requieres controles de cumplimiento específicos.
- Necesitas configuraciones de red personalizadas.
- Quieres la máxima flexibilidad.

### Plataforma como Servicio (Platform as a Service - PaaS)

**Definición:** Elimina la necesidad de gestionar la infraestructura subyente.

**Características:**
- Enfocado en el despliegue y la gestión de aplicaciones.
- Tú gestionas: Aplicaciones, datos.
- AWS gestiona: **Runtime**, middleware, sistema operativo, servidores.

**Ejemplos de AWS:**
- **AWS Elastic Beanstalk** (despliegue de aplicaciones)
- **AWS Lambda** (funciones sin servidor)
- **Amazon RDS** (bases de datos gestionadas)
- **Amazon Aurora** (base de datos compatible con MySQL/PostgreSQL)
- **AWS Fargate** (contenedores sin servidor)

**Casos de Uso:**
- Despliegue de aplicaciones web.
- Desarrollo y despliegue rápido.
- Cuando quieres enfocarte en el código, no en la infraestructura.
- Microservicios y backends de API.
- Requisitos de escalado automatizado.

**Ejemplos de Empresas Reales:**

1. **Expedia (Elastic Beanstalk)**
   - Despliega aplicaciones de reserva de viajes sin gestionar servidores.
   - Enfoca el tiempo de desarrollo en las funciones para el cliente.
   - Escalado automático durante los períodos de mayor reserva.
   - Por qué **PaaS**: Los desarrolladores se enfocan en la lógica de la aplicación, no en la infraestructura.

2. **iRobot (Lambda, RDS)**
   - Usa Lambda para procesar datos de IoT de los dispositivos Roomba.
   - RDS para bases de datos gestionadas sin la carga de un administrador de bases de datos (DBA).
   - La arquitectura sin servidor escala con la adopción de dispositivos.
   - Por qué **PaaS**: Sin gestión de infraestructura para millones de dispositivos.

3. **Thomson Reuters (Elastic Beanstalk, RDS)**
   - Despliega aplicaciones financieras rápidamente en múltiples regiones.
   - Usa RDS para operaciones de bases de datos gestionadas.
   - Reduce el tiempo desde el desarrollo hasta la producción.
   - Por qué **PaaS**: Despliegue rápido y reducción de la carga operativa.

4. **BMW (Lambda)**
   - La plataforma de coches conectados procesa datos de telemetría.
   - La arquitectura sin servidor escala de miles a millones de solicitudes.
   - Paga solo por el uso real.
   - Por qué **PaaS**: Carga de trabajo impredecible y cero gestión de servidores.

**Cuándo Elegir PaaS:**
- Quieres enfocarte en el desarrollo de aplicaciones.
- Necesitas escalado automático.
- No quieres gestionar servidores ni tiempos de ejecución.
- El despliegue rápido es la prioridad.
- Cargas de trabajo variables o impredecibles.

### Software como Servicio (Software as a Service - SaaS)

**Definición:** Producto terminado ejecutado y gestionado por el proveedor de servicios.

**Características:**
- Aplicaciones para el usuario final.
- Tú gestionas: Acceso de usuarios, entrada de datos.
- AWS gestiona: Todo lo demás.

**Ejemplos de AWS:**
- **Amazon WorkMail** (correo electrónico y calendario)
- **Amazon Chime** (videoconferencia)
- **Amazon QuickSight** (inteligencia de negocios)
- **Amazon WorkDocs** (colaboración de documentos)
- **Amazon Connect** (centro de contacto en la nube)

**Casos de Uso:**
- Correo electrónico y colaboración.
- Análisis de negocios.
- CRM y herramientas de productividad.
- Sin deseo de gestión de TI.
- Aplicaciones de negocio estándar.

**Ejemplos de Empresas Reales:**

1. **GE (QuickSight)**
   - Paneles de análisis de negocios para ejecutivos.
   - Sin infraestructura de BI para gestionar.
   - Los usuarios simplemente inician sesión y ven informes.
   - Por qué **SaaS**: Enfocarse en los conocimientos, no en la infraestructura.

2. **Capital One (Amazon Connect)**
   - Centro de contacto basado en la nube para servicio al cliente.
   - Sin equipos de PBX ni infraestructura de telefonía.
   - Escala automáticamente con el volumen de llamadas.
   - Por qué **SaaS**: Centro de contacto moderno sin complejidad de telecomunicaciones.

3. **Lyft (WorkMail, Chime)**
   - Correo electrónico y colaboración para empleados.
   - Videoconferencia para equipos remotos.
   - Sin servidores Exchange para gestionar.
   - Por qué **SaaS**: Herramientas de negocio estándar sin carga de TI.

**Otros Ejemplos de SaaS de la Industria:**
- **Salesforce:** Plataforma de CRM utilizada por más de 150,000 empresas.
- **Microsoft 365:** Correo electrónico, aplicaciones de Office, colaboración.
- **Slack:** Comunicación y colaboración en equipo.
- **Zoom:** Videoconferencia.
- **Workday:** Gestión de RR. HH. y financiera.

**Cuándo Elegir SaaS:**
- Necesitas aplicaciones de negocio estándar.
- Quieres cero gestión de infraestructura.
- Prefieres un modelo de suscripción.
- Se requiere una incorporación rápida.
- La experiencia en aplicaciones no es la competencia principal.

> **Consejo para el Examen:** Recuerda la progresión: **IaaS** = más control, **SaaS** = menos control; **IaaS** = más responsabilidad, **SaaS** = menos responsabilidad.

> **Consejo para el Examen:** El **shared responsibility model** varía según el modelo de servicio. En IaaS gestionas más (sistema operativo, aplicaciones), en PaaS gestionas menos (solo aplicaciones), en SaaS gestionas lo menos posible (solo datos/acceso). Este concepto aparece con frecuencia en el examen.

## Modelos de Despliegue en la Nube (Cloud Deployment Models)

### Tabla Comparativa

| Característica | Cloud (Public) | Hybrid | On-Premises (Private) |
|----------------|---------------|---------|----------------------|
| **Ubicación** | Totalmente en la nube de AWS | Tanto en la nube como local | Centro de datos de la empresa |
| **Inversión Inicial** | Ninguna | Media | Alta |
| **Modelo de Precios** | **Pay-as-you-go** | Mixto | **CapEx** + **OpEx** |
| **Escalabilidad** | Ilimitada | Limitada por lo local | Limitada por el hardware |
| **Mantenimiento** | AWS gestiona | Compartido | El cliente gestiona |
| **Tiempo de Despliegue** | Minutos | Días a semanas | Semanas a meses |
| **Control** | AWS controla la infraestructura | Control dividido | Control total |
| **Ideal Para** | Nuevas aplicaciones, startups | Migración gradual | Restricciones regulatorias |
| **Conectividad** | Internet | **VPN** / **Direct Connect** | Red interna |
| **Alcance Geográfico** | Global (33+ regiones) | Limitado | Ubicación única |

### Cloud (Public Cloud)

**Características:**
- Totalmente desplegada en la nube.
- Todas las partes de la aplicación se ejecutan en la nube.
- Aplicaciones construidas en la nube o migradas.
- Puede construirse sobre infraestructura de bajo nivel o servicios de alto nivel.

**Ventajas:**
- Sin inversión inicial.
- Precios de pago por uso.
- Escalable y fiable.
- Sin mantenimiento.
- Alcance global.
- Última tecnología siempre disponible.

**Desventajas:**
- Menos control sobre la infraestructura física.
- Requiere conectividad a Internet.
- Puede no cumplir con todos los requisitos de cumplimiento.

**Ejemplos:**
- Startup que construye totalmente sobre AWS.
- Aplicación SaaS.
- Backend de aplicación móvil.
- Plataforma de streaming de Netflix.
- Sistema de reservas de Airbnb.

**Ideal Para:**
- Startups con capital limitado.
- Aplicaciones con cargas de trabajo variables.
- Aplicaciones globales.
- Requisitos de innovación rápida.
- Empresas sin restricciones de residencia de datos.

### Hybrid

**Características:**
- Conecta los recursos de la nube con la infraestructura local.
- Integra la nube con la infraestructura existente.
- Útil para aplicaciones heredadas.
- Modelo de despliegue común para muchas empresas.

**Métodos de Conexión:**
- **AWS Direct Connect**: Conexión de red dedicada (1 Gbps o 10 Gbps).
- **AWS VPN**: Conexión cifrada a través de Internet.
- **AWS Storage Gateway**: Integración de almacenamiento híbrido.
- **AWS Outposts**: Infraestructura de AWS en las instalaciones locales.

**Ventajas:**
- Flexibilidad para usar ambos entornos.
- Ruta de migración gradual.
- Mantener los datos sensibles localmente.
- Aprovechar las inversiones existentes.
- Cumplir con los requisitos de cumplimiento.
- Capacidad de "**cloud bursting**" (desbordamiento a la nube).

**Desventajas:**
- Más complejo de gestionar.
- Latencia de red entre entornos.
- Requiere experiencia en ambos modelos.
- Posibles brechas de seguridad si no se configura correctamente.

**Casos de Uso:**
- Migración gradual a la nube.
- Requisitos de cumplimiento (por ejemplo, residencia de datos).
- Extensión de la capacidad local.
- Recuperación ante desastres.
- Aplicaciones sensibles a la latencia.
- Integración de sistemas heredados.

**Ejemplos:**
- Empresa que mantiene las bases de datos localmente y el cómputo en la nube.
- Copia de seguridad y recuperación ante desastres en S3/Glacier.
- Desbordamiento a la nube para cargas máximas.
- Servicios financieros con restricciones regulatorias.
- Atención médica con requisitos de datos de pacientes.

**Ejemplo de Empresa Real:**
**General Electric (GE):** Utiliza la nube híbrida para mantener los datos de fabricación patentados localmente mientras ejecuta análisis y aplicaciones orientadas al cliente en AWS. Esto les permite cumplir con los requisitos de seguridad al tiempo que obtienen los beneficios de la nube.

**Ideal Para:**
- Grandes empresas con infraestructura existente.
- Industrias reguladas (finanzas, salud).
- Empresas con requisitos de soberanía de datos.
- Organizaciones con inversiones significativas en **CapEx**.
- Estrategias de adopción de nube gradual.

### On-Premises (Private Cloud)

**Características:**
- Recursos desplegados utilizando herramientas de virtualización y gestión de recursos.
- A veces llamada "**private cloud**".
- Utiliza **AWS Outposts** para la infraestructura de AWS en las instalaciones locales.
- Mayor utilización de recursos en comparación con lo local tradicional.
- Control total sobre el hardware y los datos.

**Ventajas:**
- Control total sobre la infraestructura.
- Los datos nunca salen de las instalaciones.
- Puede cumplir con requisitos de cumplimiento estrictos.
- Rendimiento predecible.
- Sin dependencia de Internet.

**Desventajas:**
- Alto **CapEx** inicial.
- Escalabilidad limitada.
- Requiere personal y experiencia en TI.
- El cliente es responsable del mantenimiento.
- Mayor tiempo para desplegar nuevos recursos.
- Difícil de lograr redundancia geográfica.

**Casos de Uso:**
- Requisitos regulatorios estrictos.
- Necesidad de control total de los datos.
- Restricciones de sistemas heredados.
- Requisitos de latencia muy baja.
- Aplicaciones de gobierno/defensa.

**Solución de AWS:**
**AWS Outposts**: Lleva la infraestructura, los servicios y las herramientas de AWS a las instalaciones locales.
- Mismas API y herramientas de AWS.
- Gestionado por AWS.
- Se conecta a una región de AWS.
- Disponible en varias configuraciones.

**Ejemplos:**
- Agencias gubernamentales con datos clasificados.
- Bancos con sistemas mainframe heredados.
- Atención médica con requisitos de HIPAA.
- Empresas con leyes de soberanía de datos.

**Ideal Para:**
- Organizaciones con leyes estrictas de residencia de datos.
- Industrias con restricciones de cumplimiento.
- Aplicaciones que requieren una latencia ultra baja.
- Entornos con conectividad a Internet limitada.
- Empresas que no están listas para la migración a la nube.

> **Consejo para el Examen:** Conoce las diferencias entre los modelos de despliegue. **Cloud** = totalmente AWS, **Hybrid** = mezcla de nube y local, **On-premises** = todo en tu centro de datos (o AWS Outposts).

## AWS Well-Architected Framework

El **AWS Well-Architected Framework** describe conceptos clave, principios de diseño y mejores prácticas arquitectónicas para diseñar y ejecutar cargas de trabajo en la nube.

### Los Seis Pilares

#### 1. Excelencia Operativa (Operational Excellence)

**Enfoque:** Ejecutar y monitorizar sistemas para entregar valor de negocio.

**Principios de Diseño (Detallados):**

1. **Realizar operaciones como código (Perform operations as code)**
   - Define toda la carga de trabajo como código (infraestructura, configuración, etc.).
   - Limita el error humano automatizando las operaciones.
   - Usa el control de versiones para todos los procedimientos operativos.
   - Permite respuestas consistentes a los eventos.

2. **Documentación anotada (Annotate documentation)**
   - Crea automáticamente documentación a partir del código.
   - Mantén la documentación sincronizada con los cambios.
   - Haz que la documentación sea accesible para los equipos.
   - Usa el etiquetado para la facilidad de búsqueda.

3. **Realizar cambios pequeños, frecuentes y reversibles (Make frequent, small, reversible changes)**
   - Diseña las cargas de trabajo para permitir actualizaciones de componentes.
   - Realiza cambios en pequeños incrementos.
   - Permite la reversión (**rollback**) si los cambios fallan.
   - Prueba los procedimientos de reversión regularmente.

4. **Refinar los procedimientos de operaciones con frecuencia (Refine operations procedures frequently)**
   - Usa "**game days**" para probar los procedimientos.
   - Aprende de los eventos operativos.
   - Actualiza los procedimientos basados en las lecciones aprendidas.
   - Comparte el conocimiento entre los equipos.

5. **Anticiparse al fallo (Anticipate failure)**
   - Realiza ejercicios de "**pre-mortem**".
   - Identifica los posibles puntos de fallo.
   - Elimina o mitiga los puntos de fallo.
   - Prueba los escenarios de fallo regularmente.

6. **Aprender de todos los fallos operativos (Learn from all operational failures)**
   - Comparte las lecciones aprendidas.
   - Realiza mejoras de forma iterativa.
   - Implementa medidas preventivas.
   - Rastrea las tendencias en los fallos.

**Servicios Clave:**
- **AWS CloudFormation**: Infraestructura como código (**IaC**).
- **AWS Config**: Rastrea los cambios de configuración.
- **AWS CloudTrail**: Audita las llamadas a la API.
- **Amazon CloudWatch**: Monitorización y registro.
- **AWS Systems Manager**: Información operativa y automatización.
- **AWS X-Ray**: Análisis del rendimiento de la aplicación.

**Ejemplo de Buena Arquitectura:**
Una empresa utiliza plantillas de **CloudFormation** almacenadas en Git para toda la infraestructura. Cuando un desarrollador necesita un nuevo entorno, envía una solicitud de cambio (**pull request**) con los cambios en la plantilla. Tras la aprobación, una canalización de **CI/CD** aprovisiona automáticamente el entorno. Todos los cambios se registran en **CloudTrail** y las alarmas de **CloudWatch** notifican a los equipos sobre los problemas. Los **game days** mensuales prueban los procedimientos de recuperación ante desastres.

**Ejemplo de Mala Arquitectura (Anti-pattern):**
Una empresa configura manualmente los servidores a través de la consola. La documentación está en un documento de Word compartido que a menudo está desactualizado. Cuando ocurren problemas, los administradores entran en los servidores por **SSH** y realizan cambios directamente. No hay registros que rastreen qué cambios se hicieron o por qué. Cuando el administrador se va, el conocimiento se pierde.

**Anti-patterns Comunes a Evitar:**
- Cambios manuales en producción (**click-ops**).
- Procesos no documentados.
- Falta de monitorización y alertas.
- Sin manuales de ejecución (**runbooks**) para problemas comunes.
- No aprender de los incidentes.
- Cambios grandes y poco frecuentes.

**Mejores Prácticas:**
- Automatiza todo lo posible.
- Control de versiones para todas las configuraciones.
- Implementa una monitorización integral.
- Crea y mantén **runbooks**.
- Realiza **game days** regulares.
- Realiza revisiones post-incidente.
- Usa estrategias de etiquetado de forma consistente.
- Habilita el registro detallado.

**Preguntas del Examen:**
- "¿Cómo aseguramos que nuestras operaciones sean eficientes?"
- "¿Cómo apoyamos el desarrollo y ejecutamos las cargas de trabajo de manera efectiva?"
- "¿Qué servicio te permite rastrear las llamadas a la API con fines de auditoría?" (**CloudTrail**)
- "¿Qué permite la infraestructura como código?" (**CloudFormation**)

#### 2. Seguridad (Security)

**Enfoque:** Proteger la información, los sistemas y los activos.

**Principios de Diseño (Detallados):**

1. **Implementar una base de identidad sólida (Implement a strong identity foundation)**
   - Usa el principio de mínimo privilegio (**least privilege**).
   - Centraliza la gestión de identidades.
   - Elimina las credenciales a largo plazo.
   - Aplica la autenticación de múltiples factores (**MFA**).
   - Separa las tareas con diferentes roles.

2. **Habilitar la trazabilidad (Enable traceability)**
   - Monitoriza todas las acciones y cambios.
   - Integra los registros con respuestas automatizadas.
   - Genera pistas de auditoría.
   - Investiga y responde a los eventos.

3. **Aplicar seguridad en todas las capas (Apply security at all layers)**
   - Enfoque de defensa en profundidad (**defense in depth**).
   - Asegura la VPC, la subred, el equilibrador de carga, la instancia, la aplicación.
   - Múltiples controles de seguridad en cada capa.
   - Usa **Security Groups** y **NACLs**.

4. **Automatizar las mejores prácticas de seguridad (Automate security best practices)**
   - Despliegue de software automatizado.
   - Crea arquitecturas seguras a través del código.
   - Define y gestiona los controles como código.
   - Responde a los eventos de seguridad automáticamente.

5. **Proteger los datos en tránsito y en reposo (Protect data in transit and at rest)**
   - Clasifica los datos por sensibilidad.
   - Cifra todo.
   - Usa claves de cifrado que tú controles (**KMS**).
   - Reduce el acceso directo a los datos.

6. **Mantener a las personas alejadas de los datos (Keep people away from data)**
   - Usa herramientas y automatización.
   - Reduce o elimina la necesidad de acceso directo.
   - Proporciona paneles en lugar de acceso a datos sin procesar.
   - Usa mecanismos (**IAM roles**) no credenciales.

7. **Prepararse para eventos de seguridad (Prepare for security events)**
   - Ten un plan de respuesta a incidentes.
   - Realiza **game days** para eventos de seguridad.
   - Usa herramientas automatizadas para detectar y responder.
   - Preaprovisiona herramientas y acceso.

**Servicios Clave:**
- **AWS IAM**: Gestión de Identidades y Accesos.
- **AWS Organizations**: Gestión de múltiples cuentas.
- **AWS KMS**: Servicio de gestión de claves para el cifrado.
- **AWS Shield**: Protección contra DDoS.
- **Amazon GuardDuty**: Detección de amenazas.
- **AWS WAF**: Cortafuegos de aplicaciones web.
- **AWS Secrets Manager**: Rota y gestiona secretos.
- **Amazon Inspector**: Evaluaciones de seguridad.
- **AWS Security Hub**: Vista central de seguridad.

**Ejemplo de Buena Arquitectura:**
Una aplicación financiera utiliza **IAM roles** (sin credenciales a largo plazo), requiere **MFA** para todos los usuarios, cifra todos los datos con **KMS**, almacena los registros en **S3** con **CloudTrail** habilitado, utiliza **GuardDuty** para la detección de amenazas y tiene habilitados los **VPC Flow Logs**. Los **Security Groups** siguen el mínimo privilegio. Un equipo de respuesta a incidentes practica game days de seguridad trimestrales.

**Ejemplo de Mala Arquitectura (Anti-pattern):**
Una aplicación utiliza credenciales de la cuenta raíz (**root account**) codificadas en las aplicaciones, no tiene **MFA** habilitado, almacena datos sin cifrar, abre los grupos de seguridad a 0.0.0.0/0 (todo Internet), no tiene habilitado el registro y el equipo nunca ha probado los procedimientos de respuesta a incidentes.

**Anti-patterns Comunes a Evitar:**
- Usar la cuenta raíz para las operaciones diarias.
- Compartir credenciales de usuario de IAM.
- **Security Groups** demasiado permisivos (0.0.0.0/0).
- Sin cifrado de datos sensibles.
- Credenciales codificadas en el código.
- Sin registro ni monitorización.
- Capa única de seguridad.
- Sin plan de respuesta a incidentes.

**Mejores Prácticas:**
- Habilita **MFA** en todas las cuentas, especialmente en la raíz.
- Usa **IAM roles** en lugar de claves de acceso.
- Rota las credenciales regularmente.
- Cifra los datos en reposo y en tránsito.
- Usa el acceso de **least privilege**.
- Habilita **CloudTrail** en todas las regiones.
- Implementa controles detectives (**GuardDuty**).
- Evaluaciones de seguridad regulares.
- Usa **AWS Organizations** para la gobernanza.

**Preguntas del Examen:**
- "¿Cómo controlamos el acceso a los recursos?" (**IAM**, **Security Groups**)
- "¿Cómo protegemos nuestros datos?" (Cifrado con **KMS**)
- "¿Qué servicio proporciona detección de amenazas?" (**GuardDuty**)
- "¿Cómo te proteges contra los ataques DDoS?" (**AWS Shield**)

#### 3. Fiabilidad (Reliability)

**Enfoque:** Asegurar que la carga de trabajo realice su función prevista de manera correcta y consistente.

**Principios de Diseño (Detallados):**

1. **Recuperarse automáticamente de los fallos (Automatically recover from failure)**
   - Monitoriza los KPI y activa la automatización.
   - Define umbrales para las acciones de recuperación.
   - Usa **Auto Scaling** y **health checks**.
   - Reemplaza los componentes fallidos automáticamente.
   - Arquitectura de auto-reparación.

2. **Probar los procedimientos de recuperación (Test recovery procedures)**
   - Usa la automatización para simular fallos.
   - Prueba la restauración de copias de seguridad regularmente.
   - Practica la conmutación por error al sitio de **DR**.
   - Usa principios de ingeniería del caos (**chaos engineering**).
   - Valida los objetivos de tiempo de recuperación (**RTO**).

3. **Escalar horizontalmente para aumentar la disponibilidad agregada (Scale horizontally to increase aggregate availability)**
   - Distribuye la carga entre múltiples recursos más pequeños.
   - Reduce el impacto de un solo fallo.
   - Usa múltiples **Availability Zones**.
   - Sin puntos únicos de fallo (**single points of failure**).
   - Aplicaciones sin estado (**stateless**) cuando sea posible.

4. **Dejar de adivinar la capacidad (Stop guessing capacity)**
   - Monitoriza la demanda y el uso.
   - Automatiza el escalado basado en métricas.
   - Usa grupos de **Auto Scaling**.
   - Planifica para la capacidad máxima.
   - Prueba a escala de producción.

5. **Gestionar el cambio mediante la automatización (Manage change in automation)**
   - Usa **Infrastructure as Code**.
   - Despliega a través de canalizaciones de **CI/CD**.
   - Automatiza el seguimiento de los cambios.
   - Revierte los cambios fallidos automáticamente.
   - Despliegues **blue/green** o **canary**.

**Servicios Clave:**
- **Amazon RDS Multi-AZ**: Bases de datos de alta disponibilidad.
- **AWS Auto Scaling**: Escalado automático.
- **Amazon CloudWatch**: Monitorización y alarmas.
- **AWS Backup**: Copia de seguridad centralizada.
- **Amazon Route 53**: DNS con comprobaciones de estado.
- **Elastic Load Balancing (ELB)**: Distribuye el tráfico.
- **Amazon S3**: Durabilidad del 99.999999999% (11 nueves).
- **AWS Elastic Disaster Recovery**: Solución de **DR**.

**Ejemplo de Buena Arquitectura:**
Un sitio de comercio electrónico se ejecuta en 3 **Availability Zones** con grupos de **Auto Scaling**. Los equilibradores de carga (**Load Balancers**) distribuyen el tráfico con comprobaciones de estado (**health checks**). **RDS** utiliza **Multi-AZ** para la conmutación por error automática. Activos estáticos en S3 con CDN de **CloudFront**. Copias de seguridad automatizadas regulares probadas mensualmente. Las alarmas de **CloudWatch** se activan cuando aumentan las tasas de error. La aplicación puede manejar la pérdida de una **AZ** completa.

**Ejemplo de Mala Arquitectura (Anti-pattern):**
Una aplicación se ejecuta en una sola instancia **EC2** en una **Availability Zone**. La base de datos está en la misma instancia. Sin copias de seguridad. Sin comprobaciones de estado. Sin monitorización. Cuando la instancia falla (fallo de hardware), toda la aplicación cae. La recuperación requiere intervención manual y los datos pueden perderse.

**Anti-patterns Comunes a Evitar:**
- **Single point of failure**.
- Ejecución en una sola **Availability Zone**.
- Sin copias de seguridad automatizadas.
- Recuperación ante desastres no probada.
- Escalado manual.
- Instancias con estado que no pueden ser reemplazadas.
- Sin comprobaciones de estado.
- Ignorar las alertas de monitorización.

**Mejores Prácticas:**
- Despliega en múltiples **Availability Zones**.
- Usa grupos de **Auto Scaling**.
- Implementa comprobaciones de estado (**health checks**).
- Habilita copias de seguridad automatizadas.
- Prueba los procedimientos de recuperación regularmente.
- Usa servicios gestionados (**RDS**, **ELB**, etc.).
- Monitoriza las métricas clave con alarmas.
- Diseña para el fallo (**design for failure**).
- Usa comprobaciones de estado de **Route 53**.
- Implementa interruptores (**circuit breakers**).

**Preguntas del Examen:**
- "¿Cómo aseguramos que nuestra aplicación pueda recuperarse de los fallos?" (**Multi-AZ**, **Auto Scaling**)
- "¿Cómo cumplimos con los requisitos de disponibilidad?" (Múltiples **AZ**, **Load Balancing**)
- "¿Qué proporciona un 99.99% de disponibilidad para las bases de datos?" (**RDS Multi-AZ**)
- "¿Qué servicio distribuye el tráfico entre las instancias?" (**Elastic Load Balancing**)

#### 4. Eficiencia del Rendimiento (Performance Efficiency)

**Enfoque:** Usar los recursos de cómputo de manera eficiente para cumplir con los requisitos.

**Principios de Diseño (Detallados):**

1. **Democratizar las tecnologías avanzadas (Democratize advanced technologies)**
   - Usa servicios gestionados para tecnologías complejas.
   - Deja que AWS se encargue del "**undifferentiated heavy lifting**".
   - Las bases de datos NoSQL, el aprendizaje automático, etc., se convierten en servicios.
   - Enfócate en el desarrollo del producto, no en la implementación de la tecnología.
   - Los equipos pueden aprovechar tecnología de vanguardia sin una experiencia profunda.

2. **Globalizarse en minutos (Go global in minutes)**
   - Despliega en múltiples regiones fácilmente.
   - Reduce la latencia para los usuarios globales.
   - Usa **CloudFront** para la entrega de contenido.
   - Arquitecturas multiregión.
   - Atiende a los usuarios desde la ubicación más cercana.

3. **Usar arquitecturas sin servidor (Use serverless architectures)**
   - Elimina la carga operativa.
   - Sin gestión de servidores.
   - Escalado automático.
   - Paga solo por el valor.
   - **Lambda**, **Fargate**, **S3**, **DynamoDB**.

4. **Experimentar con más frecuencia (Experiment more often)**
   - Fácil de probar diferentes configuraciones.
   - Prueba diferentes tipos de instancia.
   - Usa pruebas comparativas.
   - Bajo coste de experimentación.
   - Cambia rápidamente basándote en los resultados.

5. **Tener en cuenta la afinidad mecánica (Consider mechanical sympathy)**
   - Entiende cómo funcionan los servicios de nube.
   - Usa los servicios para el propósito previsto.
   - Elige la herramienta adecuada para el trabajo.
   - Entiende las características del rendimiento.
   - Alinea la tecnología con las necesidades del negocio.

**Servicios Clave:**
- **AWS Lambda**: Cómputo sin servidor (**Serverless**).
- **Amazon EBS**: Almacenamiento de bloques con varios tipos (**gp3**, **io2**, etc.).
- **Amazon RDS**: Bases de datos gestionadas.
- **AWS Auto Scaling**: Ajuste de tamaño y escalado.
- **Amazon CloudFront**: **CDN** global.
- **Amazon ElastiCache**: Almacenamiento en caché en memoria.
- **AWS Compute Optimizer**: Recomendaciones de optimización de recursos.

**Ejemplo de Buena Arquitectura:**
Una aplicación utiliza el **CDN** de **CloudFront** para el contenido estático a nivel mundial. El cómputo utiliza instancias **EC2** del tamaño adecuado basadas en las recomendaciones de **Compute Optimizer**. La base de datos utiliza **RDS** con el tipo de instancia adecuado para la carga de trabajo. **ElastiCache** reduce la carga de la base de datos. **Lambda** gestiona las tareas en segundo plano. Las pruebas de rendimiento regulares identifican los cuellos de botella.

**Ejemplo de Mala Arquitectura (Anti-pattern):**
Todas las cargas de trabajo se ejecutan en instancias de propósito general sobreaprovisionadas "para estar seguros". No hay almacenamiento en caché implementado. Archivos estáticos servidos desde los servidores de aplicaciones. La base de datos se ejecuta en la misma instancia que la aplicación. Sin monitorización del rendimiento. Todos los usuarios globales dirigidos a una sola región.

**Anti-patterns Comunes a Evitar:**
- Tipos de instancia únicos para todo.
- Sin estrategia de almacenamiento en caché.
- Servir contenido estático desde el cómputo.
- No usar **CDN** para usuarios globales.
- Sobreaprovisionamiento "para estar seguros".
- Ignorar las métricas de rendimiento.
- No probar nunca diferentes configuraciones.
- Usar base de datos relacional para todo.

**Mejores Prácticas:**
- Usa **CloudFront** para la entrega de contenido.
- Implementa el almacenamiento en caché (**ElastiCache**, **CloudFront**).
- Elige los tipos de instancia adecuados para la carga de trabajo.
- Usa **Auto Scaling** para cargas variables.
- Aprovecha lo **serverless** donde sea apropiado.
- Monitoriza las métricas de rendimiento.
- Pruebas de rendimiento regulares.
- Usa servicios gestionados.
- Despliega en múltiples regiones.
- Usa la base de datos adecuada para el trabajo.

**Preguntas del Examen:**
- "¿Cómo seleccionamos los tipos de recursos adecuados?" (Basado en los requisitos de la carga de trabajo)
- "¿Cómo aseguramos que usamos los recursos de manera eficiente?" (Monitorización, ajuste de tamaño)
- "¿Qué servicio reduce la latencia para los usuarios globales?" (**CloudFront**)
- "¿Qué es el cómputo sin servidor?" (**Lambda**)

#### 5. Optimización de Costes (Cost Optimization)

**Enfoque:** Ejecutar sistemas para entregar valor de negocio al precio más bajo.

**Principios de Diseño (Detallados):**

1. **Implementar la gestión financiera de la nube (Implement cloud financial management)**
   - Establece la responsabilidad de los costes.
   - Define etiquetas de asignación de costes (**Cost allocation tags**).
   - Usa la detección de anomalías de costes.
   - Revisiones de costes regulares.
   - Construye una cultura consciente de los costes.

2. **Adoptar un modelo de consumo (Adopt a consumption model)**
   - Paga solo por lo que usas.
   - Escala los recursos con la demanda.
   - Sin compromisos iniciales para cargas de trabajo variables.
   - Usa **Auto Scaling**.
   - Elimina los recursos no utilizados.

3. **Medir la eficiencia general (Measure overall efficiency)**
   - Mide la producción del negocio frente al coste.
   - Usa métricas para rastrear la eficiencia.
   - Establece objetivos de coste por transacción.
   - Comparativa con la industria.
   - Optimiza basado en las mediciones.

4. **Dejar de gastar dinero en tareas pesadas no diferenciadas (Stop spending money on undifferentiated heavy lifting)**
   - Usa servicios gestionados.
   - Reduce la carga operativa.
   - Enfócate en el valor del negocio.
   - Deja que AWS gestione la infraestructura.
   - Reduce los costes de personal.

5. **Analizar y atribuir el gasto (Analyze and attribute expenditure)**
   - Etiqueta todos los recursos.
   - Rastrea los costes por proyecto/equipo.
   - Usa **Cost Explorer**.
   - Implementa el contracargo (**chargeback**).
   - Entiende a dónde va el dinero.

**Servicios Clave:**
- **AWS Cost Explorer**: Analiza y visualiza el gasto.
- **AWS Budgets**: Establece alertas personalizadas de coste y uso.
- **Reserved Instances (RI)**: Hasta un 75% de ahorro para cargas de trabajo estables.
- **Savings Plans**: Precios flexibles para el cómputo.
- **AWS Trusted Advisor**: Recomendaciones de optimización de costes.
- **AWS Compute Optimizer**: Recomendaciones de ajuste de tamaño (**right-sizing**).
- **Amazon S3 Intelligent-Tiering**: Optimización automática del almacenamiento.
- **AWS Cost Anomaly Detection**: Alerta sobre gastos inusuales.

**Ejemplo de Buena Arquitectura:**
La empresa utiliza **Reserved Instances** para la carga base y **On-Demand** para la capacidad variable. Todos los recursos están etiquetados por proyecto y entorno. **Auto Scaling** ajusta la capacidad cada hora. S3 utiliza **Intelligent-Tiering** y políticas de ciclo de vida. **Cost Explorer** se revisa semanalmente. Los presupuestos alertan a los equipos cuando se alcanza el 80% del presupuesto mensual. Las instantáneas (**snapshots**) y volúmenes antiguos se eliminan automáticamente.

**Ejemplo de Mala Arquitectura (Anti-pattern):**
La empresa ejecuta entornos de desarrollo y prueba 24/7. Sin etiquetado de recursos. Usa precios de **On-Demand** para todo a pesar de tener una base predecible. Nunca revisa las facturas. Los recursos huérfanos se acumulan. Los volúmenes de EBS y las instantáneas antiguas nunca se eliminan. Instancias sobreaprovisionadas. Sin **Auto Scaling**. Nadie es responsable de los costes.

**Anti-patterns Comunes a Evitar:**
- Ejecutar dev/test 24/7.
- No usar **Reserved Instances** o **Savings Plans**.
- Recursos huérfanos (EBS, instantáneas, IP elásticas).
- Instancias sobreaprovisionadas.
- Sin monitorización de costes ni presupuestos.
- Etiquetas de recursos faltantes.
- No usar **Auto Scaling**.
- Mantener copias de seguridad antiguas para siempre.
- Usar almacenamiento caro para acceso poco frecuente.

**Mejores Prácticas:**
- Usa **Reserved Instances** o **Savings Plans** para cargas de trabajo estables.
- Implementa **Auto Scaling**.
- Programa los entornos de dev/test (parar noches/fines de semana).
- Usa políticas de ciclo de vida de S3.
- Elimina recursos no utilizados.
- Ajusta el tamaño de las instancias basándote en las métricas (**right-sizing**).
- Usa **Spot Instances** para cargas de trabajo tolerantes a fallos.
- Etiqueta todos los recursos.
- Configura alertas de costes.
- Revisiones de costes regulares.
- Usa la región más barata cuando sea posible.
- Archiva los datos antiguos en **Glacier**.

**Preguntas del Examen:**
- "¿Cómo reducimos los costes sin impactar en el rendimiento?" (**Right-sizing**, **Reserved Instances**)
- "¿Cómo hacemos coincidir la capacidad con la demanda?" (**Auto Scaling**)
- "¿Qué servicio proporciona recomendaciones de optimización de costes?" (**Trusted Advisor**)
- "¿Cómo puedes ahorrar hasta un 75% en el cómputo?" (**Reserved Instances**)

#### 6. Sostenibilidad (Sustainability)

**Enfoque:** Minimizar los impactos ambientales de la ejecución de las cargas de trabajo en la nube.

**Principios de Diseño (Detallados):**

1. **Comprender el impacto (Understand your impact)**
   - Mide la huella de carbono.
   - Rastrea el uso de recursos.
   - Usa la herramienta **AWS Customer Carbon Footprint Tool**.
   - Establece métricas de línea base.
   - Monitoriza las mejoras.

2. **Establecer objetivos de sostenibilidad (Establish sustainability goals)**
   - Establece objetivos de reducción específicos.
   - Alínealos con los objetivos del negocio.
   - Rastrea el progreso regularmente.
   - Informa sobre los logros.
   - Mejora continua.

3. **Maximizar la utilización (Maximize utilization)**
   - Ajusta el tamaño de las cargas de trabajo (**right-size**).
   - Elimina los recursos inactivos.
   - Usa **Auto Scaling**.
   - Aumenta la utilización de los servidores.
   - Consolida las cargas de trabajo.

4. **Anticipar y adoptar hardware y software nuevos y más eficientes (Anticipate and adopt new, more efficient hardware and software)**
   - Usa los últimos tipos de instancia (por ejemplo, procesadores **Graviton**).
   - Adopta servicios gestionados con mejoras de eficiencia.
   - Mantente al día con las innovaciones de AWS.
   - Prueba nuevas tecnologías.
   - Migra a opciones más eficientes.

5. **Usar servicios gestionados (Use managed services)**
   - La infraestructura compartida reduce los gastos generales.
   - AWS opera a escala de manera eficiente.
   - Reduce los recursos redundantes.
   - Mejoras de eficiencia automáticas.
   - Menos desperdicio de recursos.

6. **Reducir el impacto descendente (Reduce downstream impact)**
   - Minimiza la transferencia de datos.
   - Usa el almacenamiento en caché para reducir el cómputo.
   - Optimiza la experiencia del usuario para reducir el uso.
   - Elige regiones eficientes.
   - Archiva los datos no utilizados.

**Servicios Clave:**
- **Amazon EC2 Auto Scaling**: Optimiza el uso de recursos.
- **AWS Lambda**: Lo sin servidor reduce el desperdicio.
- **Amazon S3 Intelligent-Tiering**: Optimización automática del almacenamiento.
- **Procesadores AWS Graviton**: Chips basados en ARM más eficientes.
- **AWS Customer Carbon Footprint Tool**: Mide las emisiones.
- **Amazon EFS Intelligent-Tiering**: Optimiza el almacenamiento de archivos.

**Ejemplo de Buena Arquitectura:**
La empresa utiliza instancias basadas en **Graviton** para obtener un mejor rendimiento por vatio. **Auto Scaling** asegura que no haya capacidad inactiva. Los entornos de desarrollo se apagan automáticamente por la noche. Se usa **Lambda** en lugar de servidores siempre encendidos. **S3 Intelligent-Tiering** mueve los datos a clases de almacenamiento eficientes. **CloudFront** reduce las solicitudes al origen. Las revisiones regulares optimizan el uso de los recursos.

**Ejemplo de Mala Arquitectura (Anti-pattern):**
La empresa ejecuta instancias de gran tamaño con una utilización del 10% las 24 horas del día. Los entornos de desarrollo nunca se apagan. Sin **Auto Scaling**. Usa tipos de instancia más antiguos. Sin políticas de ciclo de vida de datos. Datos redundantes almacenados en múltiples regiones innecesariamente. Sin monitorización de la eficiencia de los recursos.

**Anti-patterns Comunes a Evitar:**
- Recursos sobreaprovisionados.
- Operación 24/7 de dev/test.
- Uso de tipos de instancia obsoletos.
- Sin monitorización de la utilización.
- Mantener todos los datos en el almacenamiento más caro.
- Ignorar las recomendaciones de eficiencia.
- Sin **Auto Scaling**.

**Mejores Prácticas:**
- Usa instancias **AWS Graviton** donde sea posible.
- Implementa **Auto Scaling**.
- Programa las cargas de trabajo que no son de producción.
- Ajusta el tamaño basándote en el uso real.
- Usa lo **serverless** (**Lambda**) cuando sea apropiado.
- Archiva los datos antiguos (**S3 Glacier**).
- Elige regiones eficientes.
- Usa servicios gestionados.
- Monitoriza las métricas de utilización.
- Revisiones de eficiencia regulares.

**Preguntas del Examen:**
- "¿Cómo minimizamos el impacto ambiental?" (Optimizar la utilización, usar servicios gestionados)
- "¿Cómo optimizamos el uso de los recursos?" (**Auto Scaling**, **right-sizing**)
- "¿Qué tipo de procesador es más eficiente?" (**Graviton**)
- "¿Qué reduce el impacto ambiental del almacenamiento?" (**S3 Intelligent-Tiering**)

> **Punto Clave:** El **Well-Architected Framework** se prueba con frecuencia en el examen. Comprende el propósito de cada pilar y los servicios clave.

> **Consejo para el Examen:** Para las preguntas del examen sobre el Well-Architected Framework, busca palabras clave: **Operational Excellence** (automatización, IaC), **Security** (IAM, cifrado), **Reliability** (**Multi-AZ**, copias de seguridad), **Performance** (tipo de instancia correcto, almacenamiento en caché), **Cost** (**Reserved Instances**, etiquetado), **Sustainability** (utilización, recursos eficientes).

### AWS Well-Architected Tool

- Servicio gratuito en la consola de AWS.
- Revisa la arquitectura de la carga de trabajo.
- Compara con las mejores prácticas.
- Obtén recomendaciones de mejora.
- Se recomiendan revisiones periódicas.

## Economía de la Nube

### Coste Total de Propiedad (Total Cost of Ownership - TCO)

**Definición:** Estimación financiera para identificar los costes directos e indirectos.

**Componentes:**
- **Server costs**: Compra de hardware y depreciación.
- **Storage costs**: Hardware de almacenamiento y mantenimiento.
- **Network costs**: Equipos de red y ancho de banda.
- **IT labor costs**: Personal para gestionar la infraestructura.
- **Facility costs**: Energía, refrigeración, alquiler de espacio.

**Costes Ocultos en Local:**
- Ciclos de actualización de hardware.
- Sobreaprovisionamiento para la capacidad máxima.
- Infraestructura de recuperación ante desastres.
- Seguridad física.
- Cumplimiento y auditoría.

**Ventajas de AWS:**
- Sin costes iniciales de hardware.
- Paga solo por lo que usas.
- No se necesita sobreaprovisionamiento.
- Opciones de recuperación ante desastres integradas.
- AWS gestiona los costes de las instalaciones.

**AWS TCO Calculator (ahora parte de AWS Pricing Calculator):**
- Ayuda a estimar el ahorro de costes.
- Compara lo local con AWS.
- Proporciona un desglose detallado de los costes.

### Gastos de Capital (CapEx) frente a Gastos Operativos (OpEx)

#### CapEx (Local)
- **Definición:** Compra inicial de infraestructura física.
- **Características:**
  - Coste fijo, hundido.
  - Se deprecia con el tiempo.
  - Requiere planificación de capacidad.
  - Ciclos de adquisición largos.
  - Difícil de ajustar.

- **Ejemplos:**
  - Compra de servidores.
  - Construcción de centros de datos.
  - Equipos de red.
  - Matrices de almacenamiento.

#### OpEx (Nube)
- **Definición:** Paga por lo que usas.
- **Características:**
  - Coste variable basado en el consumo.
  - Sin compromiso inicial.
  - Escala con las necesidades del negocio.
  - Presupuestación más fácil.
  - Ventajas fiscales.

- **Ejemplos:**
  - Facturas mensuales de AWS.
  - Servicios de pago por uso.
  - Modelos de suscripción.

> **Consejo para el Examen:** **Cloud Computing** desplaza el gasto en TI de **CapEx** a **OpEx**, proporcionando más flexibilidad y una mejor alineación con las necesidades del negocio.

### Estrategias de Migración (Migration Strategies - Las 6 R)

Comprender estas estrategias te ayuda a elegir el enfoque adecuado para la migración a la nube.

#### 1. Rehosting (Mover y Cambiar - Lift and Shift)

**Descripción:** Mover aplicaciones sin cambios.

**Flujo de Trabajo de Migración Paso a Paso:**

1. **Fase de Descubrimiento**
   - Inventariar todos los servidores y dependencias.
   - Documentar las configuraciones actuales.
   - Identificar las interconexiones de las aplicaciones.
   - Evaluar la utilización actual de los recursos.

2. **Fase de Planificación**
   - Mapear el origen al destino (servidor a instancia EC2).
   - Elegir regiones de AWS.
   - Planificar la configuración de red (VPC, subredes).
   - Crear el cronograma de migración.

3. **Fase de Migración**
   - Usar **AWS Application Migration Service (MGN)**.
   - Replicar los datos del servidor a AWS.
   - Probar en el entorno de AWS.
   - Programar la ventana de transición (**cutover**).
   - Redirigir el tráfico a AWS.

4. **Fase de Validación**
   - Verificar la funcionalidad de la aplicación.
   - Pruebas de rendimiento.
   - Pruebas de aceptación del usuario.
   - Monitorizar problemas.

**Análisis de Coste-Beneficio:**
- **Costes Iniciales:** Bajos (AWS MGN es gratuito, paga por los recursos).
- **Tiempo para Completar:** 1-3 meses para una carga de trabajo típica.
- **Nivel de Riesgo:** Bajo (cambios mínimos).
- **Ahorro a Largo Plazo:** 20-30% (sin actualización de hardware, pago por uso).
- **Beneficio Operativo:** Moderado (todavía gestionas el sistema operativo y las aplicaciones).

**Ventajas:**
- Enfoque de migración más rápido.
- Cambios mínimos en la aplicación.
- Tiempo rápido hacia la nube.
- Menor riesgo.
- Se puede optimizar más tarde.

**Cuándo Usar:**
- Necesidad de migrar rápidamente.
- Migraciones heredadas a gran escala.
- La optimización de costes es el objetivo inmediato.
- Plazo de salida del centro de datos.
- Recursos de desarrollo limitados.

**Ejemplo de Empresa Real:**
**GE Oil & Gas** utilizó el **rehosting** para migrar más de 500 aplicaciones a AWS en 9 meses. Esta migración rápida les permitió salir de los centros de datos rápidamente. Lograron ahorros de costes inmediatos y planificaron optimizar las aplicaciones de forma incremental después.

**Ejemplo:**
- Mover servidores web locales directamente a **EC2**.
- Migrar servidores de archivos a **EC2**.
- Subir máquinas virtuales de VMware a AWS.

#### 2. Replatforming (Mover, Retocar y Cambiar - Lift, Tinker, and Shift)

**Descripción:** Realizar algunas optimizaciones en la nube sin cambiar la arquitectura principal.

**Flujo de Trabajo de Migración Paso a Paso:**

1. **Fase de Evaluación**
   - Identificar oportunidades de optimización.
   - Analizar tipos de bases de datos.
   - Revisar los requisitos de cómputo.
   - Identificar candidatos para servicios gestionados.

2. **Fase de Planificación**
   - Elegir servicios gestionados (**RDS**, **ElastiCache**, etc.).
   - Diseñar una arquitectura de alta disponibilidad.
   - Planificar el enfoque de migración de datos.
   - Actualizar las cadenas de conexión de las aplicaciones.

3. **Fase de Migración**
   - Aprovisionar servicios gestionados.
   - Usar **Database Migration Service (DMS)** para los datos.
   - Actualizar las configuraciones de las aplicaciones.
   - Probar la conectividad y el rendimiento.

4. **Fase de Optimización**
   - Habilitar copias de seguridad automatizadas.
   - Configurar **Multi-AZ** para **HA**.
   - Configurar la monitorización.
   - Ajustar el rendimiento.

**Análisis de Coste-Beneficio:**
- **Costes Iniciales:** Bajos a Medios.
- **Tiempo para Completar:** 2-4 meses.
- **Nivel de Riesgo:** Bajo a Medio.
- **Ahorro a Largo Plazo:** 30-40% (los servicios gestionados reducen los costes operativos).
- **Beneficio Operativo:** Alto (AWS gestiona la BD, el tiempo de ejecución, etc.).

**Ventajas:**
- Beneficios tangibles sin cambiar la arquitectura principal.
- Algunas mejoras de rendimiento.
- Mejor eficiencia de costes que el **rehosting**.
- Reducción de la carga operativa.

**Cuándo Usar:**
- Quieres algunos beneficios de la nube.
- No quieres cambiar la aplicación principal.
- Dispuesto a realizar cambios menores.
- Ejecutas bases de datos que podrían ser gestionadas.

**Ejemplo de Empresa Real:**
**Expedia** recalibró su plataforma de reservas, pasando de bases de datos autogestionadas a **Amazon RDS**. Esto redujo la carga de los DBA en un 80% manteniendo el mismo código de aplicación. Obtuvieron copias de seguridad automáticas, conmutación por error **Multi-AZ** y reducción de costes operativos.

**Ejemplo:**
- Migrar una base de datos autogestionada a **Amazon RDS**.
- Mover la aplicación a **Elastic Beanstalk**.
- Reemplazar la caché autogestionada por **ElastiCache**.

#### 3. Repurchasing (Recompra)

**Descripción:** Moverse a un producto diferente (a menudo **SaaS**).

**Flujo de Trabajo de Migración Paso a Paso:**

1. **Fase de Evaluación**
   - Identificar alternativas de **SaaS**.
   - Comparar funciones y precios.
   - Evaluar la fiabilidad del proveedor.
   - Calcular el **TCO**.

2. **Fase de Selección**
   - Prueba piloto de los mejores candidatos.
   - Sesiones de retroalimentación de usuarios.
   - Revisión de seguridad y cumplimiento.
   - Elegir la solución final.

3. **Fase de Migración**
   - Exportar datos del sistema heredado.
   - Transformar los datos para el nuevo sistema.
   - Importar a la plataforma **SaaS**.
   - Configurar integraciones.

4. **Formación y Transición**
   - Formar a los usuarios en el nuevo sistema.
   - Ejecutar en paralelo durante el periodo de prueba.
   - Completar la transición.
   - Desmantelar el sistema antiguo.

**Análisis de Coste-Beneficio:**
- **Costes Iniciales:** Bajos (basado en suscripción).
- **Tiempo para Completar:** 1-6 meses.
- **Nivel de Riesgo:** Medio (riesgo de adopción por parte del usuario).
- **Ahorro a Largo Plazo:** 40-60% (sin infraestructura ni operaciones).
- **Beneficio Operativo:** Muy Alto (el proveedor gestiona todo).

**Ventajas:**
- Funciones y capacidades modernas.
- Reducción de la carga de mantenimiento.
- A menudo mejor experiencia de usuario.
- Actualizaciones automáticas regulares.

**Cuándo Usar:**
- Los costes de licencia heredada son altos.
- Existe una mejor alternativa de **SaaS**.
- Quieres modernizarte rápidamente.
- Personal de TI limitado.

**Ejemplo de Empresa Real:**
**News Corp Australia** se movió de servidores de correo electrónico locales a **Amazon WorkMail**, eliminando la gestión de servidores, reduciendo los costes en un 50% y proporcionando un mejor acceso móvil para los periodistas en el campo.

**Ejemplo:**
- Mover el CRM a **Salesforce**.
- Migrar el correo electrónico a **Amazon WorkMail** o **Microsoft 365**.
- Cambiar a **Amazon QuickSight** para **BI**.

#### 4. Refactoring / Re-architecting (Refactorización / Rediseño)

**Descripción:** Reimaginar cómo se diseña la aplicación utilizando funciones nativas de la nube.

**Flujo de Trabajo de Migración Paso a Paso:**

1. **Fase de Análisis**
   - Descomponer la aplicación monolítica.
   - Identificar los límites de los microservicios.
   - Diseñar la nueva arquitectura nativa de la nube.
   - Elegir servicios (**Lambda**, contenedores, etc.).

2. **Fase de Desarrollo**
   - Construir nuevos microservicios.
   - Implementar funciones sin servidor.
   - Crear API (**API Gateway**).
   - Desarrollar en paralelo con el sistema antiguo.

3. **Fase de Pruebas**
   - Pruebas integrales.
   - Pruebas de carga y rendimiento.
   - Pruebas de seguridad.
   - Pruebas de aceptación del usuario.

4. **Migración Gradual**
   - **Strangler pattern** (reemplazar incrementalmente).
   - **Blue/green deployment**.
   - **Feature flags** para el despliegue gradual.
   - Monitorizar de cerca.

**Análisis de Coste-Beneficio:**
- **Costes Iniciales:** Altos (esfuerzo de desarrollo).
- **Tiempo para Completar:** 6-18 meses.
- **Nivel de Riesgo:** Alto (cambios arquitectónicos).
- **Ahorro a Largo Plazo:** 50-70% (**Serverless**, **Auto-scaling**).
- **Beneficio Operativo:** Muy Alto (beneficios nativos de la nube).

**Ventajas:**
- Usar funciones nativas de la nube.
- Mejor rendimiento y escalabilidad.
- Máximos beneficios a largo plazo.
- Mayor optimización de costes.
- Prácticas de desarrollo modernas.

**Desventajas:**
- Más caro por adelantado.
- Consume más tiempo.
- Requiere desarrolladores cualificados.
- Mayor riesgo inicial.

**Cuándo Usar:**
- Necesidad de escalabilidad significativa.
- Quieres añadir funciones difíciles de implementar localmente.
- Aplicaciones estratégicas a largo plazo.
- La arquitectura actual limita el crecimiento.

**Ejemplo de Empresa Real:**
**Capital One** refactorizó sus aplicaciones bancarias de una arquitectura monolítica a microservicios utilizando contenedores (**ECS**) y sin servidor (**Lambda**). Esto les permitió desplegar actualizaciones 10 veces más rápido, escalar automáticamente y reducir los costes de infraestructura en un 60%. La migración de 18 meses se consideró estratégica para obtener una ventaja competitiva.

**Ejemplo:**
- Convertir un monolito en microservicios.
- Moverse a una arquitectura sin servidor con **Lambda**.
- Contenerizar aplicaciones con **ECS**/**EKS**.

#### 5. Retire (Retirada)

**Descripción:** Identificar y desmantelar activos de TI que ya no son útiles.

**Flujo de Trabajo Paso a Paso:**

1. **Fase de Descubrimiento**
   - Inventariar todas las aplicaciones.
   - Analizar los patrones de uso.
   - Identificar sistemas redundantes.
   - Encuestar a los propietarios del negocio.

2. **Fase de Análisis**
   - Determinar el valor de negocio.
   - Identificar dependencias.
   - Evaluar el impacto de la retirada.
   - Obtener la aprobación de los interesados.

3. **Fase de Retirada**
   - Exportar/archivar los datos necesarios.
   - Documentar la retirada.
   - Desmantelar sistemas.
   - Reasignar licencias.

**Análisis de Coste-Beneficio:**
- **Costes Iniciales:** Muy Bajos.
- **Tiempo para Completar:** 1-2 meses.
- **Nivel de Riesgo:** Bajo.
- **Ahorro a Largo Plazo:** 100% (eliminación completa de costes).
- **Beneficio Operativo:** Alto (reducción de la complejidad).

**Ventajas:**
- Reducir costes inmediatamente.
- Reducir riesgos de seguridad.
- Simplificar el portafolio.
- Liberar recursos de TI.

**Cuándo Usar:**
- Aplicaciones que ya no se usan.
- Sistemas redundantes.
- Fin de la función del negocio.
- Existen mejores alternativas.

**Ejemplo de Empresa Real:**
Durante la migración, un gran minorista descubrió que el 30% de sus más de 1,000 aplicaciones no habían tenido uso en 6 meses. Retirar estas ahorró $2M anuales y redujo la superficie de ataque.

**Ejemplo:**
- Apagar el antiguo sistema de informes (reemplazado por **QuickSight**).
- Desmantelar entornos de dev/test no utilizados.
- Eliminar aplicaciones de "shadow IT".

#### 6. Retain (Retención)

**Descripción:** Mantener las aplicaciones localmente (por ahora).

**Flujo de Trabajo de Decisión Paso a Paso:**

1. **Evaluación**
   - Evaluar la complejidad de la migración.
   - Evaluar el valor de negocio.
   - Revisar las restricciones técnicas.
   - Considerar el tiempo.

2. **Documentación**
   - Documentar la razón de la retención.
   - Establecer una fecha de revisión futura.
   - Identificar los prerrequisitos para una migración futura.
   - Rastrear las dependencias.

3. **Integración Híbrida**
   - Implementar conectividad (**Direct Connect**/**VPN**).
   - Asegurar la monitorización.
   - Mantener la postura de seguridad.
   - Planificar para la migración eventual.

**Análisis de Coste-Beneficio:**
- **Costes Iniciales:** Ninguno (sin migración).
- **Tiempo para Completar:** N/A.
- **Nivel de Riesgo:** Ninguno (sin cambios).
- **Ahorro a Largo Plazo:** 0% (los costes locales continúan).
- **Beneficio Operativo:** Ninguno.

**Cuándo Mantener:**
- No listo para migrar ahora.
- Se han realizado actualizaciones importantes recientemente.
- No hay valor de negocio en la migración.
- Restricciones regulatorias.
- La aplicación se retirará pronto de todos modos.
- Dependencias complejas que aún no están listas.

**Ejemplo de Empresa Real:**
Un proveedor de atención médica mantuvo los sistemas mainframe que gestionaban el procesamiento de reclamaciones heredadas mientras migraba nuevas aplicaciones a AWS. Planificaron retirar el mainframe en 3 años, cuando las reclamaciones caducaran.

**Ejemplo:**
- Mantener los sistemas mainframe.
- Retener aplicaciones programadas para su cierre.
- Aplicaciones con problemas complejos de licencias.
- Sistemas con dependencias de hardware.

> **Consejo para el Examen:** Las 6 R ayudan a las organizaciones a desarrollar una estrategia de migración. La mayoría de las migraciones utilizan una combinación de estos enfoques. El examen puede presentar un escenario y preguntar qué estrategia es la más apropiada.

> **Consejo para el Examen:** Patrón de pregunta común: "Una empresa necesita migrar rápidamente para cumplir con un plazo de cierre de un centro de datos. ¿Qué estrategia debería usar?". Respuesta: **Rehosting** (la más rápida). "Una empresa quiere maximizar los beneficios de la nube y tiene tiempo para invertir" = **Refactoring**.

## Errores Comunes

### Errores Conceptuales

**1. Confundir CapEx con OpEx**
- **Error:** Pensar que la nube aumenta los gastos de capital.
- **Realidad:** La nube cambia de **CapEx** (comprar servidores) a **OpEx** (pagar mensualmente).
- **Trampa del Examen:** Las preguntas pueden preguntar sobre los beneficios financieros: la nube reduce el **CapEx**.

**2. Pensar que todos los servicios son IaaS**
- **Error:** Tratar a **Lambda** o **RDS** como **IaaS**.
- **Realidad**: **Lambda** es **PaaS**, **RDS** es **PaaS**, **EC2** es **IaaS**.
- **Trampa del Examen:** Conoce qué servicio encaja en qué categoría.

**3. Mezclar modelos de despliegue**
- **Error:** Pensar que **híbrido** significa múltiples proveedores de nube.
- **Realidad**: **Híbrido** = nube + local; **Multi-cloud** = múltiples proveedores de nube.
- **Trampa del Examen:** "**Hybrid**" en el examen siempre significa AWS + local.

**4. Malentendido de los pilares de Well-Architected**
- **Error:** Pensar que la Optimización de Costes significa siempre la opción más barata.
- **Realidad:** Significa el mejor valor: el equilibrio adecuado entre coste y rendimiento.
- **Trampa del Examen:** La seguridad siempre tiene prioridad sobre el coste.

**5. Confusión en la estrategia de migración**
- **Error:** Pensar que **Replatforming** y **Refactoring** son lo mismo.
- **Realidad**: **Replatforming** = cambios menores; **Refactoring** = rediseño completo.
- **Trampa del Examen**: **Rehosting** es el más rápido, **Refactoring** proporciona el mayor beneficio a largo plazo.

### Confusión de Servicios

**6. CloudWatch vs CloudTrail vs Config**
- **CloudWatch**: Monitorización del rendimiento, métricas, registros.
- **CloudTrail**: Seguimiento de llamadas a la API, "¿quién hizo qué?".
- **Config**: Seguimiento de la configuración, "¿cómo están configurados los recursos?".
- **Trampa del Examen**: "¿Quién eliminó este recurso?" = **CloudTrail**.

**7. Security Groups vs NACLs**
- **Security Groups**: Con estado (**stateful**), nivel de instancia, solo reglas de permitir.
- **NACLs**: Sin estado (**stateless**), nivel de subred, reglas de permitir y denegar.
- **Trampa del Examen**: El **security group** por defecto deniega todo el tráfico entrante.

**8. Auto Scaling vs Load Balancing**
- **Auto Scaling**: Añade/elimina instancias EC2 según la demanda.
- **Load Balancing**: Distribuye el tráfico entre las instancias existentes.
- **Trampa del Examen**: Normalmente se usan ambos juntos.

### Errores de Coste

**9. Mal uso de Reserved Instances**
- **Error:** Comprar **IR** para cargas de trabajo variables.
- **Realidad**: Las **IR** son para cargas de trabajo predecibles y estables.
- **Trampa del Examen**: Usa **On-Demand** para lo variable, **IR** para la base.

**10. Costes de transferencia de datos**
- **Error:** Olvidar que la salida de datos de AWS cuesta dinero.
- **Realidad:** La entrada de datos es gratuita, la salida de datos se cobra.
- **Trampa del Examen**: "¿Cómo reducir costes?" puede implicar reducir la transferencia de datos.

## Preguntas de Repaso

Pon a prueba tu comprensión de los conceptos de la nube:

**1. ¿Cuáles de las siguientes son ventajas de cloud computing? (Elige DOS)**
   - A. Cambiar gastos variables por gastos de capital (Trade variable expense for capital expense).
   - B. Beneficiarse de economías de escala masivas (Benefit from massive economies of scale).
   - C. Dejar de adivinar la capacidad (Stop guessing capacity).
   - D. Aumentar el gasto en mantenimiento de centros de datos.
   - E. Mantener la seguridad física de los centros de datos.

**2. ¿Qué tipo de despliegue de nube conecta la infraestructura local con los recursos de la nube?**
   - A. **Public cloud**.
   - B. **Hybrid cloud**.
   - C. **Private cloud**.
   - D. **Multi-cloud** (Multi-nube).

**3. ¿Qué pilar del AWS Well-Architected Framework se centra en la protección de la información y los sistemas?**
   - A. **Operational Excellence** (Excelencia Operativa).
   - B. **Security** (Seguridad).
   - C. **Reliability** (Fiabilidad).
   - D. **Performance Efficiency** (Eficiencia del Rendimiento).

**4. ¿Qué estrategia de migración implica mover una aplicación a la nube sin realizar cambios?**
   - A. **Replatforming** (Replataformado).
   - B. **Refactoring** (Refactorización).
   - C. **Rehosting** (Rehospedaje).
   - D. **Repurchasing** (Recompra).

**5. Una empresa quiere rastrear todas las llamadas a la API realizadas en su cuenta de AWS para fines de cumplimiento. ¿Qué servicio debería usar?**
   - A. **Amazon CloudWatch**.
   - B. **AWS CloudTrail**.
   - C. **AWS Config**.
   - D. **AWS X-Ray**.

**6. ¿Qué modelo de cómputo en la nube proporciona el MÁXIMO control sobre la infraestructura subyacente?**
   - A. **Software as a Service (SaaS)**.
   - B. **Platform as a Service (PaaS)**.
   - C. **Infrastructure as a Service (IaaS)**.
   - D. **Function as a Service (FaaS)**.

**7. Una startup quiere minimizar los costes iniciales y pagar solo por lo que usa. ¿Qué ventaja de cloud computing representa esto?**
   - A. **Go global in minutes** (Globalizarse en minutos).
   - B. **Increase speed and agility** (Aumentar la velocidad y la agilidad).
   - C. **Trade capital expense for variable expense** (Cambiar gastos de capital por gastos variables).
   - D. **Benefit from massive economies of scale** (Beneficiarse de economías de escala masivas).

**8. ¿Qué estrategia de migración sería la MÁS apropiada para una empresa que necesita migrar 200 aplicaciones a AWS en 3 meses para cumplir con un plazo de cierre de un centro de datos?**
    - A. **Refactoring** (Refactorización).
    - B. **Rehosting** (Rehospedaje).
    - C. **Repurchasing** (Recompra).
    - D. **Retire** (Retirada).

**9. ¿Cuáles de los siguientes son principios de diseño del pilar de Reliability? (Elige DOS)**
   - A. Desplegar en múltiples **Availability Zones**.
   - B. Usar el tipo de instancia más barato.
   - C. Probar los procedimientos de recuperación.
   - D. Escalar la capacidad manualmente.
   - E. Desactivar la monitorización para reducir costes.

**10. Una empresa quiere optimizar los costes moviendo automáticamente los datos a los que se accede con poca frecuencia a niveles de almacenamiento más baratos. ¿Qué servicio debería usar?**
   - A. Políticas de ciclo de vida de **Amazon S3**.
   - B. **AWS Budgets**.
   - C. **AWS Cost Explorer**.
   - D. **Amazon CloudWatch**.

**11. ¿Qué pilar del Well-Architected Framework se centra en minimizar el impacto ambiental?**
   - A. **Cost Optimization** (Optimización de Costes).
   - B. **Performance Efficiency** (Eficiencia del Rendimiento).
   - C. **Sustainability** (Sostenibilidad).
   - D. **Operational Excellence** (Excelencia Operativa).

**12. Una empresa de servicios financieros debe mantener ciertos datos localmente debido a requisitos regulatorios, pero quiere usar AWS para el cómputo. ¿Qué modelo de despliegue debería usar?**
   - A. **Public cloud** (Nube pública).
   - B. **Hybrid cloud** (Nube híbrida).
   - C. **Private cloud** (Nube privada).
   - D. **Community cloud** (Nube comunitaria).

**13. ¿Qué servicio proporciona recomendaciones para la optimización de costes, seguridad y rendimiento?**
   - A. **AWS Config** (Configuración de AWS).
   - B. **AWS CloudTrail** (Rastreo en la nube de AWS).
   - C. **AWS Trusted Advisor** (Asesor de confianza de AWS).
   - D. **Amazon Inspector** (Inspector de Amazon).

**14. Una empresa quiere reemplazar su servidor de correo electrónico local por un servicio de correo electrónico totalmente gestionado. ¿Qué estrategia de migración está utilizando?**
    - A. **Rehosting** (Rehospedaje).
    - B. **Replatforming** (Replataformado).
    - C. **Refactoring** (Refactorización).
    - D. **Repurchasing** (Recompra).

**Respuestas:**
1. B, C - Las economías de escala y dejar de adivinar la capacidad son ventajas clave.
2. B - **Hybrid cloud** conecta la nube y lo local.
3. B - El pilar de **Security** se centra en proteger la información y los sistemas.
4. C - El **rehosting** (**lift and shift**) mueve las aplicaciones sin cambios.
5. B - **CloudTrail** rastrea las llamadas a la API para auditoría y cumplimiento.
6. C - **IaaS** proporciona el máximo control (gestión de OS, aplicaciones, etc.).
7. C - El pago por uso representa el cambio de **CapEx** por gastos variables.
8. B - El **rehosting** es la estrategia de migración más rápida.
9. A, C - El despliegue **Multi-AZ** y probar la recuperación son principios de **Reliability**.
10. A - Las políticas de ciclo de vida de S3 automatizan la organización de datos por niveles.
11. C - El pilar de **Sustainability** se centra en el impacto ambiental.
12. B - **Hybrid cloud** permite una mezcla de local y nube.
13. C - **Trusted Advisor** proporciona recomendaciones de optimización.
14. D - **Repurchasing** significa moverse a un producto diferente (**SaaS** email).

## Hoja de Trucos de Cloud Concepts

### Seis Ventajas de Cloud Computing (¡MEMORÍZALAS!)

1. **Cambiar CapEx por gastos variables (Trade CapEx for Variable Expense)** - Precios de pago por uso.
2. **Beneficiarse de economías de escala masivas (Benefit from Massive Economies of Scale)** - Precios más bajos por la escala de AWS.
3. **Dejar de adivinar la capacidad (Stop Guessing Capacity)** - Escala hacia arriba/abajo según la demanda.
4. **Aumentar la velocidad y la agilidad (Increase Speed and Agility)** - Recursos en minutos.
5. **Dejar de gastar dinero en la ejecución y el mantenimiento de centros de datos (Stop Spending Money Running and Maintaining Data Centers)** - Enfocarse en el valor del negocio.
6. **Globalizarse en minutos (Go Global in Minutes)** - Desplegar en múltiples regiones rápidamente.

### Modelos de Servicio (Control vs Responsabilidad)

| Modelo | Tú Gestionas | AWS Gestiona | Ejemplo |
|-------|------------|-------------|---------|
| **IaaS** | Aplicaciones, Datos, **Runtime**, **OS** | Hardware, Redes | EC2, S3 |
| **PaaS** | Aplicaciones, Datos | **Runtime**, **OS**, Hardware | RDS, Lambda |
| **SaaS** | Datos, Acceso | Todo lo demás | WorkMail, QuickSight |

**Truco de Memoria:** **IaaS** = Más control, **SaaS** = Menos control.

### Modelos de Despliegue

- **Cloud**: 100% en AWS.
- **Hybrid**: AWS + Local (vía VPN/Direct Connect).
- **On-Premises**: Tu centro de datos (AWS Outposts disponible).

### Well-Architected Framework (6 Pilares)

| Pilar | Enfoque | Servicio Clave | Recuerda |
|--------|-------|-------------|----------|
| **Operational Excellence** | Run & monitor | **CloudFormation**, **CloudWatch** | Automatización, **IaC** |
| **Security** | Protect data & systems | **IAM**, **KMS**, **GuardDuty** | **Least privilege**, cifrado |
| **Reliability** | Recover from failures | **Multi-AZ**, **Auto Scaling** | Múltiples **AZ**, copias de seguridad |
| **Performance Efficiency** | Right resources | **CloudFront**, **Lambda** | Instancia correcta, caché |
| **Cost Optimization** | Best value | **Cost Explorer**, **RIs** | Etiqueta todo, **right-size** |
| **Sustainability** | Environmental impact | **Graviton**, **Auto Scaling** | Maximizar utilización |

### Estrategias de Migración (Las 6 R)

| Estrategia | Descripción | Velocidad | Coste | Cuándo Usar |
|----------|-------------|-------|------|----------|
| **Rehost** | **Lift & shift** (Mover y cambiar) | La más rápida | El más bajo inicial | Migración rápida necesaria |
| **Replatform** | **Lift, tinker & shift** (Mover, retocar y cambiar) | Rápida | Bajo-Medio | Quieres algunos beneficios de nube |
| **Repurchase** | Moverse a **SaaS** | Media | Bajo | Existe un mejor **SaaS** |
| **Refactor** | Rediseñar para la nube | La más lenta | El más alto | Necesitas funciones nativas de nube |
| **Retire** | Desmantelar | N/A | Ahorra el 100% | Aplicación no necesaria |
| **Retain** | Mantener local | N/A | Sin ahorros | No listo para migrar |

**Truco de Memoria:** Piensa en "velocidad frente a beneficio": **Rehost** = rápido, menos beneficio; **Refactor** = lento, máximo beneficio.

### Modelos de Coste

**CapEx (Local):**
- Compra inicial.
- Coste fijo.
- Se deprecia.
- Compromiso largo.

**OpEx (Nube):**
- **Pay-as-you-go**.
- Coste variable.
- Sin depreciación.
- Flexible.

### Referencia Rápida de Servicios Clave

**Monitorización y Registro:**
- **CloudWatch**: Métricas, registros, alarmas.
- **CloudTrail**: Seguimiento de llamadas a la API.
- **Config**: Seguimiento de la configuración.

**Gestión de Costes:**
- **Cost Explorer**: Visualizar el gasto.
- **Budgets**: Alertas de costes.
- **Trusted Advisor**: Recomendaciones de optimización.

**Alta Disponibilidad:**
- **Multi-AZ**: Desplegar en varias zonas de disponibilidad.
- **Auto Scaling**: Ajuste automático de la capacidad.
- **ELB**: Equilibrado de carga entre instancias.

**Security:**
- **IAM**: Gestión de identidades y accesos.
- **KMS**: Gestión de claves de cifrado.
- **GuardDuty**: Detección de amenazas.
- **Shield**: Protección contra DDoS.

### Resumen de Consejos para el Examen

1. **Las seis ventajas** aparecen en 3-5 preguntas: memorízalas.
2. **El Well-Architected Framework** se prueba intensamente: conoce los 6 pilares.
3. **La categorización de servicios** (**IaaS**/**PaaS**/**SaaS**) aparece regularmente.
4. **Estrategias de migración**: haz coincidir el escenario con la R correcta.
5. **CapEx frente a OpEx**: la nube desplaza del **CapEx** al **OpEx**.
6. **Híbrido siempre significa** AWS + local (no multi-nube).
7. **CloudTrail = quién hizo qué**, **CloudWatch = monitorización**.
8. **La seguridad siempre gana** cuando entra en conflicto con el coste.
9. **Multi-AZ = alta disponibilidad**, **Multi-Region = recuperación ante desastres**.
10. **Reserved Instances** para cargas estables, **On-Demand** para variables.

### Patrones de Pregunta Comunes

**Patrón 1: "Una empresa quiere [escenario]. ¿Qué ventaja?"**
- Busca palabras clave: "mínimo inicial" = **CapEx** a **OpEx**, "demanda máxima" = **Stop guessing**.

**Patrón 2: "¿Qué servicio [capacidad]?"**
- Monitorización = **CloudWatch**, auditoría de API = **CloudTrail**, Coste = **Cost Explorer**.

**Patrón 3: "¿Mejor estrategia de migración para [escenario]?"**
- "Rápido" = **Rehosting**, "Nativo de la nube" = **Refactoring**, "SaaS alternative" = **Repurchasing**.

**Patrón 4: "¿Qué pilar [se centra en]?"**
- "Fallos" = **Reliability**, "Protect" = **Security**, "Cost" = **Cost Optimization**.

### Datos Rápidos

- AWS tiene **más de 33 regiones** en todo el mundo.
- Cada región tiene **múltiples zonas de disponibilidad** (normalmente de 3 a 6).
- Durabilidad de S3: **99.999999999% (11 9's)**.
- Disponibilidad **Multi-AZ** de RDS: **99.95%**.
- Entrada de datos a AWS: **Gratis**.
- Salida de datos de AWS: **Se cobra**.
- **Reserved Instances**: Hasta un **75% de ahorro**.
- Cuenta raíz: **Habilita MFA, no la uses para tareas diarias**.

---

[← Anterior: Introducción](01-introduction.md) | [Volver al Inicio](README.md) | [Siguiente: Seguridad y Cumplimiento →](03-security-compliance.md)
