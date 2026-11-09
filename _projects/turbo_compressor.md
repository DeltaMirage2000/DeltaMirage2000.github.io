---
layout: page
title: Prototype de turbocompresseur
description: Compresseur centrifuge pour un microréacteur aéronautique, de la conception analytique et CFD au banc d'essai imprimé en 3D
importance: 1
category: /ingénierie Arts et Métiers
related_publications: false
---

Projet d'équipe sur deux semestres visant à concevoir, simuler, fabriquer et tester le compresseur centrifuge d'un turboréacteur miniature (échelle RC), puis à amorcer le dimensionnement de la chambre de combustion associée.

- **Contexte :** Projet de R&D (PJT), Arts et Métiers ParisTech, Aix-en-Provence, 2024–2025. Encadré par C. Faillat et P. Véron.
- **Outils :** Vista CCD (PCA Engineers), 3DExperience/CATIA, Siemens Star-CCM+, Abaqus, impression 3D FDM, Arduino.
- **Résultats :** Pression de sortie en CFD de **3,31 bar** pour un objectif de **3,5 bar** ; prototype imprimé fonctionnel ayant atteint **0,15–0,18 bar** à environ **21 600 tr/min** sur banc d'essai, en accord avec la tendance quadratique attendue.

<div class="row mt-3">
    <div class="col-sm-7 mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/projects/turbo-assembly-open.jpg" title="Prototype imprimé" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm-5 mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/projects/turbo-rotor-stator.jpg" title="CAO du rotor et du stator" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    À gauche : le prototype assemblé, avec le support moteur, le rotor, le stator et le carter extérieur. À droite : le rotor (vert) à l'intérieur du stator de diffusion (rouge), tel que conçu.
</div>

## 1. Cahier des charges

L'équipe de l'année précédente avait choisi un compresseur axial, copié sur les moteurs d'avions de chasse. À l'échelle RC, un unique étage centrifuge constitue le choix standard ; nous sommes donc repartis d'une analyse comparative de microréacteurs commerciaux (JetCat P100-RX, P160 RXi-B et P300-PRO). Leurs taux de compression varient de 2,9 à 3,55, pour des régimes maximaux de 105 000 à 154 000 tr/min.

