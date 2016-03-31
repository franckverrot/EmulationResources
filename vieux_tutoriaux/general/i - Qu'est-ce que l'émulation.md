<html>
<head>
<title>Qu'est-ce que l'émulation?</title>
<meta http-equiv="Content-Type" content="text/html; charset=iso-8859-1">
</head>
<body bgcolor="#bbccdd">
<center><h1><font color="#ff0000">Qu'est-ce que l'émulation?</font></h1>
<tt>Un tutorial par <a href="mailto:linuxshell@wanadoo.fr">Linuxshell</a>
<br>Téléchargé sur <a href="http://linuxshell.free.fr/">LXS_*</a></tt></center>
<br>
<center><h3>Partie 1 : Notions nécessaires</h3></center>
<br>
// Dernière version le :13/07/2002<br>
<br>
<br><hr>
<b>Table des matières:</b>
<ol type="i">
    	<li>Introduction;</li>
    	<li>Un point sur le vocabulaire;</li>
    	<li>Les différentes architectures;</li> 
		<li>Comment fonctionne une ROM?</li>
    </ol>
<hr>
<br>
<h4>i. Introduction;</h4>
Avant toute chose prennons conscience du mot émulation, il s'agit de simuler le comportement d'une machine sur une autre, d'un x86<br>
sur un Alpha, d'un Z80 sur un SH4, etc...donc tout appareil électroniquement parlant est susceptible de se voir émuler(avec toutefois<br>
un engouement plus faible ailleurs que dans le domaine du jeux vidéo).<br><br>
Donc l'émulateur se doit de respecter le mieux possible les comportements de la machine émulée, afin de pouvoir reproduire de manière<br>
exacte la moindre opération. Bien entendu faire un émulateur aussi rigoureusement relève de l'impossible étant donné que les documentations<br>
des constructeurs de composants sont difficiles à trouver, voire disparues pour certaines...<br><br>
Donc la tâche première du programmeur est de chercher le maximum de documentation sur la machine concernée, de pouvoir la comprendre<br>
pour analyser les données et pouvoir trier les informations, et une fois qu'il aura estimé l'émulation possible(si possible intégralement, donc<br>
graphique et sonore) il se mettra alors à coder.<br><br>
Mais le fait d'être fan est indispensable, surtout pour les gros projets, car les nombreuses heures passées devant l'écran rebuteront vite<br>
les moins fans :) .<br><br>
Il existe des processeurs assez simples à émuler, comme le Chip 8 que nous verrons ultérieurement dans ce tutorial puis dans les suivants<br>
qui ne nécessiteront pas beaucoup de temps, malheureusement ce dernier est vraiment incomplet niveau documentation et beaucoup de choses<br>
vont être improvisées...<br><br>
Je tiens à rermercier tout d'abord Marat Fayzullin, pour son travail remarquable sans qui l'émulation ne serait pas ce qu'elle est aujourd'hui,<br>
emulation.fr,Planet Emulation, #emulation et #emulation-fr sur Undernet, ainsi que ma copine pour supporter mes longues nuits avec...mon PC :'(.
<br>
<br>
<br>
<br>
<h4>ii. Un point sur le vocabulaire;</h4>
<b>Dans l'ordre anarchiquement alphabétique:</b><br><br>
&nbsp-&nbsp <b>Bit:</b> plus petite donnée du monde :). Il est binaire, soit 0 soit 1.<br>
&nbsp-&nbsp <b>Word:</b> 2 octets, soit 16 bit.<br>
&nbsp-&nbsp <b>Double Word(DWORD):</b> deux words :)<br>
&nbsp-&nbsp <b>Opcode:</b> instruction machine, de longueur fixe sur les processeurs de type CISC, et de taille variable sur les architectures RISC.<br> 
&nbsp-&nbsp <b>Tick:</b> coup unique d'horloge, 500Mhz = 500*10^6 ticks/s<br>
&nbsp-&nbsp <b>ROM:</b> Read Only Memory, ou mémoire morte en français, on ne peut écrire dessus, elle contient toutes les données nécessaires<br>
au fonctionnement d'un jeu, les instructions, les graphismes, etc...<br>
&nbsp-&nbsp <b>Offset:</b> décalage en français, en effet l'offset 2(par exemple), commence au deuxième octet.<br>
<!-- &nbsp-&nbsp <b>:</b><br> -->
<br>
D'autres seront ou sont rajoutées au cours des éditions :)<br>
<br>
<br>
<br>
<h4>iii. Les différentes architectures;</h4>
Il existe deux grandes familles de processeurs, les RISC, et les CISC, ceux qui ont leurs instructions de taille fixe, comme les MIPS, les Alpha<br>
ou encore les SH4, et ceux avec des instructions à taille variable, comme le x86.<br>
Nous ne rentrerons dans les détails qu'à la fin, il sera alors plus facile de comprendre le concept :).<br>
<br>
La différence fondamentale entre les CPU est donc le jeu d'instruction(Instruction Set) disponible dessus, celui-ci n'est pas modifiable, à part<br>
pour le fabricant bien entendu, chauqe instruction sera appellée par le suite opcode(OPerationCODE).<br>
Sur le Chip-8 par exemple, les instructions sont codés sur un WORD(16bits), ce qui ferait au maximum 2^16 = 65536 opérations possibles.<br>
Il n'en existe pourtant pas autant mais la simplicité de programmation de l'époque de ce processeur permettait de faire des jeux complets tels que<br>
Pong ou Brix tenir sur moins de 1000 octets...<br>
<br>
Ainsi il est facilement concevable qu'il est plus simple(façon de parler :)) d'émuler une machine à instruction de taille fixe, car le mécanisme<br>
de lecture de la ROM s'en trouve simplifié. Approchons de plus près le fonctionnement d'une ROM.<br>
<br>
<br>
<br>
<h4>iv. Comment fonctionne une ROM?;</h4>
La plupart des ROM, de n'importe quelle machine, sont ordonnées de la même manière(le Chip8 que nous étudierons n'en a pas :)).<br>
&nbsp-&nbsp <b>Header:</b> l'entête du jeu qui contiendra toutes les données du jeu, en quelques sortes sa carte d'identité.<br>
&nbsp-&nbsp <b>Instructions:</b> le fonctionnement propre du jeu, en langage machine.<br>
&nbsp-&nbsp <b>Données graphiques ou sonores:</b> tous les sprites ou autre qui sont poussées à la fin, pour des facilités de lectures.<br>
<br>
<br>
Prenons l'exemple des ROMS Nintendo 64©:<br>
Les adresses suivantes ne seront valides que pour les ROMS au format Low/High comme les Z64 du Mr Backup.<br>
La lecture se fait donc en Little Endian.<br>
<br>
<table border="1" summary="">
	<tr>
		<td>Adresse</td>
		<td>Taille</td>
		<td>Valeur</td>
		<td>Description</td>
	</tr>
	<tr>
		<td>0000h </td>
		<td>1 byte</td>
		<td>80h</td>
		<td>initial PI_BSB_DOM1_LAT_REG value</td>
	</tr>
		<tr>
		<td>0001h </td>
		<td>1 byte</td>
		<td>37h</td>
		<td>initial PI_BSB_DOM1_PGS_REG value</td>
	</tr>
		<tr>
		<td>0002h </td>
		<td>1 byte</td>
		<td>12h</td>
		<td>initial PI_BSB_DOM1_PWD_REG value</td>
	</tr>
	<tr>
		<td>0003h</td>
		<td>1 byte</td>
		<td>40h</td>
		<td> initial PI_BSB_DOM1_PGS_REG value</td>
	</tr>
	<tr>
		<td>0004h - 0007h</td>
		<td>5 bytes</td>
		<td>Pas de valeurs spécifiques</td>
		<td>Fréquence d'horloge.</td>
	</tr>
	<tr>
		<td>0008h - 000Ch </td>
		<td>1 dword</td>
		<td>Pas de valeurs spécifiques</td>
		<td>Program Counter(PC).<br>Adresse du jeu, point d'entrée du code.<br>
			Utilité inconnue étant donné que les jeux<br>commencent tous à 1000h.</td>
	</tr>
	<tr>
		<td>000Ch - 000Fh </td>
		<td>3 bytes</td>
		<td>000014h</td>
		<td>Inconnu</td>
	</tr>
	<tr>
		<td>000Fh</td>
		<td>1 byte</td>
		<td>44h (Darkrift, Fire demo)<br>
            46h (Waverace64)<br>
            48h (Bomber64, Duke64, TetPal)</td>
		<td>Inconnu</td>
	</tr>
	<tr>
		<td>0010h - 0014h</td>
		<td>1 dword</td>
		<td>Pas de valeurs spécifiques</td>
		<td>CRC1</td>
	</tr>
	<tr>
		<td>0014h - 0018h</td>
		<td>1 dword</td>
		<td>Pas de valeurs spécifiques</td>
		<td>CRC2</td>
	</tr>
	<tr>
		<td>0018h - 001Fh</td>
		<td>2 words</td>
		<td>0000000000000000h</td>
		<td>Inconnu</td>
	</tr>
	<tr>
		<td>0020h - 0033h</td>
		<td>20 bytes</td>
		<td>Rembourrage avec des 0h(NULL) ou des 20h(espaces).</td>
		<td>Nom de la ROM</td>
	</tr>
	<tr>
		<td>034h - 003Ah</td>
		<td>7 bytes</td>
		<td>00000000000000h</td>
		<td>Inconnu</td>
	</tr>
	<tr>
		<td>003Bh </td>
		<td>1 byte</td>
		<td>4Eh 'N' = Nintendo</td>
		<td>Identifier du fabricant</td>
	</tr>
	<tr>
		<td>003Ch - 003Dh </td>
		<td>2 bytes</td>
		<td>Pas de valeurs spécifiques</td>
		<td>Identifiant de la cartouche</td>
	</tr>
	<tr>
		<td>003Eh</td>
		<td>1 byte</td>
		<td>
                             0x4400 = Germany ('D')<br>
                             0x4500 = USA ('E')<br>
                             0x4A00 = Japan ('J')<br>
                             0x5000 = Europe ('P')<br>
                             0x5500 = Australia ('U')</td>
		<td>Nationalité du jeu</td>
	</tr>
	<tr>
		<td>003Fh </td>
		<td>1 byte</td>
		<td>00h</td>
		<td>Inconnu</td>
	</tr>
	<tr>
		<td>0040h - 0FFFh</td>
		<td>1008 dword</td>
		<td>Pas de valeurs spécifiques</td>
		<td> Début du secteur de boot. Quasiment tous les<br>jeux ont les même, cependant Starfox 64 diffère.</td>
	</tr>
	<tr>
		<td>1000h</td>
		<td>Reste du fichier</td>
		<td>Pas de valeurs spécifiques</td>
		<td>Là le code commence</td>
	</tr>
