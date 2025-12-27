# 🚀 Arquitectura Event-Driven y Mensajería

Proyectos avanzados que demuestran patrones de comunicación asíncrona, message brokers y arquitectura de microservicios orientada a eventos usando Apache Kafka y RabbitMQ.

---

## 🆕 Proyectos Recientes (Diciembre 2025)

### **[poc-05-rabbitmq](https://github.com/Cortadai/poc-05-rabbitmq)** ⭐
Introducción práctica a RabbitMQ con Spring Boot. Cubre los conceptos esenciales de mensajería: tipos de exchanges (direct, topic, fanout, headers), bindings, queues, Dead Letter Queues (DLQ) y patrones de retry. Incluye productor/consumidor funcionales y documentación didáctica.

- **Tecnología:** Spring Boot 3.x, RabbitMQ, Docker
- **Nivel:** Principiante
- **Conceptos Clave:** Exchanges, Bindings, DLQ, Retry patterns
- **Perfecto Para:** Aprender fundamentos de mensajería desde cero

Productor → Exchange → Queue → Consumer
               ↓
          DLQ (errores) → Retry

---

### **[rabbitmq-msvcs-example](https://github.com/Cortadai/rabbitmq-msvcs-example)** ⭐
Arquitectura event-driven con RabbitMQ y Spring Boot. Implementa comunicación asíncrona entre microservicios, gestión de solicitudes con estados, patrones de escalado horizontal y procesamiento distribuido. Demuestra cómo desacoplar servicios mediante mensajería.

- **Tecnología:** Spring Boot 3.x, RabbitMQ, Docker Compose
- **Nivel:** Intermedio
- **Conceptos Clave:** Event-driven, escalado horizontal, gestión de estados
- **Perfecto Para:** Entender microservicios desacoplados con mensajería

┌─────────────┐      ┌──────────────┐     ┌─────────────────┐
│   API REST  │────▶│   RabbitMQ   │────▶│  Microservicios │
│ (Solicitudes)│     │  (Exchange)  │     │  (Consumidores) │
└─────────────┘      └──────────────┘     └─────────────────┘
                          │
                   Escalado horizontal
                   Procesamiento distribuido

---

## 📚 Resumen de Proyectos

### Proyectos Fundamentales (Aprender los Básicos)

Estos proyectos introducen los conceptos fundamentales de arquitectura orientada a eventos y message brokers.

#### 1. **[kafka-tutorial](https://github.com/Cortadai/kafka-tutorial)**
   Introducción a Apache Kafka con Spring Boot
   - **Tecnología:** Spring Boot 3.1.8, Apache Kafka, Java
   - **Enfoque:** Patrón Producer-Consumer, serialización de mensajes
   - **Conceptos Clave:**
     - Topics, particiones y consumer groups de Kafka
     - Publicación de mensajes de texto simples
     - Publicación/consumo de objetos JSON (User DTOs)
     - Integración Spring Kafka (@KafkaListener)
   - **Endpoints REST:**
     - `GET /api/v1/kafka/publish?message=<texto>` - Publicar mensajes de texto
     - `POST /api/v1/kafka/publish/user` - Publicar mensajes JSON
   - **Resultado de Aprendizaje:** Entender los básicos de Kafka e integración con Spring Kafka
   - **Perfecto Para:** Principiantes comenzando con arquitectura orientada a eventos
   
   **Flujo de Ejemplo:**
   ```
   Cliente → App Spring Boot → Topic Kafka → Consumer Listener → Salida en Log
   ```

#### 2. **[rabbitmq-tutorial](https://github.com/Cortadai/rabbitmq-tutorial)**
   Introducción a RabbitMQ con Spring Boot
   - **Tecnología:** Spring Boot 3.x, RabbitMQ (AMQP), Java
   - **Enfoque:** Patrones de message broker, Topic Exchange, enrutamiento
   - **Conceptos Clave:**
     - Exchanges de RabbitMQ (Topic Exchange)
     - Bindings de colas y routing keys
     - Patrón Producer-Consumer con @RabbitListener
     - Serialización de mensajes (texto y JSON)
   - **Endpoints REST:**
     - Endpoints de mensajería de texto
     - Endpoints de mensajería de objetos JSON
   - **Resultado de Aprendizaje:** Dominar conceptos de message broker RabbitMQ
   - **Perfecto Para:** Desarrolladores aprendiendo AMQP y colas de mensajes
   
   **Patrón de Arquitectura:**
   ```
   Productor → Exchange → Cola (con routing key) → Consumer Listener
   ```

