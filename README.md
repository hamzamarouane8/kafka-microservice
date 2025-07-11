# 🚀 Microservices Project — Kafka, Spring Boot, Eureka, Gateway, PostgreSQL

Ce projet est un exemple de système **microservices** basé sur :
- **Spring Boot** pour le back-end
- **Apache Kafka** pour la communication asynchrone
- **Eureka Discovery Service** pour l’enregistrement dynamique
- **Spring Cloud Gateway** pour la gestion centralisée des routes
- **PostgreSQL** pour la persistance
- **Docker & Docker Compose** pour Kafka, Zookeeper et les bases de données

---

## ✅ Pré-requis

- Java 17+
- Maven
- Docker & Docker Compose
- IDE : IntelliJ / VS Code

---

## 📌 Structure du projet

/discovery-service
/gateway-service
/order-service
/payment-service
/docker-compose.yml

---

## 🐳 Docker : Lancer Kafka, Zookeeper & PostgreSQL

### 1️⃣ Démarrer tous les containers

```bash
docker-compose up -d

2️⃣ Vérifier les containers actifs
docker ps

✔️ Attendu :
	•	Kafka ➜ localhost:9092
	•	Zookeeper ➜ localhost:2181
	•	PostgreSQL ➜ localhost:5432 (ex: ordersdb et paymentsdb)

3️⃣ Accéder à Kafka en CLI
# Lister les topics existants
docker exec -it <kafka-container-id> kafka-topics.sh --list --bootstrap-server localhost:9092

# Créer un topic manuellement (exemple)
docker exec -it <kafka-container-id> kafka-topics.sh --create --topic order-topic --bootstrap-server localhost:9092 --partitions 1 --replication-factor 1

4️⃣ Se connecter à PostgreSQL
# Accès au container Postgres
docker exec -it <postgres-container-id> psql -U postgres -d ordersdb

# Lister les tables
\dt

# Voir les données
SELECT * FROM orders;

# Quitter psql
\q

5️⃣ Logs et arrêt des containers
docker logs <container-id>

docker-compose down

⚙️ Exemple de configuration (application.properties)
# Example for Order Service

server.port=8081
spring.application.name=ORDER-SERVICE

# Eureka
eureka.client.service-url.defaultZone=http://localhost:8761/eureka/

# Kafka
spring.kafka.bootstrap-servers=localhost:9092
spring.kafka.consumer.group-id=group_order
spring.kafka.producer.key-serializer=org.apache.kafka.common.serialization.StringSerializer
spring.kafka.producer.value-serializer=org.apache.kafka.common.serialization.StringSerializer
spring.kafka.consumer.key-deserializer=org.apache.kafka.common.serialization.StringDeserializer
spring.kafka.consumer.value-deserializer=org.apache.kafka.common.serialization.StringDeserializer

# PostgreSQL
spring.datasource.url=jdbc:postgresql://localhost:5432/ordersdb
spring.datasource.username=postgres
spring.datasource.password=postgres
spring.jpa.hibernate.ddl-auto=update
spring.jpa.show-sql=true

✅ Démarrage des microservices
Ordre conseillé :
1️⃣ Discovery Service ➜ 2️⃣ Gateway ➜ 3️⃣ Order ➜ 4️⃣ Payment
# Discovery Service
cd discovery-service
mvn clean install
mvn spring-boot:run

# Gateway Service
cd gateway-service
mvn clean install
mvn spring-boot:run

# Order Service
cd order-service
mvn clean install
mvn spring-boot:run

# Payment Service
cd payment-service
mvn clean install
mvn spring-boot:run

✅ Vérifier l’enregistrement sur Eureka
➡️ Accéder au dashboard Eureka :
http://localhost:8761/

✨ Auteur

Développé par MAROUANE HAMZA – Senior Full Stack Developer
Stack : Java 17, Spring Boot, React.js, Kafka, PostgreSQL