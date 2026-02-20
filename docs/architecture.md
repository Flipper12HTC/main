# Architecture Interne — Flipper 12

## Les blocs

### Capteurs
- **Boutons L/R** : le joueur appuie pour activer les raquettes, signal GPIO envoyé à l'ESP32

### ESP32
Reçoit les signaux bruts des capteurs, filtre les rebonds et transmet les événements au Game Engine via MQTT.

### Game Engine (Node.js)
Le cerveau du système, tourne en local sur la borne.
- **Game Loop** : tourne à 60 FPS, tick toutes les 16ms
- **Cannon.js** : gère toute la physique virtuelle — rebonds, collisions, détection des points quand la balle touche un bumper ou une rampe
- **Score Manager** : calcule les points, applique les multiplicateurs, gère les high scores
- **API REST** : reçoit l'authentification wallet via QR scan
- **WebSocket** : diffuse l'état du jeu aux 3 écrans en temps réel

### UI (Three.js)
3 écrans mis à jour via WebSocket :
- **Écran principal** : rendu 3D du jeu (60Hz)
- **Écran score** : score, billes restantes (1Hz)
- **Écran déco** : ambiance, animations (sur événement)

### Stockage
- **PostgreSQL** : scores, sessions, sauvegardes en local
- **Redis** : cache live des scores pendant la partie
- **Smart Contract Solana** : enregistrement on-chain des scores, gestion des cashprizes (si wallet connecté)
- **Leaderboard** : classement global mis à jour on-chain après chaque partie classée

### Mobile
- **Wallet Phantom** : le joueur scanne le QR code sur la borne pour s'authentifier et signer ses transactions

---

## 3 Décisions techniques

### 1. MQTT — communication ESP32 → Game Engine
MQTT est le protocole standard pour l'IoT. L'ESP32 a une bibliothèque native, la latence est inférieure à 5ms en réseau local, et il est asynchrone — l'ESP32 envoie l'événement et continue immédiatement sans attendre de réponse. C'est indispensable pour respecter la contrainte de 16ms sur les boutons.

### 2. Cannon.js — moteur physique
Cannon.js est fait pour aller avec Three.js. Ils partagent le même thread JavaScript donc pas de conversion de données entre les deux. Toute la physique (rebonds, collisions, points) est gérée virtuellement par Cannon.js. Les capteurs physiques ne servent qu'aux événements réels comme le tilt ou la bille perdue.

### 3. Solana — blockchain
- Confirmation en moins d'1 seconde, le joueur voit son score enregistré immédiatement après la partie
- Le framework Anchor permet d'écrire les smart contracts avec validation automatique, ce qui réduit les risques sur la gestion des cashprizes

