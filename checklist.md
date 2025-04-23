✅ Checklist Técnico – Prueba Técnica de Transacción Financiera Distribuida
🔧 1. Entorno de Desarrollo
 Docker Desktop instalado con Kubernetes habilitado.

 Repositorio unificado con carpetas separadas por servicio:

/api

/antifraude

/updatedb

/kafka

/redis

/postgres

/jaeger

/otel

 Docker Compose para desarrollo local.

 Configurar skaffold (opcional) para flujos de desarrollo continuo con K8s.

🚀 2. Servicios Principales
API Gateway (NestJS + GraphQL)
 Implementado en NestJS con GraphQL.

 Actúa como entrypoint para las transacciones.

 Es un producer de Kafka, pero desacoplado (usa un servicio producer interno o sidecar opcional).

 Implementa caching con Redis (solo para estado “final”).

 Middleware para tracing y logging con OpenTelemetry.

Servicio Antifraude
 Consumer Kafka del topic de transacciones.

 Valida operaciones de forma simulada.

 Produce al topic de respuestas (fraude aprobado/denegado).

 Manda métricas/traces con OpenTelemetry.

Servicio UpdateDB
 Consumer del topic de respuestas antifraude.

 Actualiza el estado en PostgreSQL.

 Actualiza el estado en Redis si es definitivo (no si es “pendiente”).

 Manda métricas/traces con OpenTelemetry.

💾 3. Infraestructura y Persistencia
 PostgreSQL para persistencia de transacciones.

 Usar ORM como TypeORM o Prisma.

 Indexado por ID de operación y timestamp.

 Redis (modo standalone) para cacheo de respuestas finalizadas.

📊 4. Kafka
 Usar Bitnami Kafka image en Docker Compose.

 2 Topics:

transacciones → consumer antifraude.

respuesta-validacion → consumer updatedb.

 Configurar particiones para pruebas de escalabilidad.

 Instrumentar con Kafka Exporter (opcional).

📦 5. Observabilidad
 OpenTelemetry configurado en todos los servicios (NestJS lo soporta bien).

 Jaeger Collector como deployment en K8s.

 Exponer Jaeger UI en puerto accesible (http://localhost:16686).

 Configurar traceId en todos los logs.

 Posible integración futura con Grafana + Loki para logging centralizado.

⚖️ 6. Escalabilidad
 Usar Horizontal Pod Autoscaler (HPA) para:

Servicio antifraude

Servicio updatedb

 Basado en CPU o latencia (prometheus metrics opcional).

 Ensayar cargas con k6 o Artillery (1K o 1M transacciones).

🧪 7. Pruebas y Validación
 Script de carga para simular múltiples transacciones.

 Validación que estados cambian de pendiente → denegado|aprobado.

 Validación de consistencia entre PostgreSQL y Redis.

🗺️ 8. Diagramas y Documentación
 Diagrama de arquitectura general.

 Diagrama de trazabilidad de OpenTelemetry + Jaeger.

 Incluir README por carpeta con instrucciones.

🧩 Opcional/Futuro
 Helm Charts para despliegue.

 Istio o Linkerd para tracing avanzado.

 Dashboard en Grafana con métricas de Redis/Kafka/CPU.

