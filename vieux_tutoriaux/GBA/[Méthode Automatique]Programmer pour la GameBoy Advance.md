<html>
<head>
<title>Préparations des outils pour programmer la GameBoy Advance(C)</title>
<meta author="LXS">
<meta description="Comment programmer la GameBoy Advance?">
</head>
<body>

<center>
<h1>Développer sur GameBoy Advance<br></h1>[Méthode automatique]<br>
<b>Remontez l'arborescence pour trouver la méthode manuelle!</b><br>
<tt>Par <a href="mailto:linuxshell@wanadoo.fr">Linuxshell</a></tt>
<br><br>
<table border=0 cellspacing=0 align="center" width=50%>
<td align="center">
Ce tutorial fait suite à celui de la méthode manuelle, en effet les outils présents sur le net<br>
se révèlent de meilleurs qualités qu'au début de l'aventure, de plus ils sont précompilés, pas<br>
besoin de connaitre les options de compilation de GCC, binutils,etc...que demander de plus?<br>
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
<hr><br>
<h2>Préparation de l'environnement de programmation.</h2>
Afin de créer des jeux pour Game Boy Advance, vous devrez compiler vos sources, tout comme dans<br>
la version manuelle du reste, et donc pour cela un compilateur. Il y a le compilateur ARM, mais<br>
payant, et comme nous pouvons nous en sortir aussi bien avec des logiciels libres, faisons-le!<br>
Ainsi nous nous servirons d'un kit non-officel de programmation pour Game Boy Advance, que nous<br>
téléchargerons sur <a href="http://www.io.com/~fenix/devkitadv/">http://www.io.com/~fenix/devkitadv/</a> rubrique Download.<br>
<br>
Admettons maintenant que vous développiez sous Windows, un tutorial Linux suivra pour les autres,<br>
bien que gnéralement ce genre de tutorial pour les *nixien ne soit pas indispensable :).<br>
Vous n'avez donc pas besoin de tout prendre, voici la liste des fichiers présentés:<br>
<p>
&nbsp;-&nbsp; agb-win-core-r5.zip : la 5ème version du coeur du compilateur, essentiel;<br>
&nbsp;-&nbsp; agb-win-binutils-r4.zip : nécessaire pour l'assemblage et le linkage, essentiel;<br>
&nbsp;-&nbsp; agb-win-gcc-r4.zip : compilateur GCC, essentiel! :)<br>
&nbsp;-&nbsp; agb-win-newlib-r4.zip : les librairies standards, utilesi vous ne voulez pas réinventer la roue;<br>
&nbsp;-&nbsp; agb-win-libstdcpp-r4.zip : les librairies C++, inutile si vous codez en C;<br>
&nbsp;-&nbsp; agb-win-patch-r4.zip : possibilité future de patcher ses créations?<br>
&nbsp;-&nbsp; interflip.zip : programme pour s'arranger avec le bit interwork dans les fichiers objets ARM ELF;<br>
&nbsp;-&nbsp; agb-source-r4.zip : pouvoir ajouter des fonctions à GCC...pas très utile pour le moment;
</p>
<br>
<h2>Dézipper tout ça... :)</h2>
<p>
Alors il parait(ici je me réferre à ce que j'ai lu ici et là), que le fonctionnement de tout ça peut poser<br> problème si ce n'est pas placer directement à la racine du système de fichier... autant sous Windows<br>
cela ne posera aucun problèmes, autant sous un système Unix(que ce soit Linux ou BSD le problème<br>
est le même=, vous n'aurez pas forcément accès à la racine de votre système...à part si bien sûr vous<br> êtes root enfin on verra ça dans le prochain tutorial :).<br>
Pour en revenir au dézippage de tout ça, vous devrez tout mettre dans C:\DevKitAdv.<br>
Et hop! Tous les outils sont fonctionnels sans toucher une seule ligne de code! :)
</p>
<br>
<h2>Tester  notre installation.</h2>
<p>
Ouvrez votre shell batch, puis tapez:<br>
&nbsp;-&nbsp; <b>PATH=c:\devkitadv\bin;%PATH%</b><br>
&nbsp;-&nbsp; <b>cd exemple</b><br>
&nbsp;-&nbsp; <b>make</b><br>
</p>
<p>La première ligne indique au shell où se trouve nos binaires comme GCC et make.</p>
<p>Ici normalement devrait s'afficher:<br>
&nbsp;-&nbsp; <b>gcc - mthumb-interwork -o GBADemo.elf main.o -lgcc -lm -lg -lstdc++</b>(1)<br>
&nbsp;-&nbsp; <b>Ended linking phase<br></b>
&nbsp;-&nbsp; <b>objcopy -O binary GBADemo.elf GBADemo.bin </b>(2)
</p>
<p>
Si jamais cela ne marche toujours pas, essayez de taper (1) puis (2) à la main.<br>
Puis is ça marche toujours pas je peux plus rien pour vous. :)
</p>
Donc qu'est-ce qu'ont a fait exactement?<br>
(1) : on appelle GCC par son nom(gcc), en lui demandant de compiler avec les instructions thumb,<br>
d'appeler le fichier de sortie GBADemo.elf, celui d'entrée main.o, avec les librairies gcc, C++, puis<br>
d'autres mais réferrez-vous directement à 'gcc --help' pour avoir des informations. :)
</p>
<h2>Conclusion.</h2><p>
Voilà la méthode 'automatique', la plus simple à quiconque veut se concentrer surce qu'il fait et non<br>
comment il le fait, dans les prochains tutoriaux nous verrons comment se débrouiller avec les Makefile,<br>
les même que ceux que l'ont appelle avec la commande 'make'.<br>
J'espère que ce tutorial pourra aider certains. ;)<br>
</p>
<p>Stay tuned!</p>
</body>
</html>
