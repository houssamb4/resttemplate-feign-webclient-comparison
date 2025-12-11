# 📘 Étude de cas — Clients Synchrones (RestTemplate vs Feign vs WebClient)  
### Avec Eureka & Consul | Spring Boot Microservices

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

| Service | Rôle |
|--------|------|
| **Service Voiture** | Expose une API REST listant les voitures |
| **Service Client** | Consomme l’API Voiture via RestTemplate, Feign et WebClient |
| **Eureka Server** | Découverte de services (option 1) |
| **Consul Agent** | Découverte de services (option 2) |


