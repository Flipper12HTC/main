# CAHIER DES CHARGES TECHNIQUE
---

##  Vision 

Flipper 12 est un projet dans une démarche de révolutionner le jeu de flipper, en le rendant compétitif et crypto-friendly. Il permet aux utilisateurs de jouer au flipper, organiser des compétitions entre eux et aussi participer à un tournoi global.

---

## Problématique

Les jeux d'arcade modernes sont peu compétitifs, rarement connectés à la blockchain, et ne permettent pas une redistribution automatique et transparente des gains. Le flipper reste un jeu nostalgique mais non intégré aux nouvelles pratiques Play2Earn et Web3.

Flipper 12 vise à combiner arcade physique et compétition crypto-native avec transparence, automatisation et anonymat.

---

## Objectifs 

- Simulation d'un flipper complet avec une physique le plus proche du réel
- Possibilité de jouer en compétitif - Soit en tant qu'invité, soit avec un wallet en scannant un QR-code.
- Avoir une interface permettant de customiser les couleurs/fonds du flipper et de créer des tournois avec cashprize ou non.
- Permettre à l'utilisateur de participer à des tournois avec cashprize
- Redistribution des prix d'entry dans le cashprize automatique 
- Permettre à l'utilisateur de créer des tournois rapides - Local, pas de en ligne
- Simulation du comportement de la blockchain Solana dans un format Play2Earn

## Non-Objectif

- Pas de possibilité de customisé son pseudo - Qui dit crypto dis anonymat, les joueurs seront identifiés par leur adresse de wallet.
- Pas de bot
- Impossibilité aux joueurs de voir l'entiereté du wallet, format XXX...XXX

--- 

## Personas

Julien, 55 ans, retraité -  Viens découvrir le jeu dans un bar crypto, s'intéresse au monde de la crypto et retrouve un jeu de sa jeunesse qui correspond à des nouvelles normes.

Idir, 25 ans, Degen - Viens jouer toutes les semaines dans le tournoi hebdomadaire et créer des tournois avec sa communauté twitter à travers toutes les bornes disposés dans les évenemnts crypto/bars à thème.

--- 

## Use Cases

### Tableau récapitulatif des Use Cases

#### A. Use Cases - Gameplay & Système

| ID | Nom | Acteurs | But | Priorité |
|---|---|---|---|---|
| UC-G01 | Allumer le flipper | Joueur | Démarrer le système et accéder au menu principal | Haute |
| UC-G02 | Configurer une partie | Joueur | Définir les paramètres de jeu (mode, joueurs, billes) | Haute |
| UC-G03 | Charger une partie sauvegardée | Joueur, Système | Reprendre une partie Solo sauvegardée | Moyenne |
| UC-G04 | Jouer une partie | Joueur | Jouer une partie complète selon les règles | Haute |
| UC-G05 | Contrôler les raquettes | Joueur | Contrôler les raquettes pour renvoyer la bille | Haute |
| UC-G06 | Détecter les impacts de balle | Système | Détecter et identifier les impacts pour ajuster le score | Haute |
| UC-G07 | Gérer les scores | Système | Calculer et afficher le score en temps réel | Haute |
| UC-G08 | Gérer les pertes de billes | Système | Décrémenter les billes et déterminer la continuation | Haute |
| UC-G09 | Passer au joueur suivant | Joueur, Système | Gérer la transition entre joueurs (Multi) | Haute |
| UC-G10 | Gérer la fin de partie | Système | Déterminer le gagnant et afficher les résultats | Haute |
| UC-G11 | Sauvegarder une partie | Joueur, Système | Sauvegarder l'état de jeu (Solo uniquement) | Moyenne |
| UC-G12 | Enregistrer les meilleurs scores | Système | Enregistrer et afficher les high scores locaux | Moyenne |

#### B. Use Cases - Blockchain & Compétition

| ID | Nom | Acteurs | But | Priorité |
|---|---|---|---|---|
| UC-B01 | Jouer en mode invité | Utilisateur | Jouer sans wallet (score local uniquement) | Haute |
| UC-B02 | Connecter son wallet | Utilisateur | S'authentifier via QR code Solana | Haute |
| UC-B03 | Jouer une partie classée | Utilisateur | Jouer avec enregistrement on-chain | Haute |
| UC-B04 | Participer à un tournoi | Utilisateur | S'inscrire et jouer dans un tournoi | Haute |
| UC-B05 | Créer un tournoi local | Organisateur | Créer un tournoi sans cashprize | Moyenne |
| UC-B06 | Créer un tournoi avec cashprize | Organisateur | Créer un tournoi avec smart contract | Haute |
| UC-B07 | Consulter le leaderboard | Utilisateur | Voir le classement global on-chain | Moyenne |
| UC-B08 | Customiser le flipper | Utilisateur | Personnaliser l'apparence du jeu | Basse |
| UC-B09 | Rejoindre un tournoi via QR | Utilisateur | S'inscrire à un tournoi par QR code | Haute |
| UC-B10 | Recevoir les gains | Utilisateur | Recevoir automatiquement les gains | Haute |

---

### Descriptions détaillées des Use Cases

#### A. Use Cases - Gameplay & Système

---

**UC-G01 / Allumer le flipper**

**Acteurs :** Joueur

**But :** Permettre au joueur de démarrer le système et accéder au menu principal pour choisir un mode de jeu

**Préconditions :** 
- Borne alimentée électriquement
- Système en veille ou éteint

**Déclencheur :** Le joueur appuie sur le bouton power ou approche de la borne

**Scénario nominal :**
1. Le système effectue un auto-test (vérification des raquettes, capteurs, affichage)
2. L'écran d'accueil s'affiche avec le logo Flipper 12
3. Le menu principal s'affiche avec les options : Solo, Multijoueur, Classements, Tournois, Paramètres
4. Le système est prêt à recevoir une sélection du joueur

**Extensions / Erreurs :**
- 1a. Erreur hardware détectée → Affichage message d'erreur technique, arrêt du processus
- 3a. Écran tactile défectueux → Utilisation des boutons physiques pour navigation

**Postconditions :** 
- Flipper opérationnel
- Menu principal affiché et interactif

**Notes / Règles métier :**
- L'auto-test ne doit pas dépasser 5 secondes
- En cas d'erreur critique, un mode maintenance doit être accessible

---

**UC-G02 / Configurer une partie**

**Acteurs :** Joueur

**But :** Permettre au joueur de définir les paramètres de jeu (mode Solo/Multi, nombre de joueurs, nombre de billes)

**Préconditions :** 
- Flipper allumé (UC-G01 complété)
- Menu principal affiché