#### 3. **[kafka-server-local](https://github.com/Cortadai/kafka-server-local)**
   Instalación completa de Apache Kafka 3.6.1 para desarrollo local
   - **Tecnología:** Apache Kafka, ZooKeeper, Kafka Connect
   - **Contenidos:**
     - Configuración completa de broker Kafka
     - Configuración de ZooKeeper
     - Kafka Connect para integraciones
     - Scripts de administración y herramientas (directorio /bin)
     - Archivos de configuración (directorio /config)
     - Todas las librerías necesarias (directorio /libs)
   - **Propósito:** Entorno Kafka listo para ejecutar
   - **Caso de Uso:** Desarrollo local, testing, experimentación
   - **Incluye:** Cluster de nodo único, scripts producer/consumer, herramientas de monitorización
   - **Resultado de Aprendizaje:** Entender infraestructura y despliegue de Kafka
   - **Perfecto Para:** Ingenieros DevOps, configuración de infraestructura

---

### Proyectos Intermedios (Microservicios con Eventos)

Estos proyectos muestran cómo construir microservicios que se comunican asíncronamente a través de eventos.

#### 4. **[springboot-microservices-kafka](https://github.com/Cortadai/springboot-microservices-kafka)**
   Arquitectura de microservicios usando Apache Kafka para comunicación asíncrona
   - **Tecnología:** Spring Boot 3.x, Apache Kafka, Java 17, Docker
   - **Arquitectura:** Microservicios Event-Driven
   - **Servicios:**
     1. **Order Service** (Microservicio 1)
        - Expone API REST: `POST /api/v1/orders`
        - Recibe peticiones de pedidos
        - Genera UUID para seguimiento
        - Crea `OrderEventDto` con estado "PENDING"
        - Publica evento al topic Kafka "orders"
     
     2. **Email Service** (Consumidor)
        - Escucha el topic "orders"
        - Envía notificaciones por email a clientes
        - Desacoplado del Order Service
     
     3. **Stock Service** (Consumidor)
        - Escucha el topic "orders"
        - Actualiza inventario/stock
        - Persiste en base de datos
        - Escalado y despliegue independientes
   
   - **Módulo Compartido:** base-domain (DTOs: OrderDto, OrderEventDto)
   
   - **Conceptos Clave:**
     - Publicación asíncrona de eventos
     - Múltiples consumidores independientes
     - Estandarización de payload de eventos
     - Microservicios desacoplados
     - Particionado de topics Kafka
   
   - **Flujo de Comunicación:**
   ```
   Cliente REST → Order Service → Topic Kafka (orders) → Email Service & Stock Service
                                                           ↓                ↓
                                                      Enviar Email    Actualizar Stock
   ```
   
   - **Beneficios:**
     - ✅ Servicios completamente desacoplados
     - ✅ Cada servicio escala independientemente
     - ✅ Resiliente a fallos de servicios
     - ✅ Soporta agregar nuevos consumidores sin cambiar Order Service
   
   - **Resultado de Aprendizaje:** Construir microservicios event-driven con Kafka
   - **Perfecto Para:** Entender comunicación asíncrona entre servicios

