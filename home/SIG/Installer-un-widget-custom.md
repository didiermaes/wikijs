---
title: Installation d'un widget custom sur Webapp Builder Dev Edition
description: Différentes étapes de mise ne place d'un widget custom
published: 1
date: 2021-03-17T09:23:29.881Z
tags: esri, sig, widget
editor: markdown
dateCreated: 2021-03-17T09:09:28.151Z
---

# Ajouter un widget personnalisés à votre Web AppBuilder Developer Edition

La communauté en ligne d'Esri fourni des widgets personnalisés qui ne sont pas inclus avec Web AppBuilder for ArcGIS. Par exemple, il existe le  widget Profil d'élévation, qui affiche le profil de surface d'une ligne dessinée sur la carte, et le  widget Recherche améliorée. 

> Dans le forum des widgets personnalisés Web AppBuilder dans [GeoNet](https://community.esri.com/t5/web-appbuilder-custom-widgets/tkb-p/web-appbuilder-custom-widgetstkb-board) de nombreux exemple de réalisation. Plusieurs partenaires Esri ont également développé leurs propres widgets pour fonctionner avec Web AppBuilder for ArcGIS Developer Edition.
{.is-info}

Dans ce post, j'explique comment télécharger un widget personnalisé, comment le déployer sur une  installation Web AppBuilder for ArcGIS Developer Edition (locale). 

> Vous devez déjà avoir téléchargé et installé Web AppBuilder for ArcGIS Developer Edition sur une machine locale. Il doit être configuré pour fonctionner avec l'installation de Portal for ArcGIS.
{.is-warning}

## Recherchez et téléchargez un widget personnalisé

Ci-dessous, des sources fiables :
https://github.com/AdriSolid/WAB-Custom-Widgets
https://community.esri.com/t5/web-appbuilder-custom-widgets/roberts-custom-wab-widgets/td-p/844181
https://web-appbuilder-widget-search.surge.sh/

Je vais prendre l'exemple du widget de profil d'élévation créé par le talentueux Robert Scheitlin. Rechercher dans [GeoNet](https://community.esri.com/t5/web-appbuilder-custom-widgets/tkb-p/web-appbuilder-custom-widgetstkb-board) « élévation » à l'aide de la zone de recherche. Les résultats suivants devraient apparaître.

![community_600.jpg](/community_600.jpg){.align-center}

Rendez-vous en base de l'aticle, dans la rubrique "Attachments" pour télécharger l'archive au format zip [ElevationProfile.zip](/https://community.esri.com/ccqpr47374/attachments/ccqpr47374/web-appbuilder-custom-widgetstkb-board/1716/1/ElevationProfile.zip).

## Décompressez un widget personnalisé et installez-le dans Web AppBuilder for ArcGIS Developer Edition

Il reste à décompresser l'archive et à placer dans le bon dossier de destination, à savoir dans `..\ArcGISWebAppBuilder\client\stemapp\widgets\`.

Le widget Profil d'élévation est désormais disponible pour une utilisation dans Web AppBuilder for ArcGIS Developer Edition.

Il est préférable de rédémarrer le serveur local avant d'exploiter votre nouveau Widget.





