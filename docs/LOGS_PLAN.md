# LOGS_PLAN — Flipper 12

> **Objectif** : définir quelles traces le système produit pour observer et valider que tout fonctionne correctement — boutons, jeu, scores, blockchain.

---

## 1. Vue d'ensemble

| Partie du système | Ce qui génère les logs | Où ça va |
|-------------------|------------------------|----------|
| Boutons & capteurs (ESP32) | Appuis boutons, détection bille | Fichier local |
| Communication boutons → jeu | Messages entre l'ESP32 et le moteur de jeu | Fichier local |
| Moteur de jeu | Boucle de jeu, collisions, états | Fichier local |
| Affichage | Mises à jour des 3 écrans | Fichier local |
| Blockchain / Solana | Transactions, tournois, gains | Fichier local |
| Base de données | Scores, sauvegardes | Fichier local |
| Erreurs | Toutes les parties du système | Fichier dédié |

---

## 2. Boutons & capteurs (ESP32)

### Ce qu'on observe
- L'appui sur un bouton est bien capté et envoyé en moins de 16ms
- Il n'y a pas de doubles détections sur un seul appui
- L'ESP32 est allumé et connecté

### Ce qu'on enregistre

| ID | Événement | Données enregistrées | Importance |
|----|-----------|----------------------|------------|
| HW-01 | Appui bouton raquette | côté (G/D), heure, filtre anti-double appui | Détail |
| HW-02 | Relâchement bouton | côté (G/D), durée de l'appui, heure | Détail |
| HW-03 | Bille perdue | capteur sortie, heure | Info |
| HW-04 | Bille bloquée (30s sans bouger) | durée d'immobilité | Avertissement |
| HW-05 | Tilt détecté | nombre de secousses, heure | Info |
| HW-06 | Capteur en erreur | numéro du capteur, message d'erreur | Erreur |
| HW-07 | ESP32 démarré ou reconnecté | identifiant, version du programme | Info |

### Ce que ça valide
- Que les boutons répondent en moins de 16ms
- Qu'un appui = un seul événement (pas de doubles)
- Que le matériel est opérationnel au démarrage

---

## 3. Communication boutons → moteur de jeu

### Ce qu'on observe
- Les messages arrivent bien et rapidement (moins de 5ms)
- Aucun message perdu

### Ce qu'on enregistre

| ID | Événement | Données enregistrées | Importance |
|----|-----------|----------------------|------------|
| MQ-01 | Message reçu par le moteur de jeu | sujet, contenu, délai de transmission | Détail |
| MQ-02 | Message envoyé par l'ESP32 | sujet, taille, heure | Détail |
| MQ-03 | Connexion établie | adresse du serveur, heure | Info |
| MQ-04 | Déconnexion ou reconnexion | raison, tentative n°, heure | Avertissement |
| MQ-05 | Transmission trop lente (> 5ms) | délai mesuré, délai attendu | Avertissement |

### Ce que ça valide
- Que les boutons communiquent bien avec le jeu en moins de 5ms
- Que la connexion est stable toute la partie
- Qu'aucun appui n'est perdu en route

---

## 4. Moteur de jeu

### Ce qu'on observe
- Le jeu tourne bien à 60 images/seconde
- Les changements d'état (partie lancée, bille perdue, etc.) se passent correctement
- Les collisions déclenchent les bons points

### Ce qu'on enregistre

**Boucle de jeu**

| ID | Événement | Données enregistrées | Importance |
|----|-----------|----------------------|------------|
| GE-01 | Chaque image calculée | numéro d'image, temps écoulé, FPS réel | Détail |
| GE-02 | FPS tombe sous 55 | FPS réel, temps écoulé | Avertissement |
| GE-03 | Raquette activée | côté (G/D), numéro d'image | Info |

**Collisions**

| ID | Événement | Données enregistrées | Importance |
|----|-----------|----------------------|------------|
| GE-04 | Bille touche un bumper | identifiant, points de base | Info |
| GE-05 | Bille passe sur une rampe | identifiant, points de base | Info |
| GE-06 | Bille touche une cible spéciale | identifiant, points, bonus déclenché | Info |
| GE-07 | Deux cibles touchées en même temps | liste des cibles, total des points | Info |

**Déroulement de la partie**

