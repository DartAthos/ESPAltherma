# Configuration des Capteurs de Consommation d'Énergie ESPAltherma

Ce document explique comment configurer et utiliser les nouveaux capteurs de consommation d'énergie pour votre pompe à chaleur ESPAltherma.

## Fichiers Ajoutés

### 1. `espaltherma_template_sensors.yaml` (Mis à jour)
Deux nouveaux capteurs template ont été ajoutés :
- **Heat Pump Energy Today** : Consommation d'énergie aujourd'hui
- **Heat Pump Energy This Month** : Consommation d'énergie ce mois-ci

### 2. `espaltherma_utility_meters.yaml` (Nouveau)
Configuration des compteurs utilitaires pour un suivi précis avec remise à zéro automatique :
- Consommation quotidienne
- Consommation hebdomadaire  
- Consommation mensuelle
- Consommation annuelle

## Configuration dans Home Assistant

### Étape 1 : Inclure les fichiers de configuration

Ajoutez ces lignes à votre `configuration.yaml` :

```yaml
# Template sensors existants + nouveaux capteurs d'énergie
template: !include espaltherma_template_sensors.yaml

# Nouveaux compteurs utilitaires (recommandé pour un suivi précis)
utility_meter: !include espaltherma_utility_meters.yaml
```

### Étape 2 : Redémarrer Home Assistant

Après avoir ajouté les configurations, redémarrez Home Assistant pour activer les nouveaux capteurs.

## Capteurs Disponibles

### Capteurs Template (Calcul en temps réel)
- `sensor.heat_pump_energy_today` : Énergie consommée aujourd'hui (kWh)
- `sensor.heat_pump_energy_month` : Énergie consommée ce mois-ci (kWh)

### Compteurs Utilitaires (Suivi précis avec remise à zéro)
- `sensor.heat_pump_energy_daily` : Compteur quotidien (remise à zéro à minuit)
- `sensor.heat_pump_energy_weekly` : Compteur hebdomadaire (remise à zéro le lundi)
- `sensor.heat_pump_energy_monthly` : Compteur mensuel (remise à zéro le 1er du mois)
- `sensor.heat_pump_energy_yearly` : Compteur annuel (remise à zéro le 1er janvier)

## Différences entre les Méthodes

### Capteurs Template
- **Avantages** : Calcul en temps réel, pas de dépendance externe
- **Inconvénients** : Moins précis, se remet à zéro si Home Assistant redémarre

### Compteurs Utilitaires  
- **Avantages** : Très précis, garde l'historique même après redémarrage, remise à zéro automatique
- **Inconvénients** : Nécessite que le capteur de puissance soit disponible en continu

## Recommandation

**Utilisez les compteurs utilitaires** (`espaltherma_utility_meters.yaml`) pour un suivi précis de la consommation. Les capteurs template sont fournis comme alternative si vous préférez une solution sans dépendance.

## Utilisation dans les Dashboards

Vous pouvez maintenant utiliser ces capteurs dans vos dashboards Home Assistant :

```yaml
type: energy
title: "Consommation Pompe à Chaleur"
entities:
  - entity: sensor.heat_pump_energy_daily
    name: "Aujourd'hui"
  - entity: sensor.heat_pump_energy_monthly  
    name: "Ce Mois"
  - entity: sensor.heat_pump_energy_yearly
    name: "Cette Année"
```

## Dépannage

### Le capteur affiche "unknown" ou "unavailable"
- Vérifiez que `sensor.heat_pump_total_power` fonctionne correctement
- Vérifiez que votre ESP32 envoie bien les données MQTT
- Redémarrez Home Assistant après avoir modifié la configuration

### Les valeurs semblent incorrectes
- Les compteurs utilitaires sont plus précis que les capteurs template
- Attendez quelques heures pour que les compteurs se stabilisent
- Vérifiez que la puissance électrique est calculée correctement

## Support

Pour toute question ou problème, consultez la documentation principale du projet ESPAltherma ou ouvrez une issue sur GitHub.
