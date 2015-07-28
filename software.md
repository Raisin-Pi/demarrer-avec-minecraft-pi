# Installation de l'application

Minecraft est installé par défaut dans Raspbian depuis Septembre 2014.

![icône de bureau Minecraft Pi](images/minecraft-pi-shortcut.png)

Si vous utilisez une version antérieure de Raspbian, ouvrir une fenêtre de terminal et taper les commandes suivantes (vous devez être connecté à internet) :

```bash
sudo apt-get update
sudo apt-get install minecraft-pi
```

Une fois terminé, Minecraft Pi ainsi que la bibliothèque Python devraient être installés.

## Tester Minecraft

Pour lancer Minecraft, cliquez deux fois sur l'icône du bureau ou entrez `minecraft-pi` dans le terminal.

![](images/mcpi-start.png)

Une fois Minecraft Pi chargé, cliquez sur **Start Game**, puis sur **Create new**. Vous remarquez que la fenêtre conteneur est légèrement décalée. Ca veut dire que pour déplacer la fenêtre à l'écran vous devez attraper la barre de titre derrière la fenêtre Minecraft.

![](images/mcpi-game.png)

Vous êtes désormais dans un jeu de Minecraft !

## Tester Python

Avec Minecraft en cours d'exécution, et le monde crée, sortir du jeu en appuyant sur la touche `Tab`, ce qui va liberer votre souris de la fenêtre du jeu. Ouvrir IDLE (pas IDLE3) sur le Bureau et déplacer les deux fenêtres pour qu'elles soient côte à côte.

Vous pouvez soit taper vos commandes directement dans la fenêtre Python soit créer un fichier pour pouvoir enregistrer votre code et l'exécuter de nouveau le moment venu.

Si vous souhaitez créer un fichier, passez par `File > New window` and `File > Save`. Probablement vous devrez enregistrer ce fichier dans votre répertoire racine ou un nouveau dossier projet.

Commencez par l'importation de la bibliothèque Minecraft, en créeant une connexion avec le jeu et en le testant avec l'envoi d'un message "Hello world" vers l'écran :

```python
from mcpi import minecraft

mc = minecraft.Minecraft.create()

mc.postToChat("Hello world")
```

Si vous entres des commandes directement dans la fenêtre Python, il suffit de taper "Entrée" après chaque nouvelle ligne. Si c'est un fichier, enregistrez-le avec `Ctrl + S` et lancer avec `F5`. Quand votre code est exécuté, vous devez apercevoir votre message à l'écran dans le jeu.

![](images/mcpi-idle.png)

Si vous voyez "Hello world" dans la fenêtre Minecraft, vous pouvez enchaîner avec [la feuille d'activités](worksheet.md).
