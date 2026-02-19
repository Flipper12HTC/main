## Priorité P0 – Fonctionnement essentiel du jeu (la base du projet)

1. Développer le moteur physique de la bille

- Implémenter la gravité, les rebonds et la friction

- Garantir une fluidité à 60 FPS

- La bille doit avoir un comportement réaliste

DoD : la bille se comporte de manière stable et réaliste pendant toute une partie complète.

2. Gérer les raquettes (input joueur)

- Détecter les appuis sur les boutons physiques

- Réaction 

- Interaction cohérente avec la bille

DoD : les raquettes répondent instantanément sans latence perceptible.

3. Mettre en place le système de collisions

- Détection des collisions avec les raquettes, les murs et la cible


DoD : chaque impact est reconnu correctement et déclenche l’effet attendu.

4. Implémenter le système de score

- Ajout des points en temps réel

- Gestion des multiplicateurs (plafond x10)

DoD : le score s’affiche et se met à jour correctement pendant toute la partie.

5. Gérer les billes et la fin de partie

- Détection perte de bille

- Décrémentation du compteur

- Déclenchement écran de fin

DoD : la partie se termine correctement lorsque toutes les billes sont perdues.

6. Créer l’écran de fin de partie

- Affichage du score final

- Option pour rejouer ou revenir au menu

DoD : un écran clair s’affiche avec le score final et les options disponibles.

7. Sauvegarder les meilleurs scores en local

- Mettre en place un TOP 10

- Sauvegarde persistante

DoD : les scores restent enregistrés après redémarrage du système.

8. Mettre en place le mode invité

- Possibilité de jouer sans wallet

- Score enregistré uniquement en local  

DoD : un utilisateur peut jouer entièrement sans connexion blockchain.

9. Implémenter la connexion wallet via QR code

- Génération d’un QR code

- Connexion via wallet Solana

- Affichage anonymisé de l’adresse

DoD : le wallet est connecté et reconnu par le système.

10. Enregistrer un score sur la blockchain

- Création d’une transaction Solana

- Validation via wallet

- Enregistrement du score on-chain

DoD : le score est visible sur la blockchain après validation.

## Priorité P1 – Fonctionnalités compétitives (tournois/compétitions)


11. Mettre en place un tournoi hebdomadaire

- Paiement 

- Enregistrement du score dans le tournoi

DoD : un joueur peut participer officiellement à un tournoi actif.

12. Déployer un smart contract pour les tournois

- Paramètres configurables

- Prize pool actif

DoD : le smart contract est fonctionnel et utilisable.

13. Automatiser la redistribution des gains

- Calcul automatique des récompenses

- Transfert des SOL vers les wallets gagnants

DoD : les gains sont envoyés automatiquement sans intervention manuelle.

14. Créer un classement global

- Affichage du TOP 10

- Mise à jour régulière

DoD : le classement global est consultable et actualisé.

15. Rejoindre un tournoi via QR code

- Scan QR

- Inscription validée

DoD : le joueur est ajouté au tournoi après validation.

## Priorité P2 – Améliorations et expérience utilisateur (finalisées)

16. Créer un tournoi local sans cashprize

DoD : un tournoi peut être créé sans transaction blockchain.

17. Ajouter une customisation visuelle

DoD : le joueur peut modifier thème et couleurs.

18. Permettre la sauvegarde d’une partie solo

DoD : une partie peut être reprise à l’identique.

19. Ajouter des statistiques avancées

DoD : l’historique des parties est consultable.

20. Mettre en place un mode maintenance

DoD : auto-test au démarrage avec logs accessibles.
