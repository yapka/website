---
title: "SOZip avec GDAL"
subtitle: Accéder à des Shapefiles lourds sans décompression
authors:
    - Nathanael KOUAKOU
categories:
    - article
comments: true
date: 2026-04-14
description: "SOZip est une évolution discrète mais utile. Rétrocompatible, simple à produire avec GDAL, il s'intègre sans friction dans un workflow existant."
icon: ":material-folder-zip:"
image: "Image d'illustration d'un dossier compressé"
license: default
robots: index, follow
tags:
    - Zip
    - SoZip
    - OpenData
    - QGIS
    - GDAL
---

# SOZip avec GDAL : accéder à des Shapefiles lourds sans décompression

:calendar: Date de publication initiale : {{ page.meta.date | date_localized }}

## Introduction

J'ai découvert SOZip, j'ai voulu comprendre si ça valait le coup de l'adopter comme format de distribution, voici comment j'ai testé avec GDAL et ce que j'en retiens.

![illustration](https://cdn.geotribu.fr/img/logos-icones/logiciels_librairies/gdal.png){: .img-thumbnail-left }

## Probleme
Je distribue parfois des extracts de données communales à des utilisateurs qui n'ont besoin que d'une région. Avec un ZIP classique,ils doivent télécharger 150 Mo, dézipper, puis filtrer. C'est lent,consommateur d'espace disque, et peu pratique.

La question : existe-t-il un moyen de garder la compression tout en permettant un accès partiel ?


### SOZip à la rescousse

SOZip n'est pas un nouveau format. C'est un profil du ZIP existant qui ajoute un index interne permettant l'accès aléatoire dans un fichier compressé — sans décompression préalable. Développé par Spatialys,
il est intégré à GDAL depuis la version 3.7.

![illustration](https://github.com/sozip/sozip-spec/blob/master/images/logo.png){: .img-thumbnail-left }

Hypothèse : si SOZip tient ses promesses, je devrais pouvoir filtrer spatialement directement dans le ZIP, avec des performances proches du fichier non compressé.

## Protocole de test

- `--overwrite` : Mon option préférée pour les tests. Elle écrase le ZIP
  précédent si je me suis loupé, sans râler.
- `--enable-sozip=yes` : C'est ici qu'on active la magie. Sans ça, vous
  faites juste un ZIP banal.
- `--sozip-chunk-size=64k` : On découpe le fichier en morceaux de 64 ko.
  C'est le bon compromis : assez petit pour être rapide, assez gros pour
  que l'index ne pèse pas une tonne.
- `mon_archive.zip` : Votre cible.
- `donnees_lourdes.gpkg` : Votre source (marche aussi avec des Shapefiles
  ou des dossiers)


----

<video width="700" controls>
   
      <source src="https://video.osgeo.org/w/3AWnUUPeT5qUmoHoVYxeBo" type="video/mp4">

</video>



<!-- markdownlint-disable MD033 -->
<iframe width="100%" height="315" src="https://video.osgeo.org/w/3AWnUUPeT5qUmoHoVYxeBo" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
<!-- markdownlint-enable MD033 -->

## Conclusion
SOZip est pour moi la solution idéale pour l'Open Data actuel. C'est
 simple, efficace, et ça ne demande pas de tout révolutionner

<!-- geotribu:authors-block -->

{% include "licenses/default.md" %}