#### 5. **[springboot-microservices-rabbitmq](https://github.com/Cortadai/springboot-microservices-rabbitmq)**
   Arquitectura de microservicios usando RabbitMQ para comunicación asíncrona
   - **Tecnología:** Spring Boot 3.x, RabbitMQ (AMQP), Java 17, Docker
   - **Arquitectura:** Microservicios Event-Driven (Alternativa a Kafka)
   - **Servicios:**
     1. **Order Service** (Puerto 8080)
        - API REST: `POST /api/v1/orders`
        - Publica eventos de pedidos a RabbitMQ TopicExchange
        - Genera eventos con estado
     
     2. **Stock Service** (Puerto 8081)
        - Consume eventos de pedidos
        - Actualiza inventario
        - Persiste cambios
     
     3. **Email Service** (Puerto 8082)
        - Consume eventos de pedidos
        - Envía emails de confirmación
   
   - **Componentes RabbitMQ:**
     - TopicExchange para enrutamiento flexible
     - Múltiples colas vinculadas al exchange
     - Routing keys para distribución de mensajes
   
   - **Conceptos Clave:**
     - Diferencias RabbitMQ vs Kafka
     - Patrones TopicExchange
     - Estrategias de binding de colas
     - Integración Spring AMQP
   
   - **Flujo de Comunicación:**
   ```
   Cliente REST → Order Service → TopicExchange → Stock & Email Services
                                      ↓
                             Distribuido vía Routing Keys
   ```
   
   - **Cuándo Usar RabbitMQ vs Kafka:**
     - RabbitMQ: Sistemas pequeños, enrutamiento flexible, necesidades tradicionales de cola de mensajes
     - Kafka: Streaming de alto volumen, event sourcing, sistemas basados en logs
   
   - **Resultado de Aprendizaje:** Comparar enfoques Kafka y RabbitMQ
   - **Perfecto Para:** Entender arquitecturas alternativas de event broker

---

### Proyectos Avanzados (Escenarios del Mundo Real)

Ejemplos de grado de producción demostrando patrones event-driven complejos.

#### 6. **[springboot-kafka-real-world-project](https://github.com/Cortadai/springboot-kafka-real-world-project)**
   Pipeline de streaming de eventos del mundo real: Wikimedia → Kafka → Base de Datos
   - **Tecnología:** Spring Boot, Apache Kafka, Server-Sent Events (SSE)
   - **Arquitectura:** Pipeline de streaming completo
   - **Componentes:**
     1. **Módulo Kafka Producer** (kafka-producer-wikimedia)
        - Se conecta a Wikimedia EventStreams (cambios en vivo de Wikipedia)
        - Usa protocolo Server-Sent Events (SSE)
        - Publica eventos continuamente a topic Kafka
        - Eventos: creaciones de páginas, ediciones, eliminaciones, acciones de usuarios
     
     2. **Módulo Kafka Consumer** (kafka-consumer-database)
        - Escucha topic Kafka
        - Recibe eventos de cambios de Wikipedia en tiempo real
        - Persiste en base de datos usando Spring Data JPA
        - Almacena historial completo de eventos
   
   - **Fuente de Datos del Mundo Real:**
     - API Wikimedia EventStreams (pública, sin autenticación requerida)
     - Eventos en vivo desde Wikipedia en tiempo real
     - Miles de eventos por minuto
   
   - **Ejemplos de Eventos:**
     ```
     - Página "Machine Learning" editada por usuario "Alice"
     - Nueva página "AI Safety" creada por usuario "Bob"
     - Página "Python Programming" revertida por admin
     ```
   
   - **Pipeline de Datos:**
   ```
   Wikimedia → Stream SSE → Kafka Producer → Topic Kafka → Kafka Consumer → Base de Datos
               (Externo)      (Ingesta)       (Buffer)       (Ingesta)      (Almacenamiento)
   ```
   
   - **Conceptos Clave:**
     - Ingesta de eventos en tiempo real
     - Pipeline producer-consumer
     - Manejo de eventos de alto volumen
     - Persistencia de eventos
     - Fundamentos de procesamiento de streams
   
   - **Desafíos Resueltos:**
     - ✅ Buffering con Kafka
     - ✅ Manejo de backpressure
     - ✅ Persistencia de eventos
     - ✅ Manejo de errores
     - ✅ Ingesta escalable
   
   - **Resultado de Aprendizaje:** Construir sistemas de streaming de eventos de grado de producción
   - **Perfecto Para:** Entender pipelines de eventos del mundo real

---

## 🏗️ Patrones de Arquitectura

