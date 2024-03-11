## Accéder au serveur

http://legcompute2.unine.ch:8787 


## Micromamba 

1. Créer un environment 

```sh 
micromamba create -n NOM_ENVIRONMENT
```

2. Activer l'environment 


Quand l'environment est actif, vous le verrez a droite de votre prompt dans le terminal 

```sh 
hanna@legcompute2:~$ micromamba activate NOM_ENVIRONMENT
(NOM_ENVIRONMENT) hanna@legcompute2:~$
```

3. Installer un programme 
* Cherchez la page du programme sur [ananconda.org](https://anaconda.org/)
* Suivez les instructions d'installation en remplaçant la commande `conda` par `micromamba`. 

```sh 
# conda install bioconda::bedtools 
micromamba install biconda::bedtools 
```

## Bases du terminal 

> Ici c'est un peu un speedrun pour vous donner le vocabulaire nécessaire et servir de cheatsheet. Vous trouverez des explications beaucoup plus extensives si besoin en ligne, par exemple [ici](https://evomics.org/learning/unix-tutorial/). 

On interagit avec le terminal via des commandes. On peut obtenir de l'information sur chaque commande en utilisant l'option help. Les options sont préciseez via un `-` (ou `--`) et elles changent le comportement du programme. Dans ce cas, elle fait imrpimer le "*man page*" (le manuel) du programme. 

```sh 
hanna@akala:~ $ cat --help

Usage: cat [OPTION]... [FILE]...
Concatenate FILE(s) to standard output.

With no FILE, or when FILE is -, read standard input.

  -A, --show-all           equivalent to -vET
  -b, --number-nonblank    number nonempty output lines, overrides -n
  -e                       equivalent to -vE
  -E, --show-ends          display $ at end of each line
  -n, --number             number all output lines
  -s, --squeeze-blank      suppress repeated empty output lines
  -t                       equivalent to -vT
  -T, --show-tabs          display TAB characters as ^I
  -u                       (ignored)
  -v, --show-nonprinting   use ^ and M- notation, except for LFD and TAB
      --help        display this help and exit
      --version     output version information and exit

Examples:
  cat f - g  Output f's contents, then standard input, then g's contents.
  cat        Copy standard input to standard output.
```


La man page nous indique que `cat`est une commande qui imprime le contenu d'un ou de plusieurs fichiers (*concatenate*) dans un output (**standard output**). 

Si on ne précise rien, *standard output* est juste le terminal. Mais, on peut utiliser l'opérateur `>` pour envoyer le résultat de la commande dans un ficher. 

```sh
cat file.txt > newfile.txt 
```

> Attention: L'opèrateur `>` crée un nouveau fichier du nom précisé. Si ce fichier existe déja, il effacera le contenu !. Pour joindre le résultat à la fin d'un fichier existant, utilisez `>>` 


On peut faire des chaines de commandes (apellés *pipes*), en utilisant l'opérateur `|`. On prend  l'*output* de la première commande, et on l'envoie comme *input* a la commande suivante. 

```sh
# Lister le contenu du directoire
ls

# Lister le contenu du directoire, puis, trouver chaque entrée qui contiennent ".txt". Seuls ces entrées s'afficheront. 
ls | grep ".txt" 

```



### Navigation et fichiers

* `cd` : changer de directoire
* `ls` : lister le contenu du directoire
* `ll` : lister le contenu du directoire (format long)
* `mkdir` : créer un directoire 
* `rm` : effacer un fichier
* `rm -r` : effacer un directoire 
* `head`: afficher les 10 premières lignes d'un fichier
* `tail` : afficher les 10 dernières lignes d'un fichier
* `cat` : afficher le contenu du fichier

<img src="./etc/headtailcat.jpg" width="350" height="200" />

## Commandes de base 

* `grep`: trouve toutes les lignes contenant des matchs d'un pattern (qu'on précise avec des `""`)

Par exemple : `cat file.txt | grep "hello"` 

Les patterns ont des charactères spéciaux. Ces charactères ne sont donc pas cherchées directement, mais elles ont un effet précis.  Si on veut utiliser le charactère tel quel, on doit utiliser un backslash (`\`) devant. Par. exemple : `\.` cherchera vraiment un point. 

Les charactères spéciaux sont souvent valables à chaque fois qu'on parle de "pattern", peut importe la commande précise. C'est ce qu'on appelle des *expressions régulières* 

```bash
#Operator       Effect
.	            #Matches any single character.
?	            #The preceding item is  optional and will be matched at most once.
*	            #The preceding item will be matched zero or more times.
+	            #The preceding item will be matched one or more times.
{N}	            #The preceding item is matched exactly N times.
{N,}	        #The preceding item is matched N or more times.
{N,M}	        #The preceding item is matched at least N times, but not more than M times.
-	            #represents the range if it's not first or last in a list or the ending point of a range in a list. Eg [2-4] matches 2, 3 or 4. 
^	            #Matches the empty string at the beginning of a line; also represents the characters not in the range of a list. Eg. [^2-4] matches everything that is NOT 2,3 or 4. 
$	            #Matches the empty string at the end of a line.
\b	            #Matches the empty string at the edge of a word.
\B	            #Matches the empty string provided it's not at the edge of a word.
\<	            #Match the empty string at the beginning of word.
\>	            #Match the empty string at the end of word.
```


* `sed` : chercher et remplacer des charactères 

```bash
# Imprimer le contenu de fichier.txt avec chaque instance de pattern changé en remplacement
sed 's/PATTERN/REMPLACEMENT/g' fichier.txt

# Changer pattern par remplacement dans le fichier 
sed -i 's/PATTERN/REMPLACEMENT/g' fichier.txt
```

* `cut` : découper un fichier en colonnes; 

```bash
#  Découpe un fichier en colonnes démarquées par un espace et imprimer la première colonne
cut -d " " -f 1 
```

## Formats des fichiers bioinformatiques

On bioinfo on adore inventer des nouveaux formats de fichiers. Il y'en a vraiment beaucoup, parfois ils se ressemblent, et on sait pas trop pourquoi il y'en a autant, mais, c'est comme ça. Les plus importants pour nous sont 

* les fichiers **fasta**, qui contiennent des séquences nucléotidiques 
* les fichiers **bed**, qui sont utilisées pour des préciser des intervales génomiques
* les fichiers **gff** ou **gtf** qui sont utilisées pour contenir des annotations précises sur des intervales génomiques (de façon légèrement différente, bien évidamment). 

Vous trouverez des exemples et des explications détaillées de la plupart des formats communs [ici](https://bioinformatics.uconn.edu/resources-and-events/tutorials-2/file-formats-tutorial/)

L'avantage est que ces formats sont reconnus par la plupart des programmes bionfo, donc ils nous permettent de changer facilement entre différents outils. 

