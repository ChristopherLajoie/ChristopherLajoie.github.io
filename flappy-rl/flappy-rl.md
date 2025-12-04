[↩ Retour à l'accueil](/index)

--------------------------------------------------------------------------------

# Flappy-RL : Agent Autonome avec Curriculum Learning

### Table des matières

- [Contexte](#contexte)
- [Environnement et Physique](#environnement-et-physique)
- [Architecture de l'Agent](#architecture-de-lagent)
- [Stratégie d'Entraînement](#stratégie-dentraînement)
- [Résultats et Performance](#résultats-et-performance)

## Contexte

Ce projet a été réalisé dans le cadre d'un cours sur l'apprentissage par renforcement à l'Université de Sherbrooke. L'objectif principal était de reproduire le célèbre jeu **Flappy Bird**, mais en y intégrant une simulation physique plus riche et réaliste, incluant la gravité et des vents dynamiques.

Le défi consistait à entraîner un agent capable de naviguer dans cet environnement stochastique en utilisant des méthodes de Deep Reinforcement Learning.

## Environnement et Physique

Contrairement au jeu original simpliste, cet environnement intègre des variables physiques complexes pour accroître la difficulté et le réalisme.

### Espace d'État (Observations)

L'agent perçoit son environnement à travers un vecteur d'état continu comprenant :

* Position verticale (y) et vitesse verticale (v_y) de l'oiseau.
* Distance horizontale et verticale par rapport au prochain tuyau.
* Centre et hauteur de l'ouverture (gap).
* Vitesse actuelle des tuyaux et force du vent.

### Espace d'Action

L'espace d'action est discret et binaire :

1. **Flap** : L'oiseau bat des ailes (impulsion verticale).
2. **No Flap** : L'oiseau se laisse porter par la gravité.

## Architecture de l'Agent

L'agent a été entraîné utilisant l'algorithme **PPO (Proximal Policy Optimization)**.

* **Réseau de neurones** : Perceptron multicouche (MLP) avec une architecture `[256, 256]`.
* **Technique de vision** : Utilisation du *Frame Stacking* (empilement de 4 images/états consécutifs) pour permettre à l'agent de percevoir la temporalité et l'accélération.
* **Hyperparamètres** : Le taux d'apprentissage décroît linéairement de 1.5 x 10^-4 à 5 x 10^-5.

### Fonction de Récompense

La fonction de performance est cruciale pour guider l'apprentissage. Elle est définie comme suit :

R_total = R_step + R_pipe + R_center + R_crash

* **R_step** : Récompense de survie, pénalisée par l'utilisation excessive d'énergie (flaps).
* **R_pipe** : Bonus majeur (+1.0) pour avoir franchi un tuyau.
* **R_center** : Bonus de précision (+0.1) si l'oiseau passe exactement au centre de l'ouverture.
* **R_crash** : Pénalité sévère (-3.0) en cas de collision ou de sortie de l'écran.

## Stratégie d'Entraînement

Pour pallier la difficulté de l'environnement, une stratégie de **Curriculum Learning** a été mise en place, divisant l'apprentissage en **15 étapes progressives**.

L'environnement devient de plus en plus hostile au fil des étapes :

1. **Réduction de l'ouverture** : L'espace entre les tuyaux diminue progressivement.
2. **Tuyaux mouvants** : Les obstacles commencent à osciller verticalement.
3. **Vent dynamique** : Introduction d'une force de vent variable affectant la trajectoire.
4. **Vitesse accrue** : Augmentation de la vitesse de défilement du jeu.

Cette approche permet à l'agent de maîtriser les bases avant d'affronter des conditions extrêmes.

## Résultats et Performance

L'agent PPO a été comparé à un contrôleur PID classique pour évaluer sa performance. Après 20 millions d'étapes d'entraînement (ce qui équivaudrait à environ 4 jours de jeu non-stop pour un humain à 60 FPS), les résultats sont les suivants :

### Comparatif PPO vs PID

| Métrique | Agent PPO (Apprentissage) | Contrôleur PID (Classique) |
|:---------|:--------------------------|:---------------------------|
| **Score Moyen** | **40.77** ± 6.01 obstacles | 10.95 ± 0.33 obstacles |
| **Score Médian** | **33.40** ± 5.75 obstacles | 9.80 ± 0.40 obstacles |
| **Score Maximum** | **133.40** ± 56.79 obstacles | 34.0 ± 6.29 obstacles |

### Visualisation de la Politique

L'analyse de la politique apprise révèle une compréhension fine de la physique du jeu. La heatmap ci-dessous montre la probabilité de "Flap" en fonction de la position et de la vitesse verticale. On observe une frontière de décision nette (en bleu/rouge) qui s'adapte dynamiquement.

L'agent a surpassé de manière significative l'approche algorithmique classique, démontrant une capacité d'adaptation supérieure aux perturbations aléatoires comme le vent et les tuyaux mouvants.

--------------------------------------------------------------------------------

[↩ Retour à l'accueil](/index)