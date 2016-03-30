<html>
<head>
<title>Préparations des outils pour programmer la GameBoy Advance(C)</title>
<meta author="LXS">
<meta description="Comment programmer la GameBoy Advance?">
</head>
<body>

<center>
<h1>Développer sur GameBoy Advance<br></h1>[Méthode manuelle]<br>
<b>Remontez l'arborescence pour trouver la méthode automatique!</b><br>
<tt>Par <a href="mailto:linuxshell@wanadoo.fr">Linuxshell</a></tt>
<br><br>
<table border=0 cellspacing=0 align="center" width=50%>
<td align="center">
Salut à tous pour cette introduction au développement GBA. Nous commencerons ici par une préparation des logiciels indispensables à la programmation GBA. Plusieurs méthodes, plus ou moins simples mais également plus ou moins configurées, sont possibles pour le développement. Nous nous intéresserons dans un premier temps à la configuration manuelle, la plus appropriée à mon goût(bien que des solutions très simples soient possibles, mais nous le verront plus tard).
</td>
</table>
</center>
<br><br><br>
<table align=center bgcolor="gold" border=0 cellspacing=0>
<td>
<center><b>[Disclaimer On]</b></center><br>
Bien que nous essayons d'assurer la fiabilité de l'information figurant sur cette page,<br>
l'auteur (et l'hébergeur) ne peut être tenu pour responsable de quelque perte, dommage<br>
ou désagrément provoqué par le fait d'une erreur, d'une inexactitude ou d'une omission<br>
figurant sur ces pages.<br>
<b><center>[Disclaimer Off]</center></b><br>
</td>
</table>
<br><br><br>
<h3><u><font color="#1111ee">Table des matières</u></font></h3>
[Méthode manuelle]<br>
1. Préparation de l'environnement de programmation<br>
->1.1 Obtenir les outils<br>
->1.2 Compiler Binutils<br>
->1.3 Compiler GCC<br>
->1.4 Installer la Newlib<br>
->1.5 Recompiler GCC<br>
<br>
2. Utilisation de la LibGBA<br>
->2.1 Installer la LibGBA<br>
->2.2 Tester les exemples fournis<br>
<br><br><hr><br>
<h4>Préparation de l'environnement de programmation</h4>
<b>1.1 Obtenir les outils</b><br>
Tout comme de nombreuses machines, telles la Dreamcast, la Saturn, etc..., la GBA peut se<br>
programmer via les outils gratuits distribués par GNU. Vous aurez besoin de télécharger:<br>
-binutils<br>
-gcc<br>
-newlib<br>
Une fois les outils obtenus, il nous faut configurer l'environnement, j'utiliserai des dossiers<br>
que vous pouvez tout à fait remplacer pour que cela convienne à votre machine:<br>
-répertoire où se trouvent les sources: $SRC = /usr/local/gbadev/src<br>
-répertoire destination des binaires: $BIN = /usr/local/gbadev<br>
-répertoires donc on aura besoin ultérieurement:<br>
# mkdir binutils-build<br>
# mkdir gcc-build<br>
# mkdir newlib-build<br>
<br>
<b>1.2 Compiler Binutils</b><br>
Tout d'abord il vous faudra compiler Binutils. Ce dernier contient les outils tels que l'assembleur,<br>
le débuggeur, et les outils différents nécessaires à la programmation(pas seulement la programmation<br>
GameBoy Advance mais aussi la programmation sur PC, Dreamcast, etc...).<br>
Voilà le directives:<br>
# cd $SRC<br>
# tar zxvf binutils-X-XXX<br>
# cd binutils-build<br>
# ../binutils-X-XXX/configure --prefix=$BIN --target=arm-thumb-elf<br>
# make; make install<br>
# cd ..<br>
# rm -rf binutils-build/ binutils-X-XXX/<br>
<br>
Normalement jusque là pas de problème...ou du moins j'espère pas ;)<br>
<br>
<b>1.3 Compiler GCC</b><br>
C'est ici que les choses se corsent! Explications plus en détails:<br>
La compilation de GCC produira une erreur(!). Car à la fin de la compilation de celui-ci, les Makefiles<br>
produiront un test pour vérifier si le compilateur peut produire un exécutable, or afin d'en créer un,<br>
il faut que la newlib soit installée d'abord! Mais pour compiler la newlib, il nous faut GCC!!<br>
Nous procéderons alors quand même à une installation afin d'arriver à nos fins...<br>
<br>
# export PATH=$PATH:$BIN/bin<br>
# cd $SRC<br>
# tar zxvf gcc-X-XXX<br>
# cd gcc-build<br>
# ../gcc-X-XXX/configure --prefix=$BIN --target=arm-thumb-elf<br>
# make; make install<br>
# cd ..<br>
Mais on efface pas tout cette fois-ci.<br>
<br>
<b>1.4 Installer la Newlib</b><br>
Installons à présent la Newlib ;)<br>
# cd $SRC<br>
# tar zxvf newlib-X-XXX<br>
# cd newlib-build<br>
# ../newlib-X-XXX/configure --prefix=$BIN --target=arm-thumb-elf<br>
# make; make install<br>
# cd ..<br>
# rm -rf newlib-build/ newlib-X-XXX/<br>
<br>
On commence à en avoir l'habitude de cette procédure... ;)<br>
<b>1.5 Recompiler GCC</b><br>
Maintenant que la Newlib est installée, nous allons recommmencer la construction d'un compilateur<br>
propre. Toute erreur qui surviendrait à présent, n'est pas attendue.<br>
# cd $SRC<br>
# cd gcc-build<br>
# make; make install<br>
# cd ..<br>
# rm -rf gcc-build/ gcc-X-XXX/<br>
<br><br><br>
<h4>Utilisation de la LibGBA</h4>
<b>2.1 Installer la LibGBA</b><br>
Après avoir décompresser la libgba dans le répertoire de votre choix(ici $BIN/src), nous nous confrontons<br>
au premier problème, celui du compilateur. En effet notre compilateur compile(normal :)) pour arm-thumb-elf,<br>
et l'auteur de la lib a un compilateur arm-elf. En fait nous pouvons compiler en arm-thum_elf grâce à l'option<br>
-mthumb dans un shell.<br>
<br>
Donc ligne 4 de $BIN/src/libgba/Makefile:<br>
AS = arm-thumb-elf-as -marm7<br>
CC = arm-thumb-elf-gcc<br>
AR = arm-thumb-elf-ar<br>
<br>Et la compilation se fait désormais sans aucun problèmes.
<br><br>
<b>2.2 Tester les exemples fournis</b><br>
Pour pouvoir profiter de nos créations sur un bel émulateur, il faut traduire le binaire elf en exécutable GBA.<br>
Et encore une fois nous n'avons pas la bonne ligne de commande dans le script approprié! :)<br>
Modifions ligne 6 de $BIN/src/libgba/extra/gba-elf2bin:<br>
OBJCOPY=arm-thumb-elf-objcopy<br>
<br>C'est fini!<br>
<br>
Modifions à présent les Makefile pour les accorder avec nos choix...<br>
Ligne 4 de $BIN/src/libgba/tests/Makefile:<br>
CRT = $BIN/src/libgba/extra/crt0/gbacrt0.o<br>
LDSCRIPT = $BIN/src/libgba/extra/gba.ld.Script<br>
(Note: Si $BON n'existe pas remplacer le par le chemin entier...)<br>
<br>
Ligne 8 de $BIN/src/libgba/extra/gba-elf2bin:<br>
AS = arm-thumb-elf-as -marm7<br>
CC = arm-thumb-elf-gcc<br>
ELF2BIN = $BIN/src/libgba/extra/gba-elf2bin<br>
<br>
Tout compile dès à présent.<br>
<br>
Reste plus qu'à mettre ça dans un émulateur ou sur un linker et faire partager le fruit de votre labeur :)<br>
<br>
Bon code et à bientôt!



</body>
</html>
