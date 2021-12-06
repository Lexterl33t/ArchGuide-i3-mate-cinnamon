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
    <td class="tg-yw4l">512MIB</td>
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


## Installation de la base

Nous allons maintenant installer la base de arch via pacstrap dans le dossier /mnt

```
pacstrap /mnt base base-devel pacman-contrib
```

Puis nous allons installer des outils de base de arch tel que alsa ou zip unzip etc

```
pacstrap /mnt zip unzip p7zip vim mc alsa-utils syslog-ng mtools dosfstools lsb-release ntfs-3g exfat-utils bash-completion
```

## Générer le fichier /etc/fstab

Nous allons devoir générer le fichier <b>/etc/fstab</b> afin de lister les partitions

```
genfstab -U -p /mnt >> /mnt/etc/fstab
```

## Installation de grub

Nous allons installer grub et os-prober qui permet à deux OS de cohéxister

```
pacstrap /mnt os-prober grub
```

## Passage en archroot

Nous allons devoir passer en archchroot afin de rentrer dans l'os

```
arch-chroot /mnt
```

### Configuration du clavier

Nous allons éditer le fichier <b>/etc/vconsole.conf</b> afin de lui dire que l'on veut mettre notre clavier en Fr

```
nano /etc/vconsole.conf
```

Dedans vous écrivez:

```
KEYMAP=fr-latin9
FONT=eurlatgr
```

<img width="460" height="300" src="https://imgur.com/W67lHy6.jpg">

<b>
Si lorsque vous mettez nano /etc/vconsole.conf cela vous dis que "nano command not found" il faut simplement l'installer
via 
</b>

```
pacman -S nano
```

### Configuration de la localisation

Nous allons maintenant configurer votre emplacement géographique je suis en france donc je vais me mettre en Europe/Paris

```
nano /etc/locale.conf

```

Vous écrivez dedans

```
LANG=fr_FR.UTF-8
LC_COLLATE=C
```

ensuite
```
locale-gen
```

### Hostname

Nous allons crée le nom de notre machine en créant le fichier <b>/etc/hostname</b>

```
nano /etc/hostname
```

Et puis vous écrivez simplement votre nom d'utilisateur comme ceci:

<img width="460" height="300" src="https://imgur.com/bfmMQBv.jpg">


### Configuration du fuseaux horraire

Afin de configurer votre fuseau horraire vous avez simplement à faire cela

```
ln -sf /usr/share/zoneinfo/Europe/Paris /etc/localtime
```

Pour configurer l'heure faites simplement cela

```
hwclock --systohc --utc
```

## Kernel

### Génerer l'image du kernel

Nous allons génerer l'image du kernel via 

```
mkinitcpio -p linux
```

Si vous avez "command not found" vous devez installer mkinitcpio

```
pacman -S mkinitcpio
```

Et si il ne trouve pas le kernel vous devez l'installer idem

```
pacman -S linux
```

<img widht="460" height="300" src="https://imgur.com/RwF3EEd.jpg">

## Grub

Nous mettons en place le grub

```
grub-install --no-floppy --recheck /dev/sda
```

```
grub-mkconfig -o /boot/grub/grub.cfg
```

Si vous avez une erreur pas de panique cela marchera

## Configuration de l'utilisateur root

Nous allons définir un mot de passe root via la commande

```
passwd root
```


## Configuration gestionnaire reseau

Nous allons installer networkmanager

```
pacman -S networkmanager
```

puis l'activer

```
systemctl enable NetworkManager
```

<img width="460" height="300" src="https://imgur.com/ky8s4G1.jpg">


Nous avons à présent terminé l'installation de Arch linux voici les derniere commandes à effectuer

```
exit
umount -R /mnt
reboot
```



# Environnement de I3

## Installation de i3

  Pour installer i3 nous allons simplement faire
  
  ```
  pacman -S i3
  ```

  et vous ecrivez 1 3 4 5 comme ceci

  <img width="480" height="320" src="https://imgur.com/yNJ3q0C.jpg">


  Ensuite nous allons crée le fichier .xinitrc

  et écrire cela dedans

  ```
  nano .xinitrc
  ```

  Ensuite vous mettez dedans 

  ```
  exec i3
  ```
  
  Puis vous sauvegardez

  Ensuite vous faites:
  ```
  startx
  ```

  Et si vous avez un "command not found"

  installez xorg-xinit

  ```
  pacman -S xorg-{xinit,server,apps}
  ```

  puis 

  ```
  startx
  ```

  Et vous serez sur i3
  comme ceci

  <img width="500" height="600" src="https://imgur.com/2ZBB55O.jpg">

  ## Outils de base afin que vous puissiez utiliser votre environnement

  Afin que vous puissiez installer vos programme sur votre environnement
  je vous conseil d'installer un terminal voici une liste de commande
  pour installer des terminal

  XFCE

  ```
  pacman -S xfce4-terminal
  ```

  DEEPIN

  ```
  pacman -S deepin-terminal
  ```

  XTERM

  ```
  pacman -S xterm
  ```

  # Cinnamon mise en place

  Nous allons mettre à présent en place Cinnamon qui selon moi est le meilleur environnement je vais vous placer un screen
  de mon environnement débian sur arch linux

  <img width="700" heigt="400" src="https://imgur.com/mfSKf8C.jpg">

  ## Installation de cinnamon

  Même procédure

  ```
  pacman -S cinnamon-session
  ```

  éditer le fichier .xinitrc et remplacer exec i3 par

  ```
  exec cinnamon-session
  ```

  Puis sauvegarder et faites

  ```
  startx
  ```

  

## Authors

* **Muham'RB** - *Initial work* - [MuhamRB](https://github.com/MuhamRb)



## License

This project is licensed under the MIT License - see the [LICENSE.md](LICENSE.md) file for details

