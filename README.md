# Arquitectura de soluciones: banca por internet
 
Caso de estudio de arquitectura para un sistema de banca por internet con canales web y móvil. Permite consultar movimientos históricos y realizar transferencias y pagos, entre cuentas propias e interbancarias, integrándose con un core bancario legado.
 
El documento incluye los diagramas del modelo C4 (contexto, contenedores y componentes) y las decisiones de arquitectura con sus alternativas y consecuencias.
 
## Objetivo
 
Diseñar una solución que sea segura, disponible y tolerante a fallos, y que aísle al core bancario para que su lentitud o caída no afecte a los canales.
 
## Requisitos no funcionales
 
Las metas son propuestas para el caso de estudio.
 
| Requisito | Meta propuesta |
| --- | --- |
| Alta disponibilidad | 99,95 % en consultas y transferencias |
| Recuperación ante desastres | RPO de 5 minutos y RTO de 1 hora en datos transaccionales |
| Rendimiento | Consultas con latencia p95 menor a 500 ms |
| Tolerancia a fallos | La caída de notificaciones, auditoría o core no impide las consultas |
| Protección de datos | Cifrado en tránsito y en reposo, enmascaramiento y mínimo privilegio |
| Auditoría | Registros que no se pueden alterar |
| Excelencia operativa | Autorrecuperación y trazas de punta a punta |
 
## Vistas de la arquitectura
 
- **Contexto:** el sistema de banca por internet y sus relaciones con autenticación, onboarding, core bancario, sistema interbancario, notificaciones y auditoría.
- **Contenedores:** Web SPA, app móvil, API Gateway, BFFs, microservicios, broker de mensajes, bases de datos y caché.
- **Componentes:** SPA web, app móvil, API Gateway, BFF, microservicio genérico, microservicio de integración, worker de actualización, notificaciones y auditoría.
## Decisiones de arquitectura
 
| # | Decisión | Resumen |
| --- | --- | --- |
| 1 | API Gateway y BFF por canal | Azure API Management como puerta de entrada y un BFF para cada canal |
| 2 | Microservicios y aislamiento del core | Base de datos por servicio y un microservicio de integración que es el único que habla con el core |
| 3 | Lecturas rápidas con caché | Caché de lectura en Redis con consistencia eventual; el saldo se valida contra el core antes de transferir |
| 4 | Transferencias e idempotencia | Llave de idempotencia con restricción de unicidad en base de datos, estados de la transacción, reintentos y conciliación |
| 5 | Mensajería | Apache Kafka con Avro y Schema Registry |
| 6 | Auditoría | Outbox en cada servicio, CDC hacia Kafka y base de auditoría de solo adición |
| 7 | Notificaciones | Worker asíncrono con Azure Communication Services y respaldo del historial |
| 8 | Seguridad | WAF, mTLS entre servicios, secretos en Key Vault, cifrado de datos sensibles y segundo factor en transferencias |
| 9 | Plataforma y operación | Kubernetes en varias zonas, región secundaria para desastres y observabilidad con Application Insights |
 
## Patrones aplicados
 
API Gateway, Backend for Frontend, microservicios con base de datos por servicio, arquitectura dirigida por eventos, outbox con captura de cambios (CDC), circuit breaker, idempotencia, caché de lectura (cache-aside), arquitectura limpia y aislamiento del sistema legado.
 
## Tecnologías propuestas
 
Angular, .NET MAUI o Kotlin Multiplatform, .NET, Azure API Management, Apache Kafka, Redis, SQL Server, Azure Communication Services, Azure Application Insights, Azure Key Vault y Kubernetes.
 
## Alcance
 
Es un diseño de referencia: no incluye implementación y no corresponde a un banco real. Las metas numéricas deberían validarse con el negocio y con pruebas de carga.
 
## Autor
 
Javier Díaz
 
- LinkedIn: https://www.linkedin.com/in/javiec
- Correo: javier.diaz.tech@gmail.com
## Licencia
 
Todos los derechos reservados.
 
Este repositorio se publica con fines educativos y de demostración profesional. El uso, distribución o comercialización de su contenido requiere autorización expresa del autor.
