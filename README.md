<p align="center">
  <img width="460" height="300" src="https://angristan.fr/wp-content/uploads/2017/08/arch_linux-1038x576.png">
</p>

# Guide d'installation ArchLinux UEFI/BIOS, et installation de l'environnement i3/Cinnamon/Mate

Bonjour, aujourd'hui je vous propose mon guide afin d'installer Arch linux et les environnements de votre choix.
Ce guide est accessible seulement à des utilisateurs de linux ayant beaucoup d'experience en bash. Pour le premier guide je ne 
mettrais pas encore l'UEFI mais ça viendra je mettrais les commandes qui sont différentes pour l'UEFI et le partitionnement

## Lancement de l'installation

Vous allez simplement lancer l'installation via votre menu boot de votre système, je pars du principe que vous savez comment ça se passe je vous met simplement un screen du
menu pour lancer l'installation

<p>
    <img width="460" height="300" src="https://cdn-02.memo-linux.com/wp-content/uploads/2016/02/archlinux-01.png">
</p>

Vous avez simplement à lancer le boot sur Arch x86

## Mettre son clavier dans la langue de son choix

Une fois que vous êtes sur le terminal de l'iso arch on va simplement changer le langage de notre clavier via cette commande

```bash
loadkeys fr
```

## Partitions

Tout d'abord il va falloir crée plusieurs partitions
    - Boot
    - Swap
    - Root
    - Home

Nous allons checker notre peripherique via la commande

### Lister les périphériques
```
fdisk -l
```

<img width="460" height="300" src="https://imgur.com/lKr7u6m.jpg">

### Creation des partitions
Ensuite nous allons lancer cfdisk en dos pour partitionner notre disque via 

```
cfdisk /dev/sda
```

<img width="460" height="300" src="https://imgur.com/AIAzTrc.jpg">

Voici à quoi ressemble le partitionnement du disque

<img width="460" height="300" src="https://imgur.com/5qtvTxH.jpg">

Tableau de partitionnement:


<table class="tg">
  <tr>
    <th class="tg-yw4l"><b>Name</b></th>
    <th class="tg-yw4l"><b>Type</b></th>
    <th class="tg-yw4l"><b>Size</b></th>
    <th class="tg-yw4l"><b>Bootable</b></th>
  </tr>
  <tr>
    <td class="tg-yw4l">/dev/sda1</td>
    <td class="tg-yw4l">Boot</td>
    <td class="tg-yw4l">666MIB</td>
    <td class="tg-yw4l">*</td>
  </tr>
  <tr>
    <td class="tg-yw4l">/dev/sda2</td>
    <td class="tg-yw4l">Swap</td>
    <td class="tg-yw4l">5G</td>
    <td class="tg-yw4l"></td>
  </tr>
  <tr>
    <td class="tg-yw4l">/dev/sda3</td>
    <td class="tg-yw4l">Root</td>
    <td class="tg-yw4l">15GIB</td>
    <td class="tg-yw4l"></td>
  </tr>
  <tr>
    <td class="tg-yw4l">/dev/sda4</td>
    <td class="tg-yw4l">Home</td>
    <td class="tg-yw4l">Reste</td>
    <td class="tg-yw4l"></td>
  </tr>
  
</table>

### Creation d'une zone d'echange

Afin de crée une zone d'echange sur le périphérique nous allons utiliser

```
mkswap /dev/sda2 
```
pour crée une zone d'échange sur notre swap ainsi nous pouvons l'utiliser via

```
swapon /dev/sda2
```

### Formate des partitions

Nous allons formater les partitions root, boot, home

```
mkfs.ext4 /dev/sda1
mkfs.ext4 /dev/sda3
mkfs.ext4 /dev/sda4
```
<img width="460" height="300" src="https://imgur.com/KqeEYJm.jpg">

### Montage de nos partitions

Nous allons maintenant monter nos partitions root, home, boot afin de crée nos dossier systeme boot et home et root donc la racine

```
mount /dev/sda3 /mnt
mkdir /mnt/home
mkdir /mnt/boot
mount /dev/sda1 /mnt/boot
mount /dev/sda4 /mnt/home

```

## Configuration du mirroir

Tout d'abord afin d'accèder aux mirror list voici le path <b>/etc/pacman.d/mirrorlist</b>

Nous allons éditer le fichier de configuration

```
nano /etc/pacman.d/mirrorlist
```

par défaults tout les mirrors sont séléctionné

<img width="460" height="300" src="https://imgur.com/3wAKPBp.jpg">

Nous allons utiliser le mirroir labrusse car il est rapide 

Pour ce faire vous allez devoirs faire les manipulation suivantes afin de désactiver tout les mirroirs


Vous allez appuier sur <b>ALT+R</b> ensuite dans l'input vous écrivez <b>Server</b> puis vous faites <b>ENTRER</b>
Ensuite tout les serveurs seront désactiver afin d'activer celui qui nous intéresse nous allons faire <b>CTRL+W</b>,
dans l'input vous écrivez <b>labrusse</b> et vous faites <b>ENTRER</b>,
puis vous enlever le diez juste devant Server comme ceci:

<img width="460" height="300" src="https://imgur.com/QiluJxk.jpg">

Et pour en finir avec les mirroirs vous faites <b>CTRL+X</b> puis <b>Y</b>




## Authors

* **Muham'RB** - *Initial work* - [MuhamRB](https://github.com/MuhamRb)



## License

This project is licensed under the MIT License - see the [LICENSE.md](LICENSE.md) file for details