### Patrón 1: Producer-Consumer Simple (Tutoriales)
```
┌─────────────────────────────────────────┐
│           Productor                     │
│  (Publica mensajes al topic)            │
├─────────────────────────────────────────┤
│  Endpoint REST recibe petición         │
│  → Crea mensaje                        │
│  → Publica a Kafka/RabbitMQ            │
│  → Retorna inmediatamente              │
└──────────────┬──────────────────────────┘
               │
               ▼
┌──────────────────────────┐
│   Topic Kafka/RabbitMQ   │
│   (Buffer Distribuido)   │
└──────────────┬───────────┘
               │
     ┌─────────┴──────────┐
     ▼                    ▼
┌──────────────┐    ┌──────────────┐
│ Consumidor 1 │    │ Consumidor 2 │
│ (Listener)   │    │ (Listener)   │
│              │    │              │
│ Log Mensaje  │    │ Guardar en BD│
└──────────────┘    └──────────────┘
```

### Patrón 2: Microservicios Event-Driven
```
┌────────────────────────────────────────┐
│      Petición del Cliente              │
│   POST /api/v1/orders                  │
└────────────────┬───────────────────────┘
                 │
                 ▼
        ┌──────────────────┐
        │  Order Service   │
        │  (Productor)     │
        └────────┬─────────┘
                 │
      Publica: OrderCreatedEvent
                 │
                 ▼
        ┌──────────────────┐
        │  Message Broker  │
        │  (Kafka/AMQP)    │
        └────────┬─────────┘
                 │
      ┌──────────┼──────────┐
      │          │          │
      ▼          ▼          ▼
┌──────────┐ ┌──────────┐ ┌────────────┐
│  Email   │ │  Stock   │ │ Analytics  │
│ Service  │ │ Service  │ │ Service    │
│(Consumer)│ │(Consumer)│ │(Consumer)  │
└──────────┘ └──────────┘ └────────────┘
    │            │             │
    └────┬───────┴─────────────┘
         │
    Todos reaccionan independientemente al mismo evento
    Sin acoplamiento directo
```

### Patrón 3: Pipeline de Streaming del Mundo Real
```
┌──────────────────────┐
│   Fuente Externa     │
│  (API Wikimedia)     │
│  - Alto volumen      │
│  - Stream continuo   │
└──────────┬───────────┘
           │
           ▼
┌──────────────────────────┐
│   App Kafka Producer    │
│  - Conexión SSE         │
│  - Parsing de eventos   │
│  - Publicación Kafka    │
└──────────┬──────────────┘
           │
    Eventos de alto volumen
           │
           ▼
┌──────────────────────────┐
│   Cluster Kafka         │
│  - Topics particionados │
│  - Buffer distribuido   │
│  - Tolerancia a fallos  │
└──────────┬──────────────┘
           │
     Múltiples particiones
           │
           ▼
┌──────────────────────────┐
│   App Kafka Consumer    │
│  - Deserialización      │
│  - Transformación datos │
│  - Persistencia BD      │
└──────────┬──────────────┘
           │
           ▼
┌──────────────────────────┐
│   Base de Datos         │
│  - Archivo de eventos   │
│  - Datos históricos     │
│  - Fuente analytics     │
└──────────────────────────┘
```

---

## 🛠️ Stack Tecnológico

### Message Brokers
- **Apache Kafka** - Plataforma de streaming distribuida
  - Alto rendimiento (millones de eventos/seg)
  - Almacenamiento persistente basado en logs
  - Soporte de consumer groups
  - Particionado para escalabilidad
  - Mejor para: Event sourcing, streaming tiempo real, logs de auditoría
  
- **RabbitMQ** - Message broker con AMQP
  - Enrutamiento flexible (Exchanges)
  - Mensajería basada en colas
  - Garantías de confirmación
  - Mejor para escenarios de enrutamiento complejos
  - Mejor para: Colas tradicionales, microservicios, distribución de trabajos

### Ecosistema Spring
- **Spring Boot** - Framework de aplicación
- **Spring Kafka** - Integración con Kafka
- **Spring AMQP** - Integración con RabbitMQ
- **Spring Data JPA** - Capa de persistencia

### Infraestructura
- **Docker & Docker Compose** - Contenerización y orquestación
- **Java 17+** - Lenguaje de programación
- **Maven** - Herramienta de construcción
- **ZooKeeper** - Coordinación de Kafka (incluido con Kafka)

---

## 📊 Matriz de Comparación

