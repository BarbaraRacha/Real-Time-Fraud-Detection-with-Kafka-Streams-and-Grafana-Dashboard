# Kafka Streams Fraud Detection Application

## Description
Ce projet implémente une application Kafka Streams permettant de détecter les transactions financières suspectes en temps réel. Les transactions suspectes sont celles dont le montant dépasse un seuil de 10 000. Les résultats sont publiés dans un topic Kafka et enregistrés dans une base de données InfluxDB pour être visualisés en temps réel via un tableau de bord Grafana.

## Table des matières
- [Architecture Globale](#architecture-globale)
- [Règles de Détection de Fraudes](#règles-de-détection-de-fraudes)
- [Prérequis](#prérequis)
- [Installation](#installation)
- [Configuration de Grafana et InfluxDB](#configuration-de-grafana-et-influxdb)
- [Exécution du Projet](#exécution-du-projet)
- [Validation et Résultats](#validation-et-résultats)
- [Captures d’Écran](#captures-d’écran)

## Architecture Globale
![Architecture Kafka Streams](path/to/architecture-diagram.png)

L'application se compose des éléments suivants :
- **Kafka Streams** : Pour traiter les transactions en temps réel.
- **Base de données InfluxDB** : Pour stocker les transactions suspectes.
- **Grafana** : Pour visualiser les données.
- **Docker Compose** : Pour orchestrer les services.

## Règles de Détection de Fraudes
Les transactions suspectes sont détectées selon la règle suivante :
- Montant > 10 000

## Prérequis
- Docker et Docker Compose
- Java 17 ou supérieur
- Maven
- Kafka (via Docker Compose)

## Installation
1. Clonez le dépôt :
   ```bash
   git clone https://github.com/votre-repo/kafka-streams-fraud-detection.git
   cd kafka-streams-fraud-detection
   ```

2. Construisez l'application Java :
   ```bash
   mvn clean package
   ```

3. Lancez les services avec Docker Compose :
   ```bash
   docker-compose up -d
   ```

## Configuration de Grafana et InfluxDB
### Connexion de Grafana à InfluxDB
1. Accédez à l’interface Grafana via `http://localhost:3000`.
2. Ajoutez une nouvelle source de données :
   - Type : InfluxDB
   - URL : `http://influxdb:8086`
   - Base de données : `fraud_alerts`
3. Enregistrez la configuration.

### Création des Tableaux de Bord
1. Créez un nouveau tableau de bord Grafana.
2. Ajoutez les panels suivants :
   - **Transactions suspectes par utilisateur** : Une métrique regroupée par `userId`.
   - **Montant total des transactions suspectes** : Une métrique basée sur la somme des montants sur une période donnée.

## Exécution du Projet
1. Démarrez Kafka et les services associés :
   ```bash
   docker-compose up -d
   ```
2. Publiez des transactions JSON dans le topic `transactions-input`.
3. Vérifiez les résultats dans les logs ou sur le tableau de bord Grafana.

## Validation et Résultats
1. Utilisez le jeu de données d’exemple pour simuler des transactions :
   ```json
   {
     "userId": "12345",
     "amount": 15000,
     "timestamp": "2024-12-04T15:00:00Z"
   }
   ```
2. Vérifiez que les transactions suspectes sont publiées dans le topic `fraud-alerts`.
3. Validez l’affichage des données sur le tableau de bord Grafana.

## Captures d’Écran
### Classes et Résultats des Sorties
#### TransactionProducer
![TransactionProducer Output](path/to/screenshot-transaction-producer.png)

#### TransactionProcessor
![TransactionProcessor Output](path/to/screenshot-transaction-processor.png)

#### FraudAlertConsumer
![FraudAlertConsumer Output](path/to/screenshot-fraud-alert-consumer.png)

#### Transaction
![Transaction Class Output](path/to/screenshot-transaction-class.png)

### Dashboards
#### Dashboard InfluxDB
![InfluxDB Dashboard](path/to/screenshot-influxdb-dashboard.png)

#### Dashboard Grafana
![Grafana Dashboard](path/to/screenshot-grafana-dashboard.png)

## Schéma Docker Compose
Incluez le fichier `docker-compose.yml` pour démarrer Kafka, InfluxDB et Grafana.

```yaml
version: '3.8'
services:
  zookeeper:
    image: confluentinc/cp-zookeeper
    environment:
      ZOOKEEPER_CLIENT_PORT: 2181
  kafka:
    image: confluentinc/cp-kafka
    ports:
      - "9092:9092"
    environment:
      KAFKA_ZOOKEEPER_CONNECT: zookeeper:2181
      KAFKA_ADVERTISED_LISTENERS: PLAINTEXT://kafka:9092
  influxdb:
    image: influxdb:latest
    ports:
      - "8086:8086"
  grafana:
    image: grafana/grafana:latest
    ports:
      - "3000:3000"
```

## Conclusion
Cette application démontre une intégration complète des technologies Kafka Streams, InfluxDB et Grafana pour détecter et visualiser les transactions financières suspectes en temps réel.