**Déclencheur :** Le joueur sélectionne "Solo" ou "Multijoueur" dans le menu principal

**Scénario nominal :**
1. Le joueur choisit le mode : Solo ou Multijoueur
2. **Si Solo :** Le système propose de charger une partie sauvegardée ou démarrer une nouvelle partie
3. **Si Multijoueur :** Le système demande le nombre de joueurs (2 à 4)
4. **Si Multijoueur :** Le système demande de définir l'ordre de passage des joueurs
5. Le système propose de choisir le nombre de billes (3 ou 5 par défaut)
6. Le joueur valide la configuration
7. Le système initialise la partie avec les paramètres choisis

**Extensions / Erreurs :**
- 6a. Le joueur annule → Retour au menu principal
- 7a. Erreur d'initialisation → Message d'erreur, retour à l'étape 1

**Postconditions :** 
- Partie configurée et initialisée
- Paramètres sauvegardés pour la session en cours
- Système prêt à lancer la première bille

**Notes / Règles métier :**
- Maximum 4 joueurs en mode Multijoueur
- Par défaut : 3 billes en mode standard, 5 billes en mode tournoi
- L'ordre de passage ne peut pas être modifié une fois la partie lancée

---

**UC-G03 / Charger une partie sauvegardée**

**Acteurs :** Joueur, Système

**But :** Permettre au joueur de reprendre une partie Solo précédemment sauvegardée à l'état exact où elle a été interrompue

**Préconditions :** 
- Mode Solo sélectionné
- Au moins une sauvegarde existante dans la mémoire locale
- Fichier de sauvegarde non corrompu

**Déclencheur :** Le joueur sélectionne "Continuer" ou "Charger une partie" dans le menu Solo

**Scénario nominal :**
1. Le système récupère toutes les sauvegardes disponibles
2. Le système affiche la liste des parties sauvegardées avec : date/heure, score actuel, billes restantes
3. Le joueur sélectionne une sauvegarde
4. Le système charge l'état de jeu complet (score, billes, position de la bille, bonus actifs, état des cibles)
5. Le système affiche un écran de confirmation "Partie chargée"
6. La partie reprend immédiatement

**Extensions / Erreurs :**
- 1a. Aucune sauvegarde disponible → Message "Aucune partie sauvegardée", retour au menu
- 4a. Sauvegarde corrompue → Message d'erreur, proposition de supprimer la sauvegarde
- 3a. Le joueur annule → Retour au menu Solo

**Postconditions :** 
- Partie reprise exactement à l'état sauvegardé
- État de jeu chargé en mémoire
- Joueur peut continuer à jouer

**Notes / Règles métier :**
- Seul le mode Solo permet la sauvegarde/chargement
- Maximum 5 sauvegardes simultanées par borne
- Les sauvegardes de plus de 30 jours sont automatiquement supprimées

---

**UC-G04 / Jouer une partie**

**Acteurs :** Joueur

**But :** Permettre au joueur de jouer une partie de flipper complète en suivant les règles du jeu

**Préconditions :** 
- Configuration validée (UC-G02) ou partie chargée (UC-G03)
- Système prêt à lancer la bille

**Déclencheur :** Le joueur appuie sur le bouton "Start" ou la configuration est terminée

**Scénario nominal :**
1. Le système lance la première bille
2. Le système affiche les informations : score actuel, billes restantes, multiplicateurs, objectifs
3. Le joueur contrôle les raquettes (UC-G05)
4. Le système détecte les impacts de balle (UC-G06)
5. Le système gère les scores (UC-G07)
6. Le système active les bonus/modes spéciaux selon les cibles touchées
7. Les étapes 3 à 6 se répètent jusqu'à la perte de la bille
8. Le système gère la perte de bille (UC-G08)
9. Si billes restantes > 0 : retour à l'étape 1
10. Si billes restantes = 0 : déclenchement de UC-G10 (fin de partie)

**Extensions / Erreurs :**
- 6a. Tilt détecté (secousses excessives) → Bille perdue immédiatement, pénalité -500 points
- 3a. Panne bouton raquette → Alerte maintenance, partie suspendue
- Pause possible à tout moment (mode Solo uniquement) → UC-G11

**Postconditions :** 
- Partie en cours ou terminée
- Score enregistré
- Statistiques mises à jour

**Notes / Règles métier :**
- Temps de réponse des raquettes < 16ms obligatoire
- Le tilt est activé après 3 secousses rapides détectées
- Les bonus sont cumulables mais limités à x10 maximum

---

**UC-G05 / Contrôler les raquettes**

**Acteurs :** Joueur

**But :** Permettre au joueur de contrôler les raquettes du flipper pour renvoyer la bille

**Préconditions :** 
- Partie en cours (UC-G04)
- Bille active sur le plateau

**Déclencheur :** Le joueur appuie sur les boutons physiques gauche ou droit

**Scénario nominal :**
1. Le joueur appuie sur le bouton de la raquette (gauche ou droite)
2. Le système détecte instantanément l'appui (< 16ms)
3. La raquette correspondante se lève
4. Le moteur physique calcule l'interaction avec la bille
5. La bille est renvoyée selon l'angle et la force
6. Le joueur relâche le bouton
7. La raquette redescend

**Extensions / Erreurs :**
- 2a. Bouton défectueux détecté → Log erreur hardware, notification maintenance
- 4a. La bille passe entre les raquettes → Déclenchement de UC-G08 (perte de bille)
- Double appui simultané possible pour effet spécial

**Postconditions :** 
- Bille renvoyée vers le plateau (ou perdue)
- État des raquettes normal (position basse)

**Notes / Règles métier :**
- La latence maximale tolérée est de 16ms (60 FPS)
- Les raquettes ont une force de frappe paramétrable selon le niveau de difficulté
- Un appui maintenu garde la raquette levée (max 2 secondes pour éviter abus)

---

**UC-G06 / Détecter les impacts de balle**

**Acteurs :** Système

**But :** Détecter et identifier tous les impacts de la bille sur les différentes cibles du plateau pour ajuster le score

**Préconditions :** 
- Partie en cours (UC-G04)
- Bille en mouvement sur le plateau

**Déclencheur :** La bille touche une cible, bumper, rampe ou zone spéciale

**Scénario nominal :**
1. Un capteur détecte l'impact sur une zone du plateau
2. Le système identifie le type de cible touchée (bumper, rampe, cible, trou, etc.)
3. Le système calcule les points associés selon le barème :
   - Bumpers : +100 points
   - Rampes : +500 points
   - Cibles spéciales : +1000 points + activation bonus
   - Trous/orbites : Mission spéciale activée
