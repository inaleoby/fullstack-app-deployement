# Fullstack Smartphone Manager

Une application **3 tiers** (Frontend + Backend + MongoDB) pour gérer des smartphones :  
- Ajouter un smartphone  
- Voir la liste des smartphones  
- Modifier / Supprimer un smartphone  

Le projet utilise **React.js** pour le frontend, **Express.js** pour le backend, et **MongoDB** pour la base de données.  
Le tout est conteneurisé avec **Docker Compose**, prêt pour CI/CD et déploiement sur AWS.

---

## 📁 Structure du projet

docker/
├── docker-compose.yml # Compose pour Mongo, Backend, Frontend
├── Backend/
│ ├── Dockerfile
│ ├── package.json
│ └── src/ # Code backend (Express + API)
└── Front/
├── Dockerfile
├── package.json
└── src/ # Code frontend (React.js)

---

## ⚙️ Prérequis

- [Docker](https://www.docker.com/get-started) installé
- [Docker Compose](https://docs.docker.com/compose/install/)
- (Optionnel) [Node.js](https://nodejs.org/) et npm pour dev local

---

## 🚀 Installation et lancement

1. **Cloner le projet**  

```bash
git clone git@github.com:inaleoby/fullstack-app-deployement.git
cd fullstack-app-deployement/docker
```


2. **Définir les variables d’environnement**

Créer un fichier Backend/.env avec le contenu suivant :

```bash
PORT=5000
MONGO_URI=mongodb://mongo:27017/smartphoneDB
DELETE_CODE=123
```
3. **Lancer les services Docker Compose**

```bash
docker compose up -d --build
```
4. **Accéder à l’application**

Frontend : http://localhost:5050
Backend API : http://localhost:5000/api
MongoDB : mongodb://localhost:27017

📝 Commandes utiles

Arrêter les containers :

```bash
docker compose down
```
Voir les logs :
```bash
docker compose logs -f
```
Rebuild uniquement un service :

```bash
docker compose build backend
```