</table>
<br>
<br>
<li>Ainsi que fera l'émulateur pour faire fonctionner ces jeux?<br>
Tout d'abord il lira les 2 premiers bytes, s'ils correspondent, il se trouve en face d'une ROM N64 valide, donc il va<br>
continuer, s'ils ne correspondent pas deux cas son possibles, soit les bytes sont inversé dans quel cas il peut ou non<br>
gérer le byteswapping(c'est au programmeur de décider), soit ils ne correspondent toujours pas et là la ROM n'est pas<br>
reconnu, le programme quitte.<br>
<br>
Une fois la ROM validée, l'octet suivant déterminera si oui ou non la ROM est compressée.<br>
<br>
Et ainsi de suite jusqu'à la fin, pour ensuite sauter jusqu'au point d'entrée du jeu stocké dans le champs au même nom.<br>
<br>
Chaque instruction du code est alors interprétée et le jeu peut démarrer.<br>
<br>
Maintenant, toujours avec la Nintendo 64(pourquoi changer en plein tutorial :)), exerçons nous sur les 2 premiers opcodes<br>
d'un jeu, prenons par exemple la ROM de Mario 64 (dont vous avez bien entendu l'original).<br>
Lisons les 2 premières instructions à l'offset 100h, afin de nous entraîner...<br>
<br>
Offset: 0x1000 -> 3C08 800F 3C09 000A 2508 6970 3529 0FC0<br>
Les instructions de la N64 étant sur 32 bits, les deux premières instructions sont 3C08 800F et 3C09 000A.<br>
Maintenant regardons dans les documentations des opcodes à notre disposition, celui du processeur de la N64, c'est à dire le R4300i.<br>
<br>
Voici comment sont ordonnées les données des instructions:
<table align="center" border = "1">
<tr>
<td>OPCODE</td><td>Registres affectés, etc..</td>
 </tr>
 <tr><td>6 bits</td><td>26 bits</td></tr>
 </table>
