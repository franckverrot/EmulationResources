<html>
<head>
<title>Préparation de l'environnement de développement Dreamcast(C)</title>
<meta author="LXS">
<meta description="Comment programmer la Dreamcast?">
</head>
<body>

<center>
<h1>Développer sur Dreamcast<br></h1>[Méthode automatique]<br>
<b>Bientôt la version manuelle.</b><br>
<tt>Par <a href="mailto:linuxshell@wanadoo.fr">Linuxshell</a></tt>
<br><br>
<table border=0 cellspacing=0 align="center" width=50%>
<td align="center">
Ce tutorial est le premier de ceux qui seront consacrés à la Dreamcast, en effet, nous<br>
commenceront par la prépation de l'environnement de développement pour évoluer à la création<br>
d'un programme sur cette console. Les outils utilisés ici sont libres donc tant que nous les<br>
utilisons, tous nos programmes seront légaux et resdistribuables sur le net.<br>
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
<p>Tout d'abord quelques choses sont nécessaires ici, même indispensable. A contrario de la<br>
programmation sur GameBoyAdvance, celle  sur Dreamcast nécessitera(pour l'instant) obligatoirement<br>
une Dreamcast, alors pour que la GBA nous pouvions nous contenter d'un émulateur pour tester nos
binaires. Donc voici la liste des requis, que nous acquérrons pendant ce tutorial:</p>
<p>
&nbsp;-&nbsp; Une Dreamcast;<br>
&nbsp;-&nbsp; Un cable DC<=>PC ou un Broad Band Adapter pour DC;<br>
&nbsp;-&nbsp; Cygwin;<br>
&nbsp;-&nbsp; CD qui rendra la Dramcast esclave de votre PC;<br>
&nbsp;-&nbsp; Un loader qui enverra les données à la Dreamcast;<br>
&nbsp;-&nbsp; les outils précompilés;<br>
&nbsp;-&nbsp; KOS;<br>
&nbsp;-&nbsp; des connaissances en C;<br>
</p>
<p>
Pour ce qui est de Cygwin si vous avez lu les autres tutoriaux vous devriez déjà l'avoir :),<br>
sinon téléchargez le sur <a href="http://cygwin.redhat.com" target="_blank">http://cygwin.redhat.com</a>.<br>
La dreamcast s'achète n'importe où(ou presque) pour une bouchée de pain à l'heure actuelle, et le cable ainsi<br>
que le BBA se trouvent sur <a href="http://www.liksang.com" target="_blank">Liksang</a> et <a href="http://www.fl-games.com" target="_blank">Fl-games</a>.<br>
Pour KOS, visitez  <a href="http://cvs.sourceforge.net/cgi-bin/viewcvs.cgi/cadcdev/kos/kos/" target="_blank">leur CVS</a>.<br>
Pour les outils précompilés, nous les
téléchargerons sur <a href="http://dev.dcemulation.com" target="_blank">http://dev.dcemulation.com</a>.<br>
Puis enfin pour le CD Slave allez surle site de Marcus Comstedt <a href="http://mc.pp.se/dc/serslave.html" target="_blank">http://mc.pp.se/dc/</a>.<br>
</p>
<br>
<h2>Installer le matériel et configurer les logiciels.</h2>
<p>
Branchez votre Dreamcast à votre télévision normalement puis rapprochez tout ça du PC, branchez votre câble<br>
sur le PC puis sur la Dreamcast.
Pour les logiciels installez Cygwin, puis dézippez les outils dans /usr/local/dc si le répertoire n'est pas<br>
déjà configuré ainsi. Ensuite créez un fichier Dev-SH4.bat dans la racine(c:\cygwin par exemple):
</p>
<p><b>@ECHO OFF<br>
SET MAKE_MODE=UNIX<br>
SET PATH=C:\CYGWIN\BIN;%PATH%<br>
SET PATH=C:\CYGWIN\lib\gcc-lib\i686-pc-cygwin\2.95.2;%PATH%<br>
SET PATH=C:\CYGWIN\USR\LOCAL\BIN;%PATH%<br>
BASH<br>
</b></p>
<p>
Puis placez par exemple dans votre /usr/local/ le dossier complet de KOS.<br>
Puis lancez le fichier batch, ensuite tapez "<b>cd usr/local/kos</b>". Editez le environ-dc.sh pour qu'il<br>
reflète votre installation puis lancez le de cette manière "<b>. ./environ-dc.sh</b>".<br>
Attention aux "."(point) qui servira à dire au shell qu'il faille que notre script soit exécuté dès que le shell<br>est lancé, ceci nous évitera de retaper cela toutes les fois, nous aurions pu aussi le placer dans notre
bashrc.<br>
Ensuite dirigez-vous dans votre répertoire kos/examples puis tapez make. Tout devrait compiler normalement.<br>
Ceci produire des fichiers .bin dan chaque dossier. Pour les transformer en srec:</p>
<p>BASH-2.05a$ <b>/usr/local/dc/sh-elf/bin/sh-elf-objcopy.exe -O srec fichier.elf fichier.srec</b></p>
Lancez le CD Slave dans la Dreamcast puis attendez d'avoir les bords bleus, puis lancez le loader depuis windows<br>
avec le fichier produit. Et voilà l'application.</p>
<p>Dès que je rentrerai de vacances suivra un tutorial où nous verrons en détail comment profiter de notre environnement<br>
et créer un premier programme avec notre propre code :)</p>
<p>Stay tuned!</p>
</body>
</html>
