## Hardware - Borne Flipper 12

# 1. Vue d'ensemble

La borne Flipper 12 repose sur deux composants principaux côté hardware :

- On aura un ESP32, microcontrôleur WiFi, branché à deux boutons physiques (flipper gauche et flipper droit)

- Ensuite la machine hôte (PC ou serveur local), qui fait tourner Node.js et le Game Engine

Donc l'ESP32 capte les appuis sur les boutons, filtre les rebonds électriques, puis envoie les événements au Game Engine via MQTT sur le réseau local de la borne.

# 2. L'ESP32

L'ESP32 est le microcontrôleur qui fait le lien entre les boutons physiques et le logiciel.

- Il tourne en continue et surveille l'état des pins GPIO

- Il  se connecte en Wifi au réseau local de la borne au démarrage

- Il publie les événements boutons sur le broker MQTT dès qu'un appui est détecté

-Il n'exécute aucune logique de jeu : son seul rôle est la capture et la transmission des inputs

Pins GPIO utilisés :

Bouton :            Pin GPIO :

Flipper gauche      GPIO34

Flipper droit       GPIO35

# 3. Les boutons

- Type : boutons normalement ouverts

- 2 boutons : flipper gauche et flipper droit

- Câblage : une borne du bouton sur le pin GPIO, l'autre sur le GND

- La résistance de pull-up interne de l'ESP32 est activé, donc aucune résistance externe nécessaire

Debounce software : Un délai de 50ms est appliqué après chaque détection d'appui pour ignorer les rebonds électriques. Sans ce filtre, un seul appui physique peut générer plusieurs signaux parasites en quelques millisecondes.

# 4. Communication MQTT

Le protocole MQTT est utilisé pour transmettre les events boutons de l'ESP32 vers le Game Engine.

Broker : Mosquitto, installé sur la machine hôte (même machine que Node.js)

- L'ESP32 publie sur des topics genre appui gauche flipper/input/left et appui droit flipper/input/right

- Latence cible : < 5ms sur réseau local

# 5. Pourquoi MQTT et pas autre chose 

- Pas de WebSocket direct depuis l'ESP32 : plus complexe à implémenter, moins fiable pour de l'IoT embarqué

- Pas de USB/Serial : trop contraignant physiquement, distance limitée, non adapté à une borne arcade

- MQTT est le standard IoT : léger, asynchrone (l'ESP32 publie et n'attend pas de réponse), et nativement supporté par les librairies Arduino/ESP32