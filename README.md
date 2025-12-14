# JustHeuresCreuses

L'objectif est de creer un blueprint qui va prendre la decision d'activer (ou pas) un appareil, en fonction des previsions de la production solaire pendant une periode ( heures creuses).

## Installation

[![Open your Home Assistant instance and show the blueprint import dialog with a specific blueprint pre-filled.](https://my.home-assistant.io/badges/blueprint_import.svg)](https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https://raw.githubusercontent.com/capof1000/JustHeuresCreuses/refs/heads/main/Blueprint_JustHeuresCreuses.yaml)



## Parametrage d'entree Standard
- input Boolean/Switch : correspondant a l'equipement a controller
- Capteur d'energie de l'equipement a controller: il s'agit de l'index d'energie qui s'incremente, il peut etre calculer via l'integration Powercal ou directement fourni par le hardware powermeter.
- Capteur de moment de la journée : un boolean qui va definir la periode de fonctionnent correspondant aux heures creuses (par default binary_sensor.heures_creuses)
- Capteur de prevision de production des prochaines 24h, exprime en kWh (par default sensor.energy_production_today_2)
- Seuil : input numerique , exprime en kWh correspondant au seuil minimun de prevision neceaaire
## Parametrage d'entree Avances
- Periode de reference : imput numerique, exprimant un nombre d'heures. Cette periode sera utilise pour calculer une valeur moyenne de reference , (par default 96)
- Pourcentage mini : input numerique exprimant un pourcentage de fonctionnement minimun a assurer sur 24h. (par default 30)
- mode debug : boolean , (par default desactive)

## Fonctionnement
🚿 concrètement
Hors heures creuses	                    ->   ➖ aucune action
Peu de chauffe sur 24h	                ->   🔥 ON (quoi qu’il arrive)
Chauffe suffisante + bon soleil demain	->   ❌ OFF
Chauffe suffisante + mauvais soleil	    ->   🔥 ON



L'automatisatiom ne prend des decisions uniquement lorsque "Capteur de moment de la journée" = Active, deplus
  SI       ("seuil Prevision" < "Capteur de prevision de production") OU ("Energie de référence" < "Energie de l'equipement sur 24"), L'automatisaion va eteindre l'equipement 
  SINON , L'automatisaion va allumer l'equipement

L'automatisaion , dans le cas ou le mode debug est active va logguer toutes les variables et entite avec leur valeur, afin de facilier le troubleshooting.

# Probleme / Correction

*Message malformed: invalid template (TemplateSyntaxError: unexpected char '!' at 10) for dictionary value @ data['variables']['energy_now']
-> il faut d’abord stocker toutes les entrées dans des variables

*Message malformed: not a valid value for dictionary value @ data['actions'][0]['choose'][0]['sequence'][0]['target']['entity_id']
-> Il faut utiliser la syntaxe data_template (ou service_call via template), ou plus simple : mettre la variable dans entity_id: "{{ my_target_switch }}".
    


