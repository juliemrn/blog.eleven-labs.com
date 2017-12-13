---
layout: post
title: Pour Noël, je configure mon noyau GNU/Linux !
excerpt: ~
authors:
- aandre
permalink: /fr/configurer-kernel-linux/
lang: fr
categories:
    - linux
tags:
    - linux
    - kernel
    - compilation
    - gentoo
cover: ~
---

Maintenant que nous avons compris de façon générale comment fonctionnait le noyau Linux dans le précédent article, voyons comment le configurer afin, à terme, de le compiler et l'utiliser.

Je considère dorénavant que votre choix est fait entre compiler votre Kernel par l'usage d'une des solutions suivantes :
 - Sur votre machine actuelle ;
 - Sur une ancienne machine ;
 - Sur une VM type VirtualBox ;
 - Sur un container Docker.

À noter encore une fois que nous n'écraserons pas votre ancien Kernel (safe zone), l'exemple est donc beaucoup plus parlant en utilisant votre machine ou une ancienne machine.

Afin de correspondre à une majorité de personne sous Linux, je vais jouer le rôle de l'initrd, et fournir des commandes pour Debian et toutes distributions qui héritent de Debian.
Par ailleurs, certains faits énoncés ne seront pas toujours vrais selon les distributions ainsi que les versions des distributions.
Je pars du principe que vous avez la version la plus naïve d'une distribution Debian. Donc il se peut, à votre avantage, que certaines manipulations ne soient pas à réaliser.

## Installer le nécessaire

### Pourquoi vous n'avez souvent pas l'environnement de compilation

Avant de compiler son noyau soit même, encore faut-il disposer d'un environnement propice à sa compilation. Mais bien souvent, vous n'avez pas cet environnement de compilation.
En effet, lorsque vous téléchargez une image bootable de votre future distribution, vous avez souvent le choix entre les architectures amd64 et x86. Cela correspond aux deux types
de processeurs majeurs du marché sur ordinateur standard.
Cependant il en existe d'autres pour des processeurs mais également des microcontrôleurs (les microcontrôleurs étant assimilables à des microprocesseurs de basse consommation énergétique) : arm, sparc, hppa, etc.
Dans la plupart des cas (sauf distributions isolées telles que Gentoo), le noyau ne sera donc pas compilé, mais sera copié depuis votre image ISO de la distribution que vous souhaitez utiliser.
D'où l'intérêt de choisir le type d'architecture et l'absence de l'environnement de compilation une fois la distribution installée.

### Le cas de la cross-compilation

Dans de très rares cas, la distribution que vous souhaiterez utiliser ne proposera pas votre type de microcontrôlleur ou microprocesseur.
C'est là qu'intervient là cross-compilation. Il s'agit d'utiliser une machine source, pour compiler un exécutable pour un autre type d'architecture cible, et d'aller *pusher* l'exécutable final dans la mémoire de la cible.
Ce n'est cependant pas le sujet de cet article tant cela est rare et destiné à des systèmes embarqués très particuliers.

### Let's go !

#### Récupérer le Kernel

Comme je souhaite que cet article soit le plus agnostique possible de la distribution utilisée, nous allons récupérer les sources directement depuis kernel.org, mais sachez que chaque distribution fourni en général un package
permettant de récupérer les sources de façon automatisée. Les manipulations que vous allez donc effectuer sont donc l'équivalent de ce que fait votre distribution quand vous lui dites "je veux installer les sources du kernel pour les compiler".
Sauf qu'ici nous allons outrepasser les sécurités d'usage de votre gestionnaire de paquet qui peuvent vous interdire d'installer la dernière version du noyau, car il n'a pas été validé par la Core Team de la distribution, c'est le cas des Debian-based distributions par exemple.

Lorsque j'écris ces lignes, le noyau est en version 4.x, plus précisément en 4.14.5. La liste est donc disponible ici : https://www.kernel.org/pub/linux/kernel/v4.x/. Exécutons les commandes suivantes :

```
$ sudo cd /usr/src
$ sudo wget -c --no-check-certificate https://www.kernel.org/pub/linux/kernel/v4.x/linux-4.14.5.tar.gz
$ sudo tar xvzf linux-4.14.5.tar.gz
```

En premier lieu, nous nous somme placé dans le répertoire conventionnel des sources du noyau linux (à priori il ne contient rien sauf pour certaines distributions).
À priori vous mettez les sources où vous le souhaitez (sauf dans /dev/null tant qu'à faire), mais /usr/src pour les puristes reste la zone où stocker les sources des différentes versions du kernel avec leurs configurations respectives.
J'irais même plus loin en vous disant : versionnez vos configuration de kernel avec git par exemple, mais cela fera un très bon sujet d'article de ma part sur la globalité de votre système.

Puis nous avons téléchargé l'archive contenant les sources du noyau (les plus récentes à l'écriture de ces lignes) pour finalement les extraire.
Obtenir les sources d'un noyau quelque soit la distribution en bypassant le gestionnaire de paquets, qui pourrait dire "non tu n'installes pas cette version, on est pas sûr que ça fonctionne avec notre distribution", c'est aussi simple que ça.

#### Récupérer les outils permettant de compiler le kernel

## Comprendre et apprendre de sa machine

### Installer les utilitaires nécessaires


### Exploiter LSPCI

### Google is your friend

## Choisir son type de configuration

"Il y a 10 types de personnes, celles qui configurent leur noyau à partir de la configuration de leur noyau actuel, et celles qui partent de la configuration de base imposée par les sources du Kernel pour la distribution voulue."

### A. Depuis une configuration "vierge"

Je dois reconnaitre ne pas connaître le sujet, mais j'imagine sans trop spéculer, que la configuration du Kernel Linux propose une configuration qui se doit d'être plutôt neutre par rapport à la distribution utilisée par dessus.
C'est je pense l'expérience la plus intéressante, mais également la plus chronophage qui soit. Sans parler de la frustration du nombre de reboots/boot USB + chroot pour tenter d'avoir quelque chose de fonctionnel.
Le but n'étant pas de vous dégouter de la compilation de kernel, cet article traîtera uniquement de l'option B.

### B. Depuis une configuration existante

C'est la solution la plus utilisée : partir d'une configuration de Kernel imposée par votre distribution, bien trop lourde, pour retirer l'inutile.
Par ailleurs, comme je l'expliquais, cet article reprends les concepts de Debian & Ubuntu, on se base sur un noyau assez gourmand.
Mais surtout pour pouvoir itérer en retirant des pilotes pas à pas (d'où encore, l'intérêt de versionner la configuration de son noyau).
Je recommande vivement cette méthode en faisant confiance à la configuration du Kernel proposée par votre distribution, qui est souvent en adéquation avec votre volonté de choisir cette distribution plutôt qu'une autre.

## make

