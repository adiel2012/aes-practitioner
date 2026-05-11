# Dominio 4: Facturación, Precios y Soporte

[Anterior: Tecnología y Servicios](./04-technology-services.md) | [Tabla de Contenidos](./README.md) | [Siguiente: Preguntas de Práctica](./06-practice-questions.md)

---

## Tabla de Contenidos

- [Fundamentos de Precios de AWS](#fundamentos-de-precios-de-aws)
  - [Principios Relevantes](#principios-relevantes)
  - [Capa Gratuita de AWS (AWS Free Tier)](#capa-gratuita-de-aws-aws-free-tier)
- [Modelos de Precios por Categoría de Servicio](#modelos-de-precios-por-categoria-de-servicio)
  - [Precios de Cómputo (Compute Pricing)](#precios-de-computo-compute-pricing)
  - [Precios de Almacenamiento (Storage Pricing)](#precios-de-almacenamiento-storage-pricing)
  - [Precios de Bases de Datos (Database Pricing)](#precios-de-bases-de-datos-database-pricing)
  - [Precios de Red (Network Pricing)](#precios-de-red-network-pricing)
- [Ejemplos Detallados de Precios y Cálculos](#ejemplos-detallados-de-precios-y-calculos)
  - [Escenario de Precios de EC2](#escenario-de-precios-de-ec2)
  - [Análisis de Costes de Almacenamiento en S3](#analisis-de-costes-de-almacenamiento-en-s3)
  - [Desglose de Costes de Aplicación Multi-Nivel](#desglose-de-costes-de-aplicacion-multi-nivel)
  - [Cálculos de Costes de Transferencia de Datos](#calculos-de-costes-de-transferencia-de-datos)
- [Reserved Instances vs Savings Plans](#reserved-instances-vs-savings-plans)
  - [Comparación Detallada](#comparacion-detallada)
  - [Cálculos de ROI](#calculos-de-roi)
  - [Cuándo Usar Cada Opción](#cuando-usar-cada-opcion)
- [Casos de Estudio de Optimización de Costes](#casos-de-estudio-de-optimizacion-de-costes)
  - [Caso de Estudio 1: Plataforma de Comercio Electrónico](#caso-de-estudio-1-plataforma-de-comercio-electronico)
  - [Caso de Estudio 2: Carga de Trabajo de Análisis de Datos](#caso-de-estudio-2-carga-de-trabajo-de-analisis-de-datos)
  - [Caso de Estudio 3: Entorno de Desarrollo](#caso-de-estudio-3-entorno-de-desarrollo)
- [Herramientas de Gestión de Costes](#herramientas-de-gestion-de-costes)
  - [AWS Pricing Calculator](#aws-pricing-calculator)
  - [AWS Cost Explorer](#aws-cost-explorer)
  - [AWS Budgets](#aws-budgets)
  - [AWS Cost and Usage Report](#aws-cost-and-usage-report)
  - [AWS Cost Anomaly Detection](#aws-cost-anomaly-detection)
  - [TCO Calculator Walkthrough](#tco-calculator-walkthrough)
- [Estrategias de Etiquetado para la Asignación de Costes](#estrategias-de-etiquetado-para-la-asignacion-de-costes)
  - [Mejores Prácticas de Etiquetado (Tag Best Practices)](#mejores-practicas-de-etiquetado-tag-best-practices)
  - [Esquemas de Etiquetado Comunes](#esquemas-de-etiquetado-comunes)
  - [Aplicación de Etiquetas](#aplicacion-de-etiquetas)
- [Configuración de Facturación Multi-Cuenta](#configuracion-de-facturacion-multi-cuenta)
  - [Estructura de la Organización](#estructura-de-la-organizacion)
  - [Mejores Prácticas](#mejores-practicas)
  - [Asignación de Costes](#asignacion-de-costes)
- [Facturación Consolidada y AWS Organizations](#facturacion-consolidada-y-aws-organizations)
- [Profundización en Cost Anomaly Detection](#profundizacion-en-cost-anomaly-detection)
  - [Instalación y Configuración](#instalacion-y-configuracion)
  - [Ejemplos de Alertas](#ejemplos-de-alertas)
  - [Flujos de Trabajo de Respuesta](#flujos-de-trabajo-de-respuesta)
- [Planes de Soporte de AWS (AWS Support Plans)](#planes-de-soporte-de-aws-aws-support-plans)
  - [Comparación de Planes de Soporte](#comparacion-de-planes-de-soporte)
  - [Matriz de Decisión de Planes de Soporte](#matriz-de-decision-de-planes-de-soporte)
  - [Comparación Detallada de Funciones](#comparacion-detallada-de-funciones)
  - [Recursos de Soporte Adicionales](#recursos-de-soporte-adicionales)
- [Estrategias de Optimización de Costes](#estrategias-de-optimizacion-de-costes)
  - [Optimización Específica del Servicio](#optimizacion-especifica-del-servicio)
- [Gobernanza de Costes y FinOps](#gobernanza-de-costes-y-finops)
  - [Marco de FinOps (FinOps Framework)](#marco-de-finops-finops-framework)
  - [Políticas de Gobernanza](#politicas-de-gobernanza)
  - [Responsabilidad y Propiedad](#responsabilidad-y-propiedad)
- [Resolución de Problemas de Facturación](#resolucion-de-problemas-de-facturacion)
  - [Problemas Comunes](#problemas-comunes)
  - [Pasos de Resolución](#pasos-de-resolucion)
- [Preguntas de Repaso](#preguntas-de-repaso)

---

## Fundamentos de Precios de AWS

### Principios Relevantes

Los precios de AWS se basan en varios principios fundamentales que diferencian el cómputo en la nube de la infraestructura tradicional en las instalaciones (on-premises):

1. **Pago por uso (Pay-as-you-go)**: Paga solo por lo que utilizas.
   - Sin compromisos iniciales requeridos.
   - Inicia y detén recursos en cualquier momento.
   - Solo se cobra por el consumo real.

2. **Paga menos cuando reservas (Pay less when you reserve)**: Descuentos por capacidad reservada.
   - Comprométete al uso durante 1 o 3 años.
   - Recibe descuentos significativos (hasta el 75%).
   - Disponible para **EC2**, **RDS**, **ElastiCache**, **Redshift** y más.

3. **Paga menos con descuentos por volumen (Pay less with volume-based discounts)**: Cuanto más usas, menos pagas por unidad.
   - Los precios por niveles se aplican automáticamente a medida que aumenta el uso.
   - Los precios de transferencia de datos y almacenamiento disminuyen con el volumen.
   - No se requieren negociaciones.

4. **Sin costes iniciales (No upfront costs)**: Sin gastos de capital.
   - Cambia el gasto de capital (**CAPEX**) por gasto variable (**OPEX**).
   - Sin infraestructura que comprar por adelantado.
   - Comienza con inversión cero.

5. **Sin cargos por terminación**: Detén el servicio en cualquier momento.
   - Sin contratos ni compromisos a largo plazo (a menos que elijas **Reserved Instances**).
   - Elimina recursos cuando ya no sean necesarios.
   - Deja de pagar inmediatamente.

---

### Capa Gratuita de AWS (AWS Free Tier)

AWS ofrece tres tipos de ofertas de capa gratuita para ayudar a los nuevos clientes a comenzar y experimentar con los servicios:

#### 1. Siempre Gratis (Always Free)

Servicios que nunca caducan y están disponibles para todos los clientes de AWS:

- **DynamoDB**: 25 GB de almacenamiento.
- **Lambda**: 1 millón de solicitudes al mes.
- **SNS**: 1 millón de publicaciones.
- **CloudWatch**: 10 métricas personalizadas y alarmas.
- **Panel de control de la Capa Gratuita de AWS (AWS Free Tier dashboard)**: Monitoriza el uso.

#### 2. 12 Meses Gratis (12 Months Free)

Servicios disponibles durante 12 meses a partir de la fecha de creación de la cuenta:

- **EC2**: 750 horas/mes de instancias **t2.micro** o **t3.micro**.
- **S3**: 5 GB de almacenamiento estándar (**Standard storage**).
- **RDS**: 750 horas/mes de instancias de base de datos **db.t2.micro**.
- **CloudFront**: 50 GB de transferencia de datos de salida (**data transfer out**).
- **Elastic Load Balancing**: 750 horas al mes.

> **Nota**: Las 750 horas de **EC2** son suficientes para ejecutar una instancia **t2.micro** de forma continua durante un mes completo.

#### 3. Pruebas (Trials)

Pruebas gratuitas a corto plazo para servicios específicos:

- **SageMaker**: 2 meses gratis.
- **Inspector**: 90 días gratis.
- **Lightsail**: 1 mes gratis (primer mes).
- **Amazon Comprehend Medical**: Varios periodos de prueba.

> **Importante**: Configura siempre alertas de facturación (**billing alerts**) cuando uses la Capa Gratuita para evitar cargos inesperados si superas los límites.

---

## Modelos de Precios por Categoría de Servicio

### Precios de Cómputo (Compute Pricing)

#### Amazon EC2

- **Horas de Instancia (Instance Hours)**: Paga por las instancias en ejecución (cobrado por segundo con un mínimo de 60 segundos).
- **Los precios varían según**:
  - Tipo de instancia (**t2.micro**, **m5.large**, etc.).
  - Región (**us-east-1** vs. **eu-west-1**).
  - Sistema operativo (**Linux**, **Windows**, **RHEL**).
  - Tenencia (**Shared** vs. **Dedicated**).
- **Cargos adicionales**:
  - Transferencia de datos de salida (**Data transfer out**).
  - Volúmenes de almacenamiento **EBS**.
  - Direcciones **Elastic IP** (cuando no están adjuntas).

**Opciones de Compra de EC2 (EC2 Purchase Options)**:

| Opción de Compra | Descripción | Descuento | Caso de Uso |
|----------------|-------------|----------|----------|
| **On-Demand** | Paga por segundo, sin compromiso | Base | Cargas de trabajo de corto plazo e impredecibles |
| **Reserved Instances** | Compromiso de 1 o 3 años | Hasta 75% | Cargas de trabajo de estado estable y predecibles |
| **Spot Instances** | Puja por capacidad no utilizada | Hasta 90% | Cargas de trabajo tolerantes a fallos y flexibles |
| **Savings Plans** | Compromiso de uso constante ($/hora) | Hasta 72% | Uso de cómputo flexible |
| **Dedicated Hosts** | Servidor físico dedicado para ti | Varía | Requisitos de cumplimiento y licencias |

#### AWS Lambda

- **Solicitudes (Requests)**: $0.20 por cada 1 millón de solicitudes.
- **Tiempo de Cómputo**: Cobrado por GB-segundo.
  - La duración se calcula desde el inicio de la ejecución del código hasta el retorno/terminación.
  - Redondeado al alza al milisegundo más cercano.
- **Capa Gratuita**: 1 millón de solicitudes/mes (siempre gratis).
- **Sin cargos** cuando el código no se está ejecutando.

---

### Precios de Almacenamiento (Storage Pricing)

#### Amazon S3

Componentes de precios:

1. **Almacenamiento**: Paga por GB/mes almacenado.
   - Varía según la clase de almacenamiento (**Standard**, **Infrequent Access**, **Glacier**, etc.).
   - **Standard**: ~$0.023 por GB/mes.
   - **Standard-IA**: ~$0.0125 por GB/mes.
   - **Glacier**: ~$0.004 por GB/mes.

2. **Solicitudes (Requests)**:
   - Solicitudes **PUT**, **COPY**, **POST**, **LIST**: $0.005 por cada 1,000.
   - Solicitudes **GET**, **SELECT**: $0.0004 por cada 1,000.

3. **Transferencia de datos**:
   - Transferencia de entrada (**Transfer IN**): Gratis.
   - Transferencia de salida a Internet (**Transfer OUT**): Precios por niveles (primeros 10 TB/mes a $0.09/GB).
   - Transferencia a **CloudFront**: Gratis.

4. **Funciones de gestión**:
   - **S3 Inventory**, **Analytics**, **Object Tagging**.

#### Amazon EBS

- **Almacenamiento Aprovisionado (Provisioned storage)**: Paga por la capacidad aprovisionada por GB/mes.
  - **gp3**: $0.08/GB-mes.
  - **gp2**: $0.10/GB-mes.
  - **io2**: $0.125/GB-mes + cargos por **IOPS**.
- **Snapshots**: Almacenamiento de copias de seguridad incrementales por GB/mes.
- **Varía según el tipo de volumen**: General Purpose (SSD), Provisioned IOPS (SSD), Throughput Optimized (HDD).

> **Diferencia Clave**: **EBS** cobra por la capacidad aprovisionada, no por la capacidad utilizada. Un volumen de 100 GB cuesta lo mismo si almacenas 10 GB o 100 GB.

---

### Precios de Bases de Datos (Database Pricing)

#### Amazon RDS

Componentes de precios:

1. **Horas de Instancia**: Basado en la clase de instancia (**db.t2.micro**, **db.m5.large**).
2. **Almacenamiento**: Por GB/mes de almacenamiento aprovisionado.
3. **Almacenamiento de Copia de Seguridad (Backup storage)**: Copias de seguridad automáticas más allá del tamaño de la base de datos.
4. **Transferencia de Datos**: Tarifas estándar de transferencia de datos de AWS.
5. **Funciones Adicionales**:
   - Despliegue **Multi-AZ** (duplica el coste).
   - Réplicas de lectura (**Read replicas**) (se cobran como instancias separadas).

#### Amazon DynamoDB

Dos modos de capacidad:

1. **Bajo Demanda (On-Demand)**:
   - Paga por solicitud.
   - No requiere planificación de capacidad.
   - Bueno para cargas de trabajo impredecibles.
   - Unidades de Solicitud de Escritura (**WRU**) y Unidades de Solicitud de Lectura (**RRU**).

2. **Capacidad Aprovisionada (Provisioned Capacity)**:
   - Paga por las unidades de capacidad de lectura/escritura aprovisionadas.
   - **Auto Scaling** disponible.
   - Más rentable para cargas de trabajo predecibles.
   - Reserva de capacidad para descuentos adicionales.

3. **Almacenamiento**: $0.25 por GB/mes (primeros 25 GB gratis con la capa **Always Free**).

---

### Precios de Red (Network Pricing)

Comprender los costes de transferencia de datos es crucial para la optimización de costes:

- **Transferencia de datos de entrada (Data transfer IN)**: Generalmente **gratis** desde Internet hacia AWS.
- **Transferencia de datos de salida a Internet (Data transfer OUT)**: Se **cobra** con precios por niveles.
  - Primeros 10 TB/mes: $0.09/GB.
  - Siguientes 40 TB/mes: $0.085/GB.
  - Más de 150 TB/mes: $0.05/GB.
- **Transferencia de datos entre Regiones**: Se **cobra** a tarifas inter-regionales.
- **Transferencia de datos dentro de la misma Región**:
  - Entre **AZs**: $0.01/GB en cada dirección.
  - Dentro de la misma **AZ**: Gratis (usando IPs privadas).
- **Transferencia de salida de CloudFront**: Coste menor que directo desde los servicios.
- **VPC Endpoints**: Reducen los costes de transferencia de datos para **S3** y **DynamoDB**.

> **Consejo de Optimización de Costes**: Usa **CloudFront CDN** para almacenar contenido en caché en las ubicaciones de borde (edge locations), reduciendo los costes de transferencia de datos desde los servicios de origen.

---

## Ejemplos Detallados de Precios y Cálculos

### Escenario de Precios de EC2

Calculemos el coste mensual para diferentes opciones de compra de **EC2**:

**Escenario**: Aplicación web que requiere 5 instancias **m5.large** (2 vCPU, 8 GB RAM) funcionando 24/7 en **us-east-1**.

#### Precios On-Demand
```
Instancia: m5.large
Tarifa: $0.096 por hora
Horas al mes: 730 horas (promedio)
Número de instancias: 5

Coste mensual por instancia: $0.096 × 730 = $70.08
Coste mensual total: $70.08 × 5 = $350.40/mes
Coste anual: $350.40 × 12 = $4,204.80/año
```

#### Reserved Instance de 1 Año (Partial Upfront)
```
Pago inicial por instancia: $335
Tarifa mensual por instancia: $0.028/hora

Coste mensual recurrente por instancia: $0.028 × 730 = $20.44
Coste inicial total: $335 × 5 = $1,675
Coste mensual total: $20.44 × 5 = $102.20/mes

Total primer año: $1,675 + ($102.20 × 12) = $2,901.40
Ahorro frente a On-Demand: $4,204.80 - $2,901.40 = $1,303.40 (31% de ahorro)
```

#### Reserved Instance de 3 Años (All Upfront)
```
Pago inicial por instancia: $2,140
Sin cargos mensuales

Coste inicial total: $2,140 × 5 = $10,700
Equivalente mensual: $10,700 ÷ 36 = $297.22/mes

Total tres años: $10,700
Coste On-Demand de tres años: $4,204.80 × 3 = $12,614.40
Ahorro: $12,614.40 - $10,700 = $1,914.40 (15% de ahorro)
Ahorro anual: $638.13/año (52% de ahorro anual)
```

#### Compute Savings Plan (1 Año, Partial Upfront)
```
Compromiso: $200/mes
Cobertura: Proporciona ~$285 de cómputo On-Demand al mes
Descuento efectivo: ~30%

Coste anual: $200 × 12 = $2,400 + pago inicial
Más pago inicial: ~$600
Total primer año: ~$3,000
Ahorro: $4,204.80 - $3,000 = $1,204.80 (29% de ahorro)

Ventaja de flexibilidad: Puede cambiar tipos/tamaños de instancia/regiones
```

#### Precios de Spot Instance
```
Precio promedio de Spot para m5.large: ~$0.030/hora (varía según la demanda)
Ahorro potencial: Hasta un 69% menos que On-Demand

Coste mensual por instancia: $0.030 × 730 = $21.90
Coste mensual total: $21.90 × 5 = $109.50/mes
Coste anual: $109.50 × 12 = $1,314/año
Ahorro: $4,204.80 - $1,314 = $2,890.80 (69% de ahorro)

Riesgo: Las instancias pueden ser interrumpidas con un aviso de 2 minutos
Ideal para: Aplicaciones sin estado (stateless) con capacidad de auto-reinicio
```

#### Resumen de Comparación de Costes

| Opción de Compra | Coste Mensual | Coste Anual | Coste de 3 Años | Ahorro vs On-Demand |
|----------------|--------------|-------------|-------------|----------------------|
| **On-Demand** | $350.40 | $4,204.80 | $12,614.40 | Base (0%) |
| **1-Yr RI (Partial)** | $102.20 + $1,675 inicial | $2,901.40 | - | 31% |
| **3-Yr RI (All Up)** | $297.22 equiv | $3,566.67 equiv | $10,700 | 52% |
| **Savings Plan** | $250.00 | $3,000.00 | - | 29% |
| **Spot Instances** | $109.50 | $1,314.00 | $3,942.00 | 69% |

**Costes Adicionales a Considerar**:
- Volúmenes **EBS**: $0.10/GB-mes (**gp2**) × 100 GB × 5 = $50/mes
- Transferencia de datos de salida: Variable según el uso
- **Elastic Load Balancer**: $16.20/mes + $0.008/GB procesado
- Estimación total de infraestructura: Añadir un 15-25% a los costes de cómputo

---

### Análisis de Costes de Almacenamiento en S3

**Escenario**: 10 TB de datos con diferentes patrones de acceso

#### Datos Accedidos Frecuentemente (40% = 4 TB)

**S3 Standard**:
```
Almacenamiento: 4,000 GB × $0.023/GB = $92/mes
Solicitudes PUT: 100,000 × $0.005/1,000 = $0.50
Solicitudes GET: 1,000,000 × $0.0004/1,000 = $0.40
Transferencia de datos de salida: 500 GB × $0.09/GB = $45.00

Coste mensual total: $137.90/mes
```

#### Acceso Infrecuente (30% = 3 TB)

**S3 Standard-IA**:
```
Almacenamiento: 3,000 GB × $0.0125/GB = $37.50/mes
Solicitudes PUT: 10,000 × $0.010/1,000 = $0.10
Solicitudes GET: 50,000 × $0.001/1,000 = $0.05
Tarifa de recuperación: 50 GB × $0.01/GB = $0.50
Transferencia de datos de salida: 50 GB × $0.09/GB = $4.50

Coste mensual total: $42.65/mes
```

#### Datos de Archivo (30% = 3 TB)

**S3 Glacier Flexible Retrieval**:
```
Almacenamiento: 3,000 GB × $0.0036/GB = $10.80/mes
Solicitudes PUT: 1,000 × $0.03/1,000 = $0.03
Recuperación (ocasional): 10 GB × $0.0025/GB = $0.025

Coste mensual total: $10.86/mes
```

#### Comparación de Coste Total de Almacenamiento en S3

| Mezcla de Clases de Almacenamiento | Coste Mensual | Coste Anual | Ahorro vs Todo-Standard |
|------------------|--------------|-------------|-------------------------|
| Todo **S3 Standard** (10 TB) | $230.00 | $2,760.00 | Base |
| Mezcla Optimizada (arriba) | $191.41 | $2,296.92 | 17% ($463.08) |
| Con **Intelligent-Tiering** | $185.00 | $2,220.00 | 20% ($540.00) |
| Con Políticas de Ciclo de Vida | $178.50 | $2,142.00 | 22% ($618.00) |

**Optimización con Políticas de Ciclo de Vida (Lifecycle Policy)**:
```
Día 0-30: S3 Standard (datos activos)
Día 31-90: S3 Standard-IA (acceso menos frecuente)
Día 91-365: S3 Glacier Flexible Retrieval (archivo)
Día 365+: S3 Glacier Deep Archive (cumplimiento a largo plazo)

Ahorro adicional estimado: 5-8% mediante transiciones automatizadas
```

**Información sobre Optimización de Costes**:
- Tarifa de monitorización de **Intelligent-Tiering**: $0.0025 por cada 1,000 objetos
- Se aplican cargos por duración mínima de almacenamiento (**Standard-IA**: 30 días, **Glacier**: 90 días)
- Se aplican tarifas por eliminación anticipada si los objetos se eliminan antes de la duración mínima
- Las transiciones de ciclo de vida reducen la carga de gestión manual

---

### Desglose de Costes de Aplicación Multi-Nivel

**Escenario**: Aplicación web de producción de tres niveles en **us-east-1**

#### Componentes de la Arquitectura

**Nivel Web (Web Tier)**:
```
- Application Load Balancer:
  Base: $0.0225/hora × 730 = $16.43
  Cargos por LCU: ~$15/mes (varía según el tráfico)
  Total ALB: ~$31.43/mes

- EC2 Auto Scaling (2-6 instancias, promedio 4):
  Instancia: t3.medium a $0.0416/hora
  Coste promedio: 4 × $0.0416 × 730 = $121.47/mes
  Con RI de 1 año: ~$73.00/mes (40% de ahorro)

- Volúmenes EBS: 4 × 50 GB gp3 × $0.08 = $16.00/mes

Total Nivel Web: $168.90/mes (On-Demand)
Total Nivel Web: $120.43/mes (con RIs)
```

**Nivel de Aplicación (Application Tier)**:
```
- Application Load Balancer: $31.43/mes
- EC2 Auto Scaling (3-8 instancias, promedio 5):
  Instancia: m5.large a $0.096/hora
  Coste promedio: 5 × $0.096 × 730 = $350.40/mes
  Con Compute Savings Plan: ~$245.00/mes (30% de ahorro)

- Volúmenes EBS: 5 × 100 GB gp3 × $0.08 = $40.00/mes

Total Nivel de Aplicación: $421.83/mes (On-Demand)
Total Nivel de Aplicación: $316.43/mes (con Savings Plan)
```

**Nivel de Base de Datos (Database Tier)**:
```
- RDS Multi-AZ (db.m5.large):
  Coste de instancia: $0.192/hora × 730 = $140.16/mes
  Almacenamiento: 500 GB SSD de Uso General × $0.115 = $57.50/mes
  Almacenamiento de copia de seguridad (más allá del tamaño de la DB): 200 GB × $0.095 = $19.00/mes
  Solicitudes de E/S: 1M IOPS × $0.20/1M = $0.20/mes

- Réplica de Lectura (Read Replica) (misma región):
  Coste de instancia: $0.096/hora × 730 = $70.08/mes
  Almacenamiento: 500 GB × $0.115 = $57.50/mes

Total Nivel de Base de Datos: $344.44/mes (On-Demand)
Total Nivel de Base de Datos con RI de 1 año: ~$229.00/mes (33% de ahorro)
```

**Servicios Adicionales**:
```
- S3 para activos estáticos: 100 GB Standard = $2.30/mes
- CloudFront CDN:
  Transferencia de datos de salida: 1 TB × $0.085 = $85.00/mes
  Solicitudes HTTP: 10M × $0.0075/10,000 = $7.50/mes

- Route 53:
  Zona alojada: $0.50/mes
  Consultas: 100M × $0.40/1M = $40.00/mes

- CloudWatch:
  Métricas personalizadas: 50 × $0.30 = $15.00/mes
  Ingesta de registros: 10 GB × $0.50 = $5.00/mes

- VPC:
  NAT Gateway: 2 × ($0.045/hora × 730) = $65.70/mes
  Datos de NAT Gateway: 500 GB × $0.045 = $22.50/mes

Total Servicios Adicionales: $243.50/mes
```

#### Análisis Completo de Costes de la Aplicación

| Componente | On-Demand | Con Reserva/Ahorro | Ahorro Mensual |
|-----------|-----------|----------------------|-----------------|
| Nivel Web | $168.90 | $120.43 | $48.47 |
| Nivel de Aplicación | $421.83 | $316.43 | $105.40 |
| Nivel de Base de Datos | $344.44 | $229.00 | $115.44 |
| Servicios Adicionales | $243.50 | $243.50 | $0.00 |
| **Total Mensual** | **$1,178.67** | **$909.36** | **$269.31** |
| **Anual** | **$14,144.04** | **$10,912.32** | **$3,231.72** |

**Oportunidades de Optimización de Costes**:
1. Implementar políticas de **Auto Scaling** (ahorro del 20-30% en cómputo)
2. Usar **Spot Instances** para trabajos por lotes no críticos (ahorro del 60-70%)
3. Habilitar políticas de ciclo de vida de **S3** (ahorro del 10-15% en almacenamiento)
4. Optimizar el almacenamiento en caché de **CloudFront** (reducir las solicitudes de origen en un 40%)
5. Implementar el auto-escalado de almacenamiento de **RDS** (paga solo por lo que usas)

**Coste Total Esperado Completamente Optimizado**: ~$750-850/mes (36-42% de ahorro total)

---

### Cálculos de Costes de Transferencia de Datos

**Escenario**: Aplicación global con usuarios en múltiples regiones

#### Tráfico Entrante (GRATIS)
```
Tráfico desde Internet hacia AWS: GRATIS
- Cargas de usuarios a S3: 2 TB/mes = $0.00
- Solicitudes de API a ALB/API Gateway: GRATIS
- Ingesta de datos a Kinesis: GRATIS

Total entrante: $0.00
```

#### Tráfico Saliente (CON COSTE)

**Directo desde EC2 a Internet**:
```
Primeros 10 TB/mes: $0.09/GB
Siguientes 40 TB/mes: $0.085/GB
Siguientes 100 TB/mes: $0.070/GB
Más de 150 TB/mes: $0.05/GB

Ejemplo - transferencia de 5 TB:
5,000 GB × $0.09 = $450.00/mes
```

**Vía CloudFront**:
```
CloudFront a Internet (EE. UU./Europa):
Primeros 10 TB/mes: $0.085/GB
Siguientes 40 TB/mes: $0.080/GB
Siguientes 100 TB/mes: $0.060/GB
Más de 150 TB/mes: $0.040/GB

Ejemplo - transferencia de 5 TB vía CloudFront:
5,000 GB × $0.085 = $425.00/mes
Ahorro: $25.00/mes (6% más barato + beneficio de rendimiento)
```

**Transferencia de Datos Entre Regiones**:
```
us-east-1 a eu-west-1: $0.02/GB
Transferencia: 1 TB/mes = 1,000 GB × $0.02 = $20.00/mes

Mejor Práctica: Replicar datos a la región de destino, servir localmente
Transferencia local (misma región): A menudo gratis o con coste mínimo
```

**Transferencia de Datos Entre AZs**:
```
Transferencia entre AZs: $0.01/GB en cada dirección
Ejemplo: Replicación de RDS Multi-AZ
500 GB/mes × $0.01 = $5.00/mes (cada dirección)
Total: $10.00/mes para bidireccional

Nota: Esencial para la alta disponibilidad, factúralo en el coste de la arquitectura
```

**VPC Peering**:
```
Misma Región: $0.01/GB
Entre Regiones: $0.02/GB (igual que la transferencia estándar entre regiones)

Ejemplo: Comunicación de microservicios vía VPC peering
1 TB/mes entre VPCs (misma región)
1,000 GB × $0.01 = $10.00/mes
```

#### Ejemplo Completo de Transferencia de Datos

**Aplicación con 20 TB de tráfico mensual**:
```
Escenario 1: Directo desde EC2
Primeros 10 TB: 10,000 × $0.09 = $900.00
Siguientes 10 TB: 10,000 × $0.085 = $850.00
Total: $1,750.00/mes

Escenario 2: Vía CloudFront (optimizado)
Primeros 10 TB: 10,000 × $0.085 = $850.00
Siguientes 10 TB: 10,000 × $0.080 = $800.00
Total: $1,650.00/mes
Ahorro: $100.00/mes + mejor experiencia de usuario

Escenario 3: CloudFront + Caching Regional
Tráfico de CloudFront: 15 TB (tasa de acierto de caché del 75%)
Directo desde el origen: 5 TB
Coste de CloudFront: 15,000 × $0.085 = $1,275.00
Coste de origen: 5,000 × $0.09 = $450.00
Total: $1,725.00/mes

Beneficios adicionales:
- Carga reducida en los servidores de origen
- Entrega de contenido más rápida
- Menor latencia para los usuarios finales
- Protección DDoS incluida
```

**Resumen de Optimización de Transferencia de Datos**:

| Estrategia | Coste Mensual (20 TB) | Ahorro vs Base |
|----------|---------------------|---------------------|
| Directo desde **EC2** | $1,750.00 | Base |
| Solo **CloudFront** | $1,650.00 | 6% ($100) |
| **CloudFront** + optimización de caché | $1,275.00 | 27% ($475) |
| Multi-región con servicio local | $900.00 | 49% ($850) |

**Conclusiones Clave**:
1. Usa siempre **CloudFront** para la entrega de contenido orientado al público
2. Implementa estrategias de almacenamiento en caché agresivas (objetivo de más del 80% de tasa de acierto)
3. Considera el despliegue multi-región para aplicaciones globales
4. Usa **VPC endpoints** para evitar cargos de datos de NAT gateway para servicios de AWS
5. Monitoriza los costes de transferencia de datos en **Cost Explorer** - un gasto que a menudo se pasa por alto

---

## Reserved Instances vs Savings Plans

### Comparación Detallada

#### Reserved Instances (RIs)

**Características**:
- Específico para un servicio (**EC2**, **RDS**, **ElastiCache**, **Redshift**, etc.)
- Vinculado al tipo de instancia, familia, tamaño, región y tenencia
- Se puede modificar (algunos atributos) o intercambiar (**Convertible RIs**)
- Se aplica automáticamente al uso de la instancia coincidente
- Se puede vender en el **Reserved Instance Marketplace**

**Tipos de RIs**:

1. **Standard Reserved Instances**:
   - Mayor descuento (hasta un 75% para 3 años, **All Upfront**)
   - No se puede cambiar el tipo de instancia
   - Se puede cambiar la **AZ**, el alcance (de zonal a regional), el tipo de red
   - Ideal para: Cargas de trabajo estables y predecibles sin necesidad de cambios

2. **Convertible Reserved Instances**:
   - Menor descuento (hasta un 66% para 3 años)
   - Se puede intercambiar por diferentes familias de instancias, tamaños, SO
   - No se puede vender en el **RI Marketplace**
   - Ideal para: Cargas de trabajo predecibles que pueden necesitar flexibilidad

**Opciones de Pago**:
- **All Upfront**: Mayor descuento, paga el importe total por adelantado
- **Partial Upfront**: Descuento medio, paga ~50% por adelantado + mensualidad
- **No Upfront**: Menor descuento, paga solo mensualmente

**Alcance (Scope)**:
- **Regional RI**: Se aplica al uso de instancias en cualquier **AZ** dentro de la región, incluye flexibilidad de **AZ**
- **Zonal RI**: Reserva capacidad en una **AZ** específica, proporciona reserva de capacidad

#### Savings Plans

**Características**:
- Compromiso con un importe de uso constante ($/hora) durante 1 o 3 años
- Más flexible que las **Reserved Instances**
- Se aplica automáticamente al uso elegible
- No se puede vender ni transferir
- Se aplica a todas las cuentas en la facturación consolidada

**Tipos de Savings Plans**:

1. **Compute Savings Plans**:
   - Opción más flexible
   - Hasta un 66% de descuento
   - Se aplica a:
     - Instancias **EC2** (cualquier familia, tamaño, **AZ**, región, SO, tenencia)
     - Cómputo de **Fargate**
     - Cómputo de **Lambda**
   - Se ajusta automáticamente a medida que cambian los patrones de uso
   - Ideal para: Cargas de trabajo dinámicas, uso de cómputo en múltiples servicios

2. **EC2 Instance Savings Plans**:
   - Hasta un 72% de descuento
   - Se aplica al uso de **EC2** dentro de una familia de instancias específica en la región elegida
   - Flexible en tamaños, **AZ**, SO, tenencia dentro de esa familia
   - Ejemplo: Comprométete con la familia **m5** en **us-east-1**, usa cualquier **m5.large**, **m5.xlarge**, etc.
   - Ideal para: Cargas de trabajo específicas de **EC2** con algunas necesidades de flexibilidad

3. **SageMaker Savings Plans**:
   - Hasta un 64% de descuento
   - Se aplica al uso de cómputo de **SageMaker**
   - Flexible en familias de instancias y tamaños

---

### Cálculos de ROI

#### Ejemplo 1: Standard RI vs Compute Savings Plan

**Carga de Trabajo Base**:
- 10 instancias **m5.xlarge** (4 vCPU, 16 GB RAM)
- Funcionando 24/7/365
- Región: **us-east-1**
- Tarifa On-Demand: $0.192/hora por instancia

**Coste Anual On-Demand**:
```
Por instancia: $0.192 × 24 × 365 = $1,681.92/año
Total (10 instancias): $16,819.20/año
```

**Opción 1: Standard RI de 3 Años (All Upfront)**:
```
Coste inicial por instancia: $4,140
Total: $4,140 × 10 = $41,400
Ahorro en 3 años: $50,457.60 - $41,400 = $9,057.60 (18% de ahorro)
Ahorro anual: $3,019.20/año (58% de descuento sobre On-Demand)
```

**Opción 2: Compute Savings Plan de 1 Año (Partial Upfront)**:
```
Compromiso por hora: $1.20/hora (cubre ~$1.70 de valor On-Demand)
Descuento: ~30%
Pago inicial: ~$3,600
Pago mensual: ~$100

Coste anual: $3,600 + ($100 × 12) = $4,800
Anual On-Demand: $16,819.20
Ahorro: $16,819.20 - $4,800 = $12,019.20 (71% de ahorro)

Flexibilidad: Puede cambiar a m6i.xlarge, c5.2xlarge, etc.
Se puede usar en EC2, Fargate, Lambda
```

**Análisis de ROI**:

| Opción | Coste Inicial | Coste Anual | Coste de 3 Años | % de Descuento | Flexibilidad |
|--------|--------------|-------------|-------------|------------|-------------|
| **On-Demand** | $0 | $16,819 | $50,458 | 0% | Total |
| **1-Yr Compute SP** | $3,600 | $4,800 | N/A | 71% | Alta |
| **3-Yr Standard RI** | $41,400 | $13,800 | $41,400 | 58% | Baja |
| **3-Yr Compute SP** | $8,200 | $11,000 | $33,000 | 65% | Alta |

**Árbol de Decisión de Recomendación**:
- ¿Necesitas flexibilidad para cambiar los tipos de instancia? → **Compute Savings Plan**
- ¿Carga de trabajo estable, ahorro máximo? → **Standard Reserved Instance**
- ¿Incertidumbre sobre las necesidades a largo plazo? → **Savings Plan de 1 Año**
- ¿Alta confianza en el uso a 3 años? → **Savings Plan o RI de 3 Años**

#### Ejemplo 2: RDS Reserved Instances

**Base**:
- **db.r5.2xlarge Multi-AZ**
- Región: **us-east-1**
- On-Demand: $1.664/hora
- Coste anual On-Demand: $14,574.40

**RI de 1 Año (Partial Upfront)**:
```
Inicial: $4,850
Mensual: $0.352/hora
Coste mensual: $0.352 × 730 = $257.00

Coste anual: $4,850 + ($257 × 12) = $7,934.00
Ahorro: $14,574.40 - $7,934.00 = $6,640.40 (46% de ahorro)
Ahorro mensual: $553.37/mes

Periodo de ROI: $4,850 inicial ÷ $553.37 ahorro mensual = 8.8 meses
Después de 8.8 meses, la RI se vuelve rentable
```

**RI de 3 Años (All Upfront)**:
```
Pago inicial: $19,780
Sin cargos mensuales

Equivalente anual: $19,780 ÷ 3 = $6,593.33/año
Coste On-Demand de 3 años: $14,574.40 × 3 = $43,723.20
Ahorro en 3 años: $43,723.20 - $19,780 = $23,943.20 (55% de ahorro)
Ahorro anual: $7,981.07/año

Periodo de ROI: $19,780 inicial ÷ ($14,574.40 - $6,593.33) = 2.48 años
Debe mantenerse durante 2.5 años para alcanzar el punto de equilibrio, pero el compromiso es por 3 años
Valor total realizado durante todo el plazo de 3 años
```

**Análisis de Punto de Equilibrio (Break-Even Analysis)**:
```
RI de 1 Año: Punto de equilibrio a los 8.8 meses (seguro, bajo riesgo)
RI de 3 Años: Punto de equilibrio a los 30 meses (requiere confianza en el compromiso)

Si la carga de trabajo se interrumpe prematuramente:
- 1 Año: La pérdida máxima es de ~3.2 meses de ahorro
- 3 Años: La pérdida máxima es el pago inicial completo si se detiene inmediatamente
```

---

### Cuándo usar cada opción

#### Use Standard Reserved Instances cuando:

1. **La carga de trabajo sea estable y predecible**
   - Servidores de bases de datos funcionando 24/7
   - Infraestructura central de aplicaciones
   - Controladores de dominio, servicios de directorio
   - Sistemas de monitorización y registro (logging)

2. **Desees el máximo ahorro**
   - El presupuesto es ajustado, se necesita el mayor descuento
   - Disposición a sacrificar flexibilidad por ahorro de costes
   - Gran confianza en el uso a largo plazo

3. **Necesites reserva de capacidad**
   - Las **Zonal RIs** garantizan capacidad en una **AZ** específica
   - Crítico para requisitos de cumplimiento o de negocio
   - Importante durante periodos de alta demanda

**Ejemplo de caso de uso**:
```
Cluster de base de datos RDS de producción
- Operación requerida 24/7
- Es poco probable que cambie el tipo de instancia
- El pronóstico de 3 años muestra un crecimiento continuo
- Decisión: Standard RI de 3 años para el máximo ahorro
```

#### Use Convertible Reserved Instances cuando:

1. **La carga de trabajo sea predecible pero pueda cambiar**
   - La aplicación puede necesitar diferentes tamaños de instancia
   - Puede ser necesario cambiar de región
   - Se espera una actualización tecnológica durante el plazo

2. **Desees cierta flexibilidad con buenos ahorros**
   - Equilibrio entre ahorro y flexibilidad
   - Cobertura contra cambios en la infraestructura
   - Puede ser necesario adaptarse a nuevos tipos de instancias

**Ejemplo de caso de uso**:
```
Servidores de aplicaciones
- Funcionamiento constante pero puede necesitar optimización
- Se lanzan nuevos tipos de instancias regularmente
- Puede ser necesario migrar a instancias basadas en Graviton
- Decisión: Convertible RI de 1 año para mayor flexibilidad
```

#### Use Compute Savings Plans cuando:

1. **Tengas cargas de trabajo de cómputo diversas**
   - Mezcla de **EC2**, **Fargate**, **Lambda**
   - Múltiples familias y tamaños de instancia
   - Requisitos de escalado dinámico

2. **Valores la máxima flexibilidad**
   - Deseas optimizar sin restricciones
   - Puedes adoptar contenedores o serverless
   - Incertidumbre sobre tipos de instancias específicos

3. **Tengas despliegues multi-región**
   - Los **Savings Plans** se aplican en todas las regiones
   - Puedes cambiar las cargas de trabajo entre regiones
   - Necesitas una gestión simplificada

**Ejemplo de caso de uso**:
```
Arquitectura de microservicios
- Mezcla de EC2 para servicios con estado (stateful)
- Fargate para microservicios contenedorizados
- Lambda para funciones basadas en eventos
- Escalado dinámico según la demanda
- Decisión: Compute Savings Plan para una flexibilidad total
```

#### Use EC2 Instance Savings Plans cuando:

1. **Todo el cómputo esté basado en EC2**
   - Sin uso de **Fargate** o **Lambda**
   - Deseas un descuento mayor que el de **Compute Savings Plan**
   - Comodidad al comprometerse con una familia de instancias

2. **Te estandarices en una familia de instancias específica**
   - La política de la organización utiliza la familia **m5**
   - Familia de instancias consistente en todos los despliegues
   - Necesitas flexibilidad dentro de esa familia

**Ejemplo de caso de uso**:
```
La empresa se estandariza en la familia de instancias m5
- Utiliza varios tamaños de m5 (large, xlarge, 2xlarge)
- Despliegue en múltiples AZs
- Puede cambiar los tamaños basándose en la optimización
- Decisión: EC2 Instance Savings Plan (familia m5, región)
```

#### Use On-Demand Instances cuando:

1. **La carga de trabajo sea impredecible o temporal**
   - Entornos de desarrollo y pruebas
   - Proyectos a corto plazo
   - Trabajo de prueba de concepto (**PoC**)

2. **Necesites la máxima flexibilidad sin compromiso**
   - Start-up explorando AWS
   - Incertidumbre sobre los requisitos a largo plazo
   - Preferencia por el modelo de gastos operativos (**OPEX**)

3. **La carga de trabajo tenga un uso variable**
   - Trabajos por lotes (batch) que se ejecutan ocasionalmente
   - Procesamiento basado en eventos
   - Cargas de trabajo estacionales

**Ejemplo de caso de uso**:
```
Entorno de desarrollo
- Utilizado solo durante el horario laboral
- Cambios y experimentación frecuentes
- Puede apagarse entre proyectos
- Decisión: Instancias On-Demand, apagar cuando no estén en uso
```

#### Use Spot Instances cuando:

1. **La carga de trabajo sea tolerante a fallos**
   - Puede manejar interrupciones
   - Implementa puntos de control (checkpointing)
   - Puede reiniciarse automáticamente

2. **Desees el máximo ahorro de costes**
   - Hasta un 90% de descuento
   - Proyectos con presupuesto limitado
   - El coste es prioridad sobre la disponibilidad

3. **La carga de trabajo tenga tiempos flexibles**
   - Trabajos de procesamiento por lotes (batch)
   - Tareas de análisis de datos
   - Ejecutores de canalizaciones CI/CD
   - Renderizado de vídeo

**Ejemplo de caso de uso**:
```
Procesamiento de big data con Apache Spark
- Los trabajos pueden tener puntos de control
- El cluster puede manejar fallos de nodos
- No es sensible al tiempo (puede tardar horas/días)
- Decisión: Spot Instances con respaldo automático a On-Demand
```

#### Estrategia Híbrida (La más común en la práctica)

**Despliegue de producción típico**:
```
Capacidad base (60%): Reserved Instances o Savings Plans
  - Infraestructura central siempre en funcionamiento
  - Servidores de bases de datos, aplicaciones críticas
  - Máximo ahorro de costes en carga predecible

Capacidad variable (30%): Instancias On-Demand
  - Manejo de picos de tráfico
  - Grupos de auto-escalado
  - Respuesta rápida a la demanda

Procesamiento por lotes (10%): Spot instances
  - Trabajos en segundo plano no críticos
  - Canalizaciones de procesamiento de datos
  - Cómputo optimizado en costes

Ejemplo de desglose de coste de cómputo mensual:
Base (RI): $3,000 (cubriendo $5,000 de equivalente On-Demand)
Variable (On-Demand): $1,500
Lotes (Spot): $150 (cubriendo $1,500 de equivalente On-Demand)
Total: $4,650
Equivalente On-Demand total: $8,000
Ahorro: $3,350/mes (reducción del 42%)
```

**Matriz de decisión**:

| Criterio | Standard RI | Convertible RI | Compute SP | EC2 Instance SP | On-Demand | Spot |
|----------|------------|----------------|------------|-----------------|-----------|------|
| Ahorro Máximo | ✓✓✓ | ✓✓ | ✓✓ | ✓✓✓ | ✗ | ✓✓✓✓ |
| Flexibilidad | ✗ | ✓ | ✓✓✓ | ✓✓ | ✓✓✓✓ | ✓✓ |
| Garantía de Capacidad | ✓ | ✓ | ✗ | ✗ | ✗ | ✗ |
| Entre Servicios | ✗ | ✗ | ✓✓✓ | ✗ | ✓✓✓✓ | ✓ |
| Sin Compromiso | ✗ | ✗ | ✗ | ✗ | ✓✓✓✓ | ✓✓✓✓ |
| Venta/Intercambio | ✓ | ✗ | ✗ | ✗ | N/A | N/A |

---

## Casos de Estudio de Optimización de Costes

### Caso de Estudio 1: Plataforma de Comercio Electrónico

**Perfil de la Empresa**:
- Empresa de comercio electrónico de tamaño medio
- 500,000 usuarios activos mensuales
- Tráfico pico durante las vacaciones (3 veces la carga normal)
- Base de clientes global

**Arquitectura Inicial (Costes Base)**:
```
Desglose de coste mensual:
- EC2 (20 x m5.2xlarge On-Demand 24/7): $5,529.60
- RDS Multi-AZ (db.r5.xlarge): $608.00
- ElastiCache Redis (cache.m5.large): $182.00
- S3 (5 TB de almacenamiento Standard): $115.00
- CloudFront (10 TB de transferencia): $850.00
- Application Load Balancers (2): $62.86
- NAT Gateways (2): $88.20
- CloudWatch, VPC, varios: $150.00

Coste mensual total: $7,585.66
Coste anual: $91,027.92
```

**Problemas Identificados**:
1. Funcionamiento a máxima capacidad las 24/7, incluso durante los periodos de bajo tráfico
2. Ningún uso de **Reserved Instances** o **Savings Plans**
3. Todo el almacenamiento en **S3 Standard**, incluyendo imágenes de productos antiguas
4. Altos costes de **CloudFront** debido a archivos multimedia grandes
5. **ElastiCache** infrautilizado (60% de tiempo de inactividad)
6. Ambos **NAT Gateways** en la misma **AZ** (sin beneficio)

**Estrategia de Optimización**:

**Fase 1: Ajuste de Tamaño (Right-Sizing) y Auto-Escalado (Mes 1)**
```
Acciones:
1. Implementar Auto Scaling:
   - Mínimo: 6 instancias (carga base)
   - Máximo: 24 instancias (carga pico)
   - Promedio: 10 instancias (50% de lo anterior)

2. Ajustar el tamaño de las instancias EC2:
   - El análisis mostró que la CPU estaba al 20-30% de utilización
   - Se cambió de m5.2xlarge a m5.xlarge
   - Reducción del 50% del coste por instancia

3. Optimizar ElastiCache:
   - Reducción de tamaño de cache.m5.large a cache.m5.medium
   - Suficiente para la tasa real de acierto de caché

Resultados:
- EC2: 10 x m5.xlarge promedio = $2,189.00 (60% de ahorro)
- ElastiCache: cache.m5.medium = $91.00 (50% de ahorro)
- Coste mensual: $5,196.06
- Ahorro mensual: $2,389.60 (reducción del 31%)
```

**Fase 2: Capacidad Reservada (Mes 2)**
```
Acciones:
1. Comprar Compute Savings Plan de 1 año:
   - Cubrir la base de 6 instancias
   - Compromiso: $300/mes ($3,600/año)
   - Descuento efectivo: 42%

2. Comprar RDS RI de 1 año (Partial Upfront):
   - Pago inicial: $2,020
   - Pago mensual: $128.00
   - Anual: $3,556 frente a $7,296 On-Demand (51% de ahorro)

Resultados:
- Cómputo: $300 (Savings Plan) + $973 (On-Demand restante)
- RDS: $128 (parte mensual)
- Total cómputo + base de datos: $1,401/mes
- Ahorro adicional: $1,418/mes sobre la Fase 1
```

**Fase 3: Optimización del Almacenamiento (Mes 3)**
```
Acciones:
1. Implementar Políticas de Ciclo de Vida de S3:
   - Día 0-30: S3 Standard (productos activos)
   - Día 31-90: S3 Standard-IA (productos con movimiento lento)
   - Día 91+: S3 Glacier (productos archivados)

2. Habilitar S3 Intelligent-Tiering para patrones de acceso inciertos

3. Comprimir imágenes antes de cargarlas en S3 (reducir el tamaño en un 40%)

Resultados:
- Almacenamiento S3 (3 TB después de la compresión + optimización):
  - 1 TB Standard: $23
  - 1 TB Standard-IA: $12.50
  - 1 TB Glacier: $3.60
  - Total: $39.10 (66% de ahorro desde $115)

- Beneficios de CloudFront:
  - Archivos más pequeños = menos transferencia: 6 TB frente a 10 TB
  - Coste: $510 frente a $850 (40% de ahorro)
```

**Fase 4: Optimización de Red (Mes 4)**
```
Acciones:
1. Reemplazar un NAT Gateway por VPC Endpoints:
   - S3 VPC Endpoint: Gratis
   - DynamoDB VPC Endpoint: Gratis
   - Eliminar los cargos de procesamiento de datos de NAT Gateway

2. Implementar optimizaciones de almacenamiento en caché de CloudFront:
   - Aumentar el TTL de caché para contenido estático
   - Habilitar la compresión
   - Alcanzar una tasa de acierto de caché del 85%

3. Consolidar NAT Gateway a uno por región:
   - Usar solo para servicios fuera de los VPC endpoints

Resultados:
- NAT Gateway: $44.10 (50% de ahorro)
- Ahorro en transferencia de datos: ~$50/mes
```

**Fase 5: Spot Instances para trabajos en segundo plano (Mes 4)**
```
Acciones:
1. Migrar trabajos de procesamiento por lotes a Spot:
   - Procesamiento de imágenes
   - Actualizaciones del índice de búsqueda
   - Trabajos de análisis
   - Usar Spot Fleet con diversos tipos de instancias

2. Implementar puntos de control automatizados:
   - Los trabajos pueden reanudarse si se interrumpen

Resultados:
- Cómputo en segundo plano: 4 instancias equivalentes de cómputo
- Coste On-Demand: $437.92
- Coste Spot: $65.00 (85% de ahorro)
```

**Costes Finales de la Arquitectura Optimizada**:
```
Desglose de coste mensual:
- EC2 (Savings Plan + On-Demand + Auto-Scaling): $1,273.00
- Spot instances (trabajos en segundo plano): $65.00
- RDS (Reserved Instance): $128.00
- ElastiCache (ajustado de tamaño): $91.00
- S3 (optimizado con ciclo de vida): $39.10
- CloudFront (caché optimizada): $510.00
- Application Load Balancers: $62.86
- NAT Gateway (solo 1): $44.10
- VPC Endpoints: $0.00
- CloudWatch, varios: $150.00

Coste mensual total: $2,363.06
Coste anual: $28,356.72
Más pago inicial único: $2,020 (pago inicial de RDS RI)
```

**Resumen de Resultados**:

| Métrica | Antes | Después | Mejora |
|--------|--------|-------|-------------|
| Coste Mensual | $7,585.66 | $2,363.06 | Reducción del 69% |
| Coste Anual | $91,027.92 | $28,356.72 | Ahorro de $62,671.20 |
| Rendimiento | Base | Igual o mejor | Sin degradación |
| Escalabilidad | Fija | Auto-scaling | Mejor manejo de picos |

**Cronograma e Inversión**:
- Tiempo de implementación: 4 meses
- Inversión inicial: $2,020 (RDS RI)
- Coste de mano de obra: ~40 horas de tiempo de ingeniería
- Periodo de ROI: Menos de 1 mes
- Ahorro anual: $62,671.20

**Aprendizajes Clave**:
1. Ajustar el tamaño antes de comprar RIs/SPs (ahorra un 30-40% primero)
2. El auto-escalado elimina el desperdicio durante los periodos de bajo tráfico
3. Las políticas de ciclo de vida de almacenamiento son ahorros del tipo "configurar y olvidar"
4. Los **VPC Endpoints** eliminan costes innecesarios de **NAT Gateway**
5. Las **Spot Instances** son perfectas para trabajos en segundo plano tolerantes a fallos

---

### Caso de Estudio 2: Carga de Trabajo de Análisis de Datos

**Perfil de la Empresa**:
- Empresa de análisis de datos de salud
- Procesan 100 TB de datos médicos mensualmente
- Ejecutan canalizaciones ETL complejas y modelos de ML
- Requisitos de cumplimiento (**HIPAA**)

**Arquitectura Inicial (Costes Base)**:
```
Desglose de coste mensual:
- EMR Cluster (10 x r5.4xlarge, 24/7): $6,307.20
- S3 (100 TB de almacenamiento Standard): $2,300.00
- Solicitudes S3 (miles de millones): $180.00
- Redshift (dc2.8xlarge, 5 nodos): $12,000.00
- Transferencia de datos (replicación entre regiones): $1,200.00
- Trabajos de Glue ETL: $850.00
- Consultas de Athena: $450.00
- QuickSight Enterprise: $250.00

Coste mensual total: $23,537.20
Coste anual: $282,446.40
```

**Problemas Identificados**:
1. El cluster **EMR** funcionando 24/7 a pesar de un horario de trabajo intermitente (8 horas/día de uso real)
2. Todo los datos en **S3 Standard**, incluyendo conjuntos de datos antiguos accedidos rara vez
3. Cluster de **Redshift** sobre-provisionado (40% de utilización promedio)
4. Replicación entre regiones para todos los datos (la mayoría no lo necesita)
5. Ningún uso de **Spot Instances** para los nodos de tareas de **EMR**
6. Los trabajos costosos de **Glue** podrían optimizarse

**Estrategia de Optimización**:

**Fase 1: Optimización de EMR (Mes 1)**
```
Acciones:
1. Convertir EMR en un cluster bajo demanda (ejecutar solo cuando sea necesario):
   - Ejecutar 8 horas/día, 22 días/mes = 176 horas
   - En lugar de 730 horas (24/7)

2. Usar Spot Instances para los nodos de tareas:
   - Nodos core (3): On-Demand r5.2xlarge para fiabilidad
   - Nodos de tareas (10): Spot instances (r5.2xlarge, r5a.2xlarge, r4.2xlarge)

3. Ajustar el tamaño a r5.2xlarge (desde r5.4xlarge):
   - El análisis mostró exceso de capacidad

Resultados:
Nodos core: 3 × r5.2xlarge × $0.504/hr × 176 hrs = $266.11
Nodos de tareas (Spot): 10 × ~$0.15/hr × 176 hrs = $264.00
Total EMR: $530.11/mes (frente a $6,307.20 = 92% de ahorro)
```

**Fase 2: Optimización del Almacenamiento S3 (Mes 1-2)**
```
Acciones:
1. Analizar los patrones de acceso a los datos:
   - Datos calientes (últimos 30 días): 5 TB - Mantener en Standard
   - Datos templados (31-90 días): 15 TB - Mover a Standard-IA
   - Datos fríos (91-365 días): 30 TB - Mover a Glacier Flexible
   - Archivo (365+ días): 50 TB - Mover a Glacier Deep Archive

2. Implementar Intelligent-Tiering para patrones inciertos:
   - Aplicado a 10 TB de datos de acceso variable

3. Habilitar la optimización de solicitudes S3:
   - Operaciones por lotes cuando sea posible
   - Usar S3 Select para reducir la transferencia de datos

Resultados:
- Caliente (5 TB Standard): $115.00
- Templado (15 TB Standard-IA): $187.50
- Frío (30 TB Glacier Flexible): $108.00
- Archivo (50 TB Deep Archive): $50.00
- Intelligent-Tiering (10 TB promedio): $104.00
- Almacenamiento total: $564.50 (frente a $2,300 = 75% de ahorro)
- Costes de solicitudes: $90.00 (frente a $180 = 50% de ahorro mediante el procesamiento por lotes)
```

**Fase 3: Optimización de Redshift (Mes 2)**
```
Acciones:
1. Implementar pausa/reanudación de Redshift:
   - Pausa durante las horas no laborables (16 horas/día)
   - Activo: 8 horas/día × 22 días = 176 horas frente a 730 horas
   - Ahorro: 76% de reducción en el tiempo de ejecución

2. Ajustar el tamaño del cluster:
   - Migrar a RA3.4xlarge (mejor relación precio/rendimiento)
   - Reducir de 5 nodos a 3 nodos
   - RA3 tiene almacenamiento gestionado (paga por lo que usas)

3. Habilitar Concurrency Scaling:
   - Manejar ráfagas de consultas sin cambiar el tamaño del cluster
   - Primera hora gratis por día

Resultados:
- RA3.4xlarge: $3.26/hora por nodo
- 3 nodos × $3.26 × 176 horas = $1,721.28/mes
- Almacenamiento (RA3): 50 TB × $0.024/GB = $1,200/mes
- Total: $2,921.28 (frente a $12,000 = 76% de ahorro)
```

**Fase 4: Optimización de la Transferencia de Datos (Mes 3)**
```
Acciones:
1. Eliminar la replicación entre regiones innecesaria:
   - Identificar los datos que deben replicarse (cumplimiento): 20 TB
   - Mantener los 80 TB restantes en una sola región

2. Usar S3 Batch Replication en lugar de replicación continua:
   - Replicar diariamente en lugar de en tiempo real
   - Suficiente para los requisitos de cumplimiento

3. Comprimir los datos antes de la transferencia:
   - Reducir el volumen de transferencia en un 60%

Resultados:
- Transferencia entre regiones: 20 TB × 40% (comprimido) = 8 TB
- Coste: 8,000 GB × $0.02 = $160/mes (frente a $1,200 = 87% de ahorro)
```

**Fase 5: Optimización de ETL y Consultas (Mes 3-4)**
```
Acciones:
1. Reemplazar algunos trabajos de Glue con Lambda:
   - Transformaciones simples movidas a Lambda
   - Glue reservado para ETL complejo
   - Lambda es más barato para trabajos pequeños y esporádicos

2. Implementar la optimización de consultas de Athena:
   - Particionar los datos por fecha
   - Usar el formato Parquet en lugar de CSV (compresión de 5 veces)
   - Implementar el almacenamiento en caché de resultados

3. Usar el particionamiento de Glue Data Catalog:
   - Reducir los datos escaneados por consulta

Resultados:
- Glue ETL: $320/mes (frente a $850 = 62% de ahorro)
- Lambda ETL: $45/mes (reemplaza $530 de trabajo de Glue)
- Athena: $85/mes (frente a $450 = 81% de ahorro mediante consultas optimizadas)
```

**Fase 6: Capacidad Reservada (Mes 4)**
```
Acciones:
1. Comprar Savings Plan de 1 año para el cómputo base:
   - Cubre Lambda, nodos core de EMR
   - Compromiso: $150/mes
   - 30% de descuento

2. Comprar Redshift RI (1 año, Partial Upfront):
   - Pago inicial: $5,600
   - Reduce la tarifa por hora en un 42%
   - Parte mensual: $700

Resultados:
- Compute Savings Plan: $150/mes
- Redshift con RI: $700/mes + $5,600 de pago inicial
- Ahorro anual adicional: ~$15,000
```

**Costes Finales de la Arquitectura Optimizada**:
```
Desglose de coste mensual:
- EMR Cluster (bajo demanda + Spot): $530.11
- Compute Savings Plan: $150.00
- Almacenamiento S3 (ciclo de vida optimizado): $564.50
- Solicitudes S3 (optimizadas): $90.00
- Redshift (RA3, pausado, RI): $700.00
- Almacenamiento gestionado RA3: $1,200.00
- Transferencia de datos (reducida): $160.00
- Glue ETL (optimizado): $320.00
- Lambda ETL (nuevo): $45.00
- Athena (consultas optimizadas): $85.00
- QuickSight: $250.00

Coste mensual total: $4,094.61
Coste anual: $49,135.32
Más pago inicial único: $5,600 (pago inicial de Redshift RI)
```

**Resumen de Resultados**:

| Métrica | Antes | Después | Mejora |
|--------|--------|-------|-------------|
| Coste Mensual | $23,537.20 | $4,094.61 | Reducción del 83% |
| Coste Anual | $282,446.40 | $49,135.32 | Ahorro de $233,311.08 |
| Coste de EMR | $6,307.20 | $530.11 | Reducción del 92% |
| Coste de Almacenamiento | $2,480.00 | $654.50 | Reducción del 74% |
| Coste de Redshift | $12,000.00 | $1,900.00 | Reducción del 84% |
| Rendimiento de Consultas | Base | 40% más rápido | Mejorado |

**Cronograma e Inversión**:
- Tiempo de implementación: 4 meses
- Inversión inicial: $5,600 (Redshift RI)
- Coste de mano de obra: ~80 horas de tiempo de ingeniería
- Periodo de ROI: Menos de 2 semanas
- Ahorro anual: $233,311.08

**Aprendizajes Clave**:
1. Las cargas de trabajo de análisis rara vez necesitan clusters las 24/7; prográmalas
2. Las **Spot Instances** son perfectas para los nodos de tareas de **EMR** (tolerantes a fallos por diseño)
3. Las políticas de ciclo de vida de datos en grandes conjuntos de datos generan ahorros masivos
4. La pausa/reanudación de **Redshift** es simple pero altamente efectiva
5. Los formatos columnares (**Parquet**) reducen drásticamente los costes de las consultas
6. Las instancias **RA3** ofrecen un mejor TCO para almacenes de datos en crecimiento

---

### Caso de Estudio 3: Entorno de Desarrollo

**Perfil de la Empresa**:
- Empresa de software con 50 desarrolladores
- Múltiples entornos de desarrollo, staging y pruebas
- Los entornos se utilizan principalmente durante el horario laboral
- Necesidad de mantener múltiples entornos de larga duración

**Arquitectura Inicial (Costes Base)**:
```
Desglose de coste mensual (por entorno × 5 entornos):
- EC2 (5 x m5.large, 24/7): $350.40
- RDS (db.t3.medium, Multi-AZ): $101.96
- ElastiCache (cache.t3.small): $24.00
- Application Load Balancer: $31.43
- S3 (500 GB Standard): $11.50
- NAT Gateway: $44.10

Coste por entorno: $563.39/mes
Total (5 entornos): $2,816.95/mes
Coste anual: $33,803.40
```

**Problemas Identificados**:
1. Todos los entornos funcionando 24/7, a pesar de que solo se usan en horario laboral
2. **Multi-AZ RDS** en entornos de desarrollo/pruebas (alta disponibilidad innecesaria)
3. Ningún uso de un programador de instancias para el inicio/parada automáticos
4. Sin diferenciación entre entornos (todos del mismo tamaño)
5. **Application Load Balancers** innecesarios (el acceso directo a **EC2** es suficiente)
6. Todo el almacenamiento en **S3 Standard** (los datos de prueba no necesitan acceso instantáneo)

**Estrategia de Optimización**:

**Fase 1: Implementación del Programador de Instancias (Semana 1)**
```
Acciones:
1. Desplegar AWS Instance Scheduler:
   - Configurar el horario laboral:
     Lunes-Viernes: 8 AM - 7 PM (11 horas)
     Fin de semana: Apagado
   - Tiempo de ejecución mensual: 11 hrs × 22 días = 242 hrs frente a 730 hrs (67% de reducción)

2. Etiquetar todos los recursos de desarrollo con:
   - Schedule: dev-business-hours
   - Environment: dev/test/staging

3. Configurar el inicio/parada automatizados:
   - Las instancias EC2 se inician a las 7:45 AM (pre-calentamiento)
   - Las instancias RDS se inician a las 7:45 AM
   - Todas se detienen a las 7:15 PM

Resultados:
- Las horas de cómputo se redujeron de 730 a 242 (67% de ahorro en el tiempo de ejecución)
- EC2 por entorno: $116.32 (frente a $350.40)
- Total EC2: $581.60/mes (frente a $1,752 = 67% de ahorro)
```

**Fase 2: Ajuste de Tamaño (Right-Size) y Eliminación de Servicios Innecesarios (Semana 2)**
```
Acciones:
1. Diferenciar los tamaños de los entornos:
   - Producción (cuenta separada): Tamaño completo, 24/7
   - Staging: 70% del tamaño de producción, horario laboral
   - Entornos de desarrollo (3): 50% del tamaño de producción, horario laboral
   - Pruebas (Test): 30% del tamaño de producción, solo bajo demanda

2. Reemplazar Multi-AZ RDS por Single-AZ:
   - Los entornos de desarrollo no necesitan una disponibilidad del 99.95%
   - Se puede restaurar desde una instantánea (snapshot) si ocurre un fallo
   - Ahorro inmediato del 50% en los costes de RDS

3. Eliminar los Application Load Balancers:
   - El acceso directo a EC2 es suficiente para los entornos de desarrollo
   - Usar grupos de seguridad para el control de acceso
   - ALB solo es necesario en producción

4. Reemplazar NAT Gateway por instancias NAT (o eliminarlas):
   - Usar instancias NAT t3.nano más pequeñas
   - Solo durante el horario laboral
   - O usar VPC Endpoints donde sea posible

Resultados:
Entorno de Staging:
- EC2: 4 × m5.medium × $0.096 × 242 hrs = $93.00
- RDS: db.t3.small, Single-AZ × 242 hrs = $12.37
- ElastiCache: cache.t3.micro = $8.00
- Total staging: $113.37/mes (frente a $563.39 = 80% de ahorro)

Entorno de desarrollo (×3):
- EC2: 3 × t3.medium × $0.0416 × 242 hrs = $30.23
- RDS: db.t3.micro, Single-AZ × 242 hrs = $4.85
- ElastiCache: cache.t3.micro = $8.00
- Total por desarrollo: $43.08/mes
- Total para los 3 de desarrollo: $129.24/mes

Entorno de pruebas (bajo demanda, 50 hrs/mes):
- EC2: 2 × t3.small × $0.0208 × 50 hrs = $2.08
- RDS: db.t3.micro × 50 hrs = $1.00
- Total pruebas: $3.08/mes
```

**Fase 3: Optimización del Almacenamiento y los Datos (Semana 3)**
```
Acciones:
1. Implementar ciclo de vida de S3 para datos de prueba:
   - Día 0-7: S3 Standard (pruebas activas)
   - Día 8-30: S3 Standard-IA (referencia si es necesario)
   - Día 31+: Eliminar o mover a Glacier

2. Usar instantáneas de EBS para la clonación de entornos:
   - Tomar una instantánea del entorno de desarrollo "maestro" (golden)
   - Clonar entornos desde la instantánea en lugar de ejecutarlos continuamente
   - Eliminar y recrear según sea necesario

3. Usar volúmenes EBS más pequeños:
   - Producción: 100 GB por instancia
   - Desarrollo/Pruebas: 30 GB por instancia (suficiente para la mayoría del trabajo)

Resultados:
- Almacenamiento S3 optimizado: $4.50/mes (frente a $11.50 = 61% de ahorro)
- Almacenamiento EBS reducido: 30 GB × $0.10 × 15 instancias = $45.00
  (frente a 100 GB × 30 instancias = $300.00)
- Almacenamiento de instantáneas: $25.00/mes (configuración única)
```

**Fase 4: Spot Instances para Cargas de Trabajo de Prueba (Semana 4)**
```
Acciones:
1. Usar Spot Instances para:
   - Ejecutores de pruebas automatizados
   - Agentes de canalización CI/CD
   - Pruebas de rendimiento
   - Pruebas de carga

2. Configurar Spot Fleet con diversos tipos de instancias:
   - Solicitar una mezcla de tipos de instancias t3, t3a, m5, m5a
   - Reducir el riesgo de interrupción

3. Implementar reinicio automático tras interrupción:
   - Las pruebas pueden reanudarse automáticamente
   - Ahorro de ~70% en los costes de cómputo de pruebas

Resultados:
- Cómputo de pruebas movido a Spot: $15.00/mes
- Ejecutores CI/CD en Spot: $25.00/mes
- Uso total de Spot: $40.00/mes (frente a $150 On-Demand = 73% de ahorro)
```

**Fase 5: Consolidación de Servicios Compartidos (Mes 2)**
```
Acciones:
1. Consolidar servicios compartidos en todos los entornos:
   - Un único ElastiCache compartido por todos los entornos de desarrollo
   - Una única instancia de RDS con múltiples bases de datos
   - Reduce la sobrecarga de infraestructura

2. Usar **AWS Systems Manager Session Manager**:
   - Eliminar los hosts bastión (**bastion hosts**)
   - Servicio gratuito para un acceso seguro
   - Sin necesidad de instancias **EC2** adicionales

3. Usar **AWS CodeArtifact** para el almacenamiento en caché de paquetes:
   - Reducir los costes de salida (**egress**) de los paquetes
   - Compilaciones más rápidas con almacenamiento en caché local

Resultados:
- **ElastiCache**: 1 cache.t3.small = $24.00 (frente a 5 × $24 = $120)
- Hosts bastión eliminados: $0 (frente a $50/mes)
- **CodeArtifact**: $10/mes (ahorra $30 en salida)
```

**Costes Finales de la Arquitectura Optimizada**:
```
Desglose de coste mensual:
Entorno de Staging (1):
- EC2 (horario laboral, tamaño ajustado): $93.00
- RDS (Single-AZ, horario laboral): $12.37
- Almacenamiento S3: $4.50
- Subtotal: $109.87

Entornos de desarrollo (3):
- EC2 (horario laboral, pequeño): $90.69
- Almacenamiento S3: $13.50
- Subtotal: $104.19

Entorno de pruebas (bajo demanda):
- Spot instances: $40.00
- Subtotal: $40.00

Servicios compartidos:
- ElastiCache (1 compartido): $24.00
- RDS (1 compartido para todo desarrollo): $14.85
- Volúmenes EBS (todos los entornos): $45.00
- CodeArtifact: $10.00
- Snapshots: $25.00
- Subtotal: $118.85

Total Mensual: $372.91
Coste Anual: $4,474.92
```


**Resumen de Resultados**:

| Métrica | Antes | Después | Mejora |
|--------|--------|-------|-------------|
| Coste Mensual | $2,816.95 | $372.91 | Reducción del 87% |
| Coste Anual | $33,803.40 | $4,474.92 | Ahorro de $29,328.48 |
| Por entorno | $563.39 | $74.58 promedio | Reducción del 87% |
| Horas de ejecución | 24/7 (730 hrs) | Horario laboral (242 hrs) | Reducción del 67% |
| Tiempo de actividad requerido | Siempre encendido | Programado | Flexible |

**Cronograma e Inversión**:
- Tiempo de implementación: 1 mes
- Inversión inicial: $0 (no se necesitan **Reserved Instances**)
- Coste de mano de obra: ~20 horas de tiempo de ingeniería
- Periodo de **ROI**: Inmediato
- Ahorro anual: $29,328.48

**Beneficios Adicionales**:
1. Aprovisionamiento de entornos más rápido desde instantáneas (15 min frente a 2 horas)
2. La "imagen maestra" (golden image) consistente reduce la desviación de la configuración
3. Los desarrolladores son más conscientes del uso de recursos
4. Capacidad de lanzar entornos de prueba temporales según sea necesario
5. Reducción de la sobrecarga de gestión con servicios compartidos

**Aprendizajes Clave**:
1. **Instance Scheduler** es simple pero increíblemente efectivo para entornos que no son de producción
2. Los entornos de desarrollo/pruebas no necesitan una disponibilidad de grado de producción
3. Las **Spot Instances** son perfectas para pruebas automatizadas y **CI/CD**
4. Diferenciar los tamaños de los entornos basándose en las necesidades reales
5. El modelo de servicios compartidos funciona bien para los equipos de desarrollo
6. Limpieza regular de recursos no utilizados (instancias de prueba olvidadas, instantáneas antiguas)

**Mejores Prácticas para Entornos de Desarrollo**:
```
1. Implementar el inicio/parada automatizados para todos los recursos que no sean de producción
2. Usar etiquetas (tags) para identificar y rastrear los recursos del entorno
3. Implementar la eliminación automática para entornos de prueba temporales
4. Usar **Spot Instances** para **CI/CD** y pruebas automatizadas
5. Compartir servicios entre entornos cuando sea apropiado
6. Ajustar el tamaño basándose en el uso real, no en las necesidades percibidas
7. Usar **Single-AZ** para bases de datos fuera de producción
8. Implementar la automatización de la limpieza regular (**EBS** no utilizados, instantáneas antiguas)
9. Usar infraestructura como código (**IaC**) para recrear entornos bajo demanda
10. Monitorizar y alertar sobre recursos no utilizados (0% de CPU durante 7+ días = candidato para eliminación)
```

---

### **AWS Pricing Calculator**

**Propósito**: Estimar los costes mensuales de **AWS** antes de desplegar la infraestructura.

**Características**:
- Configurar las especificaciones del servicio y obtener estimaciones de precios
- Crear estimaciones de costes para soluciones completas
- Compartir estimaciones con las partes interesadas mediante una **URL**
- Comparar diferentes configuraciones y modelos de precios
- Exportar estimaciones a **CSV** o **PDF**
- **Uso gratuito**: no se requiere una cuenta de **AWS**

**Casos de Uso**:
- Planificación de nuevos despliegues de carga de trabajo
- Comparación de precios de **Reserved Instance** frente a **On-Demand**
- Estimación de los costes de migración
- Planificación y previsión de presupuestos

**Acceso**: https://calculator.aws

---

### Recorrido por el **TCO Calculator**

**¿Qué es el **TCO** (Total Cost of Ownership)?**:
- Coste total de poseer y operar la infraestructura tecnológica
- Incluye costes visibles y ocultos
- Compara los costes de la infraestructura local (on-premises) frente a la nube de **AWS**
- Ayuda a justificar el caso de negocio para la migración a la nube

**AWS TCO Calculator**: https://awstcocalculator.com (redirige a **Migration Evaluator**)

#### Ejemplo de Cálculo de **TCO**

**Infraestructura Local (**TCO** de 3 años)**:

```
Costes de Hardware:
- Servidores (20 servidores físicos): $120,000
- Almacenamiento (100 TB): $80,000
- Equipamiento de red: $30,000
- Total hardware: $230,000

Costes de Software:
- Licencias de sistema operativo: $40,000
- Licencias de virtualización: $25,000
- Licencias de base de datos: $60,000
- Herramientas de monitorización/gestión: $15,000
- Total software: $140,000

Costes de Instalaciones:
- Espacio en el centro de datos: $45,000 (3 años)
- Energía y refrigeración: $75,000 (3 años)
- Seguridad física: $20,000 (3 años)
- Total instalaciones: $140,000

Costes de Personal:
- Administradores de sistemas (2 FTE × 3 años × $80k): $480,000
- Administradores de almacenamiento (1 FTE × 3 años × $75k): $225,000
- Administradores de red (1 FTE × 3 años × $80k): $240,000
- Total personal: $945,000

Otros Costes:
- Mantenimiento y soporte de hardware: $90,000
- Sitio de recuperación de desastres (**DR**): $120,000
- Seguros: $15,000
- Total otros: $225,000

**TCO** local de 3 años: $1,680,000
Coste anual promedio: $560,000/año
```

**Equivalente en la Nube de **AWS** (**TCO** de 3 años)**:

```
Cómputo (**EC2** con **Savings Plans**):
- 40 instancias virtuales (carga de trabajo equivalente)
- Coste promedio con **Savings Plans**: $8,000/mes
- Coste de 3 años: $288,000

Almacenamiento (**S3**, **EBS**, **Glacier**):
- **S3**: 80 TB con políticas de ciclo de vida: $1,200/mes
- **EBS**: 20 TB: $2,000/mes
- Almacenamiento total: $3,200/mes
- Coste de 3 años: $115,200

Base de Datos (**RDS** con **Reserved Instances**):
- **RDS Multi-AZ** con **RIs**: $2,500/mes
- Coste de 3 años: $90,000

Redes:
- **VPC**, **Load Balancers**, **CloudFront**: $1,500/mes
- Coste de 3 años: $54,000

Monitorización y Gestión:
- **CloudWatch**, **Systems Manager**, **Backup**: $500/mes
- Coste de 3 años: $18,000

Soporte (**Business Support Plan**):
- Estimado: $1,200/mes
- Coste de 3 años: $43,200

Personal (Reducido):
- Ingenieros de **DevOps** (2 FTE × 3 años × $95k): $570,000
- Sin administradores dedicados de almacenamiento/red (servicios gestionados)
- Total personal: $570,000

Formación y Migración:
- Formación y certificaciones de **AWS**: $30,000
- Servicios y herramientas de migración: $50,000
- Total único: $80,000

**TCO** de **AWS** de 3 años: $1,258,400
Coste anual promedio: $419,467/año
```

**Resumen de Comparación de **TCO***:

| Categoría | Local (3 años) | Nube de **AWS** (3 años) | Ahorro |
|----------|------------------|----------------|---------|
  - Amazon Chatbot (Slack/Chime)
- Set multiple alert thresholds (50%, 80%, 100%)
- Budget actions: Automated responses (stop instances, etc.)

**Pricing**:
- **First 2 budgets**: Free
- **Additional budgets**: $0.02/day per budget (~$0.60/month)

**Example Use Case**:
```
Budget Name: Development Team Monthly Budget
Budget Amount: $1,000/month
Alerts:
  - 80% threshold → Email team lead
  - 100% threshold → Email team lead + manager
  - 120% threshold → Trigger Lambda to stop non-production instances
```

---

### AWS Cost and Usage Report

**Purpose**: Most comprehensive and detailed cost and usage data available

**Features**:
- Line-item detail for all AWS costs
- Detailed breakdown of usage and costs by:
  - Service
  - Operation
  - Resource
  - Tag
  - Hour/Day/Month
- Delivered to **S3 bucket** (CSV or Parquet format)
- Integrate with analytics tools:
  - Amazon Athena (query with SQL)
  - Amazon Redshift (data warehousing)
  - Amazon QuickSight (visualization)
- Update frequency: Hourly, daily, or monthly
- Include resource IDs and tags

**Pricing**: **Free** (only pay for S3 storage)

**Use Cases**:
- Deep-dive cost analysis
- Chargeback/showback reporting
- Custom billing reports
- Financial analysis and auditing

---

### AWS Cost Anomaly Detection

**Purpose**: Detect unusual spending patterns using machine learning

**Features**:
- **Machine learning** automatically identifies anomalies
- Root cause analysis for detected anomalies
- Alert via email or SNS when anomalies detected
- Configurable detection sensitivity
- Monitor specific services, accounts, or cost allocation tags
- No manual threshold configuration needed

**Pricing**: **No additional cost**

**How it works**:
1. Analyzes historical spending patterns
2. Identifies unusual spikes or changes
3. Sends alerts with details and root cause
4. Provides recommendations

**Example**: Detects when EC2 costs increase 200% due to accidentally launching large instances

---

## Tagging Strategies for Cost Allocation

### Tag Best Practices
## Estrategias de Etiquetado para la Asignación de Costes

### Mejores Prácticas de Etiquetado

**¿Qué son las **Cost Allocation Tags**?**:
- Pares clave-valor vinculados a recursos de **AWS**
- Utilizados para organizar, rastrear y asignar costes
- Aparecen en **Cost Explorer** y **Cost and Usage Reports**
- Permiten un seguimiento granular de costes y facturación interna (chargeback/showback)

**Tipos de Etiquetas**:

1. **AWS-Generated Tags**:
   - Creadas automáticamente por **AWS**
   - Ejemplos: `aws:createdBy`, `aws:cloudformation:stack-name`
   - No pueden ser editadas ni eliminadas por los usuarios

2. **User-Defined Tags**:
   - Creadas por los usuarios para satisfacer necesidades organizativas
   - Totalmente personalizables
   - Deben activarse en la **Billing Console** para la asignación de costes

**Activación de Etiquetas**:
```
1. Ir a la AWS Billing Console
2. Navegar a Cost Allocation Tags
3. Seleccionar las etiquetas definidas por el usuario para activar
4. Tarda hasta 24 horas en aparecer en Cost Explorer
5. Solo rastrea los costes desde la fecha de activación en adelante
```

**Convenciones de Nombres de Etiquetas**:
```
Formato recomendado: PascalCase o minúsculas con guiones
Ejemplos:
- Environment
- CostCenter
- Project
- Owner
- application-name
- cost-center
- environment-type
```

---

### Esquemas de Etiquetado Comunes

#### 1. Esquema de Etiquetado Financiero

**Propósito**: Asignación de costes, facturación interna (chargeback) e informes financieros.

```
Etiquetas requeridas:
├── CostCenter: "CC-12345" (código de coste del departamento)
├── Project: "ProjectAlpha" (nombre/código del proyecto)
├── Owner: "john.doe@company.com" (propietario del recurso)
├── BillingGroup: "Engineering" (grupo al que se le cargará el coste)
└── Environment: "Production" (producción, desarrollo, staging, pruebas)

Etiquetas opcionales:
├── Budget: "Q1-2024-Infrastructure"
├── Invoice: "Customer-XYZ" (para facturación a clientes)
└── PurchaseOrder: "PO-789456"
```

**Ejemplo de Aplicación**:
```
Instancia EC2:
  Name: web-server-01
  CostCenter: CC-12345
  Project: CustomerPortal
  Owner: jane.smith@company.com
  BillingGroup: ProductTeam
  Environment: Production

Vista de Cost Explorer: Filtrar por CostCenter = CC-12345
Resultado: Muestra todos los costes atribuidos a ese centro de costes.
Informe mensual: Enviar los costes por CostCenter al equipo de finanzas por correo electrónico.
```

#### 2. Esquema de Etiquetado Técnico

**Propósito**: Organización de recursos, automatización y gestión operativa.

```
Etiquetas requeridas:
├── Application: "CustomerPortal"
├── Component: "WebServer" (DB, API, Frontend, etc.)
├── Version: "v2.5.3"
├── ManagedBy: "Terraform" (CloudFormation, Manual, etc.)
└── Environment: "Production"

Etiquetas opcionales:
├── DataClassification: "Confidential" (Público, Interno, Restringido)
├── Compliance: "HIPAA,SOC2"
├── Backup: "Daily" (política de retención)
├── MaintenanceWindow: "Sun-03:00-05:00"
└── MonitoringLevel: "Critical" (determina el umbral de alerta)
```

**Ejemplo de Aplicación**:
```
Base de datos RDS:
  Name: customerdb-prod
  Application: CustomerPortal
  Component: Database
  Version: PostgreSQL-13.7
  ManagedBy: Terraform
  Environment: Production
  DataClassification: Confidential
  Compliance: HIPAA,PCI-DSS
  Backup: Hourly

Automatización: Detener todos los recursos donde Environment=Dev a las 7 PM.
Monitorización: Alertas críticas para recursos con MonitoringLevel=Critical.
Informe de cumplimiento: Listar todos los recursos etiquetados como HIPAA.
```

#### 3. Esquema de Etiquetado de Negocio

**Propósito**: Alineación con el negocio y seguimiento estratégico.

```
Etiquetas requeridas:
├── BusinessUnit: "Sales" (o Ingeniería, Marketing, etc.)
├── Product: "CRM-Suite"
├── Customer: "Enterprise-Client-A" (para multi-inquilino)
├── ServiceLevel: "Gold" (Oro, Plata, Bronce)
└── RevenueStream: "Subscription"

Etiquetas opcionales:
├── Criticality: "Mission-Critical" (Alta, Media, Baja)
├── Stakeholder: "vp-sales@company.com"
└── BusinessImpact: "Customer-Facing" (Cara al cliente)
```

**Ejemplo de Aplicación**:
```
Bucket de S3:
  Name: customer-data-bucket
  BusinessUnit: Sales
  Product: CRM-Suite
  Customer: Enterprise-Client-A
  ServiceLevel: Gold
  RevenueStream: Subscription
  Criticality: Mission-Critical

Informes: Costes totales de AWS por producto.
Facturación interna: Asignar costes a las etiquetas de Customer para la facturación.
Monitorización de SLA: Los recursos Mission-Critical obtienen monitorización 24/7.
```

#### 4. Esquema Empresarial Integral

**Enfoque combinado para grandes organizaciones**:

```
Etiquetas obligatorias (aplicadas mediante AWS Config/SCPs):
├── CostCenter: "CC-12345"
├── Owner: "email@company.com"
├── Environment: "Production|Staging|Development|Test"
├── Application: "app-name"
└── ManagedBy: "Terraform|CloudFormation|Manual"

Etiquetas financieras:
├── Project: "project-code"
├── BillingGroup: "group-name"
└── Budget: "budget-id"

Etiquetas técnicas:
├── Component: "component-type"
├── Version: "version-number"
├── Backup: "policy-name"
└── Compliance: "compliance-frameworks"

Etiquetas de negocio:
├── BusinessUnit: "unit-name"
├── Criticality: "Critical|High|Medium|Low"
└── DataClassification: "Public|Internal|Confidential|Restricted"

Etiquetas operativas:
├── MaintenanceWindow: "schedule"
├── MonitoringLevel: "level"
└── AutoShutdown: "Yes|No"
```

**Ejemplo de Política de Gobernanza de Etiquetas**:
```yaml
TagPolicy:
  MandatoryTags:
    - CostCenter: "^CC-[0-9]{5}$"
    - Owner: "^[a-z.]+@company\.com$"
    - Environment: "^(Production|Staging|Development|Test)$"
    - Application: "^[A-Za-z0-9-]+$"

  EnforcementLevel: "Hard" # Bloquear la creación de recursos si faltan etiquetas

  ValidValues:
    Environment:
      - Production
      - Staging
      - Development
      - Test
    Criticality:
      - Mission-Critical
      - High
      - Medium
      - Low
```

---

### Aplicación de Etiquetas (Tag Enforcement)

#### 1. AWS Config Rules

**Propósito**: Detectar y alertar automáticamente sobre recursos no conformes.

```
Regla de Config: required-tags
Verificación: Todas las instancias EC2 deben tener las etiquetas:
  - CostCenter
  - Owner
  - Environment

Acción en caso de incumplimiento:
- Enviar notificación SNS
- Crear informe de cumplimiento
- Activar una función Lambda de corrección
```

**Ejemplo de Regla de Config**:
```json
{
  "ConfigRuleName": "required-tags",
  "Description": "Verifica que los recursos tengan las etiquetas requeridas",
  "Source": {
    "Owner": "AWS",
    "SourceIdentifier": "REQUIRED_TAGS"
  },
  "InputParameters": {
    "tag1Key": "CostCenter",
    "tag2Key": "Owner",
    "tag3Key": "Environment"
  },
  "Scope": {
    "ComplianceResourceTypes": [
      "AWS::EC2::Instance",
      "AWS::RDS::DBInstance",
      "AWS::S3::Bucket"
    ]
  }
}
```

#### 2. Service Control Policies (SCPs)

**Propósito**: Impedir la creación de recursos sin las etiquetas requeridas.

**Ejemplo de SCP**:
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "DenyEC2WithoutRequiredTags",
      "Effect": "Deny",
      "Action": [
        "ec2:RunInstances"
      ],
      "Resource": [
        "arn:aws:ec2:*:*:instance/*"
      ],
      "Condition": {
        "StringNotLike": {
          "aws:RequestTag/CostCenter": "*",
          "aws:RequestTag/Owner": "*",
          "aws:RequestTag/Environment": "*"
        }
      }
    }
  ]
}
```

**Efecto**: Los usuarios no pueden crear instancias **EC2** sin las etiquetas requeridas.

#### 3. Políticas IAM para la aplicación de etiquetas

**Ejemplo de Política IAM**:
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "RequireTagsOnCreate",
      "Effect": "Deny",
      "Action": [
        "ec2:RunInstances",
        "rds:CreateDBInstance",
        "s3:CreateBucket"
      ],
      "Resource": "*",
      "Condition": {
        "Null": {
          "aws:RequestTag/CostCenter": "true"
        }
      }
    },
    {
      "Sid": "PreventTagDeletion",
      "Effect": "Deny",
      "Action": [
        "ec2:DeleteTags"
      ],
      "Resource": "*",
      "Condition": {
        "ForAnyValue:StringEquals": {
          "aws:TagKeys": [
            "CostCenter",
            "Owner",
            "Environment"
          ]
        }
      }
    }
  ]
}
```

**Efecto**:
- Impide la creación de recursos sin la etiqueta **CostCenter**.
- Impide la eliminación de etiquetas críticas.

#### 4. Corrección Automatizada de Etiquetas (Automated Tag Remediation)

**Función Lambda para el etiquetado automático**:
```python
import boto3
import json

def lambda_handler(event, context):
    """Etiqueta automáticamente las instancias EC2 con el propietario basado en el usuario IAM"""
    ec2 = boto3.resource('ec2')

    # Obtener el ID de la instancia del evento de CloudWatch
    instance_id = event['detail']['instance-id']
    instance = ec2.Instance(instance_id)

    # Obtener el usuario IAM que lanzó la instancia
    iam_user = event['detail']['userIdentity']['principalId'].split(':')[1]

    # Aplicar etiquetas predeterminadas
    instance.create_tags(
        Tags=[
            {'Key': 'Owner', 'Value': f'{iam_user}@company.com'},
            {'Key': 'AutoTagged', 'Value': 'true'},
            {'Key': 'CreatedDate', 'Value': event['detail']['time']}
        ]
    )

    return {
        'statusCode': 200,
        'body': json.dumps(f'Tagged instance {instance_id}')
    }
```

**CloudWatch Event Rule** (activa Lambda al lanzar una EC2):
```json
{
  "source": ["aws.ec2"],
  "detail-type": ["EC2 Instance State-change Notification"],
  "detail": {
    "state": ["running"]
  }
}
```

#### 5. Panel de Control de Cumplimiento de Etiquetas (Tag Compliance Dashboard)

**Uso de AWS Tag Editor**:
```
1. Navegar a AWS Resource Groups & Tag Editor.
2. Crear una búsqueda de recursos a los que les falten las etiquetas requeridas.
3. Filtrar por:
   - Tipo de recurso: Todos (All)
   - Etiquetas: CostCenter (no existe)
4. Los resultados muestran todos los recursos no conformes.
5. Aplicación de etiquetas de forma masiva (Bulk tag) disponible.
```

**Informe de Cumplimiento Automatizado**:
```python
import boto3
from datetime import datetime

def generate_tag_compliance_report():
    """Genera un informe de recursos sin las etiquetas requeridas"""
    required_tags = ['CostCenter', 'Owner', 'Environment']

    ec2 = boto3.client('ec2')
    non_compliant = []

    instances = ec2.describe_instances()

    for reservation in instances['Reservations']:
        for instance in reservation['Instances']:
            instance_tags = {tag['Key']: tag['Value']
                           for tag in instance.get('Tags', [])}

            missing_tags = [tag for tag in required_tags
                          if tag not in instance_tags]

            if missing_tags:
                non_compliant.append({
                    'InstanceId': instance['InstanceId'],
                    'MissingTags': missing_tags,
                    'LaunchTime': instance['LaunchTime']
                })

    # Enviar informe al equipo de cumplimiento por correo electrónico
    return {
        'ReportDate': datetime.now().isoformat(),
        'NonCompliantResources': len(non_compliant),
        'Details': non_compliant
    }
```

---

## Configuración de Facturación Multi-Cuenta

### Estructura de la Organización

**Estrategia Multi-Cuenta recomendada**:

```
Management Account (Payer Account - Cuenta Pagadora)
├── Unidades Organizativas (OUs)
│   ├── Production OU (Producción)
│   │   ├── Prod-Application-Account
│   │   ├── Prod-Database-Account
│   │   └── Prod-Security-Account
│   ├── Non-Production OU (No Producción)
│   │   ├── Dev-Account
│   │   ├── Staging-Account
│   │   └── Test-Account
│   ├── Infrastructure OU (Infraestructura)
│   │   ├── Shared-Services-Account
│   │   ├── Networking-Account
│   │   └── Logging-Account
│   └── Security OU (Seguridad)
│       ├── Security-Audit-Account
│       ├── Security-Tools-Account
│       └── Compliance-Account
```

**Beneficios de la separación de cuentas**:
1. **Aislamiento de seguridad**: Contención del radio de explosión (**Blast radius**).
2. **Seguimiento de costes**: Atribución clara de costes por cuenta.
3. **Límites de recursos**: Cuotas de servicio separadas por cuenta.
4. **Cumplimiento**: Más fácil cumplir con los requisitos regulatorios.
5. **Autonomía del equipo**: Control de acceso independiente por equipo.
6. **Facturación simplificada**: Costes agrupados naturalmente por cuenta.

---

### Mejores Prácticas

#### 1. Seguridad de la Management Account

**Lo que se debe hacer**:
- Usar ÚNICAMENTE para la facturación y la gestión de la organización.
- Habilitar **MFA** en la cuenta raíz (**root account**).
- Habilitar **AWS CloudTrail** en todas las regiones.
- Configurar alertas de facturación.
- Configurar la facturación consolidada (**consolidated billing**).
- Aplicar **SCPs** a las **OUs**.

**Lo que NO se debe hacer**:
- NO ejecutar cargas de trabajo de producción en la **management account**.
- NO compartir las credenciales de la **management account**.
- NO crear recursos a menos que sea absolutamente necesario.
- NO otorgar permisos **IAM** amplios.

**Ejemplo de política para la Management Account**:
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "DenyResourceCreation",
      "Effect": "Deny",
      "Action": [
        "ec2:RunInstances",
        "rds:CreateDBInstance",
        "s3:CreateBucket"
      ],
      "Resource": "*",
      "Condition": {
        "StringNotEquals": {
          "aws:PrincipalAccount": "111111111111"
        }
      }
    }
  ]
}
```

#### 2. Nomenclatura y Etiquetado de Cuentas

**Convención de Nomenclatura**:
```
Formato: [Entorno]-[Propósito]-[Región]
Ejemplos:
- prod-webapp-useast1
- dev-dataplatform-euwest1
- shared-networking-global
- security-audit-global
```

**Etiquetas de Cuenta** (aplicadas a las cuentas en **AWS Organizations**):
```
Requeridas:
├── Environment: Production|Development|Staging|Test
├── CostCenter: CC-12345
├── Owner: team-email@company.com
└── Purpose: Application|Infrastructure|Security

Opcionales:
├── Compliance: HIPAA|PCI-DSS|SOC2
├── DataClassification: Confidential|Internal
└── BusinessUnit: Engineering|Sales|Marketing
```

#### 3. Service Control Policies (SCPs)

**Ejemplo: Impedir el uso de regiones fuera de las regiones aprobadas**:
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "DenyAllOutsideApprovedRegions",
      "Effect": "Deny",
      "NotAction": [
        "cloudfront:*",
        "iam:*",
        "route53:*",
        "support:*"
      ],
      "Resource": "*",
      "Condition": {
        "StringNotEquals": {
          "aws:RequestedRegion": [
            "us-east-1",
            "us-west-2",
            "eu-west-1"
          ]
        }
      }
    }
  ]
}
```

**Ejemplo: Requerir cifrado**:
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "DenyUnencryptedS3Upload",
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
```

#### 4. Registro (Logging) y Monitorización Centralizados

**Arquitectura**:
```
Todas las cuentas miembros
├── CloudTrail logs → S3 en la Logging Account
├── VPC Flow Logs → S3 en la Logging Account
├── CloudWatch Logs → Suscripción entre cuentas (cross-account)
├── GuardDuty findings → Security Account
└── Config findings → Security Account

Logging Account (Centralizada):
├── S3 Bucket: organization-cloudtrail-logs
├── S3 Bucket: organization-flowlogs
├── Athena: Consultar logs en todas las cuentas
└── Lifecycle: Archivar logs en Glacier después de 90 días
```

**Beneficios**:
- Única fuente de verdad para todos los logs.
- Impide la manipulación de logs a nivel de cuenta.
- Auditoría de cumplimiento simplificada.
- Monitorización de seguridad centralizada.
- Optimización de costes (única política de ciclo de vida de **S3**).

#### 5. Estrategia de Asignación de Costes

**Estrategia de Cuenta Vinculada (Linked Account Strategy)**:
```
Estructura de Cuentas:
├── prod-customer-portal (Aplicación de portal del cliente)
├── prod-mobile-api (Backend de API móvil)
├── dev-all-projects (Todo el trabajo de desarrollo)
└── shared-services (Infraestructura compartida)

Asignación de Costes:
1. Cada cuenta representa un centro de costes.
2. Etiquetar recursos dentro de las cuentas para una asignación secundaria.
3. Usar Cost Explorer para agrupar por Cuenta + Etiquetas.
4. Informes mensuales enviados automáticamente a los propietarios de las cuentas.
```

**Asignación secundaria basada en etiquetas**:
```
Dentro de la cuenta prod-customer-portal:
├── Recursos etiquetados: Project=Feature-A → Asignar al Equipo A
├── Recursos etiquetados: Project=Feature-B → Asignar al Equipo B
└── Recursos sin etiqueta → Asignar a gastos compartidos (overhead)

Proceso mensual:
1. Filtros de Cost Explorer por Cuenta = prod-customer-portal.
2. Agrupar por etiqueta: Project.
3. Exportar informe a CSV.
4. El equipo de finanzas asigna los costes a sus respectivos equipos.
```

#### 6. Uso compartido de Reserved Instance y Savings Plan

**Uso compartido automático** (comportamiento predeterminado):
```
Escenario:
- Cuenta de Producción: Compra 10 x m5.large RIs.
- Cuenta de Desarrollo: Ejecuta 5 x m5.large instancias.
- Servicios Compartidos: Ejecuta 3 x m5.large instancias.

Resultado:
- Producción utiliza 10 RIs.
- Si Producción solo utiliza 7, las 3 RIs restantes se aplican automáticamente a:
  - Cuenta de Desarrollo (3 instancias obtienen el precio de RI).
- Si aún quedan sin usar, se aplican a otras cuentas vinculadas.

Beneficio: Maximiza la utilización de RI en toda la organización.
```

**Desactivar el uso compartido de RI** (si es necesario):
```
1. Ir a Billing Console > Preferences.
2. Desmarcar "RI Sharing".
3. Las RIs solo se aplican dentro de la cuenta que realizó la compra.

Caso de uso: Se desea aislar los costes completamente por cuenta.
```

#### 7. Informes de Facturación Consolidada

**Estructura de Informes Mensuales**:
```
La Management Account recibe:
├── Factura consolidada para toda la organización.
├── Desglose de partidas por cuenta vinculada.
├── Informes de utilización y cobertura de RI/SP.
└── Recomendaciones para la optimización de costes.

Cada propietario de cuenta vinculada recibe:
├── Los costes específicos de su cuenta.
├── Tendencias de costes y anomalías.
├── Alertas de presupuesto (si están configuradas).
└── Recomendaciones específicas para sus recursos.
```

**Distribución de Informes Automatizada**:
```python
import boto3
from datetime import datetime, timedelta

def distribute_cost_reports():
    """Envía informes de costes mensuales a los propietarios de las cuentas"""
    ce = boto3.client('ce')
    sns = boto3.client('sns')

    # Obtener los costes por cuenta vinculada del último mes
    end_date = datetime.now().replace(day=1)
    start_date = (end_date - timedelta(days=1)).replace(day=1)

    response = ce.get_cost_and_usage(
        TimePeriod={
            'Start': start_date.strftime('%Y-%m-%d'),
            'End': end_date.strftime('%Y-%m-%d')
        },
        Granularity='MONTHLY',
        Metrics=['UnblendedCost'],
        GroupBy=[
            {
                'Type': 'DIMENSION',
                'Key': 'LINKED_ACCOUNT'
            }
        ]
    )

    for group in response['ResultsByTime'][0]['Groups']:
        account_id = group['Keys'][0]
        cost = group['Metrics']['UnblendedCost']['Amount']

        # Enviar SNS al propietario de la cuenta
        sns.publish(
            TopicArn=f'arn:aws:sns:us-east-1:111111111111:account-{account_id}-billing',
            Subject=f'Monthly AWS Cost Report - {start_date.strftime("%B %Y")}',
            Message=f'Total cost for account {account_id}: ${cost}'
        )
```

---

### Asignación de Costes (Cost Allocation)

#### 1. Categorías de Costes (Cost Categories)

**¿Qué son las Categorías de Costes?**:
- Agrupaciones personalizadas de costes que reflejan la estructura de su organización.
- Más flexibles que las etiquetas por sí solas.
- Pueden combinar múltiples reglas (etiquetas, cuentas, servicios, tipos de cargo).
- Categorización jerárquica.

**Ejemplo de Estructura de Categoría de Costes**:
```
Categoría de Coste: Departamento
├── Ingeniería
│   ├── Regla 1: IDs de cuenta (111111111111, 222222222222)
│   ├── Regla 2: Etiqueta CostCenter = CC-ENG-*
│   └── Regla 3: Etiqueta Team = Backend|Frontend|DevOps
├── Ventas
│   ├── Regla 1: ID de cuenta (333333333333)
│   └── Regla 2: Etiqueta CostCenter = CC-SALES-*
└── Marketing
    ├── Regla 1: Etiqueta CostCenter = CC-MKT-*
    └── Regla 2: Etiqueta Campaign = *
```

**Creación de Categorías de Costes** (Consola de AWS):
```
1. Ir a Billing Console > Cost Categories.
2. Crear categoría: "Departamento".
3. Definir reglas:
   - Ingeniería: (Cuenta = 111111111111 O Etiqueta:Team = Backend)
   - Ventas: (Etiqueta:CostCenter empieza por CC-SALES)
   - Marketing: (Etiqueta:BusinessUnit = Marketing)
4. Establecer la categoría predeterminada para costes no coincidentes.
5. Guardar y activar.
```

**Beneficios**:
- Categorización automática de costes sin etiquetado manual.
- Combinación de asignaciones a nivel de cuenta y a nivel de etiqueta.
- Manejo de categorización heredada/predeterminada.
- Mantenimiento de las categorías incluso cuando cambian los recursos.

#### 2. Chargeback vs Showback

**Chargeback (Facturación Interna)**:
- Facturación real a los departamentos/equipos.
- Los departamentos pagan por su uso de **AWS** desde su propio presupuesto.
- Requiere una asignación de costes detallada y un proceso de aprobación.
- A menudo se utiliza para centros de beneficios o clientes externos.

**Ejemplo de Proceso de Chargeback**:
```
Proceso Mensual:
1. Se genera el **Cost and Usage Report** con etiquetas.
2. Los costes se asignan por etiqueta de **CostCenter**.
3. Finanzas crea facturas internas por departamento.
4. Los departamentos concilian con sus presupuestos.
5. Los costes se deducen de los presupuestos de los departamentos.

Departamento de Ingeniería:
- Costes de AWS: $50,000
- Asignado al presupuesto de Ingeniería.
- Finanzas deduce $50,000 del presupuesto de Ingeniería.
```

**Showback (Visibilidad de Costes)**:
- Solo con fines informativos, sin facturación real.
- Muestra a los departamentos lo que están consumiendo.
- Promueve la conciencia de los costes sin impacto presupuestario.
- A menudo se utiliza durante la fase de adopción de la nube.

**Ejemplo de Proceso de Showback**:
```
Proceso Mensual:
1. Se generan informes de costes por equipo.
2. Los equipos reciben visibilidad de sus costes.
3. Sin impacto presupuestario ni facturación interna.
4. Se utiliza para promover un comportamiento consciente de los costes.

Departamento de Ingeniería:
- Costes de AWS: $50,000
- Informe enviado al liderazgo de ingeniería.
- Sin deducción de presupuesto.
- Conciencia de los patrones de consumo.
```

**Enfoque Híbrido** (el más común):
```
Chargeback para:
- Cargas de trabajo de producción (atribución directa de ingresos).
- Entornos de clientes externos.
- Asignaciones claras basadas en proyectos.

Showback para:
- Entornos de desarrollo y pruebas.
- Servicios compartidos (difíciles de asignar con precisión).
- Proyectos exploratorios/innovación.
```

#### 3. Reglas de Cargo Dividido (Split Charge Rules)

**Propósito**: Asignar costes compartidos entre múltiples equipos/proyectos.

**Ejemplo: Base de datos compartida**:
```
Escenario:
- Una instancia **RDS** compartida cuesta $1,000/mes.
- Utilizada por 3 aplicaciones:
  - App A: 50% de las consultas.
  - App B: 30% de las consultas.
  - App C: 20% de las consultas.

Regla de división:
Instancia **RDS** etiquetada como "Shared=true".
Asignación de costes:
- 50% → App A (CostCenter: CC-APP-A)
- 30% → App B (CostCenter: CC-APP-B)
- 20% → App C (CostCenter: CC-APP-C)

Resultado en **Cost Explorer**:
- App A ve $500 atribuidos a ellos.
- App B ve $300 atribuidos a ellos.
- App C ve $200 atribuidos a ellos.
```

**Regla de división de Categorías de Costes de AWS**:
```
Categoría de Coste: Aplicación
├── App-A (CC-APP-A): 50% de los costes de SharedDB
├── App-B (CC-APP-B): 30% de los costes de SharedDB
└── App-C (CC-APP-C): 20% de los costes de SharedDB

Definición de la regla:
SI el recurso tiene la etiqueta "Shared=true" Y el servicio="Amazon RDS"
  ENTONCES dividir el coste:
    - 50% a la categoría App-A
    - 30% a la categoría App-B
    - 20% a la categoría App-C
```

#### 4. Asignación de Costes de Reserved Instance

**Uso compartido de descuentos de RI**:
```
Escenario:
- Cuenta A (Producción): Compra 20 RIs.
- Cuenta A solo utiliza 15 RIs.
- Cuenta B (Desarrollo): Utiliza 5 instancias coincidentes.

Asignación de costes:
- A la Cuenta A se le cobran las 20 RIs (pago inicial + recurrente).
- La Cuenta B recibe el descuento de RI en 5 instancias automáticamente.
- La factura de la Cuenta B refleja la tarifa descontada.
- La Cuenta A ve "horas de RI no utilizadas" en el informe de utilización.

Opción 1: Mantener tal cual
- La Cuenta B se beneficia de la compra de la Cuenta A.
- No se necesita reasignación.

Opción 2: Facturación interna del ahorro de RI
- Finanzas calcula el ahorro de RI de la Cuenta B.
- A la Cuenta B se le cobra internamente por el beneficio del ahorro.
- La Cuenta A recibe un crédito por proporcionar las RIs.
```

**Seguimiento de la utilización de RI**:
```
El informe mensual incluye:
├── Utilización de RI por cuenta.
├── Porcentaje de cobertura de RI.
├── Horas de RI desperdiciadas (compradas pero no usadas).
└── Asignación de costes (qué cuenta se benefició de las RIs).

Acciones a tomar:
- Si la Cuenta A tiene una utilización baja → Considerar vender las RIs.
- Si la Cuenta B se beneficia con frecuencia → Considerar comprar sus propias RIs.
- Optimizar la estrategia de RI a nivel de cuenta frente a la organizacional.
```

---

## Facturación Consolidada y AWS Organizations

### Facturación Consolidada

**¿Qué es?**: Una característica de **AWS Organizations** que combina la facturación de múltiples cuentas de **AWS**.

**Beneficios**:

1. **Factura única**: Un único método de pago para todas las cuentas de la organización.
2. **Descuentos por volumen**: Uso combinado de todas las cuentas para precios por niveles (**tiered pricing**).
   - Si la Cuenta A usa 8 TB de almacenamiento **S3** y la Cuenta B usa 4 TB, obtiene el precio para un total de 12 TB.
3. **Seguimiento fácil**: Seguimiento de cargos por cuenta mientras se paga de forma centralizada.
4. **Uso compartido de la capa gratuita (Free Tier)**: La capa gratuita se aplica una vez por organización (no por cuenta).
5. **Uso compartido de Reserved Instance**: Las **RIs** se pueden compartir entre cuentas.
6. **Sin coste adicional**: Característica gratuita de **AWS Organizations**.

**Estructura de Cuentas**:
```
Management Account (Pagadora)
├── Production Account
├── Development Account
├── Testing Account
└── Security Account
```

**Casos de Uso**:
- Grandes organizaciones con múltiples departamentos.
- Entornos separados (producción, desarrollo, pruebas).
- Asignación de costes por equipo o proyecto.
- Gestión de facturación centralizada.

---

## Profundización en Cost Anomaly Detection

### Configuración

**Configuración paso a paso**:

1. **Navegar a Cost Anomaly Detection**:
   ```
   Consola de AWS > Billing > Cost Anomaly Detection
   ```

2. **Crear un Monitor de Costes (Cost Monitor)**:
   ```
   Tipos de Monitor:
   ├── AWS Services: Monitoriza todos los servicios de AWS.
   ├── Linked Account: Monitoriza cuentas específicas.
   ├── Cost Category: Monitoriza por categoría de coste.
   └── Cost Allocation Tag: Monitoriza por etiquetas específicas.
   ```

3. **Configurar la sensibilidad de detección**:
   ```
   Niveles de sensibilidad:
   ├── Low (Baja): Solo detecta anomalías significativas (> 50% de desviación).
   ├── Medium (Media): Anomalías moderadas (> 25% de desviación) [PREDETERMINADO].
   ├── High (Alta): Detecta pequeñas anomalías (> 10% de desviación).
   ```

4. **Configurar suscriptores de alertas**:
   ```
   Métodos de alerta:
   ├── Correo electrónico: Notificaciones directas por email.
   ├── SNS Topic: Publicar en SNS para automatización.
   ├── AWS Chatbot: Enviar a Slack o Chime.
   ```

5. **Configurar umbrales de alerta**:
   ```
   Umbrales disponibles:
   ├── Monto en dólares: Alerta si la anomalía es > $X.
   ├── Porcentaje: Alerta si es > X% del gasto total.
   ├── Ambos: Debe cumplir ambos criterios.
   ```

**Ejemplo de Configuración**:
```
Nombre del monitor: Production-Services-Monitor
Tipo de monitor: AWS Services
Servicios: EC2, RDS, S3, Lambda
Sensibilidad: Media (>25% de desviación)

Suscripción de alerta:
├── Correo electrónico: ops-team@company.com
├── SNS Topic: arn:aws:sns:us-east-1:111111111111:cost-anomalies
└── Umbral: $100 o 10% del gasto diario
```

---

### Ejemplos de Alertas

#### Ejemplo 1: Pico de costes de EC2

**Alerta recibida**:
```
Anomalía de coste detectada

Servicio: Amazon EC2
Región: us-east-1
Periodo de la anomalía: 2024-01-15

Gasto esperado: $500/día
Gasto real: $2,100/día
Monto de la anomalía: +$1,600 (320% de aumento)

Análisis de la causa raíz:
- Se lanzaron 15 nuevas instancias r5.8xlarge a las 02:00 UTC.
- Las instancias siguen en ejecución (no se terminaron como se esperaba).
- Lanzadas por el usuario IAM: john.doe@company.com.
- Asociadas con el grupo de AutoScaling: web-app-asg-prod.

Acciones recomendadas:
1. Verificar si las instancias son necesarias.
2. Comprobar las políticas de AutoScaling.
3. Terminar las instancias innecesarias.
4. Revisar los permisos IAM de este usuario.
```

**Pasos de investigación**:
```
1. Comprobar la consola de EC2:
   - Filtrar por hora de lanzamiento: últimas 24 horas.
   - Identificar instancias inesperadas.
   - Comprobar tipos y recuentos de instancias.

2. Revisar CloudTrail:
   - Buscar llamadas a la API RunInstances.
   - Identificar quién lanzó las instancias y por qué.

3. Tomar medidas:
   - Terminar o detener las instancias innecesarias.
   - Corregir la configuración incorrecta de AutoScaling.
   - Actualizar las políticas IAM para evitar que se repita.

4. Documentar:
   - Crear un informe post-mortem.
   - Actualizar los manuales de procedimientos (runbooks).
   - Añadir monitorización/alertas adicionales.
```

#### Ejemplo 2: Pico de almacenamiento de S3

**Alerta recibida**:
```
Anomalía de coste detectada

Servicio: Amazon S3
Región: us-west-2
Periodo de la anomalía: 2024-01-10 al 2024-01-15

Gasto esperado: $1,200/mes
Gasto real: $4,800/mes
Monto de la anomalía: +$3,600 (300% de aumento)

Análisis de la causa raíz:
- El almacenamiento aumentó de 50 TB a 200 TB.
- Crecimiento en el bucket: company-data-backup-west2.
- Principalmente nuevas solicitudes PUT y cargas de datos.
- Sin solicitudes DELETE correspondientes (acumulación de datos).

Principales factores contribuyentes:
1. El trabajo de copia de seguridad está realizando copias completas en lugar de incrementales.
2. Las copias de seguridad antiguas no se eliminan según la política de retención.
3. No se han aplicado políticas de ciclo de vida a este bucket.

Acciones recomendadas:
1. Revisar la estrategia de copia de seguridad (implementar incremental).
2. Aplicar políticas de ciclo de vida para eliminar copias antiguas.
3. Habilitar S3 Intelligent-Tiering para la optimización de costes.
4. Configurar S3 Storage Lens para la monitorización continua.
```

**Acciones de corrección**:
```python
import boto3
from datetime import datetime, timedelta

def remediate_s3_anomaly():
    s3 = boto3.client('s3')
    bucket = 'company-data-backup-west2'

    # Aplicar política de ciclo de vida
    lifecycle_policy = {
        'Rules': [
            {
                'Id': 'Delete-Old-Backups',
                'Status': 'Enabled',
                'Filter': {'Prefix': 'backups/'},
                'Expiration': {'Days': 30},
                'Transitions': [
                    {
                        'Days': 7,
                        'StorageClass': 'STANDARD_IA'
                    },
                    {
                        'Days': 14,
                        'StorageClass': 'GLACIER'
                    }
                ]
            }
        ]
    }

    s3.put_bucket_lifecycle_configuration(
        Bucket=bucket,
        LifecycleConfiguration=lifecycle_policy
    )

    # Eliminar copias de seguridad de más de 30 días inmediatamente
    paginator = s3.get_paginator('list_objects_v2')
    thirty_days_ago = datetime.now() - timedelta(days=30)

    for page in paginator.paginate(Bucket=bucket, Prefix='backups/'):
        for obj in page.get('Contents', []):
            if obj['LastModified'].replace(tzinfo=None) < thirty_days_ago:
                s3.delete_object(Bucket=bucket, Key=obj['Key'])
                print(f"Eliminado: {obj['Key']}")
```

#### Ejemplo 3: Anomalía de transferencia de datos

**Alerta recibida**:
```
Anomalía de coste detectada

Servicio: Transferencia de datos (Data Transfer)
Región: Transferencia entre regiones
Periodo de la anomalía: 2024-01-12

Gasto esperado: $200/día
Gasto real: $1,800/día
Monto de la anomalía: +$1,600 (800% de aumento)

Análisis de la causa raíz:
- 80 TB transferidos desde us-east-1 a eu-west-1.
- Transferencia normal: 10 TB/día.
- Origen: replicación de la base de datos RDS.
- Nueva réplica de lectura lanzada en eu-west-1 realizando la sincronización inicial.

Impacto en el coste:
- 80 TB × $0.02/GB = $1,640.
- Coste esperado tras la sincronización inicial: vuelve a $200/día.
- Anomalía puntual debido a la nueva infraestructura.

Acciones recomendadas:
1. Verificar si este fue un cambio de infraestructura planificado.
2. No se requiere acción inmediata (comportamiento esperado).
3. Actualizar las previsiones de costes para tener en cuenta la réplica entre regiones.
4. Considerar el uso de AWS Database Migration Service para futuras migraciones (más rentable).
```

**Resultado de la investigación**:
```
Estado: Anomalía explicada - No se requiere acción

Contexto:
- Lanzamiento planificado de la réplica de lectura de la UE.
- Sincronización de datos inicial esperada.
- Pico de coste puntual.
- Los costes continuos se normalizarán.

Seguimiento:
- Actualizar la documentación de planificación de capacidad.
- Añadir este escenario a los manuales de procedimientos.
- Configurar un presupuesto separado para los cambios de infraestructura.
- Reducir el umbral de alerta para cambios planificados.
```

#### Ejemplo 4: Pico de invocaciones de Lambda

**Alerta recibida**:
```
Anomalía de coste detectada

Servicio: AWS Lambda
Región: us-east-1
Periodo de la anomalía: 2024-01-08 14:00-16:00

Gasto esperado: $50/día
Gasto real: $420/día
Monto de la anomalía: +$370 (740% de aumento)

Análisis de la causa raíz:
- Función: image-processing-function.
- Invocaciones: 50M (frente a los 5M esperados).
- Causa: bucle infinito activado por la recursividad de eventos de S3.
- La función escribe la salida en el mismo bucket de S3 que la activa.

Cadena de eventos:
1. La función procesa la imagen → Escribe en S3.
2. El evento PUT de S3 activa la misma función de nuevo.
3. La función procesa la misma imagen → Escribe en S3.
4. El bucle continúa hasta que se detiene manualmente.

Acciones recomendadas [URGENTE]:
1. Desactivar INMEDIATAMENTE el activador de eventos de S3.
2. Actualizar la función para que escriba en un bucket diferente.
3. Implementar comprobaciones de idempotencia.
4. Añadir lógica de interruptor automático (circuit breaker).
5. Establecer un límite de ejecución concurrente reservada para Lambda.
```

**Respuesta de emergencia**:
```bash
# Desactivar la notificación de eventos de S3
aws s3api put-bucket-notification-configuration \
  --bucket source-images-bucket \
  --notification-configuration '{}'

# Establecer la concurrencia reservada de Lambda en 0 (desactivar temporalmente)
aws lambda put-function-concurrency \
  --function-name image-processing-function \
  --reserved-concurrent-executions 0

# Corregir el código de la función
# (actualizar para escribir en un bucket diferente: processed-images-bucket)

# Volver a activar con límite de concurrencia
aws lambda put-function-concurrency \
  --function-name image-processing-function \
  --reserved-concurrent-executions 100

# Volver a activar la notificación de S3 con la configuración corregida
aws s3api put-bucket-notification-configuration \
  --bucket source-images-bucket \
  --notification-configuration file://correct-notification.json
```

---

### Flujos de Trabajo de Respuesta (Response Workflows)

#### Flujo de Trabajo de Respuesta Automatizado

**Arquitectura**:
```
Anomalía de coste detectada
         ↓
   Tema SNS publicado
         ↓
   Función Lambda activada
         ↓
   ┌──────────────────┐
   │ Analizar anomalía│
   └──────────────────┘
         ↓
   ┌──────────────────────────────────┐
   │ Determinar gravedad y categoría  │
   └──────────────────────────────────┘
         ↓
   ┌─────────────────┬─────────────────┐
   │ Gravedad Alta   │  Gravedad Baja  │
   └─────────────────┴─────────────────┘
         ↓                    ↓
   ┌────────────┐      ┌───────────────┐
   │ Incidente  │      │ Mensaje Slack │
   │ PagerDuty  │      │ + Ticket Jira │
   └────────────┘      └───────────────┘
         ↓                    ↓
   ┌────────────────┐  ┌────────────────┐
   │ Mitigación     │  │ Investigación  │
   │ Automática     │  │ en cola        │
   │ (si está conf.)│  │                │
   └────────────────┘  └────────────────┘
```

**Función Lambda para respuesta automatizada**:
```python
import boto3
import json
from datetime import datetime

def lambda_handler(event, context):
    """Respuesta automatizada a anomalías de coste"""

    # Analizar mensaje SNS
    message = json.loads(event['Records'][0]['Sns']['Message'])

    anomaly = {
        'service': message['rootCauses'][0]['service'],
        'amount': message['impact']['totalImpact'],
        'percentage': message['impact']['totalImpact'] / message['dimensionValue'] * 100,
        'account': message['accountId'],
        'region': message['rootCauses'][0]['region']
    }

    # Determinar gravedad
    severity = determine_severity(anomaly)

    # Enrutar basado en gravedad
    if severity == 'CRITICAL':
        handle_critical_anomaly(anomaly)
    elif severity == 'HIGH':
        handle_high_anomaly(anomaly)
    else:
        handle_low_anomaly(anomaly)

    return {'statusCode': 200, 'body': 'Anomalía procesada'}

def determine_severity(anomaly):
    """Clasificar gravedad de la anomalía"""
    amount = float(anomaly['amount'])
    percentage = anomaly['percentage']

    if amount > 5000 or percentage > 500:
        return 'CRITICAL'
    elif amount > 1000 or percentage > 200:
        return 'HIGH'
    else:
        return 'LOW'

def handle_critical_anomaly(anomaly):
    """Manejar anomalías críticas"""
    # Crear incidente en PagerDuty
    create_pagerduty_incident(anomaly)

    # Enviar alerta urgente de Slack
    send_slack_alert(anomaly, channel='#critical-alerts', urgent=True)

    # Automatizar remediación si es posible
    if anomaly['service'] == 'Amazon EC2':
        check_and_stop_runaway_instances(anomaly)

    # Crear ticket de Jira de alta prioridad
    create_jira_ticket(anomaly, priority='Critical')

def handle_high_anomaly(anomaly):
    """Manejar anomalías de gravedad alta"""
    # Enviar mensaje de Slack
    send_slack_alert(anomaly, channel='#cost-alerts')

    # Crear ticket de Jira
    create_jira_ticket(anomaly, priority='High')

    # Registrar en CloudWatch para investigación
    log_to_cloudwatch(anomaly)

def handle_low_anomaly(anomaly):
    """Manejar anomalías de gravedad baja"""
    # Enviar mensaje de resumen de Slack
    send_slack_alert(anomaly, channel='#cost-alerts', urgent=False)

    # Solo registrar
    log_to_cloudwatch(anomaly)

def check_and_stop_runaway_instances(anomaly):
    """Detener instancias EC2 si se detecta anomalía"""
    ec2 = boto3.client('ec2', region_name=anomaly['region'])

    # Buscar instancias lanzadas en las últimas 2 horas
    instances = ec2.describe_instances(
        Filters=[
            {'Name': 'instance-state-name', 'Values': ['running']},
            {'Name': 'launch-time', 'Values': [f'>{datetime.now().isoformat()[:-7]}']}
        ]
    )

    runaway_instances = []
    for reservation in instances['Reservations']:
        for instance in reservation['Instances']:
            # Comprobar si la instancia es inusualmente grande o numerosa
            if is_unusual_instance(instance):
                runaway_instances.append(instance['InstanceId'])

    if runaway_instances:
        # Detener instancias (no terminar - permitir investigación)
        ec2.stop_instances(InstanceIds=runaway_instances)

        send_slack_alert({
            'message': f'Detenidas {len(runaway_instances)} instancias fuera de control',
            'instances': runaway_instances
        }, channel='#critical-alerts')
```

#### Flujo de Trabajo de Investigación Manual

**Manual de procedimientos (Playbook) para la investigación de anomalías de coste**:

```
Paso 1: Reconocer y Evaluar
[ ] Reconocer la alerta de anomalía.
[ ] Anotar el servicio, la región y el periodo de tiempo.
[ ] Comprobar si se trata de un cambio conocido/planificado.
[ ] Determinar la urgencia (¿sigue aumentando el gasto?).

Paso 2: Recopilar Contexto
[ ] Revisar Cost Explorer para obtener un desglose detallado.
[ ] Comprobar CloudTrail para buscar llamadas a la API relevantes.
[ ] Revisar despliegues o cambios recientes.
[ ] Comprobar los paneles de control de monitorización para buscar eventos correlacionados.

Paso 3: Identificar la Causa Raíz
[ ] Determinar qué recursos causaron el pico.
[ ] Identificar quién realizó los cambios (usuario/rol IAM).
[ ] Comprender el contexto de negocio (planificado frente a no planificado).
[ ] Evaluar si se trata de un problema puntual o continuo.

Paso 4: Tomar Medidas Inmediatas
[ ] Detener/terminar los recursos innecesarios.
[ ] Desactivar los servicios o características problemáticos.
[ ] Implementar límites de gasto temporales si es necesario.
[ ] Documentar todas las acciones tomadas.

Paso 5: Implementar una Solución Permanente
[ ] Corregir el problema subyacente (código, configuración, proceso).
[ ] Implementar controles preventivos (SCPs, cuotas, alarmas).
[ ] Actualizar las políticas IAM si el problema está relacionado con los permisos.
[ ] Crear un manual de procedimientos para escenarios futuros similares.

Paso 6: Post-Mortem y Prevención
[ ] Escribir el informe del incidente.
[ ] Compartir los aprendizajes con el equipo.
[ ] Actualizar los umbrales de detección de anomalías de coste si es necesario.
[ ] Implementar monitorización/alertas adicionales.
[ ] Programar una revisión en 30 días para verificar que la solución se mantiene.
```

**Lista de verificación de herramientas de investigación**:
```
Herramientas de la consola de AWS:
├── Cost Explorer: Análisis de costes detallado.
├── CloudTrail: Historial de llamadas a la API.
├── CloudWatch: Métricas y alarmas.
├── AWS Config: Cambios en la configuración de los recursos.
├── Trusted Advisor: Comprobaciones de optimización de costes.
└── Personal Health Dashboard: Problemas del servicio.

Comandos de la CLI:
├── aws ce get-cost-and-usage: Datos de costes programáticos.
├── aws cloudtrail lookup-events: Buscar llamadas a la API.
├── aws ec2 describe-instances: Comprobar instancias en ejecución.
├── aws rds describe-db-instances: Comprobar bases de datos.
└── aws s3api list-buckets: Revisar el uso de S3.

Herramientas de terceros:
├── CloudHealth
├── CloudCheckr
├── Datadog Cloud Cost Management
└── Apptio Cloudability
```

---

## Planes de Soporte de AWS

AWS ofrece cuatro planes de soporte, cada uno de los cuales proporciona diferentes niveles de soporte técnico y tiempos de respuesta.

### Comparativa de los Planes de Soporte

| Característica | Basic | Developer | Business | Enterprise |
|---------|-------|-----------|----------|------------|
| **Coste** | Gratuito | $29/mes o el 3% del uso mensual de AWS (el que sea mayor) | $100/mes o el 10% (escalonado del 10% al 3%) | $15,000/mes o el 10% (escalonado del 10% al 3%) |
| **Caso de uso** | Todos los clientes | Pruebas y desarrollo | Cargas de trabajo de producción | Cargas de trabajo críticas para el negocio |
| **Soporte Técnico** | Ninguno | Horario comercial por correo electrónico | 24/7 por correo electrónico, chat y teléfono | 24/7 por correo electrónico, chat y teléfono |
| **Tiempo de respuesta - Guía general** | N/A | < 24 horas | < 24 horas | < 24 horas |
| **Tiempo de respuesta - Sistema deteriorado** | N/A | < 12 horas | < 12 horas | < 12 horas |
| **Tiempo de respuesta - Sistema de producción caído** | N/A | N/A | < 4 horas | < 4 horas |
| **Tiempo de respuesta - Negocio crítico caído** | N/A | N/A | < 1 hora | < 1 hora |
| **Tiempo de respuesta - Misión crítica caída** | N/A | N/A | N/A | **< 15 minutos** |
| **Quién puede abrir casos** | N/A | 1 contacto principal | **Contactos ilimitados** | **Contactos ilimitados** |
| **Comprobaciones de Trusted Advisor** | 7 comprobaciones básicas | 7 comprobaciones básicas | **Todas las comprobaciones** | **Todas las comprobaciones** |
| **Soporte de software de terceros** | No | No | **Sí** | **Sí** |
| **Orientación arquitectónica** | No | General | Contextual al caso de uso | **Consultiva** |
| **Technical Account Manager (TAM)** | No | No | No | **Sí** |
| **Programas proactivos** | No | No | No | **Sí** (IEM, Well-Architected Reviews) |
| **Equipo de soporte Concierge** | No | No | No | **Sí** (facturación/cuenta) |

### Resumen de Tiempos de Respuesta

> **Crítico para el examen**: ¡Memoriza estos tiempos de respuesta!

**Developer**:
- Guía general: < 24 horas
- Sistema deteriorado: < 12 horas

**Business**:
- Guía general: < 24 horas
- Sistema deteriorado: < 12 horas
- Sistema de producción caído: **< 4 horas**
- Negocio crítico caído: **< 1 hora**

**Enterprise**:
- Todos los tiempos de respuesta del plan Business, ADEMÁS DE:
- Sistema de misión crítica caído: **< 15 minutos**

---

### Matriz de Decisión del Plan de Soporte

**Elige el plan de soporte adecuado según tu escenario**:

| Escenario | Plan recomendado | Razonamiento |
|----------|-----------------|-----------|
| Aprendizaje/experimentación personal | **Basic** | Gratuito, suficiente para el autoaprendizaje |
| Pequeña startup, aún sin cargas de producción | **Basic** o **Developer** | Developer si necesitas orientación técnica ocasional |
| Startup con primer despliegue en producción | **Developer** | Bajo coste, soporte por email en horario comercial |
| Pequeña empresa, producción no crítica | **Developer** | Adecuado para aplicaciones que no son críticas para el negocio |
| Empresa en crecimiento, la producción es importante | **Business** | Soporte 24/7, respuesta en < 1 hora para problemas críticos |
| Gran empresa con sistemas de misión crítica | **Enterprise** | TAM, respuesta en 15 min, orientación proactiva |
| Necesidad de integración con software de terceros | **Business** (mínimo) | Soporte para software de terceros incluido |
| Requiere consulta arquitectónica | **Business** o **Enterprise** | Orientación contextual o consultiva |
| Necesita soporte telefónico 24/7 | **Business** (mínimo) | El soporte telefónico comienza con el plan Business |
| Ejecución de cargas de trabajo críticas para el cumplimiento | **Business** o **Enterprise** | Trusted Advisor completo, tiempos de respuesta rápidos |
| Organización multicuenta (más de 10 cuentas) | **Enterprise** (recomendado) | El TAM ayuda a coordinar entre las cuentas |
| Los ingresos dependen de la disponibilidad de AWS | **Enterprise** | Respuesta en 15 minutos + monitorización proactiva |

#### Árbol de Decisión

```
¿Estás generando ingresos o ejecutando cargas de trabajo de producción?
├── No → Basic Support (gratuito)
└── Sí → Continuar...
    │
    ¿Tu aplicación es de misión crítica (coste de inactividad >100.000 $/hora)?
    ├── Sí → Enterprise Support
    └── No → Continuar...
        │
        ¿Necesitas soporte telefónico 24/7?
        ├── Sí → Business o Enterprise
        └── No → Continuar...
            │
            ¿Gasto mensual de AWS > 10.000 $?
            ├── Sí → Business Support (rentable a escala)
            └── No → Developer Support
```

#### Análisis Coste-Beneficio por Gasto Mensual

| Gasto mensual de AWS | Coste Basic | Coste Developer | Coste Business | Coste Enterprise | Mejor valor |
|-------------------|------------|----------------|---------------|-----------------|------------|
| 100 $ | 0 $ | 29 $ | 100 $ | 15.000 $ | Developer* |
| 500 $ | 0 $ | 29 $ | 100 $ | 15.000 $ | Business** |
| 1.000 $ | 0 $ | 30 $ | 100 $ | 15.000 $ | Business |
| 5.000 $ | 0 $ | 150 $ | 500 $ | 15.000 $ | Business |
| 10.000 $ | 0 $ | 300 $ | 1.000 $ | 15.000 $ | Business |
| 50.000 $ | 0 $ | 1.500 $ | 3.500 $ | 15.000 $ | Business |
| 100.000 $ | 0 $ | 3.000 $ | 5.500 $ | 15.000 $ | Business/Enterprise*** |
| 500.000 $ | 0 $ | 15.000 $ | 17.500 $ | 35.000 $ | Enterprise |
| 1.000.000 $ | 0 $ | 30.000 $ | 32.500 $ | 45.000 $ | Enterprise |

Notas:
- *Si la producción no es crítica.
- **Si necesitas soporte 24/7 o el Trusted Advisor completo.
- ***Enterprise se vuelve competitivo en costes + añade un valor significativo (TAM, etc.).

#### Ejemplos de Escenarios del Mundo Real

**Escenario 1: Startup de plataforma de e-learning**
```
Empresa: Startup de tecnología educativa, 50.000 usuarios
Gasto en AWS: 2.000 $/mes
Carga de trabajo: Aplicación de producción, pero puede tolerar cierto tiempo de inactividad
Equipo: 3 ingenieros, experiencia limitada en AWS

Recomendación: Business Support Plan (100 $/mes)

Razonamiento:
- El soporte 24/7 es importante para los periodos de exámenes de los estudiantes.
- Necesita orientación arquitectónica para el escalado.
- Trusted Advisor completo para optimizar costes.
- El coste está justificado (100 $ sobre un gasto de 2.000 $ = 5%).
- Puede escalar problemas críticos con una respuesta en < 1 hora.
```

**Escenario 2: Empresa de SaaS sanitario**
```
Empresa: Plataforma de registros médicos compatible con HIPAA
Gasto en AWS: 50.000 $/mes
Carga de trabajo: Misión crítica, maneja datos de pacientes
Equipo: 20 ingenieros, certificados en AWS
Cumplimiento: HIPAA, HITRUST

Recomendación: Enterprise Support Plan (15.000 $/mes)

Razonamiento:
- La atención al paciente depende de la disponibilidad del sistema.
- El cumplimiento requiere soporte para auditorías y revisiones.
- El TAM proporciona revisiones arquitectónicas proactivas.
- Well-Architected Review ayuda a mantener el cumplimiento.
- Respuesta en 15 minutos crítica para sistemas orientados al paciente.
- Infrastructure Event Management para despliegues importantes.
- El coste es el 30% del gasto, pero está justificado por la reducción de riesgos.
```

**Escenario 3: Agencia de marketing**
```
Empresa: Agencia de marketing digital
Gasto en AWS: 800 $/mes
Carga de trabajo: Sitios web y campañas de clientes
Equipo: 2 desarrolladores, soporte subcontratado
Horario comercial: 9-5 PM de lunes a viernes

Recomendación: Developer Support Plan (29 $/mes)

Razonamiento:
- Criticidad de producción limitada.
- El soporte en horario comercial es suficiente.
- Consciente del presupuesto (fase de startup).
- Puede esperar 12-24 horas para las respuestas.
- Complejidad arquitectónica mínima.
```

**Escenario 4: Empresa de servicios financieros**
```
Empresa: Plataforma de negociación de acciones
Gasto en AWS: 200.000 $/mes
Carga de trabajo: Negociación en tiempo real, tolerancia cero al tiempo de inactividad
Equipo: Más de 50 ingenieros, DevOps dedicados
Regulatorio: SOC2, PCI-DSS

Recomendación: Enterprise Support Plan (20.000 $/mes)

Razonamiento:
- Cada minuto de inactividad = pérdida de operaciones y reputación.
- El TAM coordina con los equipos de seguridad y cumplimiento.
- La monitorización proactiva detecta problemas antes del impacto.
- Las Well-Architected Reviews garantizan las mejores prácticas de seguridad.
- Infrastructure Event Management para actualizaciones de la plataforma.
- El coste es el 10% del gasto, fácilmente justificado por el riesgo.
```

---

### Comparativa Detallada de Características

#### Canales de Soporte

| Característica | Basic | Developer | Business | Enterprise |
|---------|-------|-----------|----------|------------|
| **Soporte por correo electrónico** | No (solo facturación) | Sí (horario comercial) | Sí (24/7) | Sí (24/7) |
| **Soporte telefónico** | No | No | **Sí (24/7)** | **Sí (24/7)** |
| **Soporte por chat** | No | No | **Sí (24/7)** | **Sí (24/7)** |
| **Soporte vía web** | Solo facturación/cuenta | Sí | Sí | Sí |
| **Número de contactos de soporte** | N/A | 1 contacto principal | **Ilimitados** | **Ilimitados** |
| **Idioma del soporte** | Solo inglés | Solo inglés | Inglés + 8 idiomas | Inglés + 8 idiomas |

#### Garantías de Tiempo de Respuesta

| Nivel de gravedad | Basic | Developer | Business | Enterprise |
|----------------|-------|-----------|----------|------------|
| **Orientación general** | Sin soporte | < 24 horas comerciales | < 24 horas | < 24 horas |
| **Sistema deteriorado** | Sin soporte | < 12 horas comerciales | < 12 horas | < 12 horas |
| **Sistema de producción deteriorado** | Sin soporte | Sin soporte | **< 4 horas** | **< 4 horas** |
| **Sistema de producción caído** | Sin soporte | Sin soporte | **< 1 hora** | **< 1 hora** |
| **Sistema crítico para el negocio caído** | Sin soporte | Sin soporte | **< 1 hora** | **< 1 hora** |
| **Sistema de misión crítica caído** | Sin soporte | Sin soporte | Sin soporte | **< 15 minutos** |

#### Trusted Advisor

| Categoría de comprobación | Basic | Developer | Business | Enterprise |
|----------------|-------|-----------|----------|------------|
| **Optimización de costes** | Solo 7 comprobaciones básicas | Solo 7 comprobaciones básicas | **Todas las comprobaciones** | **Todas las comprobaciones** |
| **Rendimiento** | Limitado | Limitado | **Todas las comprobaciones** | **Todas las comprobaciones** |
| **Seguridad** | Permisos de buckets de S3, Grupos de Seguridad | Igual que Basic | **Todas las comprobaciones** | **Todas las comprobaciones** |
| **Tolerancia a fallos** | Snapshots de EBS, backups de RDS | Igual que Basic | **Todas las comprobaciones** | **Todas las comprobaciones** |
| **Límites de servicio** | Sí (comprobaciones básicas) | Sí (comprobaciones básicas) | **Todas las comprobaciones** | **Todas las comprobaciones** |
| **Acceso programático (API)** | No | No | **Sí** | **Sí** |
| **Integración con CloudWatch** | No | No | **Sí** | **Sí** |
| **Actualización semanal** | Solo manual | Solo manual | **Automática** | **Automática** |

**7 Comprobaciones básicas de Trusted Advisor** (Basic/Developer):
1. Permisos de buckets de S3 (Seguridad)
2. Grupos de seguridad: puertos específicos sin restringir (Seguridad)
3. Uso de IAM (Seguridad)
4. MFA en la cuenta raíz (Seguridad)
5. Snapshots públicos de EBS (Seguridad)
6. Snapshots públicos de RDS (Seguridad)
7. Límites de servicio (Límites de servicio)

#### Orientación Arquitectónica

| Tipo | Basic | Developer | Business | Enterprise |
|------|-------|-----------|----------|------------|
| **Mejores prácticas generales** | Solo documentación | **Orientación general** | **Orientación contextual** | **Revisiones consultivas** |
| **Específica para el caso de uso** | No | Limitada | **Sí** | **Sí (integral)** |
| **Well-Architected Review** | No | No | Solo autoservicio | **Facilitada por el TAM** |
| **Revisión de diagramas de arquitectura** | No | No | **Sí** | **Sí (detallada)** |
| **Planificación de capacidad** | No | No | Limitada | **Sí (proactiva)** |
| **Optimización del rendimiento** | No | No | Reactiva | **Proactiva** |

#### Servicios Proactivos

| Servicio | Basic | Developer | Business | Enterprise |
|---------|-------|-----------|----------|------------|
| **Technical Account Manager (TAM)** | No | No | No | **Sí (dedicado)** |
| **Equipo de soporte Concierge** | No | No | No | **Sí** |
| **Infrastructure Event Management** | No | No | No | **Sí** |
| **Well-Architected Reviews** | No | No | Autoservicio | **Facilitada por el TAM** |
| **Revisiones de operaciones** | No | No | No | **Sí (trimestral)** |
| **Formación y talleres** | No | No | No | **Sí (disponibles)** |
| **Orientación proactiva** | No | No | No | **Sí (continua)** |

#### Acceso a Programas de AWS

| Programa | Basic | Developer | Business | Enterprise |
|---------|-------|-----------|----------|------------|
| **API de AWS Health** | No | No | **Sí** | **Sí** |
| **API de AWS Support** | No | No | **Sí** | **Sí** |
| **Flujos de trabajo de automatización de soporte** | No | No | Limitados | **Sí** |
| **AWS re:Post** | Sí | Sí | Sí | Sí |
| **Créditos de formación de AWS** | No | No | Algunos | **Sí** |
| **Acceso a programas Beta** | Limitado | Limitado | Disponible | **Acceso prioritario** |

#### Desglose de Costes

**Precios del Plan Developer**:
```
El mayor de:
- 29 $/mes (mínimo)
- 3% del uso mensual de AWS

Ejemplos:
Gasto de 500 $ en AWS: 29 $ (el 3% son 15 $, pero el mínimo es 29 $)
Gasto de 1.000 $ en AWS: 30 $ (3% de 1.000 $)
Gasto de 5.000 $ en AWS: 150 $ (3% de 5.000 $)
```

**Precios del Plan Business**:
```
El mayor de:
- 100 $/mes (mínimo)
- Porcentaje escalonado del uso mensual de AWS:
  - 10% para los primeros 0-10.000 $
  - 7% para los siguientes 10.000-80.000 $
  - 5% para los siguientes 80.000-250.000 $
  - 3% para más de 250.000 $

Ejemplos:
Gasto de 1.000 $ en AWS: 100 $ (10% son 100 $, igual al mínimo)
Gasto de 10.000 $ en AWS: 1.000 $ (10% de 10.000 $)
Gasto de 50.000 $ en AWS:
  10.000 $ × 10% = 1.000 $
  40.000 $ × 7% = 2.800 $
  Total: 3.800 $/mes
```

**Precios del Plan Enterprise**:
```
El mayor de:
- 15.000 $/mes (mínimo)
- Porcentaje escalonado del uso mensual de AWS:
  - 10% para los primeros 0-150.000 $
  - 7% para los siguientes 150.000-500.000 $
  - 5% para los siguientes 500.000-1.000.000 $
  - 3% para más de 1.000.000 $

Ejemplos:
Gasto de 50.000 $ en AWS: 15.000 $ (por debajo del mínimo)
Gasto de 200.000 $ en AWS:
  150.000 $ × 10% = 15.000 $
  50.000 $ × 7% = 3.500 $
  Total: 18.500 $/mes
Gasto de 1.000.000 $ en AWS:
  150.000 $ × 10% = 15.000 $
  350.000 $ × 7% = 24.500 $
  500.000 $ × 5% = 25.000 $
  Total: 64.500 $/mes
```

#### Políticas de Mejora/Reducción de Plan (Upgrade/Downgrade)

```
Mejora de plan (Upgrading):
- Se puede mejorar el plan en cualquier momento.
- Los nuevos beneficios son efectivos de inmediato.
- Se factura a la nueva tarifa desde la fecha de la mejora.

Reducción de plan (Downgrading):
- Se puede reducir al final del periodo de facturación actual.
- Se debe avisar con 30 días de antelación.
- Los casos abiertos pueden cerrarse o perder prioridad.
- Se pierde el acceso a las funciones premium (TAM, etc.).

Cancelación:
- Se puede cancelar el plan de soporte con 30 días de antelación.
- No se puede cancelar el soporte Basic (siempre está incluido).
- Reducir a Basic en lugar de cancelar.
```

#### Soporte de Software de Terceros

**Solo para los planes Business y Enterprise**:
```
Integraciones compatibles:
├── Sistemas operativos: Amazon Linux, RHEL, Windows Server, Ubuntu
├── Servidores web: Apache, Nginx, IIS
├── Bases de datos: MySQL, PostgreSQL, Microsoft SQL Server
├── Servidores de aplicaciones: Tomcat, JBoss, WebSphere
└── Otros: Docker, Kubernetes, Jenkins, Git, etc.

Alcance del soporte:
- Instalación y configuración en AWS.
- Interacción con los servicios de AWS.
- Rendimiento en la infraestructura de AWS.
- Resolución de problemas de conectividad.
- Mejores prácticas para la integración con AWS.

NO compatible:
- Depuración del código de la aplicación.
- Problemas de licencias de software.
- Errores del producto (consultar al proveedor).
- Solicitudes de nuevas características.
```

---

### Diferencias Clave

**Soporte Basic** (Gratuito):
- Acceso a:
  - Servicio de atención al cliente (preguntas sobre facturación y cuenta).
  - Documentación de AWS, informes técnicos (whitepapers), foros de soporte.
  - AWS Trusted Advisor (7 comprobaciones básicas).
  - AWS Personal Health Dashboard.
- **Sin soporte técnico**.

**Soporte Developer** (mínimo 29 $/mes):
- Para experimentación y pruebas.
- **Un** contacto principal puede abrir casos de soporte.
- Acceso por correo electrónico en horario comercial.
- Orientación arquitectónica general.

**Soporte Business** (mínimo 100 $/mes):
- Para cargas de trabajo de producción.
- **Contactos ilimitados** pueden abrir casos.
- **Soporte 24/7 por teléfono, correo electrónico y chat**.
- Comprobaciones completas de Trusted Advisor.
- Soporte para software de terceros (interacciones con servicios de AWS).
- Orientación arquitectónica contextual.

**Soporte Enterprise** (mínimo 15.000 $/mes):
- Para cargas de trabajo de misión crítica.
- Todas las características de Business, ADEMÁS DE:
- **Technical Account Manager (TAM)**: Punto de contacto técnico designado.
- **Equipo de soporte Concierge**: Expertos en facturación y cuentas.
- **Infrastructure Event Management**: Soporte para lanzamientos de productos, eventos.
- **Well-Architected Reviews**: Orientación arquitectónica.
- **Tiempo de respuesta de 15 minutos** para problemas de misión crítica.

---

### Recursos Adicionales de Soporte

#### AWS Personal Health Dashboard

- Vista **personalizada** del estado de los servicios de AWS que afectan a tus recursos.
- **Notificaciones proactivas** sobre mantenimiento programado, problemas de seguridad.
- **Alertas** de eventos que impactan en tus recursos.
- **Orientación detallada para la remediación**.
- **Disponible para todos los clientes** (todos los planes de soporte).
- Integrado con CloudWatch Events para la automatización.

**Diferencia con el Service Health Dashboard**:
- **Service Health Dashboard**: Estado general de los servicios de AWS (todos los clientes ven la misma vista).
- **Personal Health Dashboard**: Personalizado para TUS recursos y cuentas.

#### API de AWS Health

- **Acceso programático** a la información de AWS Health.
- Integra los eventos de estado con los sistemas de monitorización y gestión de incidentes.
- Requiere **Soporte Business o Enterprise**.
- Automatiza las respuestas a los eventos de estado (activadores de Lambda, etc.).

#### AWS Managed Services (AMS)

- AWS **opera tu infraestructura** en tu nombre.
- Características:
  - Operaciones de infraestructura 24/7.
  - Detección y gestión de incidentes.
  - Parcheo, copia de seguridad, monitorización.
  - Seguridad y cumplimiento.
  - Gestión de cambios.
- **Servicio independiente** con coste adicional.
- Ideal para organizaciones que desean que AWS se encargue de las operaciones.

#### AWS Professional Services

- **Equipo global de expertos de AWS**.
- Servicios:
  - Ayuda a diseñar y crear arquitecturas de soluciones.
  - Crea, migra y moderniza aplicaciones.
  - Trabaja junto a tu equipo.
  - Formación y transferencia de conocimientos.
- **Contratación de consultoría** (tarifas independientes).
- Equipos especializados: Migración, DevOps, Analítica, Aprendizaje Automático (Machine Learning), etc.

#### AWS Partner Network (APN)

- **Comunidad global** de socios de AWS.
- **Consulting Partners**: Servicios profesionales, integración de sistemas.
- **Technology Partners**: Soluciones de software integradas con AWS.
- **AWS Marketplace**: Compra de software y servicios de terceros.
- Encuentra socios en: https://partners.amazonaws.com

---

## Estrategias de Optimización de Costes

### 1. Dimensionamiento Adecuado (Right Sizing)

**Qué es**: Ajustar los tipos y tamaños de las instancias a los requisitos de la carga de trabajo.

**Cómo implementarlo**:
- Utilizar **métricas de CloudWatch** para identificar recursos infrautilizados.
- Utilizar **AWS Compute Optimizer** para obtener recomendaciones basadas en aprendizaje automático (ML).
- Analizar el uso de CPU, memoria, red y disco.
- Reducir el tamaño o cambiar las familias de instancias en función del uso real.
- Revisar periódicamente (mensual o trimestralmente).

**Ejemplo**:
- Ejecución de una instancia m5.2xlarge con un uso de CPU del 10%.
- Dimensionar adecuadamente a m5.large → Ahorro de ~50% en costes de computación.

**Herramientas**:
- AWS Compute Optimizer.
- Recomendaciones de dimensionamiento de AWS Cost Explorer.
- Métricas y alarmas de CloudWatch.

---

### 2. Capacidad Reservada (Reserved Capacity)

**Servicios con opciones de reserva**:
- **Instancias reservadas (Reserved Instances)**: EC2, RDS, ElastiCache, Redshift, OpenSearch (anteriormente Elasticsearch).
- **Savings Plans**: EC2, Fargate, Lambda (Compute Savings Plans).

**Ahorro**: Hasta un **75%** en comparación con los precios bajo demanda (On-Demand).

**Plazos de compromiso**:
- 1 año o 3 años.
- Opciones de pago:
  - Pago total por adelantado (All Upfront): mayor descuento.
  - Pago parcial por adelantado (Partial Upfront): descuento medio.
  - Sin pago inicial (No Upfront): menor descuento, pagos mensuales.

**Mejores prácticas**:
- Analizar los patrones de uso durante 1-3 meses antes de comprar.
- Empezar con compromisos de 1 año.
- Utilizar instancias reservadas para cargas de trabajo de estado estable.
- Considerar los Savings Plans para obtener flexibilidad entre familias de instancias.

**Ejemplo**:
- Carga de trabajo base: 10 instancias m5.large ejecutándose 24/7.
- Compra de 10 instancias reservadas (3 años, pago total por adelantado).
- Ahorro de ~72% en comparación con On-Demand.

---

### 3. Instancias Spot (Spot Instances)

**Descuento**: Hasta un **90%** en comparación con On-Demand.

**Cómo funciona**:
- Pujar por la capacidad de EC2 no utilizada.
- AWS puede reclamar las instancias con un aviso de 2 minutos.
- El precio varía en función de la oferta y la demanda.

**Ideal para**:
- Aplicaciones tolerantes a fallos.
- Horarios de inicio/finalización flexibles.
- Trabajos de procesamiento por lotes (batch).
- Análisis de Big Data.
- Cargas de trabajo contenedorizadas (con reinicio automático).
- Trabajadores de canalizaciones CI/CD.
- Renderizado y transcodificación.

**NO apto para**:
- Bases de datos (sin la arquitectura adecuada).
- Aplicaciones con estado (sin puntos de control o checkpointing).
- Aplicaciones que requieren disponibilidad garantizada.

**Mejores prácticas**:
- Utilizar **Spot Fleet** para lanzar múltiples tipos de instancias y zonas de disponibilidad (AZs).
- Implementar **puntos de control (checkpointing)** para guardar el progreso.
- Utilizar las **notificaciones de interrupción de instancias Spot** (aviso de 2 minutos).

---

### 4. Auto Scaling

**Beneficios**:
- Escalar los recursos en función de la demanda real.
- Evitar el exceso de aprovisionamiento.
- Reducir los costes durante los periodos de baja demanda.
- Mantener el rendimiento durante la alta demanda.

**Servicios con Auto Scaling**:
- EC2 Auto Scaling.
- DynamoDB Auto Scaling.
- Aurora Auto Scaling.
- ECS/EKS Auto Scaling.
- Application Auto Scaling (Lambda, etc.).

**Ejemplo**:
- Aplicación web con tráfico variable.
- Escalar de 2 instancias (fuera de horas punta) a 10 instancias (horas punta).
- Promedio de 4 instancias en lugar de ejecutar siempre 10.
- Ahorro de ~60% en costes de computación.

---

### 5. Optimización del Almacenamiento

**Clases de almacenamiento de S3**:

| Clase de almacenamiento | Caso de uso | Coste (relativo) |
|--------------|----------|-----------------|
| S3 Standard | Acceso frecuente | Base ($$$) |
| S3 Intelligent-Tiering | Acceso desconocido o cambiante | Optimización automática |
| S3 Standard-IA | Acceso infrecuente | ~50% más barato ($$) |
| S3 One Zone-IA | Infrecuente, no crítico | ~60% más barato ($) |
| S3 Glacier Instant Retrieval | Archivo, recuperación en milisegundos | ~70% más barato ($) |
| S3 Glacier Flexible Retrieval | Archivo, recuperación en minutos/horas | ~80% más barato ($) |
| S3 Glacier Deep Archive | Archivo a largo plazo, recuperación en 12 horas | ~90% más barato ($) |

**Estrategias de optimización**:
- Implementar **políticas de ciclo de vida de S3 (S3 Lifecycle Policies)** para transicionar objetos automáticamente.
- Utilizar **S3 Intelligent-Tiering** para patrones de acceso impredecibles.
- Eliminar **volúmenes EBS no utilizados** y **snapshots**.
- Utilizar **EBS gp3** en lugar de gp2 (un 20% más barato con mejor rendimiento).
- Habilitar el **archivo de snapshots de EBS** para copias de seguridad a largo plazo.

**Ejemplo de política de ciclo de vida**:
```
Día 0-30: S3 Standard
Día 31-90: S3 Standard-IA
Día 91-365: S3 Glacier Flexible Retrieval
Día 365+: Eliminar o mover a Glacier Deep Archive
```

---

### 6. Optimización de la Transferencia de Datos

**Estrategias**:

1. **Utilizar CloudFront** para la entrega de contenidos.
   - Almacenar en caché el contenido en ubicaciones de borde (edge locations).
   - Reducir la transferencia de datos desde el origen.
   - Precios de transferencia de datos más bajos que directamente desde S3/EC2.

2. **Mantener los datos en la misma región** siempre que sea posible.
   - Evitar cargos por transferencia de datos entre regiones.
   - Utilizar multi-AZ para alta disponibilidad (coste mínimo).

3. **Utilizar puntos de enlace de la VPC (VPC Endpoints)** para S3 y DynamoDB.
   - El tráfico permanece dentro de la red de AWS.
   - Sin cargos por transferencia de datos.
   - No es necesario una pasarela de Internet (Internet Gateway).

4. **Comprimir los datos** antes de la transferencia.
   - Reducir la cantidad de datos transferidos.
   - Utilizar gzip, Brotli u otra compresión.

5. **Utilizar AWS Direct Connect** para grandes transferencias de datos.
   - Conexión de red dedicada a AWS.
   - Costes de transferencia de datos más bajos que por Internet.
   - Rendimiento más consistente.

**Comparativa de costes**:
```
Escenario: Transferir 1 TB/mes desde EC2 a Internet
- Directamente desde EC2: 1.024 GB × 0,09 $/GB = 92,16 $
- A través de CloudFront: 85,00 $ (precios de CloudFront)
- Ahorro: ~$7/TB
```

---

### 7. Utilizar las Herramientas de Optimización de Costes de AWS

**Herramientas y servicios**:

1. **AWS Compute Optimizer**
   - Recomendaciones basadas en ML para EC2, EBS, Lambda.
   - Analizar patrones de utilización.
   - Sugerir recursos con el tamaño adecuado.

2. **AWS Trusted Advisor** (Soporte Business/Enterprise)
   - Comprobaciones de optimización de costes:
     - Instancias de RDS inactivas.
     - Instancias de EC2 infrautilizadas.
     - Direcciones IP elásticas no asociadas.
     - Volúmenes de EBS con baja utilización.
     - Equilibradores de carga (Load Balancers) inactivos.

3. **Recomendaciones de Cost Explorer**
   - Recomendaciones de compra de instancias reservadas.
   - Recomendaciones de Savings Plans.
   - Basadas en tu historial de uso.

4. **AWS Cost Anomaly Detection**
   - Detectar picos de costes inesperados.
   - Recibir alertas sobre gastos inusuales.

**Mejor práctica**: Revisar las recomendaciones mensualmente e implementar las sugerencias aplicables.

---

### Optimización por Servicio Específico

#### Optimización de Costes de EC2

**1. Dimensionamiento adecuado de instancias**:
```
Acciones:
├── Utilizar métricas de CloudWatch (CPU, memoria, red, disco)
├── Recomendaciones de AWS Compute Optimizer
├── Revisar la utilización durante un periodo mínimo de 2 semanas
├── Reducir el tamaño o cambiar la familia de instancias
└── Probar el rendimiento tras los cambios

Ejemplo:
Actual: m5.2xlarge @ 15% de utilización de CPU
Ajustado: m5.large (ahorro del 50%)
O
Actual: m5.xlarge (propósito general)
Optimizado: c5.large (optimizado para computación, mejor $/rendimiento)
```

**2. Integración de instancias Spot**:
```
Estrategias:
├── Spot Fleet con tipos de instancias diversificados
├── Grupos de Auto Scaling mixtos (Spot + On-Demand)
├── Spot Block para cargas de trabajo de duración definida
└── EC2 Fleet para requisitos complejos

Ahorro: 60-90% frente a On-Demand
Ideal para: Trabajos por lotes, CI/CD, contenedores, Big Data
```

**3. Escalado programado**:
```python
# Detener instancias de desarrollo fuera del horario comercial
import boto3

ec2 = boto3.client('ec2')

def stop_dev_instances():
    """Detener instancias etiquetadas como Environment=Dev a las 7 PM"""
    instances = ec2.describe_instances(
        Filters=[
            {'Name': 'tag:Environment', 'Values': ['Dev', 'Test']},
            {'Name': 'instance-state-name', 'Values': ['running']}
        ]
    )

    for reservation in instances['Reservations']:
        for instance in reservation['Instances']:
            ec2.stop_instances(InstanceIds=[instance['InstanceId']])
            print(f"Detenida: {instance['InstanceId']}")

# Programar con EventBridge: 7 PM entre semana
# Ahorro: 67% (11 h/día frente a 24 h/día)
```

**4. Estrategia de instancias reservadas**:
```
Análisis de carga de trabajo base:
- Monitorizar los patrones de uso de 30 días.
- Identificar instancias siempre activas.
- Comprar RIs para el 70% de la base.
- Mantener un 30% flexible para el escalado.

Ejemplo:
Utilización media: 20 instancias
Compra de RI: 14 instancias (RI estándar de 1 año)
Variable: 6 instancias (On-Demand + Spot)
Ahorro: 40% en la base + 70% en la variable (Spot)
```

#### Optimización de Costes de RDS

**1. Dimensionamiento adecuado de instancias**:
```
Métricas a monitorizar:
├── Utilización de CPU (objetivo: 40-70%)
├── DatabaseConnections (frente a max_connections)
├── FreeableMemory (debería mantenerse > 1 GB)
├── ReadIOPS / WriteIOPS (comprobar si se necesitan IOPS aprovisionadas)
└── Rendimiento de red (Network throughput)

Acciones:
- Reducir el tamaño si la CPU está consistentemente por debajo del 40%.
- Considerar Aurora Serverless v2 para cargas de trabajo variables.
- Utilizar réplicas de lectura (Read Replicas) en lugar de una instancia principal más grande.
```

**2. Optimización del almacenamiento**:
```
Estrategia:
├── Utilizar gp3 en lugar de gp2 (un 20% más barato, mejor rendimiento)
├── Habilitar el autoescalado del almacenamiento (pagar por lo que se usa)
├── Establecer el almacenamiento máximo asignado de forma adecuada
├── Limpiar backups automatizados antiguos (mantener 7-14 días)
└── Utilizar AWS Backup para la retención a largo plazo (más barato que los backups de RDS)

Ejemplo:
Actual: 1 TB gp2 aprovisionado, 400 GB utilizados
Optimizado: 500 GB gp3 con autoescalado
Ahorro inmediato: 50% en capacidad no utilizada
Continuo: Ahorro del 20% al utilizar gp3
```

**3. Consideraciones de Multi-AZ**:
```
Pregunta: ¿Necesitas Multi-AZ?

Bases de datos de producción: SÍ (99,95% SLA)
Desarrollo/Pruebas: NO (Single-AZ, ahorro del 50%)
Staging: TAL VEZ (depende de las necesidades de las pruebas)

Alternativa para desarrollo/pruebas:
- Single-AZ con snapshots automatizados.
- Restaurar desde un snapshot si es necesario (15-30 min).
- Coste: reducción del 50%.
```

**4. Aurora Serverless frente a Provisioned**:
```
Aurora Serverless v2:
- Cargas de trabajo variables (patrones diarios/semanales).
- Tráfico impredecible.
- Bases de datos de desarrollo y pruebas.
- Pagar solo por las ACUs (Aurora Capacity Units) utilizadas.

Aurora Provisioned:
- Cargas de trabajo constantes y predecibles.
- Necesidad de un tamaño de instancia específico.
- Utilizar instancias reservadas para ahorrar.

Ejemplo de carga de trabajo (uso variable):
Aurora Provisioned: db.r5.large 24/7 = 350 $/mes
Aurora Serverless v2: Promedio de 2 ACUs, 12 h/día = 108 $/mes
Ahorro: 69%
```

#### Optimización de Costes de S3

**1. Selección de la clase de almacenamiento**:
```
Árbol de decisión:
├── ¿Acceso frecuente? → S3 Standard
├── ¿Acceso < 1 vez al mes? → S3 Standard-IA
├── ¿Patrón desconocido? → S3 Intelligent-Tiering
├── ¿Archivo (acceso muy raro)? → S3 Glacier Flexible Retrieval
└── ¿Archivo a largo plazo (7-10 años)? → S3 Glacier Deep Archive

Optimización automática:
Utilizar políticas de ciclo de vida para transicionar automáticamente.
```

**2. Ejemplos de políticas de ciclo de vida**:
```xml
<!-- Ciclo de vida de los archivos de registro (logs) -->
<LifecycleConfiguration>
  <Rule>
    <Status>Enabled</Status>
    <Filter>
      <Prefix>logs/</Prefix>
    </Filter>
    <Transition>
      <Days>30</Days>
      <StorageClass>STANDARD_IA</StorageClass>
    </Transition>
    <Transition>
      <Days>90</Days>
      <StorageClass>GLACIER</StorageClass>
    </Transition>
    <Expiration>
      <Days>365</Days>
    </Expiration>
  </Rule>
</LifecycleConfiguration>

Ejemplo de impacto en el coste (1 TB de logs):
Día 0-30: 1 TB Standard @ 23 $/mes
Día 31-90: 1 TB Standard-IA @ 12,50 $/mes (ahorro del 46%)
Día 91-365: 1 TB Glacier @ 4 $/mes (ahorro del 83%)
Ahorro anual: ~$180/TB
```

**3. Optimización de solicitudes (Requests)**:
```
Operaciones costosas:
- Solicitudes LIST: 0,005 $ por cada 1.000
- Solicitudes PUT/POST: 0,005 $ por cada 1.000

Optimizaciones:
├── Operaciones por lotes (batch) en lugar de solicitudes individuales
├── Utilizar S3 Inventory en lugar de LIST para buckets grandes
├── Implementar almacenamiento en caché en el lado del cliente
└── Utilizar CloudFront para lecturas frecuentes (menores costes de solicitud)

Ejemplo:
Actual: 1M de solicitudes LIST/mes = 5 $
Optimizado: S3 Inventory diario + caché de cliente = 0,50 $
Ahorro: 90%
```

**4. S3 Select y Glacier Select**:
```
Problema: Recuperar objetos completos y luego filtrar.
Solución: Consultar in situ con S3 Select.

Ejemplo:
Archivo CSV: 100 GB, se necesitan 1 GB de datos filtrados.
Enfoque estándar: Descargar 100 GB, filtrar localmente.
  Coste: 100 GB × 0,09 $ = 9,00 $

S3 Select: Filtrar en el lado del servidor.
  Coste: 100 GB escaneados × 0,002 $/GB + 1 GB devuelto × 0,0007 $/GB
  = 0,20 $ + 0,0007 $ = 0,20 $
Ahorro: 97%
```

#### Optimización de Costes de Lambda

**1. Optimización de la memoria**:
```
Precios de Lambda:
- Por solicitud: 0,20 $ por cada 1M de solicitudes
- Por GB-segundo: 0,0000166667 $

Información clave: Más memoria = ejecución más rápida (hasta cierto punto).

Ejemplo de optimización:
Configuración A: 128 MB, 3000 ms de ejecución
Coste: 0,128 GB × 3 segundos = 0,384 GB-segundos

Configuración B: 512 MB, 800 ms de ejecución
Coste: 0,512 GB × 0,8 segundos = 0,410 GB-segundos

Configuración C: 1024 MB, 400 ms de ejecución
Coste: 1,024 GB × 0,4 segundos = 0,410 GB-segundos

Resultado: B o C pueden ser óptimos (ejecución más rápida, coste similar).

Utiliza la herramienta AWS Lambda Power Tuning:
https://github.com/alexcasalboni/aws-lambda-power-tuning
```

**2. Optimización del código**:
```python
# ANTES: Ineficiente (crea una nueva conexión en cada invocación)
def lambda_handler(event, context):
    import boto3
    s3 = boto3.client('s3')  # Nueva conexión cada vez
    # Procesar datos
    return response

# DESPUÉS: Eficiente (reutiliza la conexión)
import boto3
s3 = boto3.client('s3')  # Creado una vez, reutilizado entre invocaciones

def lambda_handler(event, context):
    # Reutilizar el cliente s3 existente
    # Procesar datos
    return response

Ahorro: reducción del 30-50% en el tiempo de ejecución
```

**3. Concurrencia reservada (¡con cuidado!)**:
```
Reservar concurrencia para funciones críticas.
PERO: La concurrencia reservada cuenta para el límite de la cuenta.

Caso de uso:
- Función de API de producción: Reservar 100
- Procesamiento en segundo plano: Sin reserva (utilizar la capacidad disponible)

Impacto en el coste: Sin coste directo, pero evita el sobreaprovisionamiento.
```

**4. Lambda frente a Fargate frente a EC2**:
```
Lambda es ideal para:
- Cargas de trabajo esporádicas y basadas en eventos.
- Tiempo de ejecución < 15 minutos.
- Precisión de facturación en milisegundos.

Fargate es ideal para:
- Procesos contenedorizados de larga duración.
- Patrones de uso constantes.
- Ejecución de 15 minutos a horas.

EC2 es ideal para:
- Aplicaciones siempre activas.
- Requisitos de cumplimiento específicos.
- Necesidades de un SO personalizado.

Comparativa de costes (ejemplo de carga de trabajo: 10 horas/mes):
Lambda: 10 h × 1 GB × 3600 s × 0,0000166667 $ = 0,60 $
Fargate: 10 h × 1 vCPU, 2 GB = 4,50 $
EC2 (t3.small, On-Demand): 730 h × 0,0208 $ = 15,18 $
EC2 con parada/inicio: 10 h × 0,0208 $ = 0,21 $

Ganador para este caso de uso: Lambda o EC2 con parada.
```

#### Optimización de Costes de CloudFront

**1. Optimizar la transferencia de datos**:
```
Estrategias:
├── Comprimir contenido (gzip, brotli)
├── Servir tamaños de imagen adecuados
├── Utilizar formatos modernos (WebP, AVIF)
├── Implementar almacenamiento en caché en el lado del cliente
└── Establecer valores de TTL adecuados

Ejemplo:
Sin comprimir: 10 TB/mes × 0,085 $/GB = 850 $
Comprimido (reducción del 70%): 3 TB/mes × 0,085 $/GB = 255 $
Ahorro: 595 $/mes (70%)
```

**2. Origin Shield**:
```
Qué: Capa de almacenamiento en caché adicional entre CloudFront y el origen.

Cuándo usarlo:
- Múltiples distribuciones de CloudFront que acceden al mismo origen.
- El origen tiene límites de velocidad o problemas de escalado.
- Invalidaciones de caché frecuentes.

Coste: 0,01 $/10.000 solicitudes + una pequeña tarifa horaria.
Beneficio: Reduce las solicitudes al origen en un 50-80%.

Ejemplo:
Solicitudes al origen sin Shield: 100M/mes
Coste (API Gateway): 100M × 3,50 $/M = 350 $

Con Origin Shield:
CloudFront Shield: 100 $ (tarifa horaria + solicitudes)
Solicitudes al origen reducidas a 20M: 20M × 3,50 $/M = 70 $
Total: 170 $ (ahorro de 180 $/mes, 51%)
```

**3. Clases de precio regionales (Price Classes)**:
```
Las clases de precio determinan el uso de las ubicaciones de borde:

Class All: Todas las ubicaciones de borde globales (coste más alto).
Class 200: América del Norte, Europa, Asia, Oriente Medio, África.
Class 100: Solo América del Norte y Europa.

Ejemplo (transferencia de 10 TB):
Todas las ubicaciones: 850 $
Price Class 200: 765 $ (ahorro del 10%)
Price Class 100: 680 $ (ahorro del 20%)

Elegir en función de la geografía de los usuarios.
```

#### Optimización de Costes de EBS

**1. Selección del tipo de volumen**:
```
Árbol de decisión del tipo de volumen:
├── ¿Base de datos transaccional? → io2 o io2 Block Express
├── ¿Propósito general, SSD? → gp3 (¡no gp2!)
├── ¿E/S secuencial grande? → st1 (HDD)
├── ¿Acceso infrecuente? → sc1 (HDD, el más barato)
└── ¿Volumen de arranque? → gp3

Comparativa de precios (1 TB):
gp2: 100 $/mes
gp3: 80 $/mes (20% más barato)
io2: 125 $/mes + 65 $ por cada 1.000 IOPS
st1: 45 $/mes
sc1: 15 $/mes
```

**2. Optimización de Snapshots de EBS**:
```
Problema: Los snapshots incrementales acumulan costes.

Soluciones:
├── Eliminar snapshots antiguos (automatizar con Data Lifecycle Manager)
├── Utilizar EBS Snapshot Archive (75% más barato)
├── Copiar snapshots a S3 Glacier para retención a largo plazo
└── Utilizar AWS Backup para una gestión centralizada

Ejemplo (snapshots de 100 GB, retención de 12 meses):
Snapshots estándar: 12 × 100 GB × 0,05 $ = 60 $/mes
Snapshot Archive: 12 × 100 GB × 0,0125 $ = 15 $/mes
Ahorro: 75%
```

**3. Limpieza de volúmenes no utilizados**:
```python
import boto3
from datetime import datetime

ec2 = boto3.client('ec2')

def find_unused_volumes():
    """Buscar volúmenes EBS no asociados"""
    volumes = ec2.describe_volumes(
        Filters=[{'Name': 'status', 'Values': ['available']}]
    )

    unused = []
    for volume in volumes['Volumes']:
        age_days = (datetime.now().replace(tzinfo=None) - volume['CreateTime'].replace(tzinfo=None)).days

        if age_days > 7:  # No asociado durante más de 7 días
            unused.append({
                'VolumeId': volume['VolumeId'],
                'Size': volume['Size'],
                'CreateTime': volume['CreateTime'],
                'MonthlyCost': volume['Size'] * 0,10  # Precios de gp3
            })

    return unused

# Problema común: Volúmenes de instancias terminadas
# Acción: Eliminar o hacer un snapshot y luego eliminar
```

---

## Gobernanza de Costes y FinOps

### Marco de Trabajo FinOps (FinOps Framework)

**Qué es FinOps**:
- Operaciones financieras (Financial Operations) para la nube.
- Colaboración entre Finanzas, Ingeniería y Negocio.
- Objetivo: Maximizar el valor de negocio del gasto en la nube.
- Optimización continua, no un proyecto puntual.

**Las tres fases de FinOps**:

```
1. Fase de Información (Inform)
   ├── Visibilidad de los costes de la nube.
   ├── Asignación precisa de costes.
   ├── Evaluación comparativa (benchmarking) y previsión.
   └── Elaboración de informes y analítica.

2. Fase de Optimización (Optimize)
   ├── Dimensionamiento adecuado de los recursos.
   ├── Eliminación de desperdicios.
   ├── Descuentos basados en compromisos (RIs/SPs).
   └── Optimización arquitectónica.

3. Fase de Operación (Operate)
   ├── Monitorización continua.
   ├── Políticas automatizadas.
   ├── Gobernanza y cumplimiento.
   └── Adopción cultural.
```

**Estructura del Equipo FinOps**:
```
Líder de FinOps (trasfondo financiero)
├── Arquitectos de la nube (optimización técnica)
├── Equipos de ingeniería (implementadores)
├── Analistas financieros (informes, previsiones)
├── Gerentes de producto (alineación con el valor de negocio)
└── Ejecutivos (estrategia, responsabilidad)

Responsabilidades:
- Revisiones mensuales de costes.
- Planificación trimestral.
- Presupuestación anual.
- Educación continua.
```

---

### Políticas de Gobernanza

#### 1. Barreras de Protección del Gasto (Spending Guardrails)

**Políticas de Control de Servicios (SCPs)**:
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "PreventExpensiveInstances",
      "Effect": "Deny",
      "Action": "ec2:RunInstances",
      "Resource": "arn:aws:ec2:*:*:instance/*",
      "Condition": {
        "StringEquals": {
          "ec2:InstanceType": [
            "*.24xlarge",
            "*.32xlarge",
            "p3.16xlarge",
            "p4d.24xlarge"
          ]
        }
      }
    }
  ]
}
```

**Cuotas de Servicio (Service Quotas)**:
```
Establecer límites de servicio para evitar costes descontrolados:
├── EC2: Máximo 50 instancias por cuenta.
├── RDS: Máximo 10 instancias de DB.
├── S3: Límites de tasa de solicitudes.
└── Lambda: Límite de ejecuciones concurrentes reservadas.

Monitorizar con la consola de Service Quotas.
Alertar cuando se acerque a los límites.
```

#### 2. Cumplimiento del Presupuesto (Budget Enforcement)

**AWS Budgets con Acciones**:
```yaml
Configuración del presupuesto:
  Nombre: Presupuesto-Mensual-Produccion
  Importe: 10.000 $
  Periodo: Mensual

  Umbrales de alerta:
    - 80% (8.000 $): Correo electrónico al líder del equipo.
    - 90% (9.000 $): Correo electrónico al equipo + gerente.
    - 100% (10.000 $): Activar acción de Lambda.

  Acciones del presupuesto (al 100%):
    - Aplicar una SCP restrictiva para evitar la creación de nuevos recursos.
    - Detener instancias de EC2 no críticas.
    - Enviar alerta de PagerDuty.
    - Crear ticket de Jira para revisión.
```

**Lambda de Respuesta Automatizada**:
```python
import boto3

def budget_action_handler(event, context):
    """Ejecutar acciones de cumplimiento del presupuesto"""

    budget_limit = event['budgetLimit']
    actual_spend = event['actualSpend']
    percentage = (actual_spend / budget_limit) * 100

    if percentage >= 100:
        # Detener instancias que no sean de producción
        stop_non_production_instances()

        # Aplicar SCP de emergencia
        apply_emergency_scp()

        # Notificar a las partes interesadas
        send_urgent_notification(actual_spend, budget_limit)

    elif percentage >= 90:
        # Notificación de advertencia
        send_warning_notification(actual_spend, budget_limit)

def stop_non_production_instances():
    ec2 = boto3.client('ec2')

    instances = ec2.describe_instances(
        Filters=[
            {'Name': 'tag:Environment', 'Values': ['Dev', 'Test', 'Staging']},
            {'Name': 'instance-state-name', 'Values': ['running']}
        ]
    )

    for reservation in instances['Reservations']:
        for instance in reservation['Instances']:
            ec2.stop_instances(InstanceIds=[instance['InstanceId']])
```

#### 3. Políticas de Etiquetado

**Políticas de Etiquetas de AWS Organizations**:
```json
{
  "tags": {
    "CostCenter": {
      "tag_key": {
        "@@assign": "CostCenter"
      },
      "tag_value": {
        "@@assign": [
          "CC-ENG-001",
          "CC-SALES-001",
          "CC-MKT-001"
        ]
      },
      "enforced_for": {
        "@@assign": [
          "ec2:instance",
          "rds:db",
          "s3:bucket"
        ]
      }
    },
    "Environment": {
      "tag_key": {
        "@@assign": "Environment"
      },
      "tag_value": {
        "@@assign": [
          "Production",
          "Staging",
          "Development",
          "Test"
        ]
      },
      "enforced_for": {
        "@@assign": [
          "ec2:*",
          "rds:*"
        ]
      }
    }
  }
}
```

---

### Responsabilidad y Propiedad (Accountability and Ownership)

#### 1. Modelo de Propiedad de Costes (Cost Ownership Model)

**Propiedad de Ingeniería**:
```
Principio: Los equipos son dueños de sus costes de infraestructura.

Implementación:
├── Cada equipo tiene una cuenta de AWS dedicada.
├── El líder del equipo revisa los costes mensuales.
├── Los costes se atribuyen al presupuesto del equipo.
├── Revisiones trimestrales de optimización de costes.
└── Las métricas de rendimiento incluyen la eficiencia de costes.

Beneficios:
- Responsabilidad directa.
- Decisiones de optimización más rápidas.
- Eficiencia impulsada por la ingeniería.
- Reducción de la carga de trabajo de Finanzas.
```

**Responsabilidad Compartida**:
```
Equipo de Finanzas:
├── Proporcionar herramientas de visibilidad de costes.
├── Generar informes e información (insights).
├── Establecer políticas de gobernanza.
├── Negociar programas de descuento empresarial (Enterprise Discount Programs).
└── Apoyar la planificación presupuestaria.

Equipos de Ingeniería:
├── Diseñar soluciones eficientes en costes.
├── Dimensionar adecuadamente los recursos.
├── Implementar el autoescalado (auto-scaling).
├── Eliminar recursos no utilizados.
└── Optimizar continuamente.

Equipos de Producto:
├── Justificar el gasto en infraestructura con el valor de negocio.
├── Priorizar características basadas en el ROI.
├── Aprobar cambios importantes en la infraestructura.
└── Establecer equilibrios entre rendimiento y coste.
```

#### 2. Asignación de Centros de Coste

**Asignación de Costes Jerárquica**:
```
Total de la empresa: 500.000 $/mes
├── Ingeniería (300.000 $ - 60%)
│   ├── Equipo de Producto A (120.000 $)
│   ├── Equipo de Producto B (100.000 $)
│   ├── Equipo de Plataforma (50.000 $)
│   └── Equipo de Datos (30.000 $)
├── Ventas (100.000 $ - 20%)
│   ├── Sistemas CRM (60.000 $)
│   └── Analítica (40.000 $)
├── Marketing (80.000 $ - 16%)
│   └── Infraestructura de Campañas (80.000 $)
└── Servicios Compartidos (20.000 $ - 4%)
    ├── Registro/Monitorización (10.000 $)
    └── Herramientas de Seguridad (10.000 $)
```

**Métodos de Asignación**:
```
1. Atribución Directa:
   - Recursos etiquetados con CostCenter.
   - Costes asignados automáticamente.
   - Método más preciso.

2. Asignación Proporcional:
   - Los recursos compartidos se dividen por uso.
   - Ejemplo: Costes de NAT Gateway divididos por transferencia de datos.
   - Requiere métricas de uso.

3. Asignación Fija:
   - Los costes generales se dividen a partes iguales o por número de empleados.
   - Ejemplo: Cuenta de servicios compartidos.
   - Sencillo pero menos preciso.
```

#### 3. KPIs y Métricas

**KPIs Financieros**:
```
Métricas de coste:
├── Tasa de crecimiento mes a mes (objetivo: < 10%).
├── Coste por cliente/transacción (seguimiento de la tendencia).
├── Coste de infraestructura como % de los ingresos (objetivo: < 25%).
├── Gasto desperdiciado (recursos no utilizados) (objetivo: < 5%).
└── Cobertura de instancias reservadas/Savings Plans (objetivo: > 70%).

Métricas de eficiencia:
├── Utilización media de CPU en EC2 (objetivo: 60-80%).
├── Utilización del almacenamiento (objetivo: > 70%).
├── Adopción de instancias Spot (objetivo: > 30% de las cargas de trabajo por lotes).
└── Efectividad del autoescalado (eventos de escalado por semana).
```

**KPIs de Optimización**:
```
Métricas de proceso:
├── Tiempo para implementar recomendaciones (objetivo: < 30 días).
├── Número de anomalías de coste detectadas (monitorizar tendencia).
├── Porcentaje de recursos con las etiquetas requeridas (objetivo: 100%).
├── Precisión de la previsión presupuestaria (objetivo: ± 10%).
└── Tasa de finalización de la revisión de costes mensual (objetivo: 100%).

Compromiso del equipo:
├── Equipos de ingeniería con formación en costes (objetivo: 100%).
├── Ideas de optimización de costes enviadas (fomentar la participación).
├── Ahorros de costes implementados por equipo (gamificación).
└── Revisiones arquitectónicas conscientes del coste (% utilizando Well-Architected).
```

---

## Resolución de Problemas de Facturación (Billing Troubleshooting)

### Problemas Comunes

#### 1. Cargos Inesperados

**Problema**: Factura más alta de lo esperado.

**Pasos de investigación**:
```
1. Identificar el servicio o servicios con cargos inesperados:
   - Revisar Cost Explorer.
   - Comparar mes a mes por servicio.
   - Comprobar las alertas de detección de anomalías.

2. Profundizar en recursos específicos:
   - Utilizar el Informe de Costes y Uso (Cost and Usage Report).
   - Filtrar por servicio, región, ID de recurso.
   - Comprobar las etiquetas para la propiedad.

3. Revisar los registros de CloudTrail:
   - Buscar quién creó los recursos.
   - Cuándo se crearon.
   - Por qué se crearon (comprobar notas, tickets).

4. Causas comunes:
   - Recursos olvidados (instancias de prueba dejadas en ejecución).
   - Eventos de autoescalado.
   - Costes de transferencia de datos.
   - Acumulación de snapshots.
   - Capacidad reservada no utilizada por completo.
```

**Ejemplo de investigación**:
```
Síntoma: Los costes de EC2 aumentaron de 5.000 $ a 15.000 $.

Paso 1: Cost Explorer muestra un pico en us-west-2.
#### 2. Excesos del Nivel Gratuito (Free Tier Overages)

**Problema**: Cargos a pesar de esperar cobertura del Nivel Gratuito.

**Errores comunes**:
- **Nivel Gratuito caducado**: Las ofertas de 12 meses (EC2, S3, RDS) terminan tras 1 año.
- **Límites superados**: 750 horas/mes totales de EC2, no por instancia.
- **Tipo incorrecto**: Solo t2.micro/t3.micro están incluidos.
- **Transferencia de datos**: El Nivel Gratuito tiene límites estrictos de salida a Internet.

#### 3. La Instancia Reservada no se aplica

**Problema**: Se compró una RI pero se siguen viendo cargos On-Demand.

**Razones**:
- **Desajuste de atributos**: El tipo de instancia, región, plataforma o tenencia no coincide.
- **Tiempo de activación**: Puede tardar hasta 48 horas en reflejarse en la factura.
- **RI caducada**: Verificar la fecha de vencimiento en la consola de EC2.

#### 4. Cargos por Transferencia de Datos

**Problema**: Altos costes de transferencia.

**Causas comunes**:
- **Transferencia entre regiones**: Mover datos entre regiones de AWS tiene coste.
- **NAT Gateway**: El procesamiento de datos tiene un coste por GB.
- **Uso de IP pública**: El tráfico entre AZs usando IPs públicas se cobra como tráfico a Internet.

---

### Pasos de Resolución

#### Proceso de Resolución de Problemas Estándar

```
Paso 1: Identificar el problema (Revisar alertas, notar cargos).
Paso 2: Recopilar datos (Cost Explorer, CloudTrail, CloudWatch).
Paso 3: Determinar la causa raíz (Identificar recursos y responsables).
Paso 4: Acciones inmediatas (Detener recursos, fijar límites).
Paso 5: Prevención a largo plazo (SCPs, alertas, formación).
Paso 6: Solicitar crédito (Abrir caso de soporte si procede).
```

#### Cuándo contactar con el Soporte de AWS

**Contactar con el soporte para**:
- Disputas de facturación.
- Problemas técnicos que afectan a los costes.
- Solicitudes de crédito por interrupciones o errores de documentación.
- Orientación experta (planes Business/Enterprise).

---

### Accountability and Ownership

#### 1. Cost Ownership Model

**Engineering Ownership**:
```
Principle: Teams own their infrastructure costs
├── Each team has dedicated AWS account
├── Team lead reviews monthly costs
├── Costs attributed to team budget
├── Quarterly cost optimization reviews
└── Performance metrics include cost efficiency

Benefits:
- Direct accountability
- Faster optimization decisions
- Engineering-driven efficiency
- Reduced Finance overhead
```

**Shared Responsibility**:
```
Finance Team:
├── Provide cost visibility tools
├── Generate reports and insights
├── Establish governance policies
├── Negotiate Enterprise Discount Programs
└── Support budget planning

Engineering Teams:
├── Architect cost-efficient solutions
├── Right-size resources
├── Implement auto-scaling
├── Delete unused resources
└── Optimize continuously

Product Teams:
├── Justify infrastructure spend with business value
├── Prioritize features based on ROI
├── Approve major infrastructure changes
└── Set performance vs cost trade-offs
```

#### 2. Cost Center Allocation

**Hierarchical Cost Allocation**:
```
Company Total: $500,000/month
├── Engineering ($300,000 - 60%)
│   ├── Product Team A ($120,000)
│   ├── Product Team B ($100,000)
│   ├── Platform Team ($50,000)
│   └── Data Team ($30,000)
├── Sales ($100,000 - 20%)
│   ├── CRM Systems ($60,000)
│   └── Analytics ($40,000)
├── Marketing ($80,000 - 16%)
│   └── Campaign Infrastructure ($80,000)
└── Shared Services ($20,000 - 4%)
    ├── Logging/Monitoring ($10,000)
    └── Security Tools ($10,000)
```

**Allocation Methods**:
```
1. Direct Attribution:
   - Resources tagged with CostCenter
   - Costs automatically allocated
   - Most accurate method

2. Proportional Allocation:
   - Shared resources split by usage
   - Example: NAT Gateway costs split by data transfer
   - Requires usage metrics

3. Fixed Allocation:
   - Overhead costs split evenly or by headcount
   - Example: Shared services account
   - Simple but less accurate
```

#### 3. KPIs and Metrics

**Financial KPIs**:
```
Cost Metrics:
├── Month-over-month growth rate (target: < 10%)
├── Cost per customer/transaction (track trend)
├── Infrastructure cost as % of revenue (target: < 25%)
├── Wasted spend (unused resources) (target: < 5%)
└── Reserved Instance/Savings Plans coverage (target: > 70%)

Efficiency Metrics:
├── Average EC2 CPU utilization (target: 60-80%)
├── Storage utilization (target: > 70%)
├── Spot instance adoption (target: > 30% of batch workloads)
└── Auto-scaling effectiveness (scale events per week)
```

**Optimization KPIs**:
```
Process Metrics:
├── Time to implement recommendations (target: < 30 days)
├── Number of cost anomalies detected (monitor trend)
├── Percentage of resources with required tags (target: 100%)
├── Budget forecast accuracy (target: ± 10%)
└── Monthly cost review completion rate (target: 100%)

Team Engagement:
├── Engineering teams with cost training (target: 100%)
├── Cost optimization ideas submitted (encourage participation)
├── Cost savings implemented per team (gamification)
└── Cost-aware architectural reviews (% using Well-Architected)
```

---

   - Find who created the resources
   - When were they created
   - Why were they created (check notes, tickets)

4. Common causes:
   - Forgotten resources (test instances left running)
   - Auto-scaling events
   - Data transfer costs
   - Snapshot accumulation
   - Reserved capacity not fully utilized
```

**Example Investigation**:
```
Symptom: EC2 costs increased from $5,000 to $15,000

Step 1: Cost Explorer shows spike in us-west-2
Step 2: Drill down reveals 20 new m5.4xlarge instances
Step 3: C### Pasos de Resolución

#### Proceso de Resolución de Problemas Estándar

```
Paso 1: Identificar el problema
[ ] Revisar la alerta de facturación o notar un cargo inesperado.
[ ] Anotar el rango de fechas y el importe.
[ ] Identificar el servicio o servicios específicos involucrados.

Paso 2: Recopilar datos
[ ] Cost Explorer: Ver costes por servicio, región, etiqueta.
[ ] Informe de Costes y Uso: Análisis detallado de partidas individuales.
[ ] CloudTrail: Llamadas a la API y eventos de creación de recursos.
[ ] CloudWatch: Métricas de utilización de recursos.

Paso 3: Determinar la causa raíz
[ ] Identificar los recursos específicos que causan los cargos.
[ ] Buscar quién creó/modificó los recursos (usuario/rol de IAM).
[ ] Comprender el contexto de negocio (¿estaba planificado?).
[ ] Comprobar si hay configuraciones incorrectas o errores.

Paso 4: Acciones inmediatas
[ ] Detener/terminar los recursos innecesarios.
[ ] Deshabilitar las características problemáticas.
[ ] Aplicar límites de gasto temporales.
[ ] Documentar los hallazgos.

Paso 5: Prevención a largo plazo
[ ] Implementar barreras de protección (SCPs, políticas de IAM).
[ ] Añadir monitorización/alertas.
[ ] Actualizar los libros de ejecución (runbooks).
[ ] Formar a los miembros del equipo.
[ ] Programar revisiones periódicas.

Paso 6: Solicitar crédito (si procede)
[ ] Recopilar pruebas del problema.
[ ] Abrir un caso de soporte.
[ ] Explicar la situación con claridad.
[ ] Proporcionar los pasos de mitigación tomados.
[ ] Solicitar la consideración de un crédito.
```

#### Cuándo contactar con el Soporte de AWS

**Contactar con el soporte para**:
```
1. Disputas de facturación:
   - Cargos que crees que son incorrectos.
   - Instancia reservada que no se aplica correctamente.
   - Créditos prometidos pero no recibidos.

2. Problemas específicos del servicio:
   - Comportamiento inesperado del servicio.
   - Característica que no funciona como se documenta.
   - Problemas de rendimiento que afectan a los costes.

3. Solicitudes de crédito:
   - Una interrupción del servicio causó excesos.
   - Configuración incorrecta debido a una documentación poco clara.
   - Un problema de la infraestructura de AWS provocó costes.

4. Orientación:
   - Preguntas complejas sobre facturación.
   - Estrategias de optimización de costes (Business/Enterprise).
   - Recomendaciones de instancias reservadas.
```

**Información a proporcionar**:
```
Al abrir un caso de soporte:
├── ID de cuenta.
├── Rango de fechas afectado.
├── Recursos específicos (IDs de instancia, ARNs).
├── Capturas de pantalla de Cost Explorer.
├── Pasos ya tomados para investigar.
├── Impacto en el negocio.
└── Resolución solicitada.

Ejemplo:
"Cuenta: 123456789012
Fecha: 15-18 de enero de 2024
Problema: Cargos inesperados de EC2 en us-west-2 (10.000 $ por encima del presupuesto)
Recursos: 20 instancias m5.4xlarge (IDs: i-xxx, i-yyy...)
Investigación: CloudTrail muestra que AutoScaling lanzó instancias debido a una
               configuración incorrecta de una alarma de CloudWatch.
Acciones tomadas: Se terminaron las instancias, se corrigió el umbral de la alarma.
Impacto en el negocio: Presupuesto de desarrollo excedido, equipo bloqueado.
Solicitud: Por favor, considere un crédito por las 20 horas de uso no intencionado
          (2.000 $ estimados), ya que se trató de un error de configuración detectado rápidamente."
```

---

**Mejor práctica**: Revisar las recomendaciones mensualmente e implementar las sugerencias aplicables.

---

## Resumen de Conceptos Clave

### Modelos de Precios para Recordar

| Modelo | Descuento | Compromiso | Ideal para |
|-------|----------|------------|----------|
| **On-Demand** | 0% | Ninguno | Cargas de trabajo impredecibles. |
| **Reserved** (1 año) | ~40% | 1 año | Cargas de trabajo constantes. |
| **Reserved** (3 años) | ~60-75% | 3 años | Cargas de trabajo constantes a largo plazo. |
| **Spot Instances** | ~90% | Ninguno (puede interrumpirse) | Tolerante a fallos, flexible. |
| **Savings Plans** | ~72% | 1-3 años | Uso de computación flexible. |

### Planes de Soporte para Recordar

| Plan | Coste | TAM | Respuesta (Crítica) | Trusted Advisor |
|------|------|-----|---------------------|----------------|
| **Basic** | Gratuito | No | N/A | 7 comprobaciones. |
| **Developer** | 29 $/mes | No | N/A | 7 comprobaciones. |
| **Business** | 100 $/mes | No | < 1 hora | Todas las comprobaciones. |
| **Enterprise** | 15.000 $/mes | **Sí** | **< 15 min** | Todas las comprobaciones. |

### Nivel Gratuito para Recordar

- **EC2**: 750 horas/mes durante 12 meses.
- **S3**: 5 GB de almacenamiento durante 12 meses.
- **Lambda**: 1 millón de solicitudes/mes (siempre gratuito).
- **DynamoDB**: 25 GB de almacenamiento (siempre gratuito).
- **CloudFront**: 50 GB de transferencia de salida durante 12 meses.

---
### Optimización de Transferencia de Datos

```
4. Transferencia de datos entre AZ:
   - Se cobra como transferencia de internet si se usan IPs públicas.
   - Solución: Use IPs privadas (gratuito dentro de la misma AZ).

5. Replicación innecesaria:
   - Replicación entre regiones de S3 (CRR) activada.
   - Replicar datos que no son necesarios en ambas regiones.
   - Solución: Desactive CRR o use S3 Batch Replication.
```

**Optimización**:
```
1. Revisión de arquitectura:
   - Mantenga los recursos relacionados en la misma región.
   - Use CloudFront para la entrega de contenido.
   - Implemente VPC Endpoints.

2. Compresión:
   - Comprima los datos antes de la transferencia.
   - Use gzip/brotli.
   - Reducción típica del 60-80%.

3. Caching:
   - Implemente ElastiCache.
   - Reduzca las consultas a la base de datos.
   - Disminuya las necesidades de transferencia de datos.

4. Monitoreo con Cost Explorer:
   - Filtre por cargos de transferencia de datos.
   - Identifique los principales contribuyentes.
   - Optimice primero los costos más altos.
```

---

## Preguntas de Repaso

### Pregunta 1
¿Qué principio de precios de **AWS** permite a los clientes pagar solo por los recursos de cómputo que consumen?

A. Pague menos al reservar (**Pay less when you reserve**)
B. Pago por uso (**Pay-as-you-go**)
C. Descuentos por volumen (**Volume-based discounts**)
D. Capacidad reservada (**Reserved capacity**)

<details>
<summary>Mostrar respuesta</summary>

**Respuesta: B. Pago por uso (Pay-as-you-go)**

Explicación: El modelo de precios de pago por uso es el principio fundamental que permite a los clientes pagar solo por lo que usan, sin costos iniciales ni compromisos a largo plazo.
</details>

---

### Pregunta 2
Una empresa desea reducir los costos de **EC2** para una carga de trabajo de producción en estado estable que se ejecuta las 24 horas, los 7 días de la semana. ¿Qué opción de compra proporciona el MAYOR ahorro de costos?

A. Instancias **On-Demand**
B. Instancias **Spot**
C. **Reserved Instances** de 3 años con pago **All Upfront**
D. **Dedicated Hosts**

<details>
<summary>Mostrar respuesta</summary>

**Respuesta: C. Reserved Instances de 3 años con pago All Upfront**

Explicación: Para cargas de trabajo constantes que se ejecutan continuamente, las **Reserved Instances** con un compromiso de 3 años y pago total por adelantado (**All Upfront**) proporcionan el descuento más alto (hasta un 75%). Las instancias **Spot** ofrecen descuentos más altos pero pueden interrumpirse, lo que las hace inadecuadas para cargas de trabajo de producción estables.
</details>

---

### Pregunta 3
¿Qué plan de soporte de **AWS** proporciona un Gerente Técnico de Cuentas (**TAM**)?

A. **Basic**
B. **Developer**
C. **Business**
D. **Enterprise**

<details>
<summary>Mostrar respuesta</summary>

**Respuesta: D. Enterprise**

Explicación: Solo el plan de soporte **Enterprise** incluye un Gerente Técnico de Cuentas (**TAM**) que sirve como un punto de contacto técnico designado.
</details>

---

### Pregunta 4
¿Cuál es el tiempo de respuesta para un problema de caída de sistema crítico para el negocio bajo el plan de soporte **Business**?

A. < 15 minutos
B. < 1 hora
C. < 4 horas
D. < 12 horas

<details>
<summary>Mostrar respuesta</summary>

**Respuesta: B. < 1 hora**

Explicación: El soporte **Business** proporciona una respuesta de < 1 hora para problemas de caída de sistemas críticos para el negocio. El soporte **Enterprise** proporciona < 15 minutos para problemas de misión crítica.
</details>

---

### Pregunta 5
¿Qué herramienta debería usar una empresa para estimar los costos ANTES de desplegar recursos en **AWS**?

A. **AWS Cost Explorer**
B. **AWS Pricing Calculator**
C. **AWS Budgets**
D. **AWS Cost and Usage Report**

<details>
<summary>Mostrar respuesta</summary>

**Respuesta: B. AWS Pricing Calculator**

Explicación: **AWS Pricing Calculator** está diseñado para estimar costos antes del despliegue. **Cost Explorer** analiza los costos históricos, **Budgets** establece alertas de costos y el **Cost and Usage Report** proporciona datos de facturación detallados.
</details>

---

### Pregunta 6
Una empresa tiene múltiples cuentas de **AWS** y desea recibir una sola factura para todas las cuentas. ¿Qué característica deberían usar?

A. **AWS Organizations** con facturación consolidada
B. **AWS Cost Explorer**
C. **AWS Budgets**
D. Etiquetas de asignación de costos de **AWS**

<details>
<summary>Mostrar respuesta</summary>

**Respuesta: A. AWS Organizations con facturación consolidada**

Explicación: La facturación consolidada a través de **AWS Organizations** combina el uso de todas las cuentas en una sola factura, lo que potencialmente proporciona descuentos por volumen.
</details>

---

### Pregunta 7
¿Qué servicio de **AWS** utiliza el aprendizaje automático para detectar patrones de gasto inusuales y enviar alertas?

A. **AWS Budgets**
B. **AWS Cost Anomaly Detection**
C. **AWS Cost Explorer**
D. **AWS Trusted Advisor**

<details>
<summary>Mostrar respuesta</summary>

**Respuesta: B. AWS Cost Anomaly Detection**

Explicación: **AWS Cost Anomaly Detection** utiliza el aprendizaje automático para identificar automáticamente patrones de gasto inusuales y enviar alertas, sin necesidad de configuración manual de umbrales.
</details>

---

### Pregunta 8
¿Cuántas verificaciones de **Trusted Advisor** están disponibles con los planes de soporte **Basic** y **Developer**?

A. Ninguna
B. 7 verificaciones principales
C. 50 verificaciones
D. Todas las verificaciones

<details>
<summary>Mostrar respuesta</summary>

**Respuesta: B. 7 verificaciones principales**

Explicación: Los planes de soporte **Basic** y **Developer** tienen acceso a 7 verificaciones principales de **Trusted Advisor**. Los planes **Business** y **Enterprise** tienen acceso a todas las verificaciones (más de 50).
</details>

---

### Pregunta 9
¿Qué oferta del Nivel Gratuito de **AWS** NUNCA expira?

A. 750 horas/mes de **EC2 t2.micro**
B. 5 GB de almacenamiento **S3 Standard**
C. 1 millón de solicitudes de **Lambda** por mes
D. 750 horas/mes de **RDS db.t2.micro**

<details>
<summary>Mostrar respuesta</summary>

**Respuesta: C. 1 millón de solicitudes de Lambda por mes**

Explicación: El millón de solicitudes mensuales de **Lambda** es parte del nivel "Siempre gratis" (**Always Free**) que nunca expira. Las ofertas de **EC2**, **S3** y **RDS** mencionadas son parte del nivel gratuito de 12 meses.
</details>

---

### Pregunta 10
Un equipo de desarrollo necesita rastrear su gasto en **AWS** y recibir alertas cuando los costos superen los $1,000 por mes. ¿Qué servicio deberían usar?

A. **AWS Cost Explorer**
B. **AWS Budgets**
C. **AWS Pricing Calculator**
D. **AWS Organizations**

<details>
<summary>Mostrar respuesta</summary>

**Respuesta: B. AWS Budgets**

Explicación: **AWS Budgets** le permite establecer presupuestos de costos personalizados y recibir alertas (vía correo electrónico o **SNS**) cuando el gasto supera los umbrales. Los dos primeros presupuestos son gratuitos.
</details>

---

### Pregunta 11
¿Qué escenario de transferencia de datos suele ser GRATUITO en **AWS**?

A. Transferencia de datos hacia internet
B. Transferencia de datos desde **S3** hacia internet
C. Transferencia de datos ENTRANTE a **AWS** desde internet
D. Transferencia de datos entre regiones de **AWS**

<details>
<summary>Mostrar respuesta</summary>

**Respuesta: C. Transferencia de datos ENTRANTE a AWS desde internet**

Explicación: La transferencia de datos HACIA **AWS** desde internet suele ser gratuita. La transferencia de datos SALIENTE hacia internet y entre regiones tiene cargo.
</details>

---

### Pregunta 12
¿Qué opción de precio de **EC2** es MEJOR para cargas de trabajo tolerantes a fallos que pueden manejar interrupciones?

A. Instancias **On-Demand**
B. Instancias **Reserved**
C. Instancias **Spot**
D. **Dedicated Hosts**

<details>
<summary>Mostrar respuesta</summary>

**Respuesta: C. Instancias Spot**

Explicación: Las instancias **Spot** ofrecen hasta un 90% de descuento pero **AWS** puede interrumpirlas con un aviso de 2 minutos, lo que las hace ideales para cargas de trabajo flexibles y tolerantes a fallos como el procesamiento por lotes.
</details>

---

### Pregunta 13
¿Cuál es la fuente más completa de datos detallados de costos y uso de **AWS**?

A. **AWS Cost Explorer**
B. **AWS Cost and Usage Report**
C. **AWS Budgets**
D. Estado de cuenta mensual

<details>
<summary>Mostrar respuesta</summary>

**Respuesta: B. AWS Cost and Usage Report**

Explicación: El **AWS Cost and Usage Report** proporciona el desglose más detallado de los costos y el uso, entregado a **S3** para su análisis con herramientas como **Athena** o **Redshift**.
</details>

---

### Pregunta 14
¿Cuál es el plan de soporte de **AWS** MÍNIMO requerido para soporte telefónico 24/7?

A. **Basic**
B. **Developer**
C. **Business**
D. **Enterprise**

<details>
<summary>Mostrar respuesta</summary>

**Respuesta: C. Business**

Explicación: El soporte **Business** es el plan mínimo que proporciona soporte telefónico, por correo electrónico y por chat las 24 horas, los 7 días de la semana. **Developer** solo proporciona soporte por correo electrónico en horario comercial.
</details>

---

### Pregunta 15
Una empresa desea orientación arquitectónica específica para sus casos de uso y entorno de producción. ¿Qué plan de soporte deberían elegir como MÍNIMO?

A. **Basic**
B. **Developer** (orientación general)
C. **Business** (orientación contextual)
D. **Enterprise** (orientación consultiva)

<details>
<summary>Mostrar respuesta</summary>

**Respuesta: C. Business**

Explicación: El soporte **Business** proporciona orientación arquitectónica contextual relacionada con casos de uso específicos. **Developer** solo proporciona orientación general, mientras que **Enterprise** proporciona orientación consultiva con un **TAM**.
</details>

---

### Pregunta 16
¿Qué servicio de **AWS** le ayuda a pronosticar costos futuros basados en patrones de uso históricos?

A. **AWS Budgets**
B. **AWS Cost Explorer**
C. **AWS Pricing Calculator**
D. **AWS Cost and Usage Report**

<details>
<summary>Mostrar respuesta</summary>

**Respuesta: B. AWS Cost Explorer**

Explicación: **Cost Explorer** incluye capacidades de pronóstico que predicen los costos futuros basados en patrones de uso históricos hasta por 12 meses. **AWS Budgets** establece límites de gasto, **Pricing Calculator** estima nuevos despliegues y el **Cost and Usage Report** proporciona datos detallados pero no pronósticos.
</details>

---

### Pregunta 17
Una empresa quiere evitar que los usuarios lancen instancias **EC2** en regiones fuera de **us-east-1** y **us-west-2**. ¿Qué característica de **AWS** deberían usar?

A. Políticas de **IAM**
B. Políticas de Control de Servicios (**SCPs**)
C. **AWS Budgets**
D. Grupos de Recursos (**Resource Groups**)

<details>
<summary>Mostrar respuesta</summary>

**Respuesta: B. Políticas de Control de Servicios (SCPs)**

Explicación: Las **Service Control Policies (SCPs)** en **AWS Organizations** pueden restringir acciones a través de las cuentas, incluyendo evitar la creación de recursos en regiones específicas. Aunque las políticas de **IAM** también pueden restringir regiones, las **SCPs** proporcionan una aplicación a nivel de toda la organización.
</details>

---

### Pregunta 18
¿Cuál es el beneficio principal de usar etiquetas de asignación de costos en **AWS**?

A. Mejorar el rendimiento de la aplicación
B. Rastrear y asignar costos a proyectos o equipos específicos
C. Reducir los costos de transferencia de datos
D. Aumentar los límites de instancias **EC2**

<details>
<summary>Mostrar respuesta</summary>

**Respuesta: B. Rastrear y asignar costos a proyectos o equipos específicos**

Explicación: Las etiquetas de asignación de costos le permiten organizar y rastrear los costos de **AWS** etiquetando los recursos con etiquetas significativas (como proyecto, departamento o entorno), lo que permite un seguimiento detallado de los costos y informes de facturación interna (**chargeback/showback**).
</details>

---

### Pregunta 19
¿Qué opción de pago de **Reserved Instance** proporciona el descuento MÁS alto?

A. **No Upfront** (Sin pago inicial)
B. **Partial Upfront** (Pago inicial parcial)
C. **All Upfront** (Todo el pago inicial)
D. **On-Demand** (Bajo demanda)

<details>
<summary>Mostrar respuesta</summary>

**Respuesta: C. All Upfront**

Explicación: El pago **All Upfront** para **Reserved Instances** proporciona el descuento más alto porque usted paga el costo total por adelantado. **Partial Upfront** ofrece un descuento medio y **No Upfront** proporciona el descuento más bajo (pero no requiere pago inicial).
</details>

---

### Pregunta 20
Un equipo de desarrollo solo usa sus recursos de **AWS** durante el horario comercial (8 AM - 6 PM, lunes a viernes). ¿Cuál es la MEJOR manera de optimizar los costos?

A. Comprar **Reserved Instances**
B. Usar **Spot Instances**
C. Implementar escalado programado para detener/iniciar instancias
D. Usar **Savings Plans**

<details>
<summary>Mostrar respuesta</summary>

**Respuesta: C. Implementar escalado programado para detener/iniciar instancias**

Explicación: Para los recursos utilizados solo durante el horario comercial, detener las instancias cuando no están en uso (usando **AWS Instance Scheduler** o escalado programado) proporciona la mejor optimización de costos. Las **Reserved Instances** y los **Savings Plans** requieren un compromiso a largo plazo y son mejores para cargas de trabajo 24/7.
</details>

---

### Pregunta 21
¿Qué plan de soporte de **AWS** incluye acceso a TODAS las verificaciones de **Trusted Advisor**?

A. **Basic**
B. **Developer**
C. **Business**
D. Tanto **Business** como **Enterprise**

<details>
<summary>Mostrar respuesta</summary>

**Respuesta: D. Tanto Business como Enterprise**

Explicación: Tanto los planes de soporte **Business** como **Enterprise** proporcionan acceso a todas las verificaciones de **Trusted Advisor** (más de 50 verificaciones). Los planes **Basic** y **Developer** solo tienen acceso a 7 verificaciones principales que cubren seguridad básica y límites de servicio.
</details>

---

### Pregunta 22
¿Cuál es el **SLA** de tiempo de respuesta para un problema de caída de sistema crítico bajo el plan de soporte **Enterprise**?

A. < 1 hora
B. < 30 minutos
C. < 15 minutos
D. < 4 horas

<details>
<summary>Mostrar respuesta</summary>

**Respuesta: C. < 15 minutos**

Explicación: El soporte **Enterprise** proporciona un tiempo de respuesta de < 15 minutos para problemas de caída de sistemas críticos. Este es el tiempo de respuesta más rápido disponible y es exclusivo del plan **Enterprise**.
</details>

---

### Pregunta 23
Una empresa tiene un conjunto de datos de 100 TB al que se accede una vez al año para auditorías de cumplimiento. ¿Qué clase de almacenamiento de **S3** proporciona el costo MÁS bajo?

A. **S3 Standard**
B. **S3 Standard-IA**
C. **S3 Glacier Flexible Retrieval**
D. **S3 Glacier Deep Archive**

<details>
<summary>Mostrar respuesta</summary>

**Respuesta: D. S3 Glacier Deep Archive**

Explicación: **S3 Glacier Deep Archive** es la clase de almacenamiento de más bajo costo, diseñada para datos que rara vez se acceden (una o dos veces al año). Es ideal para el archivo a largo plazo y datos de cumplimiento con tiempos de recuperación de 12 a 48 horas.
</details>

---

### Pregunta 24
¿Qué beneficio de facturación consolidada permite que múltiples cuentas de **AWS** reciban descuentos por volumen?

A. El uso combinado entre cuentas califica para precios por niveles
B. **Reserved Instances** compartidas
C. Transferencia de datos gratuita entre cuentas
D. Políticas de **IAM** unificadas

<details>
<summary>Mostrar respuesta</summary>

**Respuesta: A. El uso combinado entre cuentas califica para precios por niveles**

Explicación: La facturación consolidada combina el uso de todas las cuentas vinculadas, lo que permite a la organización alcanzar niveles de volumen más altos más rápido y recibir mejores precios. Por ejemplo, si una cuenta usa 8 TB de **S3** y otra usa 4 TB, los 12 TB combinados califican para descuentos por volumen.
</details>

---

### Pregunta 25
¿Qué herramienta de **AWS** proporciona recomendaciones impulsadas por **ML** para ajustar el tamaño de las instancias **EC2**?

A. **AWS Trusted Advisor**
B. **AWS Compute Optimizer**
C. **AWS Cost Explorer**
D. **AWS Budgets**

<details>
<summary>Mostrar respuesta</summary>

**Respuesta: B. AWS Compute Optimizer**

Explicación: **AWS Compute Optimizer** utiliza el aprendizaje automático (**machine learning**) para analizar las métricas de utilización históricas y proporcionar recomendaciones para los tipos óptimos de instancias **EC2**, volúmenes **EBS** y funciones **Lambda**. **Trusted Advisor** también proporciona recomendaciones, pero **Compute Optimizer** utiliza un análisis de **ML** más sofisticado.
</details>

---

### Pregunta 26
¿Qué escenario de transferencia de datos suele ser GRATUITO en **AWS**?

A. Transferencia de datos desde **EC2** a internet
B. Transferencia de datos desde **EC2** a **S3** en la misma región
C. Transferencia de datos entre regiones de **AWS**
D. Transferencia de datos desde **CloudFront** a internet

<details>
<summary>Mostrar respuesta</summary>

**Respuesta: B. Transferencia de datos desde EC2 a S3 en la misma región**

Explicación: La transferencia de datos entre servicios de **AWS** dentro de la misma región suele ser gratuita. La transferencia de datos SALIENTE a internet, entre regiones y desde **CloudFront** incurre en cargos (aunque las tarifas de **CloudFront** suelen ser más bajas que las transferencias directas).
</details>

---

### Pregunta 27
Una empresa quiere eliminar automáticamente los objetos de **S3** con más de 90 días. ¿Qué característica deberían usar?

A. Control de versiones de **S3** (**S3 Versioning**)
B. Políticas de ciclo de vida de **S3** (**S3 Lifecycle Policies**)
C. Replicación de **S3** (**S3 Replication**)
D. Inventario de **S3** (**S3 Inventory**)

<details>
<summary>Mostrar respuesta</summary>

**Respuesta: B. Políticas de ciclo de vida de S3 (S3 Lifecycle Policies)**

Explicación: Las **S3 Lifecycle Policies** le permiten transicionar automáticamente objetos a diferentes clases de almacenamiento o eliminarlos según la antigüedad u otros criterios. Esto es ideal para automatizar la retención de datos y reducir los costos de almacenamiento.
</details>

---

### Pregunta 28
¿Qué servicio de **AWS** proporciona una vista personalizada del estado del servicio de **AWS** que afecta a SUS recursos específicos?

A. **AWS Service Health Dashboard**
B. **AWS Personal Health Dashboard**
C. **AWS Trusted Advisor**
D. **AWS CloudWatch**

<details>
<summary>Mostrar respuesta</summary>

**Respuesta: B. AWS Personal Health Dashboard**

Explicación: **AWS Personal Health Dashboard** proporciona notificaciones personalizadas y específicas de la cuenta sobre eventos que afectan a sus recursos. El **Service Health Dashboard** muestra el estado general del servicio de **AWS** para todos los clientes, no información personalizada.
</details>

---

### Pregunta 29
¿Cuál es el costo mensual mínimo para el soporte **AWS Business**?

A. Gratis
B. $29
C. $100
D. $15,000

<details>
<summary>Mostrar respuesta</summary>

**Respuesta: C. $100**

Explicación: El soporte **Business** tiene un costo mensual mínimo de $100 o el 10% del uso mensual de **AWS** (lo que sea mayor, con precios por niveles). **Developer** tiene un mínimo de $29 y **Enterprise** tiene un mínimo de $15,000.
</details>

---

### Pregunta 30
Una empresa ha comprado **Reserved Instances** pero no se están aplicando a sus instancias **EC2** en ejecución. ¿Cuál es la razón MÁS probable?

A. Las **Reserved Instances** tardan 30 días en activarse
B. Desajuste en el tipo de instancia o región
C. Las **Reserved Instances** solo se aplican a nuevas instancias
D. El ciclo de facturación no se ha completado

<details>
<summary>Mostrar respuesta</summary>

**Respuesta: B. Desajuste en el tipo de instancia o región**

Explicación: Las **Reserved Instances** deben coincidir con el tipo de instancia, la plataforma (**OS**), la tenencia y la región de las instancias en ejecución. Si alguno de estos atributos no coincide, el descuento de **RI** no se aplicará. Las **RIs** suelen activarse en cuestión de horas, no días.
</details>

---

### Pregunta 31
¿Qué opción de compra de **EC2** puede proporcionar hasta un 90% de descuento pero las instancias pueden interrumpirse con un aviso de 2 minutos?

A. Instancias **On-Demand**
B. Instancias **Reserved**
C. Instancias **Spot**
D. **Dedicated Hosts**

<details>
<summary>Mostrar respuesta</summary>

**Respuesta: C. Instancias Spot**

Explicación: Las instancias **Spot** ofrecen hasta un 90% de descuento en comparación con los precios **On-Demand** al utilizar la capacidad **EC2** no utilizada. Sin embargo, **AWS** puede reclamar estas instancias con un aviso de 2 minutos cuando se necesita capacidad, lo que las hace adecuadas para cargas de trabajo tolerantes a fallos.
</details>

---

### Pregunta 32
¿Cuál es la diferencia principal entre los **Compute Savings Plans** y los **EC2 Instance Savings Plans**?

A. Los **Compute SPs** ofrecen descuentos más altos
B. Los **Compute SPs** se aplican a **EC2**, **Fargate** y **Lambda**; los **EC2 Instance SPs** solo se aplican a familias de instancias **EC2** específicas
C. Los **EC2 Instance SPs** son más flexibles
D. Los **Compute SPs** requieren compromisos más largos

<details>
<summary>Mostrar respuesta</summary>

**Respuesta: B. Los Compute SPs se aplican a EC2, Fargate y Lambda; los EC2 Instance SPs solo se aplican a familias de instancias EC2 específicas**

Explicación: Los **Compute Savings Plans** proporcionan la mayor flexibilidad, aplicándose a **EC2**, **Fargate** y **Lambda** en cualquier familia de instancias, tamaño, región o sistema operativo. Los **EC2 Instance Savings Plans** ofrecen descuentos más altos pero solo se aplican a una familia de instancias específica en una región elegida.
</details>

---

### Pregunta 33
¿Qué herramienta de **AWS** debería usar para crear una estimación de costos detallada ANTES de desplegar recursos?

A. **AWS Cost Explorer**
B. **AWS Budgets**
C. **AWS Pricing Calculator**
D. **AWS Cost and Usage Report**

<details>
<summary>Mostrar respuesta</summary>

**Respuesta: C. AWS Pricing Calculator**

Explicación: **AWS Pricing Calculator** le permite modelar y estimar los costos de los servicios de **AWS** antes del despliegue. **Cost Explorer** analiza los costos históricos, **Budgets** establece límites de gasto y el **Cost and Usage Report** proporciona datos de facturación detallados para los recursos existentes.
</details>

---

### Pregunta 34
Una empresa quiere recibir alertas cuando se pronostica que su factura mensual de **AWS** superará los $5,000. ¿Qué servicio deberían usar?

A. **AWS Cost Anomaly Detection**
B. **AWS Budgets**
C. **AWS Trusted Advisor**
D. Alarmas de **CloudWatch**

<details>
<summary>Mostrar respuesta</summary>

**Respuesta: B. AWS Budgets**

Explicación: **AWS Budgets** le permite establecer presupuestos de costos y uso personalizados con alertas basadas en el gasto real o en los montos pronosticados. Puede configurar alertas en umbrales específicos (por ejemplo, cuando se pronostica que superará los $5,000).
</details>

---

### Pregunta 35
¿Qué plan de soporte de **AWS** proporciona un Gerente Técnico de Cuentas (**TAM**) y Gestión de Eventos de Infraestructura (**Infrastructure Event Management**)?

A. **Developer**
B. **Business**
C. **Enterprise**
D. Tanto **Business** como **Enterprise**

<details>
<summary>Mostrar respuesta</summary>

**Respuesta: C. Enterprise**

Explicación: Solo el soporte **Enterprise** incluye un Gerente Técnico de Cuentas (**TAM**) dedicado y Gestión de Eventos de Infraestructura (**IEM**). Estos servicios proporcionan orientación proactiva, coordinación para lanzamientos de productos y revisiones operativas continuas.
</details>

---

## Conclusiones Clave

✅ **Fundamentos de Precios**: Entienda el pago por uso, la capacidad reservada, los descuentos por volumen y la ausencia de costos iniciales.

✅ **Capa Gratuita**: Conozca los tres tipos: Siempre Gratis, 12 meses gratis y pruebas.

✅ **Herramientas de Gestión de Costos**: **Pricing Calculator** (estimar), **Cost Explorer** (analizar), **Budgets** (alertar), **Cost and Usage Report** (datos detallados).

✅ **Planes de Soporte**: Memorice los tiempos de respuesta, la disponibilidad del **TAM** (solo **Enterprise**) y el acceso a **Trusted Advisor**.

✅ **Optimización de Costos**: Ajuste de tamaño (**right-sizing**), **Reserved Instances**, **Spot Instances**, **Auto Scaling**, optimización de almacenamiento y optimización de transferencia de datos.

✅ **Facturación Consolidada**: Combine cuentas en **AWS Organizations** para obtener descuentos por volumen y una facturación única.

✅ **Transferencia de Datos**: La entrada es gratuita, la salida y entre regiones tienen cargo.

---

[← Anterior: Tecnología y Servicios](04-technology-services.md) | [Volver al Inicio](README.md) | [Siguiente: Plan de Estudio →](06-study-plan.md)
