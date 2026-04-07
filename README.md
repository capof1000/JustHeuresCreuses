# JustHeuresCreuses

⏱️ **JustHeuresCreuses** est un blueprint Home Assistant dont l'objectif est de décider automatiquement **d'allumer ou d'éteindre un équipement électrique** (ex : cumulus / chauffe-eau) pendant une **plage horaire définie**, en fonction :

- 🔋 d'un **besoin minimal vital** sur les **24 dernières heures glissantes**

🚿 Il est donc idéal pour alimenter ton cumulus pendant les heures creuses tout en minimisant les douches froides !

---

## Installation

[![Open your Home Assistant instance and show the blueprint import dialog with a specific blueprint pre-filled.](https://my.home-assistant.io/badges/blueprint_import.svg)](https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https://raw.githubusercontent.com/capof1000/JustHeuresCreuses/refs/heads/main/Blueprint_JustHeuresCreuses.yaml)

---

## Principe général

- ⏱ L'automatisation se déclenche **toutes les minutes** ainsi qu'à chaque **changement d'état** de la plage horaire
- 🌙 Elle agit **uniquement pendant la plage horaire active**
- 🔋 Elle garantit un **minimum d'énergie consommée sur 24h**
- 🖐 Elle **préserve le contrôle manuel** de l'équipement en dehors de la plage horaire

---

## Fonctionnement 🌙

| Situation | Décision |
|-----------|----------|
| Fin de la plage horaire (transition → off) | ❌ OFF |
| Hors plage horaire | ➖ Aucune action (contrôle manuel possible) |
| Consommation 24h insuffisante | 🔥 ON (priorité vitale) |
| Consommation 24h suffisante | ❌ OFF |

---

## Flux de décision