| Característica | Kafka Tutorial | RabbitMQ Tutorial | Kafka Microservices | RabbitMQ Microservices | Real-World Pipeline |
|----------------|---|---|---|---|---|
| **Broker** | Kafka | RabbitMQ | Kafka | RabbitMQ | Kafka |
| **Patrón** | Producer-Consumer | Producer-Consumer | Servicios Event-Driven | Servicios Event-Driven | Pipeline Streaming |
| **Servicios** | 1 app | 1 app | 3+ microservicios | 3 microservicios | 2 apps |
| **Complejidad** | Principiante | Principiante | Intermedio | Intermedio | Avanzado |
| **Fuente de Datos** | Manual | Manual | Interno (pedidos) | Interno (pedidos) | Externo (Wikimedia) |
| **Escala** | Pequeña | Pequeña | Media | Media | Grande (alto volumen) |
| **Caso de Uso** | Aprendizaje | Aprendizaje | Listo para producción | Listo para producción | Producción real |
| **Tiempo Aprendizaje** | 1-2 días | 1-2 días | 1 semana | 1 semana | 2+ semanas |

---

## 🚀 Guías de Inicio Rápido

### Empezando con Kafka
```bash
# 1. Iniciar Kafka localmente usando configuración kafka-server-local
cd kafka-server-local
# Seguir instrucciones de configuración

# 2. Ejecutar Kafka Tutorial
git clone https://github.com/Cortadai/kafka-tutorial.git
cd kafka-tutorial
mvn clean install
mvn spring-boot:run

# 3. Publicar un mensaje
curl "http://localhost:8080/api/v1/kafka/publish?message=Hola%20Kafka"

# 4. Comprobar consola para consumo de mensaje
```

### Empezando con RabbitMQ
```bash
# 1. Iniciar RabbitMQ (Docker)
docker run -d --name rabbitmq \
  -p 5672:5672 \
  -p 15672:15672 \
  rabbitmq:3-management

# 2. Acceder a RabbitMQ Management UI
# Abrir navegador: http://localhost:15672
# Usuario: guest
# Contraseña: guest

# 3. Ejecutar RabbitMQ Tutorial
git clone https://github.com/Cortadai/rabbitmq-tutorial.git
cd rabbitmq-tutorial
mvn clean install
mvn spring-boot:run

# 4. Publicar mensaje vía REST
curl -X POST http://localhost:8080/api/v1/rabbitmq/publish \
  -H "Content-Type: application/json" \
  -d '{"id":1,"firstName":"John","lastName":"Doe"}'
```

### Ejecutando Microservicios con Docker Compose
```bash
# Para Microservicios Kafka
git clone https://github.com/Cortadai/springboot-microservices-kafka.git
cd springboot-microservices-kafka
docker-compose up -d

# Servicios disponibles en:
# - Order Service: http://localhost:8080
# - Kafka: localhost:9092
```

---

## 🎯 Ruta de Aprendizaje

### Semana 1: Fundamentos

#### Día 1-2: Básicos de Message Broker
- ¿Qué son los message brokers?
- Comunicación Síncrona vs Asíncrona
- Básicos de arquitectura event-driven
- Comparación Kafka vs RabbitMQ

#### Día 3-4: Inmersión Profunda en Apache Kafka
- Topics, particiones y réplicas
- Productores y consumidores
- Consumer groups
- Integración Spring Kafka
- **Práctica:** Ejecutar kafka-tutorial

#### Día 5: Conceptos RabbitMQ
- Protocolo AMQP
- Exchanges (Topic, Fanout, Direct)
- Bindings de colas
- Integración Spring AMQP
- **Práctica:** Ejecutar rabbitmq-tutorial

### Semana 2: Microservicios

#### Día 6-7: Microservicios Event-Driven
- Desacoplamiento de servicios
- Patrones de publicación de eventos
- Múltiples consumidores
- Manejo de fallos
- **Práctica:** Ejecutar kafka-microservices

#### Día 8-9: Implementaciones Alternativas
- Comparación microservicios RabbitMQ
- Cuándo usar cada broker
- Análisis de trade-offs
- **Práctica:** Ejecutar rabbitmq-microservices

#### Día 10: Patrones del Mundo Real
- Pipelines de streaming
- Event sourcing
- Patrones CQRS
- **Práctica:** Revisar real-world-project

### Semana 3: Temas Avanzados