| ID | Événement | Données enregistrées | Importance |
|----|-----------|----------------------|------------|
| GE-08 | Partie lancée | mode (solo/multi), nombre de joueurs, billes | Info |
| GE-09 | Bille lancée | numéro de bille, joueur, heure | Info |
| GE-10 | Bille perdue | numéro, billes restantes, score à ce moment | Info |
| GE-11 | Fin de partie | mode, scores de tous les joueurs, gagnant, durée | Info |
| GE-12 | Bonus activé | type de bonus, multiplicateur, durée | Info |
| GE-13 | Tilt → bille perdue | pénalité (-500 pts), joueur | Info |

### Ce que ça valide
- 60 FPS stable tout au long de la partie
- Chaque point est traçable depuis la collision jusqu'au score final
- Le temps de réaction des raquettes (écart entre HW-01 et GE-03)

---

## 5. Calcul du score

### Ce qu'on observe
- Le calcul est correct à chaque impact
- Les multiplicateurs s'appliquent bien
- Les meilleurs scores sont bien détectés

### Ce qu'on enregistre

| ID | Événement | Données enregistrées | Importance |
|----|-----------|----------------------|------------|
| SC-01 | Points calculés | points de base, multiplicateur, total ajouté, score global | Détail |
| SC-02 | Multiplicateur changé | ancienne valeur, nouvelle valeur, raison | Info |
| SC-03 | Score sauvegardé (toutes les 10s) | score, joueur, heure | Détail |
| SC-04 | Nouveau meilleur score local | score, ancien record, position dans le top 10 | Info |
| SC-05 | Score plafonné (trop élevé) | score tentant de dépasser la limite | Avertissement |

### Ce que ça valide
- Chaque point vient bien d'un impact (lien avec GE-04/05/06)
- Le score ne dépasse jamais la limite sans être bloqué
- Les sauvegardes automatiques toutes les 10s fonctionnent

---

## 6. Mise à jour des écrans

### Ce qu'on observe
- Les 3 écrans reçoivent bien les informations
- Rien ne se perd entre le moteur de jeu et l'affichage

### Ce qu'on enregistre

| ID | Événement | Données enregistrées | Importance |
|----|-----------|----------------------|------------|
| WS-01 | Écran connecté | lequel (principal / score / déco), heure | Info |
| WS-02 | Écran déconnecté | lequel, raison, heure | Avertissement |
| WS-03 | Image envoyée à l'écran principal | numéro d'image, taille des données | Détail |
| WS-04 | Score envoyé à l'écran score | score, billes restantes, joueur | Détail |
| WS-05 | Effet déco déclenché | type (impact bumper / bille perdue / jackpot), heure | Info |
| WS-06 | Envoi trop lent (> 16ms) | délai mesuré | Avertissement |

### Ce que ça valide
- Les 3 écrans sont connectés toute la partie
- L'affichage est fluide (moins de 16ms de délai)
- Le score affiché correspond bien au score interne

---

## 7. Blockchain & Solana

### Ce qu'on observe
- Les transactions partent bien et sont confirmées rapidement
- Les gains des tournois sont redistribués correctement

### Ce qu'on enregistre

**Connexion wallet**

| ID | Événement | Données enregistrées | Importance |
|----|-----------|----------------------|------------|
| BC-01 | QR code affiché | identifiant de session, expiration, heure | Info |
| BC-02 | Wallet connecté | adresse anonymisée (XXX...XXX), heure | Info |
| BC-03 | QR code expiré | identifiant, heure de création, heure d'expiration | Avertissement |
| BC-04 | Connexion refusée par le joueur | identifiant de session, heure | Info |

**Transactions**

| ID | Événement | Données enregistrées | Importance |
|----|-----------|----------------------|------------|
| BC-05 | Transaction créée | type, wallet anonymisé, montant en SOL, heure | Info |
| BC-06 | Transaction envoyée à Solana | identifiant de transaction, type, heure | Info |
| BC-07 | Transaction confirmée | identifiant, temps de confirmation | Info |
| BC-08 | Transaction échouée | identifiant, erreur, tentative n° | Erreur |
| BC-09 | Nouvelle tentative | identifiant, tentative n°, délai d'attente | Avertissement |
| BC-10 | Changement de serveur Solana | serveur en échec, serveur suivant, heure | Avertissement |

**Tournois**

