# JustHeuresCreuses

L'objectif est de creer un blueprint qui va prendre la decision d'activer (ou pas) un appareil, en fonction des previsions de la production solaire pendant une periode ( heures creuses).

## Installation

[![Open your Home Assistant instance and show the blueprint import dialog with a specific blueprint pre-filled.](https://my.home-assistant.io/badges/blueprint_import.svg)](https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https://github.com/capof1000/JustHeuresCreuses/blob/main/Blueprint_JustHeuresCreuses.yaml)


## Parametrage d'entree
- input Boolean/Switch : correspondant a l'equipement a controller
- Capteur d'energie de l'equipement a controller: il s'agit de l'index d'energie qui s'incremente, il peut etre calculer via l'integration Powercal ou directement fourni par le hardware powermeter.
- Capteur de moment de la journée : un boolean qui va definir la periode de fonctionnent correspondant aux heures creuses (par default binary_sensor.heures_creuses)
- Capteur de prevision de production des prochaines 24h, exprime en kWh (par default sensor.energy_production_today_2)
- Seuil : input numerique , exprime en kWh correspondant au seuil minimun de prevision neceaaire
- Periode de reference : imput numerique, exprimant un nombre d'heures. Cette periode sera utilise pour calculer une valeur moyenne de reference , (par default 96)
- Pourcentage mini : input numerique exprimant un pourcentage de fonctionnement minimun a assurer sur 24h. (par default 30)

## Fonctionnement
L'automatisataon va etre declencher chaque minute
L'automatisataon va calculer une variabe interne, appele "Energie de reference", servant a stocker (la consommation quotidienne moyenne de l'equipement pendant la periode de reference) * Pourcentage mini
L'automatisataon va calculer une variabe interne, appele "Energie de l'equipement sur 24", servant a stocker et calculer , grace au capteur d'energie fourni: la consommation de l'appareil au cours de 24h dernieres heure, exprimee en kWh.
L'automatisatiom ne prend des decisions uniquement lorsque "Capteur de moment de la journée" = Active, deplus
  SI       ("seuil Prevision" < "Capteur de prevision de production") OU ("Energie de reference" > "Energie de l'equipement sur 24"), L'automatisaion va eteindre l'equipement 
  SINON , L'automatisaion va allumer l'equipement
    