#### Día 11-12: Consideraciones de Producción
- Estrategias de escalado
- Monitorización y observabilidad
- Manejo de errores y reintentos
- Consistencia de datos
- Recuperación ante desastres

#### Día 13-14: Integración y Operaciones
- Configuraciones multi-broker
- Kafka Streams
- Kafka Connect
- Monitorización con Prometheus/Grafana

#### Día 15: Proyecto Final
- Diseñar e implementar tu propio sistema event-driven
- Elegir entre Kafka y RabbitMQ
- Implementar múltiples consumidores
- Manejar fallos gracefully

---

## 🔗 Conceptos Clave Explicados

### Evento (Mensaje)
```java
// Un dato que representa algo que sucedió
public class OrderCreatedEvent {
    private UUID orderId;           // Identificador único
    private LocalDateTime timestamp; // Cuándo sucedió
    private String status;          // Estado actual
    private OrderDto order;         // Datos asociados
}
```

### Topic (Kafka) / Exchange (RabbitMQ)
- **Kafka Topic:** Canal con nombre donde se publican eventos
- **RabbitMQ Exchange:** Enrutador que distribuye mensajes a colas
- Propósito: Desacoplar productores de consumidores

### Consumer Group (Específico de Kafka)
- Múltiples consumidores pueden suscribirse al mismo topic
- Cada consumer group recibe todos los mensajes
- Los mensajes se particionan entre miembros del grupo
- Permite escalado y tolerancia a fallos

### Routing Key (Específico de RabbitMQ)
- Metadata usada por exchanges para enrutar mensajes
- TopicExchange soporta patrones wildcard
- Ejemplo: `order.*.confirmed` coincide con `order.payment.confirmed`

### Partition (Específico de Kafka)
- Los topics se dividen en particiones
- Cada partición se ordena independientemente
- Diferentes consumidores pueden leer diferentes particiones
- Permite escalado horizontal

---

## 📈 Cuándo Usar Cada Broker

### Usa Kafka Cuando:
- ✅ Streaming de eventos de alto volumen (millones/seg)
- ✅ Necesitas reproducir eventos (log inmutable)
- ✅ Construyendo sistemas de event sourcing
- ✅ Implementando CQRS
- ✅ Se requiere retención de datos a largo plazo
- ✅ Necesitas procesamiento complejo de streams
- ✅ Construyendo pipelines de datos

### Usa RabbitMQ Cuando:
- ✅ Se necesita lógica de enrutamiento compleja
- ✅ Patrones tradicionales de cola de mensajes
- ✅ Volumen moderado de mensajes
- ✅ Configuración y operaciones más simples
- ✅ Distribución de tareas (patrón job queue)
- ✅ Necesitas mecanismos de reintento integrados
- ✅ Tipos de exchange flexibles (Topic, Direct, Fanout)

---

## 🧪 Testing de Sistemas Event-Driven

### Testing Unitario de Productores
```java
@Test
void testOrderPublishing() {
    // Arrange
    Order order = new Order(...);
    
    // Act
    orderService.createOrder(order);
    
    // Assert
    verify(kafkaTemplate).send(eq("orders-topic"), any(OrderEvent.class));
}
```

### Testing de Integración de Consumidores
```java
@SpringBootTest
@EmbeddedKafka(partitions = 1, brokerProperties = {
    "listeners=PLAINTEXT://localhost:9092",
    "port=9092"
})
class OrderConsumerTest {
    
    @Test
    void testOrderEventProcessing() {
        // Enviar evento a Kafka embebido
        // Verificar que consumer lo procesó
        // Asegurar que estado de BD cambió
    }
}
```

---

## 💡 Mejores Prácticas

### 1. Diseño de Eventos
- ✅ Incluir ID de evento y timestamp
- ✅ Usar versionado semántico para eventos
- ✅ Hacer eventos inmutables
- ✅ Incluir todo el contexto necesario

### 2. Fiabilidad del Consumidor
- ✅ Implementar idempotencia
- ✅ Manejar reentrega gracefully
- ✅ Usar DLQ (Dead Letter Queue) para fallos
- ✅ Registrar todos los eventos importantes

### 3. Escalabilidad
- ✅ Usar particiones para escalar consumo
- ✅ Monitorizar consumer lag
- ✅ Implementar manejo de backpressure
- ✅ Usar consumer groups para procesamiento paralelo