4. Le système envoie les points à UC-G07 pour mise à jour du score
5. Le système déclenche les effets visuels et sonores correspondants
6. Si cible spéciale : activation de modes bonus (multiball, jackpot, etc.)

**Extensions / Erreurs :**
- 1a. Capteur défaillant → Log erreur, utilisation du système de vision par caméra en backup
- 3a. Impact simultané sur plusieurs cibles → Cumul des points de toutes les cibles
- 6a. Mode bonus déjà actif → Extension de durée du bonus (+10 secondes)

**Postconditions :** 
- Points calculés et transmis
- Bonus éventuellement activé
- Cibles marquées comme touchées (pour combos)

**Notes / Règles métier :**
- Chaque cible a un temps de cooldown de 100ms pour éviter les doubles détections
- Les combos augmentent les points : 3 cibles consécutives = x2, 5 cibles = x3
- Certaines cibles débloquent des missions à compléter pour jackpot

---

**UC-G07 / Gérer les scores**

**Acteurs :** Système

**But :** Calculer, mettre à jour et afficher le score du joueur en temps réel

**Préconditions :** 
- Partie en cours (UC-G04)
- Points générés par UC-G06

**Déclencheur :** Réception de points depuis UC-G06 (détection d'impact)

**Scénario nominal :**
1. Le système reçoit les points de base de l'impact
2. Le système vérifie les multiplicateurs actifs (x2, x3, etc.)
3. Le système calcule les points finaux : points_base × multiplicateur
4. Le système ajoute les points au score total du joueur actuel
5. Le système met à jour l'affichage du score en temps réel
6. Le système vérifie si le score dépasse le high score local actuel
7. Si nouveau high score : affichage temporaire "NEW HIGH SCORE!"
8. Si mode Multijoueur : mise à jour du classement intermédiaire

**Extensions / Erreurs :**
- 6a. Score > high score local → Marquage pour UC-G12
- 3a. Overflow du score (> 999,999,999) → Plafonnement à la valeur max
- 7a. Mode tournoi actif → Envoi du score on-chain (UC-B03)

**Postconditions :** 
- Score total mis à jour
- Affichage actualisé
- High score potentiellement battu

**Notes / Règles métier :**
- Le score est affiché avec séparateurs de milliers (ex: 1,234,500)
- Les multiplicateurs sont cumulables mais plafonnés à x10
- Le score est sauvegardé toutes les 10 secondes en cas de coupure

---

**UC-G08 / Gérer les pertes de billes**

**Acteurs :** Système

**But :** Détecter la perte d'une bille, décrémenter le compteur et déterminer si la partie continue ou se termine

**Préconditions :** 
- Partie en cours (UC-G04)
- Bille active sur le plateau

**Déclencheur :** La bille passe entre les raquettes ou sort du plateau de jeu

**Scénario nominal :**
1. Le capteur de sortie détecte la perte de bille
2. Le système décrémente le compteur de billes restantes (-1)
3. Le système affiche "BALL LOST" avec le score du tour
4. Le système enregistre les statistiques du tour (durée, points)
5. Le système vérifie le nombre de billes restantes
6. **Si billes restantes > 0 :**
   - Pause de 3 secondes
   - Relance d'une nouvelle bille (retour à UC-G04, étape 1)
7. **Si billes restantes = 0 :**
   - Si mode Solo : Déclenchement de UC-G10 (fin de partie)
   - Si mode Multi : Déclenchement de UC-G09 (joueur suivant)

**Extensions / Erreurs :**
- 1a. Bille bloquée détectée (pas de mouvement pendant 30s) → Éjection automatique, pas de pénalité
- 6a. Mode "Extra Ball" actif → Pas de décrémentation, bille bonus accordée
- 3a. Le joueur a réalisé un objectif spécial avant la perte → Affichage du bonus gagné

**Postconditions :** 
- Compteur de billes mis à jour
- Nouvelle bille lancée ou fin de tour
- Statistiques enregistrées

**Notes / Règles métier :**
- Les "Extra Balls" sont limitées à 2 par partie maximum
- Le compteur de billes est affiché en permanence à l'écran
- En cas de bille bloquée, un mécanisme physique d'éjection s'active après 30 secondes

---

**UC-G09 / Passer au joueur suivant (Multijoueur)**

**Acteurs :** Joueur, Système

**But :** Gérer la transition entre joueurs en mode Multijoueur et activer le tour du joueur suivant

**Préconditions :** 
- Mode Multijoueur actif
- Le joueur actuel a perdu toutes ses billes (UC-G08 terminé)
- Au moins un autre joueur n'a pas encore terminé son tour

**Déclencheur :** Fin du tour du joueur actuel (toutes billes perdues)

**Scénario nominal :**
1. Le système sauvegarde le score final du joueur actuel
2. Le système affiche l'écran des scores intermédiaires avec classement provisoire
3. Le système affiche "AU TOUR DE [NOM JOUEUR X]" pendant 5 secondes
4. Le système active le profil du joueur suivant dans l'ordre défini
5. Le système initialise le compteur de billes pour ce joueur
6. Le système lance la première bille du nouveau joueur
7. Le nouveau joueur commence sa partie (UC-G04)

**Extensions / Erreurs :**
- 4a. Tous les joueurs ont terminé → Déclenchement de UC-G10 (fin de partie)
- 3a. Timeout sans interaction (30s) → Passage automatique au joueur suivant
- Le joueur peut appuyer sur "Start" pour sauter l'écran de transition

**Postconditions :** 
- Tour du joueur suivant activé
- Compteur de billes réinitialisé pour ce joueur
- Partie continue

**Notes / Règles métier :**
- L'ordre de passage ne peut pas être modifié en cours de partie
- Chaque joueur joue avec le même nombre de billes initial
- Les scores sont affichés en permanence pour tous les joueurs

---

**UC-G10 / Gérer la fin de partie**

**Acteurs :** Système

**But :** Détecter la fin de partie, déterminer le(s) gagnant(s) et afficher les résultats finaux

**Préconditions :** 
- Tous les joueurs ont perdu toutes leurs billes
- OU le joueur Solo a perdu toutes ses billes

**Déclencheur :** Dernière bille perdue du dernier joueur (ou joueur Solo)

**Scénario nominal :**
1. Le système détecte la fin de partie
2. **Si mode Multijoueur :**
   - Calcul du classement final (tri par score décroissant)
   - Désignation du gagnant
3. **Si mode Solo :**
   - Vérification si high score local battu
4. Le système affiche l'écran de fin de partie :
   - Animation de victoire (confettis, lumières)
   - **Multi :** Podium avec classement complet
   - **Solo :** Score final et comparaison avec high score
5. Le système enregistre les statistiques de la partie
6. Le système propose les actions : Rejouer, Sauvegarder le score (si wallet), Retour au menu
7. **Si high score battu :** Déclenchement automatique de UC-G12

**Extensions / Erreurs :**
- 3a. High score battu → Animation spéciale "NEW RECORD!"
- 6a. Wallet connecté + score éligible → Proposition d'enregistrer on-chain
- 6b. Timeout 60 secondes sans action → Retour automatique au menu

**Postconditions :** 
- Partie terminée et archivée
- Scores enregistrés localement
- Statistiques mises à jour
- Système prêt pour une nouvelle partie

**Notes / Règles métier :**
- Les statistiques conservées : score, durée, nb de billes, meilleur combo
- En mode tournoi, les scores sont automatiquement envoyés on-chain
- L'écran de fin reste affiché max 60 secondes avant retour auto au menu

---

**UC-G11 / Sauvegarder une partie (Solo uniquement)**

**Acteurs :** Joueur, Système

**But :** Permettre au joueur de sauvegarder l'état actuel de sa partie Solo pour la reprendre ultérieurement

**Préconditions :** 
- Mode Solo uniquement (pas de sauvegarde en Multi)
- Partie en cours (UC-G04)
- Au moins une bille jouée
- Moins de 5 sauvegardes existantes

**Déclencheur :** Le joueur appuie sur le bouton "Pause" puis sélectionne "Sauvegarder et quitter"

**Scénario nominal :**
1. Le joueur met la partie en pause
2. Le menu pause s'affiche avec les options : Continuer, Sauvegarder, Quitter
3. Le joueur sélectionne "Sauvegarder et quitter"
4. Le système capture l'état complet de la partie :
   - Score actuel, billes restantes
   - Position de la bille (coordonnées x, y, vélocité)
   - Bonus actifs et leur durée restante
   - État des cibles, missions en cours, timestamp
5. Le système enregistre les données en mémoire locale
6. Le système affiche une confirmation : "Partie sauvegardée - [Date/Heure]"
7. Le système retourne au menu principal

**Extensions / Erreurs :**
- 4a. Mémoire de sauvegarde pleine (5 sauvegardes max) → Proposer de supprimer une ancienne sauvegarde
- 5a. Erreur d'écriture mémoire → Message d'erreur, proposition de réessayer
- 2a. Le joueur sélectionne "Quitter sans sauvegarder" → Confirmation demandée, perte de la progression

**Postconditions :** 
- État de jeu sauvegardé en mémoire locale
- Partie interrompue
- Joueur retourné au menu principal
- Sauvegarde accessible via UC-G03

**Notes / Règles métier :**
- Maximum 5 sauvegardes simultanées par borne
- Les sauvegardes de plus de 30 jours sont automatiquement supprimées
- La sauvegarde en mode Multijoueur est désactivée pour éviter les triches
- Taille d'une sauvegarde : ~50 KB

---

**UC-G12 / Enregistrer les meilleurs scores**

**Acteurs :** Système

**But :** Enregistrer et afficher le nouveau high score lorsqu'un joueur bat le record local

**Préconditions :** 
- Partie terminée (UC-G10)
- Score final > 10ème meilleur score local

**Déclencheur :** Fin de partie avec score supérieur au dernier score du TOP 10 local

**Scénario nominal :**
1. Le système compare le score final avec le TOP 10 local
2. Le système détermine la position du nouveau score (1er à 10ème)
3. Le système affiche l'animation "NEW HIGH SCORE!" avec effets spéciaux
4. Le système insère le score dans le classement local : Position, Score, Date/Heure, Mode, Joueur
5. Le système décale les scores inférieurs d'une position
6. Si plus de 10 scores : suppression du 11ème
7. Le système sauvegarde le nouveau TOP 10 en mémoire
8. Le système affiche le classement complet mis à jour
9. **Si wallet connecté :** Proposition d'enregistrer le score on-chain (UC-B03)

**Extensions / Erreurs :**
- 1a. Score ne bat aucun record → Pas d'enregistrement dans le TOP 10
- 4a. Erreur de sauvegarde → Retry automatique 3 fois, sinon log erreur
- 9a. Joueur refuse l'enregistrement on-chain → Score reste local uniquement

**Postconditions :** 
- TOP 10 local mis à jour
- Nouveau high score affiché
- Sauvegarde persistante en mémoire

**Notes / Règles métier :**
- Le TOP 10 est réinitialisé tous les 6 mois (1er janvier et 1er juillet)
- Seuls les scores en mode standard sont éligibles au TOP 10 local
- Les scores en mode tournoi sont enregistrés séparément on-chain
- Format d'affichage : #Position - Score - Date - Joueur

---

#### B. Use Cases - Blockchain & Compétition

---

**UC-B01 / Jouer en mode invité (Guest)**

**Acteurs :** Utilisateur non-connecté (Julien)

**But :** Permettre à un utilisateur sans wallet de jouer une partie simple sans enregistrement on-chain

**Préconditions :** 
- Flipper allumé
- Menu principal accessible

**Déclencheur :** L'utilisateur sélectionne "Jouer en invité" ou "Guest Mode" sur l'écran d'accueil

**Scénario nominal :**
1. L'utilisateur sélectionne "Mode Invité"
2. Le système charge la configuration par défaut du flipper
3. L'utilisateur configure sa partie (UC-G02)
4. L'utilisateur joue sa partie (UC-G04)
5. Le score est affiché en fin de partie
6. Le système enregistre le score uniquement en local (pas on-chain)
7. Le score est ajouté au classement local temporaire

**Extensions / Erreurs :**
- 6a. Mémoire locale pleine → Suppression des anciens scores invités (> 7 jours)
- 4a. Coupure électrique → Perte totale de la progression (pas de sauvegarde pour invités)

**Postconditions :** 
- Score stocké uniquement en local
- Aucune trace on-chain
- Possibilité de rejouer immédiatement

**Notes / Règles métier :**
- Les scores invités ne sont jamais enregistrés on-chain
- Maximum 100 scores invités conservés localement
- Les scores invités sont automatiquement supprimés après 7 jours
- Mode invité ne donne pas accès aux tournois avec cashprize

---

**UC-B02 / Connecter son wallet via QR code**

**Acteurs :** Utilisateur avec wallet Solana (Julien/Idir)

**But :** Permettre à l'utilisateur de s'authentifier avec son wallet Solana pour accéder aux fonctionnalités blockchain

**Préconditions :** 
- Flipper allumé
- L'utilisateur possède un wallet Solana (Phantom, Solflare, etc.)
- Le wallet contient au minimum du SOL pour les frais de transaction

**Déclencheur :** L'utilisateur sélectionne "Connecter wallet" dans le menu

**Scénario nominal :**
1. L'utilisateur clique sur "Connecter wallet"
2. Le système génère un QR code WalletConnect unique
3. Le QR code s'affiche à l'écran de la borne
4. L'utilisateur scanne le QR code avec son application wallet mobile
5. L'utilisateur approuve la demande de connexion dans son wallet
6. Le système reçoit la confirmation de connexion
7. La borne affiche l'adresse wallet anonymisée (XXX...XXX)
8. Le système charge le profil utilisateur (stats, tournois, customisation)
9. Le menu principal s'affiche avec toutes les options débloquées

**Extensions / Erreurs :**
- 2a. Problème de génération du QR code → Réessayer, sinon log erreur technique
- 4a. QR code expiré après 2 minutes → Génération automatique d'un nouveau QR code
- 5a. Connexion refusée par l'utilisateur → Retour à l'écran d'accueil
- 6a. Timeout de connexion (> 3 minutes) → Message d'erreur, retour au menu

**Postconditions :** 
- Wallet connecté et authentifié
- Session active avec l'adresse wallet
- Profil utilisateur chargé
- Accès complet aux fonctionnalités blockchain

**Notes / Règles métier :**
- Le QR code expire après 2 minutes pour des raisons de sécurité
- L'adresse wallet est toujours affichée en format anonymisé (XXX...XXX)
- Une seule session active par wallet à la fois
- La déconnexion est automatique après 24h d'inactivité

---

**UC-B03 / Jouer une partie classée**

**Acteurs :** Utilisateur connecté (Idir)

**But :** Permettre au joueur de jouer une partie dont le score sera enregistré on-chain sur Solana

**Préconditions :** 
- Wallet connecté (UC-B02 complété)
- Solde suffisant pour les frais de transaction (~0.001 SOL)
- Connexion Internet active

**Déclencheur :** L'utilisateur lance une partie avec son wallet connecté

**Scénario nominal :**
1. L'utilisateur lance une partie classée depuis le menu
2. Le système charge les paramètres personnalisés du joueur (customisation)
3. L'utilisateur joue sa partie (UC-G04)
4. À la fin de la partie, le score final est calculé
5. Le système crée une transaction Solana pour enregistrer le score
6. Le système affiche un résumé : score, frais estimés, leaderboard position
7. L'utilisateur approuve la transaction dans son wallet
8. La transaction est validée sur Solana
9. Le score est enregistré on-chain avec metadata (date, borne, mode)
10. Le leaderboard global est mis à jour
11. Confirmation affichée à l'écran avec nouvelle position

**Extensions / Erreurs :**
- 5a. Problème réseau → Tentative de retry 3 fois
- 7a. Transaction refusée par l'utilisateur → Score non enregistré, proposition de rejouer
- 8a. Transaction échouée (solde insuffisant) → Message d'erreur avec solde requis
- 5b. Le joueur choisit de ne pas enregistrer on-chain → Score reste local uniquement

**Postconditions :** 
- Score enregistré on-chain (si approuvé)
- Leaderboard global actualisé
- Statistiques du joueur mises à jour
- Transaction visible dans l'historique du wallet

**Notes / Règles métier :**
- Les frais de transaction sont estimés avant approbation
- Le score on-chain est immuable une fois enregistré
- Seuls les scores > 10,000 points sont éligibles au leaderboard global
- Maximum 10 enregistrements on-chain par jour par wallet

---

**UC-B04 / Participer à un tournoi hebdomadaire**

**Acteurs :** Joueur régulier (Idir)

**But :** Permettre au joueur de s'inscrire et participer au tournoi hebdomadaire avec cashprize

**Préconditions :** 
- Wallet connecté (UC-B02 complété)
- Solde suffisant pour l'entry fee du tournoi
- Le tournoi hebdomadaire est ouvert (statut : ACTIVE)
- Connexion Internet active

**Déclencheur :** L'utilisateur sélectionne "Tournoi hebdomadaire" dans le menu Tournois

**Scénario nominal :**
1. L'utilisateur accède au menu Tournois
2. Le système affiche le tournoi hebdomadaire actif avec :
   - Entry fee (ex: 0.1 SOL)
   - Prize pool actuel
   - Nombre de participants
   - Date de fin
   - Répartition des gains (ex: 50%/30%/20%)
3. L'utilisateur clique sur "Participer"
4. Le système affiche un récapitulatif de l'inscription
5. Le système crée une transaction pour l'entry fee
6. L'utilisateur approuve la transaction dans son wallet
7. Le smart contract enregistre la participation
8. L'entry fee est ajouté au prize pool
9. Confirmation d'inscription affichée
10. L'utilisateur joue sa partie (UC-G04)
11. Le score est automatiquement enregistré on-chain dans le tournoi
12. Le classement du tournoi est mis à jour en temps réel

**Extensions / Erreurs :**
- 5a. Solde insuffisant → Message d'erreur avec solde requis, annulation
- 6a. Transaction refusée → Retour au menu tournois
- 7a. Transaction échouée → Retry automatique 2 fois, sinon annulation
- 2a. Tournoi terminé → Affichage du prochain tournoi + date de début
- 2b. Tournoi complet (limite participants atteinte) → Impossible de s'inscrire

**Postconditions :** 
- Participation enregistrée on-chain
- Entry fee ajouté au prize pool
- Score classé dans le tournoi
- Éligible à la redistribution des gains (UC-B10)

**Notes / Règles métier :**
- Un joueur ne peut participer qu'une seule fois par tournoi hebdomadaire
- Le tournoi démarre tous les lundis à 00h00 UTC et se termine le dimanche à 23h59 UTC
- La redistribution automatique se fait 1h après la fin du tournoi
- Minimum 10 participants pour valider un tournoi

---

**UC-B05 / Créer un tournoi local (sans cashprize)**

**Acteurs :** Organisateur local (Idir)

**But :** Permettre à un utilisateur de créer un tournoi local sans enjeu financier pour jouer entre amis

**Préconditions :** 
- Wallet connecté (UC-B02 complété)

**Déclencheur :** L'utilisateur clique sur "Créer tournoi local" dans le menu Tournois

**Scénario nominal :**
1. L'utilisateur accède à l'interface de création de tournoi
2. Le système affiche le formulaire de configuration :
   - Nom du tournoi
   - Durée (heures) ou nombre de parties
   - Nombre de participants max (2-20)
3. L'utilisateur remplit les paramètres
4. Le système valide les paramètres
5. Le tournoi est créé localement (pas de smart contract)
6. Le système génère un QR code unique pour rejoindre le tournoi
7. Le QR code est affiché à l'écran
8. Les autres joueurs peuvent scanner le QR code pour s'inscrire (UC-B09)
9. Les parties sont jouées normalement (UC-G04)
10. Les scores sont enregistrés en local
11. Un classement temporaire est affiché sur la borne

**Extensions / Erreurs :**
- 3a. L'utilisateur annule → Retour au menu tournois
- 4a. Paramètres invalides (ex: durée = 0) → Message d'erreur, retour à l'étape 2
- 8a. Tournoi complet → QR code désactivé, message affiché

**Postconditions :** 
- Tournoi local créé et actif
- QR code généré et partageable
- Classement local visible
- Aucune transaction blockchain

**Notes / Règles métier :**
- Les tournois locaux ne nécessitent pas de SOL
- Durée maximum : 24 heures
- Les scores locaux sont effacés après la fin du tournoi
- Pas d'enregistrement on-chain des résultats

---

**UC-B06 / Créer un tournoi avec cashprize**

**Acteurs :** Organisateur (Idir)

**But :** Permettre de créer un tournoi compétitif avec prize pool et redistribution automatique des gains

**Préconditions :** 
- Wallet connecté (UC-B02 complété)
- Solde suffisant pour :
  - Prize pool initial (optionnel)
  - Frais de déploiement du smart contract (~0.01 SOL)

**Déclencheur :** L'utilisateur clique sur "Créer tournoi avec cashprize"

**Scénario nominal :**
1. L'utilisateur accède à l'interface de création avancée
2. Le système affiche le formulaire :
   - Nom du tournoi
   - Entry fee (en SOL)
   - Prize pool initial (contribution de l'organisateur, optionnel)
   - Durée (date de fin)
   - Nombre de participants max (optionnel, sinon illimité)
   - Répartition des gains (ex: 50%/30%/20% pour top 3)
3. L'utilisateur configure tous les paramètres
4. Le système calcule et affiche :
   - Frais de déploiement du smart contract
   - Prize pool total estimé
   - Gains potentiels par position
5. L'utilisateur valide la création
6. Le système déploie un smart contract Solana avec les paramètres
7. L'utilisateur approuve la transaction de déploiement + prize pool initial
8. Le smart contract est déployé et actif
9. Un QR code unique est généré pour rejoindre le tournoi
10. Le tournoi est publié et visible dans la liste des tournois actifs
11. Partage possible du QR code sur réseaux sociaux

**Extensions / Erreurs :**
- 3a. L'utilisateur annule → Retour au menu, pas de frais
- 4a. Paramètres invalides (ex: répartition ≠ 100%) → Message d'erreur, correction
- 7a. Transaction refusée → Annulation de la création, pas de frais
- 6a. Erreur de déploiement → Retry automatique, sinon remboursement des frais

**Postconditions :** 
- Tournoi créé avec smart contract actif
- Prize pool initialisé
- QR code généré et partageable
- Tournoi visible publiquement
- Redistribution automatique programmée

**Notes / Règles métier :**
- Entry fee minimum : 0.01 SOL
- Durée minimum : 1 heure, maximum : 30 jours
- La redistribution est automatique 1h après la fin du tournoi
- L'organisateur ne peut pas modifier les paramètres une fois le tournoi lancé
- Frais de plateforme : 2% du prize pool final

---

**UC-B07 / Consulter le leaderboard global**

**Acteurs :** Tout utilisateur (Julien/Idir)

**But :** Afficher le classement global des meilleurs joueurs enregistrés on-chain

**Préconditions :** 
- Connexion Internet active

**Déclencheur :** L'utilisateur accède au menu "Classement" ou "Leaderboard"

**Scénario nominal :**
1. L'utilisateur ouvre le leaderboard
2. Le système récupère les données on-chain depuis Solana
3. Le système affiche le TOP 100 avec :
   - Position (#1, #2, ...)
   - Adresse wallet anonymisée (XXX...XXX)
   - Score total (cumul ou meilleur score selon filtre)
   - Nombre de parties jouées
   - Badge spécial si top 10
4. L'utilisateur peut filtrer par :
   - Période (aujourd'hui, cette semaine, ce mois, all-time)
   - Mode de jeu (solo, tournois)
5. Si l'utilisateur est connecté, sa position est mise en surbrillance
6. L'utilisateur peut faire défiler le classement

**Extensions / Erreurs :**
- 2a. Problème réseau → Affichage d'un message d'erreur, proposition de réessayer
- 2b. Données en cache disponibles → Affichage avec mention "Dernière MàJ : [timestamp]"
- 5a. L'utilisateur n'est pas dans le top 100 → Affichage de sa position exacte en bas

**Postconditions :** 
- Classement affiché et consultable
- Position de l'utilisateur identifiée (si connecté)

**Notes / Règles métier :**
- Le leaderboard est mis à jour toutes les 5 minutes
- Seuls les scores > 10,000 points apparaissent
- Les scores en mode tournoi sont comptabilisés séparément
- Le classement all-time est réinitialisé tous les 6 mois (saisons)

---

**UC-B08 / Customiser l'apparence du flipper**

**Acteurs :** Joueur régulier (Idir)

**But :** Permettre au joueur de personnaliser l'apparence visuelle du flipper (couleurs, thèmes, effets)

**Préconditions :** 
- Wallet connecté (UC-B02 complété)

**Déclencheur :** L'utilisateur accède au menu "Personnalisation" ou "Customisation"

**Scénario nominal :**
1. L'utilisateur ouvre le menu de customisation
2. Le système affiche les catégories disponibles :
   - **Couleurs du plateau** (bumpers, rampes, raquettes)
   - **Thème de fond** (espace, rétro, néon, minimal, etc.)
   - **Effets visuels** (particules, trails de bille, éclairs)
   - **Musique d'ambiance**
3. L'utilisateur navigue dans les options
4. Pour chaque sélection, un aperçu en temps réel est affiché
5. L'utilisateur confirme ses choix
6. Le système sauvegarde les préférences :
   - Localement (cache)
   - On-chain (optionnel, frais ~0.001 SOL)
7. Le système affiche une confirmation
8. Les préférences sont appliquées immédiatement pour les prochaines parties

**Extensions / Erreurs :**
- 5a. L'utilisateur annule → Retour au menu sans sauvegarde
- 6a. Sauvegarde on-chain refusée → Sauvegarde locale uniquement
- 2a. Certaines options sont "Premium" (débloquables) → Affichage avec cadenas

**Postconditions :** 
- Personnalisation sauvegardée
- Thème appliqué pour les futures parties
- Préférences synchronisées (si on-chain)

**Notes / Règles métier :**
- La customisation est gratuite en local
- L'enregistrement on-chain permet de synchroniser sur toutes les bornes
- Certains thèmes sont débloquables via achievements ou tournois
- Les effets visuels peuvent être désactivés pour de meilleures performances

---

**UC-B09 / Rejoindre un tournoi via QR code**

**Acteurs :** Participant (Julien/Idir)

**But :** Permettre à un joueur de rejoindre rapidement un tournoi en scannant un QR code

**Préconditions :** 
- QR code valide (généré par UC-B05 ou UC-B06)
- Si tournoi avec cashprize : wallet connecté + solde suffisant

**Déclencheur :** L'utilisateur scanne un QR code de tournoi

**Scénario nominal :**
1. L'utilisateur scanne le QR code (via son mobile ou la borne)
2. Le système décode le QR code et identifie le tournoi
3. Le système récupère les informations du tournoi :
   - Nom
   - Organisateur
   - Entry fee (si cashprize)
   - Participants actuels / max
   - Prize pool
   - Règles
4. Le système affiche les détails du tournoi à l'écran
5. L'utilisateur clique sur "Rejoindre"
6. **Si tournoi avec cashprize :**
   - Le système vérifie que le wallet est connecté
   - Le système crée une transaction pour l'entry fee
   - L'utilisateur approuve la transaction
   - Le smart contract enregistre la participation
7. **Si tournoi local :**
   - Inscription directe sans transaction
8. Confirmation d'inscription affichée
9. L'utilisateur peut maintenant jouer sa partie dans le tournoi

**Extensions / Erreurs :**
- 2a. QR code invalide ou expiré → Message d'erreur
- 3a. Tournoi terminé → Affichage "Tournoi terminé", impossible de rejoindre
- 3b. Tournoi complet → Message "Tournoi complet", retour au menu
- 6a. Wallet non connecté → Redirection vers UC-B02
- 6b. Solde insuffisant → Message d'erreur avec montant requis
- 6c. Transaction refusée → Annulation, retour au menu

**Postconditions :** 
- Participation enregistrée au tournoi
- Entry fee payé (si cashprize)
- Joueur éligible à jouer et concourir

**Notes / Règles métier :**
- Un joueur ne peut rejoindre qu'une fois le même tournoi
- Le QR code expire à la fin du tournoi
- Les tournois locaux n'expirent pas les QR codes

---

**UC-B10 / Recevoir les gains d'un tournoi**

**Acteurs :** Gagnant de tournoi (Idir)

**But :** Recevoir automatiquement les gains d'un tournoi avec cashprize via smart contract

**Préconditions :** 
- Participation à un tournoi avec cashprize (UC-B04 ou UC-B06 complété)
- Classement dans une position gagnante (définie par la répartition)
- Le tournoi est terminé (date de fin atteinte)

**Déclencheur :** Le tournoi atteint sa date/heure de fin

**Scénario nominal :**
1. Le tournoi atteint sa date de fin
2. Le smart contract attend 1 heure (délai de grâce pour contestations)
3. Le smart contract récupère le classement final on-chain
4. Le smart contract calcule la répartition selon les règles définies :
   - Exemple : 1er = 50%, 2ème = 30%, 3ème = 20%
5. Le smart contract calcule les montants exacts pour chaque gagnant
6. Le smart contract exécute automatiquement les transferts de SOL
7. Les SOL sont transférés directement sur les wallets des gagnants
8. Le smart contract émet un événement de redistribution on-chain
9. **Si le joueur est connecté sur une borne :**
   - Notification affichée : "🏆 Vous avez gagné X SOL !"
   - Animation de victoire
10. Le joueur peut vérifier la transaction dans son wallet

**Extensions / Erreurs :**
- 6a. Problème technique blockchain → Retry automatique toutes les heures pendant 24h
- 7a. Wallet du gagnant inaccessible → Fonds en escrow, récupérables via claim manuel
- 3a. Égalité de scores → Répartition égale entre les joueurs ex-aequo

**Postconditions :** 
- SOL transférés sur le wallet du gagnant
- Transaction visible on-chain
- Tournoi marqué comme "COMPLETED"
- Statistiques mises à jour (victoires, gains totaux)

**Notes / Règles métier :**
- La redistribution est 100% automatique, pas d'intervention humaine
- Délai de grâce : 1 heure pour permettre la vérification
- Les frais de plateforme (2%) sont prélevés avant redistribution
- En cas d'erreur technique, les fonds restent en escrow et sont récupérables manuellement
- Historique complet des redistributions consultable on-chain

---

## Architecture technique

### Vue d'ensemble

Le système Flipper 12 est composé de plusieurs modules interconnectés fonctionnant sur une borne physique dédiée.

```

BORNE FLIPPER 12                         


 MOTEUR DE JEU (Three.js)             
  ─ Physique (Moteur physique temps réel)         
  ─ Gestion score (Calcul & affichage)           
  ─ UI (Interface utilisateur)                      
  ─ Audio (Effets sonores)                          

 MODULE BLOCKCHAIN                           
 ─ RPC Solana (Bibliothèque blockchain)   
 ─ Smart contracts                         
 ─ Gestion transactions  
                            

 BASE DE DONNÉES LOCALE                       
 ─ Scores locaux (Base de données relationnelle) 
 ─ Sauvegardes parties (Fichiers de sauvegarde)                      
 ─ Configuration borne                              
 ─ Cache leaderboard                               

 HARDWARE INTERFACE                               
 ─ Boutons raquettes (GPIO)                        
 ─ Écran tactile                                                                

        │
        │ HTTPS / RPC Solana
        ▼
       
  BLOCKCHAIN SOLANA 
   ─ Smart contracts
   ─ Leaderboard    
   ─ Tournois       
                    
```

### Communication

- **Internet → Blockchain** : HTTPS via RPC Solana (endpoints publics ou dédiés)
- **Borne → Wallet mobile** : QR code (WebSocket)
- **Stockage local** : Base de données relationnelle pour données structurées, fichiers de sauvegarde pour les parties
- **Hardware** : USB pour écran tactile

### Système d'exploitation

- **OS recommandé** : Système d'exploitation embarqué adapté
- **Gestion des processus** : Service système pour auto-démarrage
- **Mise à jour** : OTA (Over-The-Air) pour mises à jour logicielles

---

## Stack technique

| Composant | Technologie | Justification |
|-----------|-------------|---------------|----------------------|
| **Moteur de jeu** | Three.js|
| **Blockchain** | Solana | Frais de transaction très faibles (~0.00025 SOL)|
| **Smart contracts** | Framework de développement blockchain | Framework standard, sécurité, développement rapide |
| **Backend RPC** | Runtime applicatif + Bibliothèque blockchain | API RPC native, gestion asynchrone, écosystème riche | 
| **Stockage local** | Base de données relationnelle |
| **Sauvegardes** | Fichiers de sauvegarde | Format simple, lisible, facile à déboguer |
| **OS** | Système d'exploitation embarqué | Stable, open-source, support long terme |
| **Langage principal** | Javascript |
| **Gestion de version** | Système de contrôle de version | Standard industrie, collaboration, CI/CD |

### Dépendances principales

- **Bibliothèque blockchain** : Version récente
- **Framework smart contracts** : Version récente
- **Base de données** : Version récente

---

## Risques & contraintes

### Risques techniques

| Risque | Probabilité | Impact | Mitigation |
|--------|-------------|--------|------------|
| **Frais Solana variables** | Moyenne | Moyen | Simulation avant transaction + buffer de 20% sur frais estimés |
| **Latence réseau** | Haute | Fort | Mode offline avec queue de transactions, cache local des données |
| **Fail smart contract** | Faible | Critique | Audit de sécurité avant déploiement, tests exhaustifs, mécanisme de rollback |
| **Panne hardware** | Moyenne | Fort | Auto-diagnostics, logs détaillés, mode maintenance accessible |
| **Coupure électrique** | Faible | Moyen | Sauvegarde automatique toutes les 10s, UPS recommandé pour bornes critiques |
| **QR code expiré** | Moyenne | Faible | Régénération automatique après 2 minutes, notification visuelle |
| **Transaction rejetée** | Moyenne | Moyen | Retry automatique 3 fois avec backoff exponentiel, message clair à l'utilisateur |
| **Surcharge réseau Solana** | Faible | Fort | Utilisation de RPC multiples (fallback), priorisation des transactions critiques |
| **Manipulation des scores** | Faible | Critique | Validation côté smart contract, signature cryptographique, anti-replay |

### Contraintes

- **Pas de jeu en ligne** : Toutes les parties se jouent localement sur la borne physique
- **Borne physique uniquement** : Pas de version web ou mobile standalone
- **Solana uniquement** : Pas de support multi-chain dans la v1
- **Connexion Internet requise** : Pour fonctionnalités blockchain (tournois, leaderboard)
- **Wallet mobile requis** : Pas de wallet intégré dans la borne (sécurité)
- **Limite de participants** : Maximum 20 participants par tournoi local, illimité pour tournois on-chain

---

## Sécurité

### Validation des scores

- **Vérification côté client** : Score calculé en temps réel avec logs détaillés
- **Validation on-chain** : Smart contract vérifie la cohérence du score (plage raisonnable, pas de valeurs négatives)
- **Signature cryptographique** : Chaque score est signé par la clé privée du wallet (non accessible depuis la borne)
- **Anti-replay** : Timestamp + nonce unique pour chaque transaction
- **Plafonds** : Score maximum théorique calculé selon durée de partie et règles

### Gestion des erreurs RPC

- **Retry automatique** : 3 tentatives avec backoff exponentiel (1s, 2s, 4s)
- **Fallback RPC** : Liste de 3+ endpoints RPC, bascule automatique en cas d'échec
- **Mode dégradé** : En cas de panne réseau prolongée, fonctionnalités locales restent actives

### Audit et logs

- **Logs complets** : Enregistrement de toutes les actions critiques (connexions, transactions, erreurs)
- **Anonymisation** : Adresses wallet anonymisées dans les logs (XXX...XXX)
- **Rétention** : Logs conservés 90 jours localement, archivage optionnel
- **Alertes** : Notification automatique en cas d'anomalie détectée (tentative de triche, erreur critique)

---



## Conventions équipe

### Contrôle de version

- **Branches** : `feature/`, `fix/`, `hotfix/`, `refactor/`
- **Commits** : Format conventionnel
  - `feat: ajout système anti-tilt`
  - `fix: correction calcul score on-chain`
  - `docs: mise à jour CDC`
  - `refactor: optimisation requêtes RPC`
- **Fusion de code** : Description détaillée, review obligatoire avant merge

### Code

- **Commentaires** : En Anglais pour la logique métier, en anglais pour les APIs externes
- **Documentation** : Documentation standard pour toutes les fonctions publiques
- **Tests** :
- **Linting** : Outils de linting adaptés au langage utilisé

### Déploiement

- **Environnements** : dev → testnet → mainnet
- **CI/CD** : Tests automatiques sur chaque fusion, déploiement testnet automatique
- **Rollback** : Plan de rollback documenté pour chaque déploiement


---

## Questions ouvertes

### Techniques

1. **Moteur de jeu** : Allons nous uitliser Three.js ? 
   - *Décision attendue* : Évaluer performance et complexité physique requise
   - *Impact* : Architecture technique et stack

2. **OS de la borne** : Quel système d'exploitation embarqué ?
   - *Décision attendue* : Contraintes hardware, maintenance, coûts
   - *Impact* : Déploiement et support

3. **Gestion des sauvegardes** : Limite de 5 sauvegardes suffisante ?
   - *Décision attendue* : Retour utilisateurs
   - *Impact* : Expérience utilisateur

### Produit

6. **Frais de plateforme** : 2% du prize pool est-il acceptable ?
   - *Décision attendue* : Modèle économique, compétitivité
   - *Impact* : Viabilité économique

7. **Limite participants tournoi** : 20 pour locaux, illimité pour on-chain ?
   - *Décision attendue* : Cas d'usage réels
   - *Impact* : Scalabilité

8. **Customisation** : Gratuite ou certains thèmes payants ?
   - *Décision attendue* : Stratégie monétisation
   - *Impact* : Revenus additionnels

### Sécurité

9. **Audit smart contracts** : Audit externe obligatoire ou interne suffisant pour MVP ?
   - *Décision attendue* : Budget, timeline, niveau de risque acceptable
   - *Impact* : Sécurité et confiance utilisateurs

10. **Mode maintenance** : Accès admin local ou à distance ?
    - *Décision attendue* : Sécurité vs praticité
    - *Impact* : Support et maintenance

### Légaux / Compliance

11. **Réglementation** : Jeux d'argent ou compétition de skill ?
    - *Décision attendue* : Consultation légale
    - *Impact* : Déploiement géographique

12. **Données personnelles** : RGPD applicable (adresses wallet = données personnelles) ?
    - *Décision attendue* : Consultation légale
    - *Impact* : Conformité et architecture données

---
Objectif final
Créer un prototype jouable de flipper connecté combinant :
rendu 3D web
contrôles physiques ESP32
synchronisation temps réel 
Blockchain Solana 
Play2earn 