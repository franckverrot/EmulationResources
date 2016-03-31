<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=iso-8859-1">
<title>Création d'un émulateur - Partie i : Choix de la technique d'émulation </title>
<meta author="LXS">
<meta description="Programmer un émulateur">
</head>
<body>

<center>
<h1><font color='#ee0000'>Création d'un émulateur - Techniques d'émulation.</font><br></h1>
<tt>Par <a href="mailto:linuxshell@wanadoo.fr">Linuxshell</a></tt>
<br><br>
</center>

<u><h2>I - Considérations sur l'émulation:</u></h2>
<p>Afin de parvenir à émuler un système, quelqu'il soit, la première chose à faire<br>
est de choisir la ou les méthodes qu'il va falloir utiliser afin de mener le<br>
projet à bien, et bien entendu choisir la ou les plus adaptées.<br>
En effet, dans la mesure où chaque processeur dispose d'un environnment, ou<br>
d'un contexte extérieur différent, les possibilités d'émulation sont donc aussi<br>
variées et différentes.<br>
L'émulation comporte plusieurs méthodes, toutes résultantes sur le but fixé qui<br>
est la simulation logicielle de la machine visée, avec chacune leurs atouts, et<br>
bien évidemment leurs faiblesses.</p>

<u><h2>II - Survol rapide des techniques dominantes:</u></h2>
<p>A l'heure actuelle, et probablement à jamais, il existe qu'un nombre limité de<br>
techniques d'émulation, l'interprétation basique(fetch-decode loop), la recompilation<br>
statique et la recompilation dynamique. Certains émulateurs mixent ces deux dernières afin de<br>
correctement associer les performances différents de ces techniques.<br>
On peut voir ainsi Project64 qui allie un recompilateur statique avec un recompilateur dynamique,<br>
ainsi que Satourne, ave un recompilateur dynamique pour SH2.</p>
<p><b>Interprétation linéaire:</b><br>
L'interprétation linéaire d'une rom est une boucle simple, qui va lire instruction par instruction<br>
une rom, et qui va la décoder au fur et à mesure des opcodes lus. On peut donc comparer cette technique<br>
à une interprétation d'un langage très haut niveau(le langage machine du système cible).</p>
<p><b>Recompilation statique:</b><br>
La recompilation statique va permettre de traduire les instructions de la machine à émuler en instructions<br>
compréhensibles par le système cible(par exemple: assembleur Z80 -> assembleur x86).<br>
Le défaut de ce système est que la recompilation va être bête et méchante, car la lecture totale de la ROM<br>
se fera, et par là la traduction de toutes les données se fera...ce qui est à éviter absolument car décoder<br>
des données qui contenaient des sprites va être plus que nuisible...<br></p>
<p><b>Recompilation dynamique:</b><br>
La recompilation dynamique est la plus dure à metre en oeuvre, mais aussi la plus puissante et la plus rapide.<br>
Les gains de performances par rapport à tout autre technique d'émulation sont énormes, on a d'ailleurs vu Satourne<br>
passer de 5-6fps en interprétation linéaire à 60fps en recompilation dynamique...ça laisse rêveur! :)<br>
Le problème se situe ici sur les blocs à lire, et la possible mutation du code en cours d'éxécution.<br>
Il n'est donc pas rare de voir l'association de plusieurs techniques afin de pallier à ces défauts...</p>

<u><h2>III - Conclusions:</u></h2>
<p>La technique d'émulation à choisir ne dépendra uniquement que du système à émuler, nul besoin d'utiliser un<br>
recompilateur dynamique pour émuler un petit chip, alors que les bienfaits de cette technique se sont révélés, sur<br>
des gros processeurs comme le SH2, bien en dessus de toute attente.<br></p>
<p>
Ainsi, pour un premier projet d'émulation(voire même pour tous les projets que vous auriez), il se peut que l'interprétation<br>
linéaire soit un très bon choix, du moment que vous optimisiez un tant soit peu votre boucle de décodage des instructions.<br>
</p>


</body>
</html>