<br>
Donc les 6 premiers bits de 3C08 800F valant 001111, cherchons dans la documentation la signification de notre premier opcode.<br>
Il s'agit ici de l'instruction LUI, LOAD UPPER IMMEDIATE, qui chargera les 16 bits immédiats de l'instruction, car LUI se décompose<br>
ainsi:
<table align="center" border = "1">
<tr>
<td>001111</td><td>00000</td><td>rt</td><td>16 bits</td></tr>
 </table>
<br>
Nous pouvons en conclure ainsi que 3C08 800F, en binaire 0011 1100 0000 1000 1000 0000 0000 1111 est en assembleur LUI RegistreGeneral[8],0x800F.<br>
Il en est de même pour 3C09 000A qui est LUI RegistreGeneral[9],0xA.<br>
Il y a plus simplepour décoder ces instructions, mais voici la méthode de base, qui se rèvele très performante, du moins assez pour comprendre<br>
manuellement(et mentalement) comment décoder toutes ces lignes de code. :)<br>
<br>
Nous aurions pu aussi lire le premier byte puis faire un rotation sur la droite de 2 bits mais on en est pas encore là! :)<br>
<br>
Dans le prochain tutorial nous verrons comment écrire un désassembleur, mais pour le Chip8 cette fois-ci, car on ne va pas commencer non plus par<br>
les gros systèmes, donc nous nous ferons la main sur ce petit processeur aux 41 fonctions différentes.<br>
Par contre, dès lors il faudra voir quelques notions de programmation en C, car aucune explication sur les bases de ce langage ne seront faites.<br>
<br>
Merci d'avoir lu ce tutorial, et à la prochaine!!
</body>
</html>
