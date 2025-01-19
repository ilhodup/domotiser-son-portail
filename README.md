# Domotiser son portail
# Portail Automatique double vantaux Nice.

Petit rappel, l'électricité n'est pas un jeu, assurez-vous de toujour travailler hors tension.

DOMOTISER son portail automatique avec un esp32 sous esphome pour 25 euros.

Si vous souhaitez domotiser une Touche 1 Vantail sur votre automatisme 2 vantaux il faudra d'abord faire un test de faisabilité:
Motorisation Hors tension, debranchez le moteur2 du vantail puis remettre sous tension. Faite un essai avec la télècommande ouverture total 2 vantaux puis fermeture.
Si tout est ok, vous pouvez domotiser la touche 1 vantail.

materiel requis:
- 1 esp32 avec antenne deporté. ( entre 4.50 et 7 euros)
- 1 carte 4 relais 5V. (2.80 euros)
- 2 optocoupleurs 24VDC ou 230VAC suivant le/les moteurs que vous avez.(3,60 euros les 2)
- 2 diodes 1N4007 ou autres selement si vous avez des moteurs en 24VDC.
- 1 tranfo 230VAC/5VDC 3W minimum. (3.5 euros)
- 1 fin de course 5V étanche SN04-N 5V 30V comme sur la photo. (7.50 euros). Faire BIEN attention de prendre un capteur 5V-30V et non un 10V-30V car le 10v+30v je ne l'ai pas testé.

Faite un achat groupé pour reduire les frais de livraison.

  ![Screenshot_20241204_133002_AliExpress - Copie](https://github.com/user-attachments/assets/cbbf3a94-3efd-42c8-a344-54b178512394)





  

La plupart des portails automatiques ont un contact sec(pas à pas) pour commander l'ouverture avec un interphone ou un bouton poussoir: Ouverture/Stop/Fermeture. C'est à partir de cette fonction que l'on va domotiser:

- 1 bouton pour Ouvrir/Fermer 1 vantail.
- 1 bouton pour Ouvrir/Fermer les 2 vantaux.
- 1 bouton pour Ouverture parcielle d'un vantail.
- 1 bouton poussoir pour Allumer/Eteindre une lampe.
- 1 bouton pour une tempo de 5min de la même lampe.
- 1 crénau horaire pour l'éclairage de 17h30 à 8h00.


![Screenshot_20241214_135434_Home Assistant](https://github.com/user-attachments/assets/f06015b5-6a12-4832-bd92-6deffd84bee9)

![Screenshot_20241214_154510_Home Assistant](https://github.com/user-attachments/assets/51731468-0e3e-4a00-9315-5657aa2c5a07)

Sur la carte 5Vcc 4 relais: 
- Le 1e relais IN1 sur le pin 16 de l'esp32 sera le contact sec pour Ouvrir/Fermer.
- Le 2e relais IN2 sur le pin 17 sera pour stoper le 2e moteur pour faire la fonction 1 vantail.
- Le 3e relais IN3 sur le pin 18 sera pour l'eclairage de la lampe.
- le 4e relais IN4 sur le pin 19 era pour le crénaux horaire J/N.

Attention, bien respecter le schema de cablage sur la carte relais pour l'eclairage: relai 3 et ralai 4.

ACHAT:
Meme si vous voulez pas mettre de lampe exterieur, je vous conseil de prendre quand même une carte 4 relais vu son prix. Si un jour l'envie d'en mettre une vous serez pas bloqué.
Je vous conseil vivement de prendre un esp32 avec une antenne deportée pour une meilleur reception surtout que l'esp32 sera fermé dans une boite. J'ai pris un WROOM-32U avec antenne 2.4G.

les 2 optocoupleurs servent à savoir dans quel sens le moteur tourne, si c'est en ouverture ou en fermeture. L'inversion de la tension 24Vcc sur ce moteur nous oblige à mettre 2 diodes 1N400x pour determiner ce sens.
Pour les 2 diodes voici comment je les ai integrés: un bout de fil internet cat6 sur la diode avec une petite soudure et 2 bouts de gaine thermo pour isoler et tenir l'ensemble. Laisser au 10cm de longueur pour pouvoir cabler facilement sur l'optocoupleur.

![20241213_233337](https://github.com/user-attachments/assets/a86f2d1d-249f-4166-aed7-bd01418cd27e)
![20241213_235005](https://github.com/user-attachments/assets/73a0f288-ece3-4e72-bfd5-5773b4240bde)


LA FIN DE COURSE:
La fin de course est nécessaire, c'est lui qui va donner l'information que le portail est bien fermé et surtout donner l'ordre de remettre à zero et notamment le débloquage du moteur2 si celui-ci est bloqué.

![20241214_114451](https://github.com/user-attachments/assets/68a6647e-5797-42a3-8ee0-fa8fd37eb93a)


Même si c'est un capteur étanche pour l'exterieur, j'ai préferé le proteger du soleil car il est situé plein sud. N'ayant pas d'imprimante 3D j'ai modelé dans une chute de goulotte élèctrique avec un décapeur thermique. Par sécurité j'ai mis ce capteur un peu incliné par rapport au bras du moteur. Pour la fixation faite en sorte que ce soit réglable.

REGLAGES:
Une fois tout cablé,  il faudra chronomètrer le temps d'ouverture et le temps de fermeture puis prendre le temps le plus long pour modifier les reglages dans le code Yaml. Mon portail à une ouverture de 19s et une fermeture de 28s environ. dans les lignes 111, 305, 315, 318 les valeurs sont ainsi portées à 2500 et apres les essaies cela represente bien le temps de fermeture. Par exemple, si le temps le plus long est de 35s, il faut remplacer les chiffres 2500 par 3000 dans les lignes 111, 305, 315, 318 puis flasher l'esp32 et tester.
Du fait que l'ouverture à une durée plus courte que la fermeture, à 65% d'ouverture le portail passera à 100%. C'est à la ligne 304 pour paser de 65% à
à  100%. Ainsi à la fermeture on aura belle est bien le bon decompte. Ces lignes seront à supprimer si vous avez une ouverture et fermeture lineaires.
Pour l'ouverture partielle, à la ligne 173 il y a 6s. A modifier selon votre motorisation.

Sur mon dashboard, pour avoir 1 vantail ou 1 vantail partiel on doit faire un clic long pour éviter un appuis par erreur et laisser le bouton 2 vantaux prioritaire.
l'icon gyrophare me sert pour faire clignoter l'icon ouverture ou fermeture lorsque celui ci est en mouvemant.







![20241206_175830](https://github.com/user-attachments/assets/ea25d35e-8b67-41d7-abc7-e8dcedba501a)

Dans cette boite vous pouvez voir l'esp32, les 2 opto-coupleurs, la carte 4 relais et un tranfo 230AC/5VDC.

![20241214_111033](https://github.com/user-attachments/assets/eff83ba7-c609-4ed2-b209-c53424352cd8)
Le montage fini avec l'antenne de l'esp32 déporté pour une meilleur réception.


