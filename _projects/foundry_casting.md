---
layout: page
title: Fonderie d'aluminium en sable
description: Conception, simulation et coulé d'une pièce de mangeoire à oiseaux en AlSi13 dans un moule en sable imprimé en 3D
importance: 3
category: /ingénierie Arts et Métiers
related_publications: false
---

- **Contexte :** Projet de fonderie Arts et Métiers (équipe de 3), automne 2024
- **Outils :** CATIA V5, ESI QuikCAST (simulation thermique et de remplissage), moule en sable imprimé en 3D

- [Rapport complet (PDF)](/assets/pdf/foundry_report.pdf)
- [Présentation (images) (PDF)](/assets/pdf/foundry_presentation.pdf)

Nous avons conçu la base d'une mangeoire à oiseaux (une coupelle fine avec deux bras hélicoïdaux), de la CAO jusqu'à la pièce en aluminium réelle, coulée à la fonderie de l'école.

<div class="row mt-3">
  <div class="col-sm-6">
    <img src="/assets/img/projects/foundry-cast-part.jpg" alt="La pièce moulée avec ses masselottes et son système de remplissage, à côté du godet de coulée"
         class="img-fluid rounded z-depth-1" loading="lazy">
  </div>
  <div class="col-sm-6">
    <img src="/assets/img/projects/foundry-final-part.jpg" alt="La pièce de mangeoire à oiseaux en aluminium terminée après parachèvement"
         class="img-fluid rounded z-depth-1" loading="lazy">
  </div>
</div>
<div class="caption">À gauche : le brut de fonderie à la sortie du moule, avec ses masselottes et ses canaux de coulée. À droite : la pièce après parachèvement.</div>

**Conception et simulation**

- Comparaison des orientations de coulée sous QuikCAST et conservation de celle qui éloignait la retassure du bossage de vis mince, puis épaississement de ce bossage pour pouvoir y placer une masselotte.
- Dimensionnement de trois masselottes cylindro-sphériques en utilisant les règles du module, de la contraction volumique et de la distance d'alimentation ($$V_{masselotte} \geq R\,S\,V_{pièce}$$, avec $$R = 5,5\%$$).
- Dimensionnement d'un système de remplissage à deux attaques : temps de remplissage calculé à partir de la section la plus fine ($$M' = 1,16$$ mm, $$t_r \approx 1,4$$ s), puis itération sur l'efficacité hydraulique pour obtenir la section de la descente de charge.
- Re-simulation de l'ensemble du tracé : l'empreinte se remplit en environ 1,5 s avec un métal encore 38 °C au-dessus du liquidus, et la fin de solidification s'effectue à l'intérieur des masselottes.

<img src="/assets/img/projects/foundry-quikcast.jpg" alt="Simulation du fractionnement solide sous QuikCAST"
     class="img-fluid rounded z-depth-1 d-block mx-auto" style="max-width: 520px;" loading="lazy">
<div class="caption">Simulation QuikCAST de la fraction solide.</div>

**Moule et moulage**

- Séparation du moule pour l'impression 3D sable (un noyau conique pour les bras hélicoïdaux, un bouchon pour le nettoyage de la masselotte supérieure, et des assemblages à rainure et languette), puis flambage du moule et coulée.
- Le rendement mesuré correspond très exactement aux prédictions : un ratio métal/pièce de 2,89, contre 2,93 attendu.
- La découpe des masselottes a montré que les retassures se sont bien installées à l'intérieur de celles-ci, comme prévu.

<div class="row mt-3">
  <div class="col-sm-6">
    <img src="/assets/img/projects/foundry-feeder-top.jpg" alt="Coupe transversale de la masselotte supérieure montrant une retassure"
         class="img-fluid rounded z-depth-1" loading="lazy">
  </div>
  <div class="col-sm-6">
    <img src="/assets/img/projects/foundry-feeder-side.jpg" alt="Coupe transversale d'une masselotte latérale et d'une partie du canal de coulée montrant une retassure"
         class="img-fluid rounded z-depth-1" loading="lazy">
  </div>
</div>
<div class="caption">Masselottes sectionnées : la masselotte supérieure (à gauche) et une masselotte latérale avec une partie du canal (à droite). Les retassures se trouvent dans les masselottes et non dans la pièce.</div>

- Quelques petites porosités de retassure sont restées sur la coupelle. Nous les avons attribuées à la séparation du front de solidification à travers la coupelle, ce qu'une épaisseur évolutive de la coupelle aurait permis de corriger.


Équipe : Maxence Chauvel, Dorian Mandrino, Mateo Mangialomini.