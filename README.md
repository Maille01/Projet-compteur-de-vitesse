# Projet-compteur-de-vitesse
Les composants nécessaires au projet sont:
un capteur magnétique
,un afficheur oled
,une carte arduino

Le fonctionnement du circuit se déroule de la manière suivante:
Un aimant fixé à l'extrémité d'un rayon d'une roue de vélo (avec un diamètre définit) passe à chaque tour de roue devant le capteur magnétique accroché à la fourche.
Celui ci va donc pouvoir compter la fréquence de passage de l'aimant, ce qui se traduit en nombre de tour de par seconde. Et grâce à cette valeur on peut facilement calucler la vitesse: v=nb.diamètre.π .
La vitesse s'affiche par la suite sur l'afficheur oled en km/h.
