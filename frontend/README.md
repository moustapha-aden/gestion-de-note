# Frontend React – Gestionnaire de Notes

Ce dossier contient l'application cliente développée avec **React 19**, **Vite 7** et **TailwindCSS**.  
Elle communique avec l'API Spring Boot (port `8080`) pour l'authentification et la gestion des notes.

## Installation
```bash
npm install
```

## Lancement en développement
```bash
npm run dev
```
L'application est disponible sur `http://localhost:5173`.  
Veillez à démarrer le backend pour que les appels API fonctionnent.

## Build de production
```bash
npm run build
```
Le dossier `dist/` contient les fichiers prêts à être servis.

## Structure principale
- `src/pages/Login.jsx` : inscription / connexion (stockage local de l'utilisateur).
- `src/pages/Dash.jsx` : tableau de bord avec chargement des notes.
- `src/pages/Notes.jsx` : affichage, ajout, édition et suppression des notes (modales Tailwind).
- `src/pages/Profile.jsx` : gestion du profil utilisateur (PUT `/api/auth/{id}`).
- `src/pages/Sidebar.jsx` : navigation latérale.

## Personnalisation
- Modifier les couleurs et polices (classes Tailwind) dans les fichiers `*.jsx`.
- Mettre à jour les URLs d'API si le backend est déployé ailleurs qu'en local.

> 📘 Pour une vue d'ensemble du projet (backend + frontend), consultez le `README.md` à la racine du dépôt.
