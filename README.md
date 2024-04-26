# Projet-compteur-de-vitesse
Les composants nécessaires au projet sont:
un capteur magnétique
un afficheur 7 segments
une carte arduino

Le fonctionnement du circuit se déroule de la manière suivante:
Un aimant fixé à l'extrémité d'un rayon d'une roue de vélo (avec un diamètre définit) passe pour chaque tour de roue devant le capteur magnétique accroché à la fourche.
Celui ci va donc pouvoir compter la fréquence de passage de l'aimant et grâce à cette valeur on peut facilement calucler la vitesse: v=nb*diamètre 
La vitesse s'affiche par la suite sur l'afficheur 7 segments en km/h.
