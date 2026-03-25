## Cartographie du projet

# 5 blocs 

- Gameplay :
  - Moteur fonctionnel, raquettes réactives, système de score en temps réel, gestion des billes, affichage du score final.

- UI / Front (ecran) :
  - Menu, animation, QR code, leaderboard.

- Local storage :
  - Stockage des sauvegardes de parties locales (solo), top 10 parties locales.

- Blockchain :
  - Connexion wallet via QR code, création/validation transactions, écriture on-chain (scores, tournois).
  
- Tournament :  
  - inscriptions, classement, redistribution (si cashprize), logique du tournoi (local ou on chain)

# Structure 

/docs
  /uml
     architecture.puml
     class.puml
     sequence.puml
  architecture.md
  cdc-technique.md
  mvp.md
  roadmap.md
  backlog.md
  PROJECT_MAP.md

/src
  /gameplay
  /ui
  /storage
  /blockchain
  /tournaments
  /config

/tests

/contracts
  /programs
  README.md
