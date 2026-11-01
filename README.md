---
title: Analyse fonctionnelle d'un projet
date: 2026-10-31
subtitle: UL1IN021
author: Clautina Kieki-Mafuta, Nayirah Abdool, Chen Legrand, Myra Souici Amar
---

# Système de pointage RFID

## Introduction

L'idée du projet vient d'une observation lors des cours à l'université : la prise de présence manuelle prend du temps et peut provoquer des erreurs. 

Le projet a pour objectif de développer un système de pointage automatique : un boîtier posé sur le bureau de l'enseignant enregistre l'entrée et la sortie des étudiants de la classe lorsqu'ils passent leur carte RFID, et un site web permet à l'enseignant de consulter les présences en direct, de corriger une erreur d'enregistrement et de réinitialiser le boîtier pour le prochain cours.


## Schéma général du projet

Le dispositif sera composé :
* Capteur RFID (RC522)
* LED (Verte / Orange / Rouge)
* Raspberry PI
* Buzzer


![Schéma général du projet](Schema_general.png "Schéma général du fonctionnement du boîtier")

Figure 1. Schéma général du système de pointage RFID.

La figure 1 présente le dispositif. Le capteur RFID lit la carte de l'étudiant et transmet l'information au Raspberry PI, qui enregistre la présence dans la base de données et signale le résultat par une LED de couleur (et un buzzer en cas de refus). Les présences sont consultables par l'enseignant sur le site web. Les flèches rouges en pointillés représentent les commandes de l'enseignant : depuis le site, il peut changer de cours (ce qui réinitialise le boîtier pour la prochaine séance) ou sélectionner la sortie d'un étudiant pour l'annuler directement dans la base de données en cas d'erreur.


## Scénarios d'usages

<u>Scénario 1 :</u> L'étudiant passe sa carte devant le capteur RFID du boîtier. Si la carte appartient à un étudiant inscrit au cours actuel, le Raspberry Pi enregistre l'heure dans la base de données et la LED verte s'allume : c'est l'entrée. Un délai minimum de 1 minute propre à chaque carte est imposé entre deux passages pour éviter les doubles badgeages. Une fois ce délai écoulé, un nouveau passage de la carte enregistre la sortie. Le système enregistre au maximum deux passages par séance (entrée et sortie).

<u>Scénario 2 :</u> Si l'étudiant repasse sa carte alors que son passage a déjà été pris en compte, c'est-à-dire avant la fin de son délai de 1 minute ou après l'enregistrement de son entrée et de sa sortie, la LED orange s'allume. Rien n'est enregistré.

<u>Scénario 3 :</u> Si la carte n'est pas reconnue ou n'appartient pas à un étudiant inscrit au cours, la LED rouge s'allume et le buzzer retentit. La présence n'est pas enregistrée.

<u>Scénario 4 :</u> L'enseignant se connecte au site web depuis son ordinateur et regarde en direct la liste des étudiants présents. S'il remarque qu'une sortie a été enregistrée par erreur, il clique sur « Annuler la sortie » à côté du nom de l'étudiant. L'heure de sortie est supprimée de la base de données et l'étudiant redevient présent.

<u>Scénario 5 :</u> L'enseignant souhaite passer à un autre cours. Il clique sur le bouton « Changer de cours » du site web, ce qui réinitialise le boîtier et le prépare à enregistrer les présences de la séance suivante.


## Description des fonctionnalités

| N° | Nom | Description | Sur quoi la fonction agit | Importance |
|:--- |:------ |:------------ |:-------------- |:------ |
| 1 | Lecture donnée | Le raspberry récupère la donnée récoltée par le capteur qui sera ensuite enregistrée dans la base de données | Raspberry, capteur RFID, base de données | 1 |
| 2 | Communiquer les données | Via une connexion WIFI, la base de données met à jour le site Internet pour que l'utilisateur soit informé en temps réel | Base de données, utilisateur | 1 |
| 3 | Donner le résultat après la lecture de la donnée | Selon la donnée reçue par le capteur, le LED envoie une couleur qui nous indique comment la donnée est lue ainsi que le buzzer dans un cas| Capteur, buzzer, LED, raspberry | 1 |
| 4 | Manipuler les données | Selon la situation, l'utilisateur peut choisir d'annuler une donnée / supprimer toutes les données (sur le site Internet) | Utilisateur, base de données | 2 |









