# Gestionnaire de Notes – Spring Boot & React

Application full-stack permettant à un utilisateur de s'inscrire, se connecter et gérer ses notes personnelles.  
Le backend est développé avec Spring Boot (Java 17) et communique avec une base MySQL.  
Le frontend est réalisé en React (Vite) avec TailwindCSS pour le style.

## Table des matières
- [Aperçu des fonctionnalités](#aperçu-des-fonctionnalités)
- [Architecture](#architecture)
- [Prérequis](#prérequis)
- [Configuration de la base de données](#configuration-de-la-base-de-données)
- [Lancer le backend Spring Boot](#lancer-le-backend-spring-boot)
- [Lancer le frontend React](#lancer-le-frontend-react)
- [API REST](#api-rest)
- [Personnalisation](#personnalisation)
- [Dépannage](#dépannage)
- [Pistes d'amélioration](#pistes-damélioration)

## Aperçu des fonctionnalités
- **Inscription & connexion** avec hachage des mots de passe (BCrypt).
- **Gestion des notes** : création, modification, suppression et consultation par utilisateur.
- **Tableau de bord** React avec navigation latérale, modal d'édition/suppression et feed-back utilisateur.
- **Profil** : affichage (et mise à jour côté backend) des informations de l'utilisateur connecté.
- **Persistant** : données stockées dans MySQL avec JPA/Hibernate (`ddl-auto=update`).

## Architecture
```
note/
├─ backend/        # Projet Spring Boot (API REST + accès MySQL)
│  └─ src/main/java/first/note/...
└─ frontend/       # Application React + Vite + TailwindCSS
   └─ src/pages/...  (Login, Dash, Notes, Profile, Sidebar)
```

La communication entre les deux services se fait via HTTP (`http://localhost:8080` pour l'API et `http://localhost:5173` pour le client).

## Prérequis
- Java **17** et Maven 3.9+
- Node.js **20+** (recommandé) et npm
- MySQL **8+**
- (Optionnel) Postman/Insomnia pour tester l'API

## Configuration de la base de données
1. Créer une base `notes_app` :
   ```sql
   CREATE DATABASE notes_app CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
   ```
2. Mettre à jour les identifiants dans `backend/src/main/resources/application.properties` :
   ```
   spring.datasource.url=jdbc:mysql://localhost:3306/notes_app
   spring.datasource.username=<votre_user>
   spring.datasource.password=<votre_mot_de_passe>
   ```
3. Laisser `spring.jpa.hibernate.ddl-auto=update` pour générer/mettre à jour les tables automatiquement en développement.

> ⚠️ Pour la production, préférez des migrations contrôlées (Flyway/Liquibase) et désactivez les logs SQL.

## Lancer le backend Spring Boot
```bash
cd backend
mvn spring-boot:run
```
Le backend écoute sur `http://localhost:8080`.

### Variables utiles
- `SPRING_DATASOURCE_URL`, `SPRING_DATASOURCE_USERNAME`, `SPRING_DATASOURCE_PASSWORD` peuvent être surchargées via des variables d'environnement.
- Ajouter `spring.jpa.show-sql=false` pour réduire le bruit en production.

## Lancer le frontend React
```bash
cd frontend
npm install
npm run dev
```
Le client tourne sur `http://localhost:5173`.  
Assurez-vous que le backend soit démarré pour éviter les erreurs réseau.

## API REST

| Méthode | Endpoint                       | Description                              | Corps attendu / Query |
|--------:|--------------------------------|------------------------------------------|-----------------------|
| POST    | `/api/auth/register`          | Inscription d'un nouvel utilisateur      | `{ "userName", "email", "password" }` |
| POST    | `/api/auth/login`             | Connexion, renvoie l'utilisateur ou null | `{ "email", "password" }` |
| PUT     | `/api/auth/{userId}`          | Mise à jour du profil utilisateur        | `{ "name?", "email?", "password?" }` |
| GET     | `/api/notes/{userId}`         | Liste des notes pour l'utilisateur       | —                     |
| POST    | `/api/notes`                  | Création d'une note                      | `{ "titre", "description", "userId" }` |
| PUT     | `/api/notes/{noteId}`         | Modification d'une note                  | `{ "titre", "description" }` |
| DELETE  | `/api/notes/{noteId}`         | Suppression d'une note                   | —                     |

Les contrôleurs autorisent les requêtes CORS depuis `http://localhost:5173`.

## Personnalisation
- **Style** : TailwindCSS est configuré (`tailwind.config.js`, `App.css`).  
  Adapter les couleurs/espacements dans `frontend/src/pages/*.jsx`.
- **Validation** : ajouter des contrôles côté backend (Bean Validation) et côté frontend (messages d'erreur plus précis).
- **Sécurité** : pour une app réelle, mettre en place Spring Security (JWT/session) au lieu de renvoyer l'utilisateur directement.
- **Profil** : le composant `Profile.jsx` contient un TODO pour la gestion fine du changement de mot de passe.

## Dépannage
- **`Access denied for user`** : vérifier vos identifiants MySQL et les droits sur la base.
- **`CORS` bloqué** : s'assurer d'accéder au frontend via `http://localhost:5173` ou mettre à jour les origines autorisées dans les contrôleurs.
- **`Cannot GET /dash` en rechargeant la page** : configurer un fallback côté serveur (ex. middleware Vite ou configuration proxy) car l'app utilise React Router.
- **Problème de ports** : si `8080` ou `5173` sont occupés, changer les configurations correspondantes (`server.port` côté backend, `vite.config.js` côté frontend).

## Pistes d'amélioration
- Authentification avec JWT et rôles.
- Tests unitaires et d'intégration (Spring Boot / React Testing Library).
- Gestion des erreurs centralisée côté backend (`@ControllerAdvice`).
- Ajout de tags/recherche pour les notes.
- Internationalisation (i18n) et meilleure accessibilité UI.

---

