# Getting Started with Minecraft Pi

Minecraft est un jeu populaire de type "bac à sable" en construction libre de mondes. Une version gratuite de Minecraft est disponible pour le Raspberry Pi; il est aussi fourni avec une interface de programmation. Ca veut dire que vous pouvez écrire de commandes et de scripts en code Python pour construire des choses automatiquement dans le jeu. C'est un moyen génial pour apprendre le langage Python !

![bannière Minecraft Pi](images/minecraft-pi-banner.png)

## Lancer Minecraft

Pour lancer Minecraft, cliquez deux fois sur l'icône du bureau ou entrez `minecraft-pi` dans le terminal.

![](images/mcpi-start.png)

Une fois Minecraft Pi chargé, cliquez sur **Start Game**, puis sur **Create new**. Vous remarquez que la fenêtre conteneur est légèrement décalée. Ca veut dire que pour déplacer la fenêtre à l'écran vous devez attraper la barre de titre derrière la fenêtre Minecraft.

![](images/mcpi-game.png)

Vous êtes maintenant dans un jeu de Minecraft! Allez-y, marchez, creusez des choses, et construisez des choses !

Utilisez la souris pour regarder autour de vous et utilisez les touches suivantes du clavier :

| Touche       | Action                 |
| :---:        | :-----:                |
| W            | Avancer                |
| A            | Gauche                 |
| S            | Reculer                |
| D            | Droit                  |
| E            | Inventaire             |
| Espace       | Sauter                 |
| Double Espace| Voler / Tomber         |
| Echap        | Pause / Menu du jeu    |
| Tab          | Libérer curseur du jeu |

Vous pouvez selectionner un article du panneau d'accès rapide avec la molette de la souris (ou utilisez les chiffres de votre clavier), ou appuyer sur `E` et selectionner autre chose de l'inventaire.

![](images/mcpi-inventory.png)

Vous pouvez aussi taper deux fois sur la barre d'espace pour s'envoler dans l'air. Vous arretez de voler en lachant la barre d'espace, et si vous la tapez deux fois encore vous retombez à terre.

![](images/mcpi-flying.png)

Avec l'épée à la main, vous pouvez cliquez sur les blocs devant vous pour les enlever (ou pour creuser). Avec un bloc à la main, vous pouvez utiliser un clic droit pour placer le bloc devant vous, ou un clic gauche pour enlever un bloc.

## Utilisation de l'interface de programmation Python

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

### Trouver votre position

Pour trouver votre position, taper :

```python
pos = mc.player.getPos()
```

`pos` contient désormais votre emplacement; accéder à chaque élément dans l'ensemble des coordonnées avec `pos.x`, `pos.y` et `pos.z`.

Autrement, une façon sympa de mettre les coordonnées dans des variables séparés est d'utiliser la technique de déballage dans Python :

```python
x, y, z = mc.player.getPos()
```

Maintenant `x`, `y`, et `z` contiennent chaque partie des coordonnées de votre position. `x` et `z` sont les directions de marche (avancer/reculer et gauche/droit) et `y` est haut/bas.

A noter que `getPos()` retourne l'emplacement du joueur à un temps donné, et si vous bouger de cet emplacement vous êtes obligé d'appeler la fonction de nouveau ou utiliser les valeurs de position déjà stockées.

### Téléportation

En plus de trouver votre position actuelle, vous pouvez aussi spécifier un emplacement vers lequel vous souhaiteriez être téléporté.

```python
x, y, z = mc.player.getPos()
mc.player.setPos(x, y+100, z)
```

Ca va transporter votre personnage à la hauteur de 100 espaces dans l'air. Ca veut dire que vous allez être téléporté en plein millieu du ciel avant de retomber à terre à votre point de départ.

Essayez d'être téléporté quelque part ailleurs !

### Fixer un bloc

Vous pouvez déposer un bloc unique à un endroit spécifié par des coordonnées avec `mc.setBlock()`:

```python
x, y, z = mc.player.getPos()
mc.setBlock(x+1, y, z, 1)
```

