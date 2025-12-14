# JustHeuresCreuses

JustHeuresCreuses est un blueprint Home Assistant dont l’objectif est de décider automatiquement
d’allumer ou d’éteindre un équipement électrique (ex : cumulus / chauffe-eau) pendant les heures
creuses, en fonction :

- d’un besoin minimal vital sur les 24 dernières heures glissantes
- des prévisions de production solaire

Objectif principal :
éviter les douches froides tout en limitant la consommation inutile la nuit.

## Installation

https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https://raw.githubusercontent.com/capof1000/JustHeuresCreuses/refs/heads/main/Blueprint_JustHeuresCreuses.yaml

## Principe général

- L’automatisation se déclenche toutes les minutes
- Elle agit uniquement pendant les heures creuses
- Elle garantit un minimum d’énergie consommée sur 24h
- Elle coupe l’équipement si le solaire prévu est suffisant

## Fonctionnement (vue synthétique)

Hors heures creuses
-> Aucune action

Consommation 24h insuffisante
-> ON (priorité vitale)

Consommation suffisante + bon solaire
-> OFF

Consommation suffisante + mauvais solaire
-> ON

## Flux de décision

Déclenchement (toutes les minutes)
|
v
Heures creuses actives ?
- NON -> Aucune action
- OUI -> Lire énergie sur 24h
           |
           v
   Énergie 24h < énergie minimale ?
   - OUI -> ALLUMER (priorité vitale)
   - NON -> Lire prévision solaire
               |
               v
       Prévision solaire > seuil ?
       - OUI -> ÉTEINDRE (solaire suffisant)
       - NON -> ALLUMER

## Paramétrage du blueprint

### Équipement à contrôler
Type : switch ou input_boolean
Exemples :
- switch.cumulus
- input_boolean.sim_equipement

### Énergie consommée sur 24h (IMPORTANT)

Ce capteur doit représenter la consommation réelle sur les dernières 24 heures glissantes.
Il ne doit jamais se remettre à zéro à minuit.

Méthode recommandée (Statistics via l’interface) :

1) Identifier l’index d’énergie cumulatif de l’équipement
   Exemple : sensor.equipement_energy
   Ce capteur ne doit jamais se remettre à zéro.

2) Aller dans :
   Paramètres > Appareils et services > Entrées > Créer une entrée

3) Choisir : Statistic

4) Paramétrer :
   - Nom : Consommation équipement 24h
   - Entité source : sensor.equipement_energy
   - Caractéristique statistique : Somme des différences
   - Âge maximum : 24 heures

5) Enregistrer et sélectionner ce capteur dans le blueprint

NE PAS utiliser un Utility Meter journalier (reset à minuit),
sinon la valeur sera fausse pendant les heures creuses nocturnes.

### Capteur Heures Creuses

Capteur binaire indiquant la période d’activation.

Méthode simple via l’interface :
Paramètres > Appareils et services > Entrées > Créer une entrée

Choisir : Capteur de moment de la journée

Exemple :
- Nom : heures_creuses
- Activation : 00:30
- Désactivation : 05:00

### Énergie minimale à assurer sur 24h (kWh)

Seuil vital garantissant le confort.
Exemple chauffe-eau : 5 kWh

Si l’énergie consommée sur 24h est inférieure à ce seuil,
l’équipement sera forcé ON pendant les heures creuses.

### Prévision solaire sur 24h

Capteur de prévision photovoltaïque.
Exemple : sensor.energy_production_today

### Seuil minimum de production solaire

Si la prévision solaire dépasse ce seuil,
l’équipement restera éteint pendant les heures creuses.

Sinon, il pourra être allumé si nécessaire.

### Mode debug

Active des logs détaillés visibles dans :
Paramètres > Système > Journaux

Exemple :
[JustHeuresCreuses][DEBUG]
HC=on | Switch=off | Energy24h=3.2 kWh (min 5)
| Solar=1.1 kWh (seuil 2)
| Decision=ON (needs energy)

## Exemple concret

Énergie 24h | Solaire prévu | Décision
2 < 5 kWh   | 6 > seuil    | ON (priorité besoin)
6 >= 5 kWh  | 6 > seuil    | OFF (solaire OK)
6 >= 5 kWh  | 0.5 < seuil  | ON (pas assez solaire)

## Résumé

- Minimum vital garanti
- Optimisation solaire
- Compatible heures creuses nocturnes
- Logs debug explicites
- Idéal pour chauffe-eau / cumulus
