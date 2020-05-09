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

## Créations des partitions

Tout d'abord il va falloir crée plusieurs partitions
    - Boot
    - Swap
    - Root
    - Home

Nous allons checker notre peripherique via la commande

```
fdisk -l
```

<img width="460" height="300" src="https://imgur.com/lKr7u6m.jpg">

Ensuite nous allons lancer cfdisk en dos pour partitionner nos disque via 

```
cfdisk /dev/sda
```

<img width="460" height="300" src="https://imgur.com/AIAzTrc.jpg">

Voici à quoi ressemble le partitionnement des disque

<img width="460" height="300" src="https://imgur.com/5qtvTxH.jpg">

Tableau de partitionnement:

/***
| Name | Type | | Size |
| --- | --- | | --- |
| /dev/sda1 | Boot | | 666MIB |
| /dev/sda2 | Swap | | 5GIB |
| /dev/sda3 | Root | | 15GIB |
| /dev/sda4 | Home | | Reste du disque |

/***

<table class="tg">
  <tr>
    <th class="tg-yw4l"><b>Animals</b></th>
    <th class="tg-yw4l"><b>Sports</b></th>
    <th class="tg-yw4l"><b>Fruits</b></th>
  </tr>
  <tr>
    <td class="tg-yw4l">Cat</td>
    <td class="tg-yw4l">Soccer</td>
    <td class="tg-yw4l">Apple</td>
  </tr>
  <tr>
    <td class="tg-yw4l">Dog</td>
    <td class="tg-yw4l">Basketball</td>
    <td class="tg-yw4l">Orange</td>
  </tr>
</table>

## Authors

* **Muham'RB** - *Initial work* - [MuhamRB](https://github.com/MuhamRb)



## License

This project is licensed under the MIT License - see the [LICENSE.md](LICENSE.md) file for details