| ID | Événement | Données enregistrées | Importance |
|----|-----------|----------------------|------------|
| BC-11 | Score enregistré sur la blockchain | wallet anonymisé, score, tournoi | Info |
| BC-12 | Tournoi créé | identifiant, mise d'entrée, prize pool, date de fin | Info |
| BC-13 | Joueur inscrit au tournoi | tournoi, wallet anonymisé, mise payée | Info |
| BC-14 | Redistribution des gains déclenchée | tournoi, prize pool total, nombre de gagnants | Info |
| BC-15 | Gains envoyés à un joueur | tournoi, wallet anonymisé, montant, classement | Info |
| BC-16 | Redistribution échouée | tournoi, erreur, montant mis en attente | Erreur |

### Ce que ça valide
- Chaque score blockchain est lié à une partie traçable
- Les montants redistribués correspondent bien au prize pool total
- La confirmation Solana prend moins d'1 seconde
- Aucune adresse wallet complète n'est écrite dans les logs

---

## 8. Base de données & sauvegardes

### Ce qu'on observe
- Les scores et parties sont bien enregistrés
- Les sauvegardes automatiques fonctionnent
- Le cache de score pendant la partie est cohérent avec la base

### Ce qu'on enregistre

**Base de données principale**

| ID | Événement | Données enregistrées | Importance |
|----|-----------|----------------------|------------|
| DB-01 | Score final sauvegardé | joueur, score, mode, heure | Info |
| DB-02 | Partie en cours sauvegardée | identifiant, joueur, billes restantes, score | Info |
| DB-03 | Sauvegarde ancienne supprimée (> 30 jours) | identifiant, date de création, date de suppression | Info |
| DB-04 | Erreur d'écriture | opération, message d'erreur, tentative n° | Erreur |
| DB-05 | Sauvegarde corrompue | identifiant, problème d'intégrité détecté | Erreur |

**Cache score (pendant la partie)**

| ID | Événement | Données enregistrées | Importance |
|----|-----------|----------------------|------------|
| RD-01 | Score mis à jour dans le cache | joueur, score | Détail |
| RD-02 | Cache vidé en fin de partie | joueur, score final | Info |
| RD-03 | Erreur cache | opération, message d'erreur | Erreur |

### Ce que ça valide
- Le score en cache pendant la partie = le score en base en fin de partie
- La sauvegarde automatique toutes les 10s fonctionne (SC-03 → DB-02)
- Le nettoyage automatique des vieilles sauvegardes fonctionne

---

## 9. Erreurs & alertes

Toutes les erreurs importantes sont regroupées dans un fichier dédié.

| Niveau | Situation | Action |
|--------|-----------|--------|
| Avertissement | Bouton répond en plus de 16ms | Log + compteur |
| Avertissement | FPS tombe sous 55 pendant 3 images | Log |
| Avertissement | Transaction qui doit être renvoyée | Log |
| Erreur | Capteur physique en panne | Log + alerte maintenance |
| Erreur | Transaction échouée 3 fois de suite | Log + alerte |
| Erreur | Smart contract en échec | Log + gains mis en attente |
| Erreur | Impossible d'écrire en base | Log + 3 nouvelles tentatives |
| Critique | ESP32 injoignable depuis 5s en jeu | Log + partie suspendue |

---

## 10. Où sont stockés les logs

| Type | Fichier | Durée de conservation |
|------|---------|-----------------------|
| Logs généraux | `app.log` | 90 jours |
| Logs détaillés (debug) | `debug.log` | 7 jours |
| Logs erreurs | `errors.log` | 90 jours |
| Logs matériel | `hardware.log` | 30 jours |
| Logs blockchain | `blockchain.log` | 90 jours |
| Logs base de données | Gérés automatiquement | 30 jours |

Les fichiers sont nettoyés automatiquement chaque jour. Les adresses wallet sont toujours anonymisées avant écriture.

---

## 11. Ce que les logs prouvent pour le MVP

| Critère | Comment on le vérifie |
|---------|-----------------------|
| Raquettes réactives (< 16ms) | Écart entre HW-01 (appui) et GE-03 (raquette activée) |
| 60 FPS stable | Suivi de GE-01 tout au long d'une partie |
| Score correct en temps réel | Chaîne GE-04/05/06 → SC-01 → WS-04 |
| Billes bien comptées | Séquence GE-09 / GE-10 / GE-11 |
| Score final = somme des impacts | Remonter les logs GE-04 à GE-11 |
| Blockchain rapide (< 1s) | Temps de confirmation dans BC-07 |
| Anonymat respecté | Vérifier qu'aucun log ne contient d'adresse complète |
| Redistribution correcte | Total BC-14 = somme des montants BC-15 |