Une analyse dimensionnelle (Vaschy-Buckingham) a réduit les six grandeurs régissantes (diamètre, longueur, vitesse, débit massique, pressions d'entrée et de sortie) à quatre groupes sans dimension :

$$
\pi_1 = \frac{P_s}{P_e}, \qquad
\pi_2 = \frac{N}{\sqrt{\gamma P_e / \rho}}, \qquad
\pi_3 = \frac{\rho N D_{ext}}{\mu}, \qquad
\pi_4 = \gamma
$$

Leurs valeurs s'étant avérées cohérentes à travers l'étude comparative, nous avons dimensionné notre modèle à partir du **JetCat P300-PRO** : un taux de compression de **3,5**, un débit massique de **0,5 kg/s** et une vitesse de **105 000 tr/min**.

## 2. Conception du rotor et du stator

La géométrie de la roue provient de Vista CCD, un outil de conception par ligne moyenne de PCA Engineers, s'appuyant sur les règles de l'art de la littérature IMechE :

- **Angle de pale à l'entrée** $\beta_1 = 60°$, optimal pour un débit massique maximal proche de Mach 0,55, plutôt que 80° pour un compresseur fixe, le moteur étant destiné à voler.
- **10° d'inclinaison vers l'arrière** (*backsweep*) au bord de fuite, ce qui retarde le pompage. Les moteurs de référence utilisent la même valeur.
- **7 pales principales et 7 pales intermédiaires** (*splitters*), d'une épaisseur de 2 mm, limitée par les capacités d'usinage d'une fraise sphérique 5 axes à cette échelle. Des congés de 1,5 mm correspondent à un outil sphérique de 1,5 mm.
- **Moyeu de 20 mm** pour l'arbre, jeu en pointe de 0,1 mm et longueur axiale égale à la moitié du diamètre, soit un rotor de **86 mm**.

L'impression 3D ne permettant pas de garantir un jeu de 0,1 mm en pointe, la version imprimée intègre un flasque (*shroud*) directement sur le rotor.

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/projects/turbo-rotor-cad.jpg" title="Rotor" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/projects/turbo-rotor-shroud.jpg" title="Rotor flasqué" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/projects/turbo-stator-cad.jpg" title="Stator" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    La roue ouverte avec pales intermédiaires, la version flasquée pour l'impression, et le stator de diffusion à 20 aubes (3DExperience).
</div>

Le **stator** redresse l'écoulement issu du rotor pour l'aligner vers l'axe et transformer la vitesse tangeantielle du fluide en pression statique tout en minimisant les pertes de pression totale. Ses 20 aubes directrices ont été définies à partir du triangle des vitesses de sortie à 100 000 tr/min, en supposant un débit massique constant, l'absence de pertes et un écoulement suivant les pales :

$$
U_2 = R_{ext}\,\omega = 0{,}043 \times 100\,000 \times \tfrac{2\pi}{60} \approx 451~\text{m/s}
$$

$$
C_r = \frac{\dot m}{\rho\, S_{out}} \approx 90~\text{m/s}, \qquad
- C_2 = \sqrt{C_r^2 + \left(U_2C_r 
\sin 10°\right)^2} \approx 444~\text{m/s}, \qquad
\alpha = \arctan\frac{C_r}{C_2} \approx 11{,}5°
$$

La section de passage du stator a été maintenue constante le long du canal malgré la variation du rayon.

## 3. Validation CFD (Star-CCM+)

Afin de maintenir un temps de calcul raisonnable, nous avons simulé **un seul canal inter-pale** en appliquant une symétrie de révolution pour le reste. Le volume d'air entre deux pales a été extrait par une opération booléenne, puis maillé et résolu au point nominal :

- **Conditions aux limites :** entrée d'arrêt à 1 bar (air au repos), sortie en pression.
- **Physique :** stationnaire, couplée, gaz parfait, turbulence **k-ω SST** (le modèle k-ε étant inadapté à ce type d'écoulement).
- **Maillage :** cellules polyédriques avec une taille de base de 0,2 mm et des couches limites prismatiques, maintenant un **$y^+ < 5$** sur la quasi-totalité des parois. Des maillages plus grossiers de 1 mm et 0,5 mm divergeaient. 500 itérations ont nécessité environ 3 heures.

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/projects/turbo-cfd-mesh.jpg" title="Maillage en couches prismatiques" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/projects/turbo-cfd-velocity.jpg" title="Module de vitesse" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/projects/turbo-cfd-pressure.jpg" title="Pression totale" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Un canal inter-pale : le maillage de couche limite, le champ de vitesse (augmentant radialement vers la sortie) et le champ de pression totale.
</div>

La pression de sortie moyenne a convergé vers **3,31 bar** pour un objectif de 3,5 bar, soit environ 5 % de moins. La vitesse de sortie moyenne s'est établie à 581 m/s. Les résidus ont stagné entre $10^{-1}$ et $10^{-2}$, en deçà de l'objectif habituel, sans pour autant diverger. Le maillage respectant les critères de gauchissement et de variation de volume de Star-CCM+, nous avons retenu ce résultat comme une première validation raisonnable de la conception.

## 4. Vérification structurelle (Abaqus)

Nous avons également modélisé un quart de rotor en aluminium sous Abaqus, soumis à une pression interne de 3,5 bar et à un chargement centrifuge. Nous avons simulé 11 cas de 1 000 à 100 000 tr/min. Les contraintes et déformations ont évolués linéairement avec la vitesse, pour culminer à seulement 0,016 MPa. Un calcul analytique manuel montre l'impossibilité de ce résultat : 10 g de pale à $r = 43$ mm et 100 000 tr/min donnent

$$
F = m\,\omega^2 r \approx 0{,}01 \times (10\,472)^2 \times 0{,}043 \approx 47~\text{kN}
$$

ce qui représente environ 470 MPa sur une section de 10 mm², bien au-delà de la limite d'élasticité de la plupart des alliages d'aluminium. Nous en avons conclu que le chargement ou le paramétrage des unités sous Abaqus était erroné, constituant un point ouvert à corriger plutôt qu'un résultat valide.

<div class="row mt-3">
    <div class="col-sm-8 mt-3 mt-md-0 mx-auto">
        {% include figure.liquid loading="eager" path="assets/img/projects/turbo-abaqus.jpg" title="Modèle du quart de rotor sous Abaqus" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Contrainte de von Mises sur le modèle du quart de rotor. Les valeurs quasi uniformes et infimes ont mis en évidence une erreur de paramétrage.
</div>

## 5. Prototype et banc d'essai

Le stator, le carter et le support moteur ont été imprimés en PLA. Le rotor a été obtenu par impression résine, garantissant une meilleur précision dimentionnelle et résistance mécanique comparé à l'impression FDM (ender 3v2). Les surfaces en contact ont été poncées pour réduire les jeux et lisser l'écoulement d'air. Plusieurs pièces ont dû être réimprimées en raison de dérives des tolérances d'impression par rapport à la CAO.

- **Arbre :** un arbre en acier usiné avait été dessiné et chiffré, mais par manque de temps d'atelier, une **vis M8×85** l'a remplacé. L'alésage du rotor pour cette vis a généré un important déséquilibrage.
- **Guidage :** un roulement 20×8×7 calé contre son épaulement maintient le rotor à distance du carter. Un second logement de roulement a été ajouté à l'entrée du rotor pour assurer un bon guidage.
- **Accouplement :** un manchon souple en **TPU** se visse sur l'arbre M8 et l'arbre moteur M5.
- **Carter :** une enveloppe cylindrique ferme l'étage et canalise l'air vers un plenum arrière, où se situe la prise de pression.

<div class="row mt-3">
    <div class="col-sm-8 mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/projects/turbo-printed-parts.jpg" title="Stator et rotor imprimés" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm-4 mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/projects/turbo-assembly.jpg" title="Assemblage fermé" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    À gauche : le stator et le rotor flasqué imprimés. À droite : l'assemblage avec le moteur et le variateur (ESC) montés au-dessus du rotor.
</div>

**Entraînement.** Un moteur brushless iFlight XING-E Pro 2207 1800KV est alimenté par une batterie LiPo via un variateur Hobbywing XRotor 45A. Une carte Arduino lit un potentiomètre et génère la commande PWM. L'estimation de puissance a montré que ce moteur ne peut atteindre le régime nominal de 100 000 tr/min ; le banc était donc destiné dès l'origine à des tests à vitesse réduite. La vitesse est estimée à partir de la consigne :

$$
\text{RPM} = \frac{\text{PWM} - 1000}{1000} \times V_{bat} \times K_V, \qquad \text{PWM} \in [1000, 2000]
$$

**Mesure.** Un capteur de pression différentielle MPX700DP associé à un étage d'amplification opérationnel transmet les données à l'Arduino. Il a été étalonné de 0 à 3 bar face au manomètre du compresseur du laboratoire, restant dans une marge de 5 % par rapport à la référence.

<div class="row mt-3">
    <div class="col-sm-7 mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/projects/turbo-test-bench.jpg" title="Montage d'essai" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm-5 mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/projects/turbo-wiring.jpg" title="Câblage moteur" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    À gauche : le banc d'essai avec la carte de mesure, l'Arduino et le compresseur. À droite : le câblage de la LiPo, du variateur, du moteur et de l'Arduino avec son potentiomètre de vitesse.
</div>

## 6. Résultats d'essai

Chaque point de mesure correspond à une consigne de vitesse stabilisée pour enregistrer l'élévation de pression. Nous avons réalisé **trois séries de 0 à 21 600 tr/min**. Au-delà, les vibrations dues au déséquilibre du rotor risquaient de détruire les pièces.

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/projects/turbo-test-measured.png" title="Élévation de pression mesurée" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Élévation de pression en fonction de la vitesse estimée du rotor pour les trois essais, après correction du décalage de zéro. La ligne pointillée représente une parabole passant par l'origine et le point nominal (3,5 bar à 100 000 tr/min).
</div>

Les trois séries confirment une réelle compression, atteignant 0,15–0,18 bar à vitesse maximale, et suivent fidèlement la tendance quadratique attendue $\Delta P \propto N^2$. La plage testée ne représentant que le premier cinquième de la vitesse nominale, cela conforte la conception sans pour autant valider l'objectif des 3,5 bar.

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/projects/turbo-test-extrapolation.png" title="Plage testée vs point nominal" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Les mêmes données sur toute la plage de vitesse : les essais couvrent la zone ombragée, tandis que le point nominal se situe bien au-delà.
</div>

## 7. Prochaine étape : chambre de combustion

Une fois le compresseur dimensionné, nous avons amorcé l'étude de la chambre de combustion selon la **méthode de Melconian et Modak**. Nous avons opté pour une structure **annulaire**, l'arbre traversant le centre du moteur rendant des chambres individuelles (*can-type*) trop exigües à cette échelle. À partir du débit de 0,5 kg/s du compresseur et d'un facteur de répartition (*pattern factor*) estimé à 0,25 :

| Quantité | Valeur |
|---|---|
| Section de référence $A_{ref}$ | 0,0128 m² |
| Diamètres du carter (externe / interne) | 130 mm / 24 mm |
| Diamètres de la tube à flamme (externe / interne) | 112 mm / 42 mm |
| Vitesse de référence $U_{ref}$ | 21,6 m/s (Mach 0,31) |
| Longueur de la tube à flamme $L_L$ | 117 mm |
| Zones primaire / secondaire / dilution | 26 / 17,5 / 60 mm |

<div class="row mt-3">
    <div class="col-sm-5 mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/projects/turbo-combustor-schematic.jpg" title="Écoulement dans la chambre" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm-4 mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/projects/turbo-combustor-cad.jpg" title="CAO de la tube à flamme" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm-3 mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/projects/turbo-engine-cad.jpg" title="Compresseur et chambre de combustion" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Répartition du débit d'air dans les zones primaire, secondaire et de dilution ; première CAO de la tube à flamme perforée ; et assemblage du compresseur sur la chambre de combustion.
</div>

L'injection de carburant et la turbine n'ont pas été arrêtées, ces deux éléments dépendant des conditions de sortie du compresseur mesurées sur un prototype métallique.

## Bilan et perspectives

- **Équilibrer le rotor en premier.** Le balourd, et non la puissance du moteur, a limité les essais à environ 22 000 tr/min. Un arbre usiné et un rotor équilibré constituent les prérequis indispensables pour monter en régime.
- **Réaliser une version métallique.** Un rotor usiné en aluminium permettrait de réaliser des essais proches de la vitesse nominale. Le plan de l'arbre et les devis de matière sont prêts.
- **Améliorer l'instrumentation :** intégrer un tachymètre optique à la place de l'estimation par la commande PWM, ainsi que des capteurs de température pour évaluer le rendement.
- **Corriger le modèle structurel** et le valider par rapport à l'estimation analytique avant de l'exploiter.

## Documents

- [Poster (PDF)](/assets/pdf/poster_pjt.pdf)
- [Diapositives de soutenance (PDF)](/assets/pdf/sountenance_pjt.pdf)

<iframe src="/assets/pdf/poster_pjt.pdf" width="100%" height="700"
        style="border:1px solid #888; border-radius:8px;"></iframe>
<p><a href="/assets/pdf/poster_pjt.pdf">Ouvrir le PDF dans un nouvel onglet</a></p>

Équipe : Dorian Mandrino, Mateo Mangialomini, Alexandre Poncelet, Charny Xu (avec T. Lhote et G. Rique au premier semestre).