### 4. Monitorización
- ✅ Rastrear throughput de mensajes
- ✅ Monitorizar consumer lag
- ✅ Alertar sobre fallos
- ✅ Registrar todos los eventos para pista de auditoría

### 5. Manejo de Errores
- ✅ Implementar lógica de reintento
- ✅ Usar Dead Letter Queues
- ✅ Circuit breakers para fallos
- ✅ Degradación graceful

---

## 📚 Colecciones Relacionadas
- [Spring Boot Basics](https://github.com/Cortadai/spring-boot-basics) - Fundamentos de API REST
- [Microservices Architecture](https://github.com/Cortadai/microservices-architecture) - Configuración completa de microservicios
- [Spring Security Course](https://github.com/Cortadai/spring-security-course) - Asegurando sistemas de eventos

---

## 🏷️ Topics Aplicados

Todos los proyectos etiquetados con:
- `#event-driven` - Arquitectura event-driven
- `#mensajeria` - Message brokers
- `#microservicios` - Patrones de microservicios
- `#aprendizaje` - Material educativo
- `#tutorial` - Estilo tutorial

Topics específicos por proyecto:
- Proyectos Kafka: `#kafka`, `#streaming`
- Proyectos RabbitMQ: `#rabbitmq`, `#amqp`
- Microservicios: `#spring-boot`, `#sistemas-distribuidos`
- Mundo real: `#produccion`, `#proyecto-real`

---

## 📊 Estadísticas del Proyecto

| Métrica | Valor |
|---------|-------|
| **Total Proyectos** | 6 |
| **Message Brokers** | 2 (Kafka, RabbitMQ) |
| **Microservicios** | 6 (en todos los proyectos) |
| **Nivel de Aprendizaje** | Principiante a Avanzado |
| **Tiempo Estimado de Aprendizaje** | 3-4 semanas |

---

## 🎓 Resultados de Aprendizaje

Después de completar esta colección, comprenderás:
- ✅ Arquitectura asíncrona event-driven
- ✅ Conceptos y uso de Apache Kafka
- ✅ Message brokering con RabbitMQ
- ✅ Patrones producer-consumer
- ✅ Construcción de microservicios event-driven
- ✅ Streaming de eventos en tiempo real
- ✅ Manejo de eventos de alto volumen
- ✅ Persistencia de eventos y pistas de auditoría
- ✅ Consumer groups y escalado
- ✅ Manejo de errores en sistemas async
- ✅ Comparación de arquitecturas de brokers
- ✅ Patrones de despliegue en producción

---

## 📬 Recursos Adicionales

### Documentación Oficial
- [Documentación Apache Kafka](https://kafka.apache.org/documentation/)
- [Documentación RabbitMQ](https://www.rabbitmq.com/documentation.html)
- [Documentación Spring Kafka](https://spring.io/projects/spring-kafka)
- [Documentación Spring AMQP](https://spring.io/projects/spring-amqp)

### Tutoriales y Guías
- [Kafka Quick Start](https://kafka.apache.org/quickstart)
- [Tutoriales RabbitMQ](https://www.rabbitmq.com/getstarted.html)
- [Patrones Event-Driven Architecture](https://www.eventdrivenarchitecture.io/)

### Herramientas y Utilidades
- [Kafdrop](https://github.com/obsidiandynamics/kafdrop) - UI de Kafka
- [RabbitMQ Management UI](https://www.rabbitmq.com/management.html)
- [Kafka Cat (kcat)](https://github.com/edenhill/kcat) - Herramienta de línea de comandos

---

## 🎯 Próximos Pasos

1. **Empezar con tutoriales:** kafka-tutorial y rabbitmq-tutorial
2. **Entender básicos:** Message brokers, productores, consumidores
3. **Pasar a microservicios:** Ver cómo múltiples servicios se comunican
4. **Explorar mundo real:** Aprender de kafka-real-world-project
5. **Construir el tuyo propio:** Crear una aplicación event-driven
6. **Optimizar:** Aprender escalado, monitorización y operaciones

---

*Última actualización: Diciembre 2025*
*Hub: Arquitectura Event-Driven y Mensajería v1.0*
