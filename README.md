# 📘 Étude de cas — Clients Synchrones (RestTemplate vs Feign vs WebClient)  
### Avec Eureka & Consul | Spring Boot Microservices

---

## � Prérequis

Avant de commencer, assurez-vous d'avoir installé :

- **Java 17** ou supérieur
- **Maven 3.6+** pour la gestion des dépendances
- **Consul** (par défaut) ou **Eureka** pour la découverte de services
  - Consul : Téléchargez depuis [consul.io](https://www.consul.io/downloads)
  - Eureka : Inclus dans le projet (service de découverte intégré)

---

## 🛠️ Technologies Utilisées

- **Spring Boot 3.2.0** : Framework principal
- **Spring Cloud 2023.0.0** : Pour la découverte de services et les clients HTTP
- **OpenFeign** : Client déclaratif HTTP
- **WebClient** : Client réactif (utilisé en mode synchrone)
- **RestTemplate** : Client HTTP traditionnel
- **Consul/Eureka** : Découverte de services
- **Spring Boot Actuator** : Métriques et monitoring
- **Micrometer + Prometheus** : Collecte de métriques

---

## 🚀 Installation et Configuration

### 1. Cloner le projet
```bash
git clone https://github.com/houssamb4/resttemplate-feign-webclient-comparison.git
cd resttemplate-feign-webclient-comparison
```

### 2. Construire le projet
```bash
mvn clean install
```

### 3. Configuration de la découverte de services

#### Option A : Utiliser Consul (Recommandé - Par défaut)
1. Démarrer Consul en mode développement :
```bash
consul agent -dev
```
Consul sera accessible sur `http://localhost:8500`

#### Option B : Utiliser Eureka
1. Modifier les fichiers `pom.xml` dans `service-voiture` et `service-client` :
   - Décommenter la dépendance Eureka
   - Commenter la dépendance Consul
2. Modifier les fichiers `application.yml` :
   - Décommenter la section Eureka
   - Commenter la section Consul

---

## ▶️ Démarrage des Services

### Ordre de démarrage recommandé :

1. **Démarrer le service de découverte**
   - Avec Eureka : `mvn spring-boot:run -pl discovery-service`
   - Avec Consul : Consul doit déjà être démarré

2. **Démarrer le Service Voiture**
```bash
mvn spring-boot:run -pl service-voiture
```

3. **Démarrer le Service Client**
```bash
mvn spring-boot:run -pl service-client
```

### Vérification
- Eureka Dashboard : `http://localhost:8761`
- Consul Dashboard : `http://localhost:8500`
- Service Client : `http://localhost:8081`
- Service Voiture : `http://localhost:8082`

---

## 📡 API Endpoints

### Service Voiture
- `GET /api/cars/byClient/{clientId}` : Récupère une voiture par ID client

### Service Client
- `GET /api/clients/{id}/car/rest` : Utilise RestTemplate
- `GET /api/clients/{id}/car/feign` : Utilise Feign Client
- `GET /api/clients/{id}/car/webclient` : Utilise WebClient

### Métriques (Actuator)
- `GET /actuator/health` : État du service
- `GET /actuator/metrics` : Métriques disponibles
- `GET /actuator/prometheus` : Métriques Prometheus

---

## 🧪 Tests Fonctionnels

### Test simple
```bash
# Tester RestTemplate
curl http://localhost:8081/api/clients/1/car/rest

# Tester Feign
curl http://localhost:8081/api/clients/1/car/feign

# Tester WebClient
curl http://localhost:8081/api/clients/1/car/webclient
```

### Réponse attendue
```json
{
  "id": 1,
  "marque": "Toyota",
  "modele": "Yaris",
  "clientId": 1
}
```

---

## 📌 Description du projet

Ce projet est une étude comparative des trois principaux clients HTTP synchrones utilisés dans l’écosystème Spring :

- **RestTemplate**
- **Feign Client**
- **WebClient (mode synchrone)**

L’objectif est d’analyser leurs performances, leur facilité d’intégration et leur comportement en situation de panne, dans une architecture **microservices Spring Boot** avec **découverte de services** via **Eureka** et **Consul**.

---

## 🎯 Objectifs pédagogiques

À la fin du projet, il sera possible de :

- Implémenter deux microservices communiquant de manière synchrone.  
- Configurer la découverte de services avec **Eureka** et **Consul**.  
- Implémenter 3 types de clients HTTP dans le **Service Client**.  
- Réaliser des tests de performance (latence / throughput).  
- Collecter et analyser des métriques (Prometheus + Grafana optionnel).  
- Tester la résilience face à :
  - la panne du Service Voiture  
  - la panne du service Discovery (Eureka/Consul)  
  - des temps de réponse élevés  

---

## 🏗️ Architecture Cible

### Services

| Service | Rôle | Port |
|--------|------|------|
| **Service Voiture** | Expose une API REST listant les voitures | 8082 |
| **Service Client** | Consomme l’API Voiture via RestTemplate, Feign et WebClient | 8081 |
| **Eureka Server** | Découverte de services (option 1) | 8761 |
| **Consul Agent** | Découverte de services (option 2) | 8500 |



# Guide de Tests de Performance - TP24

## 📊 Tests JMeter

### Configuration JMeter

1. **Créer un Thread Group**
   - Nombre de threads : 10, 50, 100, 200, 500
   - Ramp-Up Period : 10 secondes
   - Loop Count : 10

2. **Ajouter HTTP Request Sampler**

**RestTemplate:**
```
Server: localhost
Port: 8081
Path: /api/clients/1/car/rest
Method: GET
```

**Feign:**
```
Server: localhost
Port: 8081
Path: /api/clients/1/car/feign
Method: GET
```

**WebClient:**
```
Server: localhost
Port: 8081
Path: /api/clients/1/car/webclient
Method GET
```

3. **Ajouter Listeners**
   - Summary Report
   - Aggregate Report
   - View Results Tree

---

## 📋 Tableaux de Résultats

### Tableau 1: Performance avec Eureka

| Méthode | Charge (threads) | Temps Moyen (ms) | P95 (ms) | Débit (req/s) | Erreurs (%) |
|---------|------------------|------------------|----------|---------------|-------------|
| **RestTemplate** | 10 | | | | |
| | 50 | | | | |
| | 100 | | | | |
| | 200 | | | | |
| | 500 | | | | |
| **Feign** | 10 | | | | |
| | 50 | | | | |
| | 100 | | | | |
| | 200 | | | | |
| | 500 | | | | |
| **WebClient** | 10 | | | | |
| | 50 | | | | |
| | 100 | | | | |
| | 200 | | | | |
| | 500 | | | | |

### Tableau 2: Performance avec Consul

| Méthode | Charge (threads) | Temps Moyen (ms) | P95 (ms) | Débit (req/s) | Erreurs (%) |
|---------|------------------|------------------|----------|---------------|-------------|
| **RestTemplate** | 100 | | | | |
| **Feign** | 100 | | | | |
| **WebClient** | 100 | | | | |

### Tableau 3: Consommation Ressources

| Méthode | CPU (%) | RAM (MB) | Threads actifs |
|---------|---------|----------|----------------|
| **RestTemplate** | | | |
| **Feign** | | | |
| **WebClient** | | | |

### Tableau 4: Tests de Résilience

| Scénario | RestTemplate | Feign | WebClient |
|----------|--------------|-------|-----------|
| **Panne Service Voiture** | | | |
| - Taux d'échec (%) | | | |
| - Temps de reprise (s) | | | |
| **Panne Discovery** | | | |
| - Comportement | | | |
| - Cache actif? | | | |
| **Redémarrage Service** | | | |
| - Temps re-registration (s) | | | |

---

## 🎯 Métriques à Collecter

### Avec JMeter
- ✅ Temps de réponse moyen
- ✅ Temps de réponse médian
- ✅ P90, P95, P99
- ✅ Débit (Throughput)
- ✅ Taux d'erreur
- ✅ Min/Max response time

### Avec Task Manager / htop
- ✅ CPU % du processus Java
- ✅ Mémoire utilisée (MB)
- ✅ Nombre de threads

### Avec Actuator
- ✅ `/actuator/metrics/jvm.memory.used`
- ✅ `/actuator/metrics/jvm.threads.live`
- ✅ `/actuator/health`

---

## 🔥 Scénarios de Tests

### Test 1: Charge Progressive
```
10 threads → 50 → 100 → 200 → 500
```

### Test 2: Panne Service Voiture
```
1. Démarrer test 100 threads
2. À 30s: arrêter Service Voiture
3. À 45s: redémarrer Service Voiture
4. Observer récupération
```

### Test 3: Panne Discovery
```
1. Services enregistrés
2. Arrêter Consul/Eureka
3. Tester appels (cache local?)
4. Redémarrer Discovery
```

---

## 📝 Template Analyse

### Section 1: Méthodologie
- Environnement de test (machine, OS, Java version)
- Configuration des services
- Charge appliquée
- Outils utilisés

### Section 2: Résultats Performance
- Présenter les tableaux
- Graphiques (optionnel)
- Observations

### Section 3: Consommation Ressources
- CPU et RAM par méthode
- Impact sur les performances

### Section 4: Résilience
- Comportement lors des pannes
- Temps de récupération
- Recommandations

### Section 5: Conclusion
- Meilleure méthode selon critère (latence/simplicité/résilience)
- Recommandations pour production
- Limites de l'étude

---

**Bon courage pour vos tests!** 🚀
