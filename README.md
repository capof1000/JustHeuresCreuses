# JustHeuresCreuses

L'objectif est de creer un blue print qui va prendre la decision d'activer (ou pas) un appareil, en fonction des previsions de la production solaire pendant une periode ( heures creuses).

## Installation

[![Open your Home Assistant instance and show the blueprint import dialog with a specific blueprint pre-filled.](https://my.home-assistant.io/badges/blueprint_import.svg)](https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https://github.com/capof1000/JustHeuresCreuses/blob/main/Blueprint_JustHeuresCreuses.yaml)


## Parametrage d'entree
- input Boolean/Switch : correspondant a l'equipement a controller
- Capteur de moment de la journée : un boolean qui va definir la periode de fonctionnent correspondant aux heures creuses (par default binary_sensor.heures_creuses)
- Capteur de prevision de production des prochaines 24h, exprime en kWh (par default sensor.energy_production_today_2)
- Seuil : input numerique , exprime en kWh correspondant au seuil minimun de prevision neceaaire
- 

## Fonctionnement
L'automatisataon va etre declencher chaque minute
L'automatisatiom ne prend des decisions que lorsque "Capteur de moment de la journée" = Active
L'automatisaion va allumer l'equipement si :
  - l'equipement est eteind et,
  - seuil > Capteur de prevision de production 
  
L'automatisaion va eteindre l'equipement si :
  - l'equipement est allume et,
  - seuil <= Capteur de prevision de production 