Maintenant un bloc de pierre devrait apparaître à coté de là où vous êtes. Si ce n'est pas directement devant vous ça peut être juste à coté ou derrière vous. Revenez dans la fenêtre Minecraft et utilisez la souris pour tourner sur place jusqu'au point où vous trouvez le bloc gris devant vous.

![](images/mcpi-setblock.png)

Les paramètres passés dans `set block` sont `x`, `y`, `z` et `id`. Le `(x, y, z)` concerne la position dans le monde (nous avons spécifié une distance d'un bloc de là où se trouve le joueur avec `x + 1`) et l' `id` concerne le type de bloc que nous voulons placer. `1` correspond à pierre.

D'autres blocs que vous pouvez essayer :

```
Air:   0
Herbe: 2
Terre:  3
```

Maintenant avec le bloc en pleine vue, essayez de le changer en autre chose :

```python
mc.setBlock(x+1, y, z, 2)
```

Vous devrez voir le bloc gris de pierre se transformer devant vos yeux !

![](images/mcpi-setblock2.png)

#### Des blocs comme des variables

Vous pouvez utiliser un variable pour stocker un ID afin de rendre le code plus lisible. Les IDs sont récupérés via `block`:

```python
from mcpi import block

dirt = block.DIRT.id
mc.setBlock(x, y, z, dirt)
```

Sinon si vous connaissez déjà l'ID, vous pouvez juste le paramétrer directement :

```python
dirt = 3
mc.setBlock(x, y, z, dirt)
```

### Des blocs spéciaux

Certains blocs posedent des paramètres supplémentaires, comme la laine qui a un paramètre supplémentaire pour spécifier la couleur. Pour la configurer, utilisez la quatrième valeur facultative dans `setBlock`:

```python
wool = 35
mc.setBlock(x, y, z, wool, 1)
```

Ici le quatrième paramètre `1` modifie la couleur de la leine en orange. Sans ce quatrième paramètre, la valeur reste par défaut (`0`) ce qui est blanc. Quelques couleurs disponibles sont :

```
2: Magenta
3: Bleu clair
4: Jaune
```

Essayez d'autres numéros et regardez comme le bloc change !

D'autres blocs qui ont des propriétés supplémentaires sont le bois (`17`): chène(oak), épicéa(spruce), bouleau(birch), etc; l'herbe haute (`31`): arbustre(shrub), herbe(grass), fougère(fern); flambeau (`50`): direction est, ouest, nord, sud; et autre. Voir la [référence API](http://www.stuffaboutcode.com/p/minecraft-api-reference.html) en anglais pour tous les détails.

### Fixer de multiples blocs

En plus de fixer un bloc unique avec `setBlock` vous pouvez remplir un volume dans l'espace tout en même temps avec `setBlocks` :

```python
stone = 1
x, y, z = mc.player.getPos()
mc.setBlocks(x+1, y+1, z+1, x+11, y+11, z+11, stone)
```

Un cube de 10 x 10 x 10 de pierre solide sera rempli.

![](images/mcpi-setblocks.png)

Vous pouvez créer de plus grands volumes avec la fonction `setBlocks` mais ça risque de prendre plus longtemps.

## Faire tomber des blocs en marchant

Maintenant que vous savez faire tomber des blocs, nous allons s'en servir pour faire tomber des blocs pendant que vous vous déplacez.

Le code suivant fera tomber une fleur derrière vous là où vous marchez :

```python
from mcpi import minecraft
from time import sleep

mc = minecraft.Minecraft.create()

flower = 38

while True:
    x, y, z = mc.player.getPos()
    mc.setBlock(x, y, z, flower)
    sleep(0.1)
```

Maintenant avancez une certaine distance et retournez-vous pour voir les fleurs que vous avez laissé derrière vous.

![](images/mcpi-flowers.png)

Puisque nous avons utilisé une boucle `while True` ça va continuer à l'infini. Pour l'arreter, tapez `Ctrl + C` dans la fenêtre Python.

Essayez de voler dans l'air et voir les fleurs que vous laissez dans le ciel :

![](images/mcpi-flowers-sky.png)

Et si on voulait simplement faire tomber des fleurs quand le joueur marche sur l'herbe ? Nous pouvons utiliser `getBlock` pour trouver quel type de bloc c'est :

```python
x, y, z = mc.player.getPos()  # player position (x, y, z)
this_block = mc.getBlock(x, y, z)  # block ID
print(this_block)
```

Ceci vous informe de la position du bloc *dans* lequel vous vous trouvez (ça sera `0` - un bloc d'air). Nous aimerions connaître le type de bloc *sur* lequel nous nous situons. Pour cela, nous devons soustraire 1 de la valeur de `y` et nous utilisons `getBlock()` afin de déterminer sur quel type de bloc on se trouve :

```python
x, y, z = mc.player.getpos()  # player position (x, y, z)
block_beneath = mc.getBlock(x, y-1, z)  # block ID
print(block_beneath)
```

Ceci nous retourne l'ID du bloc sous les pieds du personnage.

Testez en lançant une boucle qui imprime l'ID du bloc de votre position actuelle :

```python
while True:
    x, y, z = mc.player.getPos()
    block_beneath = mc.getBlock(x, y-1, z)
    print(block_beneath)
```

![](images/mcpi-block-test.png)

Nous pouvons utiliser une déclaration `if` pour choisir si on va planter une fleur ou pas :

```python
grass = 2
flower = 38

while True:
    x, y, z = mc.player.getPos()  # player position (x, y, z)
    block_beneath = mc.getBlock(x, y-1, z)  # block ID

    if block_beneath == grass:
        mc.setBlock(x, y, z, flower)
    sleep(0.1)
```

Peut-être nous pouvons ensuite transformer le carreau sur lequel on est debout en herbe si ce n'est pas déjà de l'herbe :

```python
if block_beneath == grass:
    mc.setBlock(x, y, z, flower)
else:
    mc.setBlock(x, y-1, z, grass)
```

Maintenant nous pouvons avancer et si nous marchons sur l'herbe on va laisser une fleur derrière nous. Si le prochain bloc n'est pas de l'herbe, ça se transforme en herbe. Quand nous faisons demi-tour et marchons dans l'autre sens nous laissons une fleur derrière nous.

![](images/mcpi-flowers-grass.png)

## Jouer avec des blocs de TNT

Un autre bloc intéressant est le TNT ! Pour placer un bloc normal de TNT, utilisez :

```python
tnt = 46
mc.setBlock(x, y, z, tnt)
```

![](images/mcpi-tnt.png)

Toutefois, ce bloc de TNT est assez anodin. Essayez d'appliquer la propriété `data` comme `1`:

```python
tnt = 46
mc.setBlock(x, y, z, tnt, 1)
```

Maintenant utilisez votre épée et faites clic droit sur le bloc de TNT : il sera activé et explosera au bout de quelques secondes !

Maintenant essayez de crée un cube géant de blocs de TNT !

```python
tnt = 46
mc.setBlocks(x+1, y+1, z+1, x+11, y+11, z+11, tnt, 1)
```

![](images/mcpi-tnt-blocks.png)

Alors vous allez voir un grand cube plein de blocs de TNT. Allez activer un des blocs et retirez vous vite pour regarder le spectacle ! Ca sera très lent pour rendre les grapiques car tant de choses sont en train de changer en même temps.

![](images/mcpi-tnt-explode.png)

## Et puis ?

Il y a énormément de choses que vous pouvez faire maintenant que vous maitrisez le monde de Minecraft et comment se servir de l'interface Python.

### Jeu en réseau

Si de multiples personnes connectent leurs Raspberry Pi sur un réseau local, ils peuvent se joindre au même monde Minecraft et jouer ensemble. Les joeurs peuvent se voir dans le monde de Minecraft.

### Référence API

Pour de la documentation plus approfondie au niveau des fonction et une liste complète des ID des blocs, regardez une référence de l'API à [stuffaboutcode.com](http://www.stuffaboutcode.com/p/minecraft-api-reference.html).

### Créer un jeu

Essayez une autre ressource et créer un jeu de Tape une Taupe : [Minecraft Whac-a-Block](https://www.raspberrypi.org/learning/minecraft-whac-a-block-game/).
