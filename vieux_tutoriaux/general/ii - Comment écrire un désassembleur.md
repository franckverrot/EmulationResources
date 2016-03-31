<html>
<head>
<title>Comment écrire un désassembleur?</title>
<meta http-equiv="Content-Type" content="text/html; charset=iso-8859-1">
</head>
<body bgcolor="#bbccdd">
<center><h1><font color="#ff0000">Comment écrire un désassembleur?</font></h1>
<tt>Un tutorial par <a href="mailto:linuxshell@wanadoo.fr">Linuxshell</a>
<br>Téléchargé sur <a href="http://linuxshell.free.fr/">LXS_*</a></tt></center>
<br>
<center><h3>Partie 2 : Conception du tool-kit.</h3></center>
<br>
// Dernière version le :14/07/2002<br>
<br>
<br><hr>
<b>Table des matières:</b>
<ol type="i">
    	<li>Un point sur les connaissances;</li>
    	<li>Réunir les informations;</li>
    	<li>Ecrire le désassembleur;</li> 
		<li>Optimiser notre programme;</li>
    </ol>
<hr>
<br>
<h4>i. Un point sur les connaissances;</h4>
Dans le premier volet de cette série nous avons décrit le fonctionnement d'une ROM ainsi que la manière qu'a un CPU pour traiter les<br>
informations contenues dans cette ROM. Afin de pouvoir comprendre à peu près totalement cette documentation, il est nécessaire d'avoir<br>
des connaissances en C assez solides, et un gros moral! :)
<br>
<br>
<br>
<br>
<h4>ii. Réunir les informations;</h4>
Voila la partie la plus cruciale de notre travail, il va falloir réunir le maximum d'informations sur l'architecture émulée, afin de<br>
connaître intégralement le fonctionnement de la machine, de pouvoir comprendre quel rôle a tel ou tel processeur dans la machine.<br>
La machine ici étudiée sera le Chip8, beaucoup utilisé par les militaires à son époque, grâce notamment à sa facilité de programmation<br>
et son échelle de température allant de -45 à +55°C...<br>
Récupérons donc le peu de documentation que nous avons sur le sujet...<br>
Etant donné qu'il n'y a vraiment pas beaucoup de documentations disponibles, nous disposerons uniquement du jeu d'instructiondu processeur<br>
ainsi que les ROMs en domaine publique (ou libres) disponibles sur le net.<br>
<br>
Après lecture de l'"instruction set", deux choses sortent:<br>
&nbsp;-&nbsp;Les instructions sont fixées à 2 bytes;<br>
&nbsp;-&nbsp;Un seul processeur a besoin d'être émuler, il produire à la fois le son et les graphismes.<br>
<br>
<br>
<br>
<br>
<h4>iii. Ecrire le désassembleur;</h4>
Le jeu d'instruction sous les yeux, l'environnement de développement lancé ainsi qu'un éditeur héxadécimal avec 2 ou 3 ROMs chargées nous<br>
sommes prêt à commencer(ici nous utiliserons une fenêtre DOS, libre à vous de faire ça dans une fenêtre windows).<br>
Tout d'abord les fondamentales:<br><br><b>
#include &lt;stdio.h&gt;<br>
<br>
int main(int argc, char **argv)<br>
{<br><br>
if(argc<2) // Si aucune rom n´est donnée<br>
{<br>
printf("\tDésassembleur Chip8 par LXS\n"<br>
"Utilisation: %s <rom>", argv[0]);<br>
return 0;<br>
}</b><br>
}<br>
<br>
Donc jusqu'ici rien de terrible, si aucune rom n'est donnée on quitte.<br>
Rajoutons la gestion d'ouverture de la ROM:<br><br><b>
FILE *fp;<br>
if((fp= fopen(argv[1],"rb")) <= NULL)<br>
{<br>
printf("Erreur: Fichier introuvable...\n");<br>
return 0;<br>
}<br>
</b>
<br>
Ajoutons alors la lecture de la ROM, étant donné que les roms sont minuscules pour nos PC, chargeons les en entier en mémoire pour plus<br>
de facilité à manipuler les données et dans les déplacements en mémoire:<br>
<br><b>
char* buffer;<br>
<br>
fseek(fp,0,SEEK_END);<br>
int taille = ftell(fp); // détermination de la taille du fichier;<br>
fseek(fp,0,SEEK_SET);<br><br>
// allocation de la mémoire<br>
(char *)buffer = (char*)malloc(sizeof(fp));<br>
<br>	
// lecture intégrale du fichier, les données sont à présent stockées dans buffer<br>
fread(buffer,sizeof(char),taille,fp);<br>
<br></b>
<br>
Les données extraites, il ne reste plus qu'à lire puis de les analyser...<br>
<br>
<br>
<br>
<b>
int opcode = 0x0;<br>
char reg_1; // registre numéro 1<br>
char reg_2; // registre numéro 2<br>
<br>
<br>
for(int i = 0; i < taille;) // pour toute la rom<br>
	{<br>
		// buffer[i++] & 0xFF est l'instruction suivante à la position i<br>
        //affichage tel que: #op: identifiant_opcode @offset__héxa instruction_asm<br>
		printf("#op:%X @0x%X\t", opcode = buffer[i]>>4 & 0xF, &buffer[i]-&buffer[0]);<br>
		switch(opcode)<br>
		{<br>
<br>
		case 0:<br>
<br>
			reg_1 = (buffer[i] & 0xF0) >> 1;<br>
			if(reg_1 == 0)<br>
			{<br>
			i++;<br>
			reg_2 = buffer[i] & 0xF0;<br>
			if(reg_2 == 0xC)<br>
				printf("scdown %01X\n", reg_2 & 0x0F);<br>
			else<br>
			{<br>
				reg_2 = buffer[i] & 0xFF;<br>
			switch(reg_2 & 0xFF)<br>
			{<br>
			case 0xE0: printf("cls\n"); break;<br>
			case 0xEE: printf("rts\n"); break;<br>
			case 0xFB: printf("scright\n"); break;<br>
			case 0xFC: printf("scleft\n"); break;<br>
			case 0xFE: printf("low\n"); break;<br>
			case 0xFF: printf("high\n"); break;<br>
			default: printf("unknown 0x00xx opcode\n");<br>
			}<br>
			}<br>
			i++;<br>
			}<br>
<br>
			else<br>
			{<br>
				printf("unknown 0x0xxx opcode\n"); i++;<br>
			}<br>
<br>
			break;<br>
<br>
		case 1:<br>
			reg_1 = buffer[i] & 0x0F;<br>
			printf("jmp %01X%02X\n", reg_1, buffer[++i] & 0xFF);<br>
			i++;<br>
			break;<br>
<br>
		case 2:<br>
			reg_1 = buffer[i] & 0x0F;<br>
			printf("jsr %01X%02X\n", reg_1, buffer[++i] & 0xFF);<br>
			i++;<br>
			break;<br>
<br>
		case 3:<br>
			reg_1 = buffer[i] & 0x0F;<br>
			printf("skeq V[%01X],%02X\n", reg_1, buffer[++i] & 0xFF);<br>
			i++;<br>
			break;<br>
<br>
		case 4:<br>
			reg_1 = buffer[i] & 0x0F;<br>
			printf("skne V[%01X],%01X\n", reg_1, buffer[++i] & 0xFF);<br>
			i++;<br>
			break;<br>
<br>
		case 5:<br>
			reg_1 = buffer[i] & 0x0F;<br>
			reg_2 = buffer[++i] & 0x0F;<br>
			if(reg_2 == 0)<br>
			printf("skeq V[%01X],V[%01X]\n", reg_1, (buffer[i] & 0xF0)>>1);<br>
			else {printf("unknown 5xxx opcode\n");}<br>
			i++;<br>
			break;<br>
<br>
		case 6: <br>
			reg_1 = buffer[i] & 0x0F; <br>
			printf("mv V[%X],%02X\n", reg_1, buffer[++i] & 0xFF);<br>
			i++;<br>
			break;<br>
<br>
		case 7: <br>
			reg_1 = buffer[i] & 0x0F; <br>
			printf("add V[%X],%02X\n", reg_1, buffer[++i] & 0xFF);<br>
			i++;<br>
			break;<br>
<br>
		case 8:<br>
			reg_1 = buffer[i] & 0x0F;<br>
			reg_2 = buffer[++i] & 0x0F;<br>
			switch(reg_2)<br>
			{<br>
			case 0:<br>
			printf("mv V[%01X],V[%01X]\n", reg_1, (buffer[i] & 0xF0)>>4 );break;<br>
<br>
			case 1:<br>
			printf("or V[%01X],V[%01X]\n", reg_1, (buffer[i] & 0xF0)>>4);break;<br>
<br>
			case 2:<br>
			printf("and V[%01X],V[%01X]\n", reg_1, (buffer[i] & 0xF0)>>4);break;<br>
<br>
			case 3:<br>
			printf("xor V[%01X],V[%01X]\n", reg_1, (buffer[i] & 0xF0)>>4);break;<br>
<br>
			case 4:<br>
			printf("add V[%01X],V[%01X]\n", reg_1, (buffer[i] & 0xF0)>>4);break;<br>
<br>
			case 5:<br>
			printf("sub V[%01X],V[%01X]\n", reg_1, (buffer[i] & 0xF0)>>4);break;<br>
<br>
			case 0x6:<br>
				switch(buffer[i] & 0xF0)<br>
				{<br>
				case 0:<br>
				printf("shr V[%01X]\n", reg_1);break;<br>
<br>
				default:printf("unknown 5xxE opcode\n");<br>
				}<br>
				break;<br>
<br>
			case 7:<br>
			printf("rsb V[%01X],V[%01X]\n", reg_1, (buffer[i] & 0xF0)>>4);break;<br>
<br>
			case 0xE:<br>
				switch(buffer[i] & 0xF0)<br>
				{<br>
				case 0:<br>
				printf("shl V[%01X]\n", reg_1);break;<br>
<br>
				default:printf("unknown 5xxE opcode\n");<br>
				}<br>
				break;<br>
<br>
			default: printf("unknown 5xxx opcode\n");<br>
			}<br>
			i++;<br>
			break;<br>
<br>
<br>
		case 9:<br>
			reg_1 = buffer[i] & 0x0F;<br>
			reg_2 = ((buffer[++i] & 0xF0)>>1);<br>
			switch((buffer[i] & 0x0F)& 0x0F)<br>
			{<br>
			case 0:<br>
				printf("skne V[%01X],V[%01X]\n", reg_1, reg_2);<br>
				break;<br>
			default:<br>
				printf("unknown opcode 9xxx\n");<br>
			}<br>
			i++;<br>
			break;<br>
<br>
		case 0xA:<br>
			reg_1 = buffer[i] & 0x0F; <br>
			printf("mvi %01X%02X\n",reg_1, buffer[++i] & 0xFF);<br>
			i++;<br>
			break;<br>
<br>
		case 0xB:<br>
			reg_1 = buffer[i] & 0x0F; <br>
			printf("jmi %01X%02X\n",reg_1, buffer[++i] & 0xFF);<br>
			i++;<br>
			break;<br>
<br>
		case 0xC:<br>
			reg_1 = buffer[i] & 0x0F; <br>
			printf("rand V[%01X],%02X\n",reg_1, buffer[++i] & 0xFF);<br>
			i++;<br>
			break;<br>
<br>
		case 0xD:<br>
			reg_1 = buffer[i] & 0x0F;<br>
			reg_2 = (buffer[++i] & 0xF0)>>4;<br>
			switch(buffer[i] & 0x0F)<br>
			{<br>
			case 0: printf("xsprite V[%X],V[%X]\n", reg_1, reg_2); break; // SCHIP8 uniquement<br>
			<br>
			default: printf("sprite V[%X],V[%X],%X\n", reg_1, reg_2, buffer[i] & 0x0F);<br>
			}<br>
			i++;<br>
			break;<br>
<br>
		case 0xE:<br>
			reg_1 = buffer[i] & 0x0F;<br>
			reg_2 = buffer[++i] & 0xFF;<br>
			switch(reg_2 & 0xFF)<br>
			{<br>
			case 0x9e:	printf("skpr %01X\n", reg_1); i++; break;<br>
			case 0xa1:	printf("skup %01X\n", reg_1); i++; break;<br>
			default: printf("unknown opcode!\n"); i++;<br>
			}<br>
			break;<br>
<br>
		case 0xF:<br>
			reg_1 = buffer[i] & 0x0F;<br>
			reg_2 = buffer[++i] & 0xFF;<br>
			switch(reg_2 & 0xFF)<br>
			{<br>
			case 0x07:	printf("gdelay V[%01X]\n", reg_1); i++; break;<br>
			case 0x0a:	printf("key V[%01X]\n", reg_1); i++; break;<br>
			case 0x15:	printf("sdelay V[%01X]\n", reg_1); i++; break;<br>
			case 0x18:	printf("ssound V[%01X]\n", reg_1); i++; break;<br>
			case 0x1e:	printf("adi V[%01X]\n", reg_1); i++; break;<br>
			case 0x29:	printf("font V[%01X]\n", reg_1); i++; break;<br>
			case 0x30:	printf("xfont V[%01X]\n", reg_1); i++; break;<br>
			case 0x33:	printf("bcd V[%01X]\n", reg_1); i++; break;<br>
			case 0x55:	printf("str V[0]-V[%01X]\n", reg_1); i++; break;<br>
			case 0x65:	printf("ldr V[0]-V[%01X]\n", reg_1); i++; break;<br>
			default: printf("unknown opcode 0xf!\n"); i++;<br>
			}<br>
			break;<br>
<br>
		default: printf("unknown opcode\n"); i+=2;<br>
		}<br>
	}<br>
	return 1;<br>
}</b><br>
<br>
<br>
<br>
Bon on peut facilmeent voir qu'ici rien n'est optimisé, de plus à la fin de la lecture le désassembleur essayera<br>
de traduire en assembleur les données graphiques...donc nous avons tout de même un petit désassembleur fait maison<br>
bientôt près à égaler ceux présent sur les (trop peu nombreux) sites d'émulation.<br>
Au passage rien ne vous empêche de rediriger cette sortie sur un fichier(fprintf par exemple) afin de pouvoir disposer<br>
des informations extraites à n'importe quel moment.<br>
<br>
Donc ici le désassembleur lira les 2 premiers bytes de la ROM, isolera le premier nombre héxadécimal, puis redirigera le<br>
tout à l'endroit correspondant à son traitement(les 'case').<br>
Une fois les registres isolés à leur tour, le traitement s'effectue simplement et la ROM est désassemblée!<br>
<br>
<br>
<br>
<br>
<h4>iv. Optimiser notre programme;</h4>
Et oui afin de diposer d'un désassembleur correct, il nous faut régler le petit problème cité plus haut, c'est à dire<br>
l'interprétation des données graphiques.<br>
De plus nous pourrions ajouter les labels des JMP, JSR, etc...<br>
Tout est possible pour optimiser notre programme, de plus il est portable sous *nix,Win, BSD, etc...<br>
Une dernière chose très importante c'est l'endianness de la machine. Ici il faut êtr en Little Endian, donc dans le code<br>
final il faudra jongler avec le byteswapping si vous êtes en Big Endian.<br>
<br><br><br>
C'est tout pour ce second tutorial, la prochaine fois nous parlerons..d'autre chose :)

</body>
</html>
