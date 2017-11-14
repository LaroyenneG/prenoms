# prenoms
TP Unix 2017 ENSISA

Objectif :
Créer une commande unix qui va permettre de fabriquer une liste de prénoms provenant de plusieurs sites dédiés. 
Dans le TP il faudra télécharger des fichiers et isoler des prénoms provenant de divers sites. La diversité des sites est grande, ainsi certains séparent les prénoms féminins des prénoms masculins, d’autre découpent en plusieurs pages de 20 prénoms enfin certain filtrent en fonction de l’initiale. Bref un cauchemar qu’il va falloir gérer. De plus, il pourrait être sympa de pouvoir préparer les téléchargements, qui seront effectués dans la soirée, en fabriquant une liste de sites qu’il faut traiter. Pour cela le concept de campagne va être proposé. Ce concept permet de préparer une aspiration de sites selon les critères spécifiques.
Délivrable 
A la fin de la deuxième séance de TP il faut déposer sur MOODLE un zip contenant LE seul et unique fichier script. Je ne corrige pas les fichiers expédiés par mail et je ne corrige pas des TP non zippé et ne contenant pas un seul fichier. Le TP doit être opérationnel sur une configuration type salle de TP de l’école. Enfin, vous ne pouvez pas supposer connaitre le nom de la commande (pas d’astuce de lancement) lors des tests ni sa localisation dans le système de fichier (vous pouvez inventer des répertoires enfants mais pas les parents, je déconseille d’utiliser cd).
Déroulement 
Pour vous simplifier le développement, cette commande aura donc plusieurs phases : 
* Initialisation du programme (créer la liste des sites) 
* Effacement de la campagne précédente 
* Ajout d’une nouvelle règle dans la campagne courante 
* Téléchargement des pages web nécessaires décrites par les règles de la campagne 
* Extraction des informations exposées par les sites téléchargés 
* Fusion intelligente des tous les prénoms découverts 
Dans ce TP, la notion de phase signifie que j'exécute une commande unix avec des options et la commande fait le travail demandé. Si je souhaite exécuter une nouvelle phase j'exécute une nouvelle commande unix. Ainsi les phases sont totalement disjointes et indépendantes si des données doivent persister à vous de les stocker dans des fichiers.
En principe, un utilisateur initialise le programme. Puis, quand il souhaite construire une campagne, il commence par effacer la campagne précédente et il ajoute autant de règles que nécessaire dans la campagne courante. Quand la campagne est prête, il attend la nuit pour lancer les téléchargements. Le lendemain matin, il retrouve son ordinateur et il va extraire les données utiles des téléchargements en il va finir par fusionner les résultats. Bien sûr, il va se rendre compte qu’il a oublié une règle, il l’ajoute à la campagne et relance le téléchargement, l’analyse et la fusion. Ce qui est génial, c’est que la commande est tellement futée que seules les pages nécessaires (celles ajoutées) seront téléchargées et analysées. Le concept de campagne est parfait car il permet même la collecte au fil du temps des données pertinentes.
Remarque : il est inutile de vouloir converser avec l'utilisateur, il n'y aura personne devant le clavier. Il est inutile d'être bavard sur la console ou la sortie d'erreur les flux sont neutralisés et vont vers des fichiers qui seront juste vérifiés en fin de script de test. Des messages d’erreurs sur la sortie d’erreur (ou standard) marqueront définitivement votre script comme suspect.
Commande 
prenom option* 
Option : 
        -i         Initialisation 
        -c         Effacement de la campagne précédente. 
        -a site lettre genre debut fin         Ajout d’une nouvelle règle dans la campagne.  
1. Téléchargement des fichiers décrits par toutes les règles de la campagne 
2. Extraction des prénoms et filtrages éventuels de tous les téléchargements 
-m fichier        Fusion de tous les prénoms découverts par la campagne en un seul fichier résultat dénommé fichier à la racine du répertoire courant.
Quand j’écris option* cela signifie que je peux écrire une ou plusieurs options sur la même ligne. Ainsi, je peux écrire deux commandes unix (gauche) ou juste une seule (droite), voire l'exemple en fin de sujet.


prenom –i prenom –c 
prenom –m toto
	prenom –i -c –m toto
	

Détails concernant l’initialisation 
L'initialisation ne sera effectuée qu'une fois par an, voire … Elle permet de générer les fichiers qui sont bien pratiques s'ils sont déjà sur le disque. Les huit sites proposés sont obligatoirement renseignés dans l'ordre ci-dessous: 
http://prenoms.aujourdhui.com/guide/commencant-par-%letter%.asp http://www.mamanpourlavie.com/prenoms/lettre/%letter%/%gender% http://www.bebepassion.com/prenoms/prenom-%gender%-%letter%.htm 
https://www.prenoms.com/prenom/recherche.html?advanced=True&Genre=%gender%&start_with=%letter%&page=%page%
http://nominis.cef.fr/contenus/prenom/alphabetique/%letter%-%page%.html http://www.quelprenom.com/liste_prenoms.php?code=FR&sexe%5B0%5D=%gender%&start=%letter%&page=%page%
http://www.magicmaman.com/prenom/recherche/commencantpar=%letter%#?commencantpar=%letter%&frai&sexe=%gender%&page=%page%
https://www.naissance.fr/prenoms/recherche?gender=%gender%&begin=%letter%&page=%page%


Dans ces 8 URLs vous pouvez voir des noms spéciaux encadrés par des %. Ces noms spéciaux signifient qu'il faut les remplacer par un élément de la requête. Exemple : %letter% peut devenir a si vous voulez les prénoms commençants par la lettre a et %page% par le numéro de page si la liste est découpée en plusieurs pages. Dans le cas de site avec un affichage paginé, souvent la page 1 est codée différemment mais elle semble accessible malgré tout (ou pas), il arrive que la page 0 existe ….
Détails concernant l'effacement 
L'effacement d’une campagne (et uniquement d’une campagne) sera effectué régulièrement ; on peut imaginer qu'elle débute une série de commandes d’ajout de sites pour créer une campagne. L'objectif est d'effacer toutes les informations temporaires provenant de l’utilisation de la campagne précédente. 
Détails concernant l’ajout d’une nouvelle règle 
Plusieurs paramètres sont requis (heureusement pour vous que je ne les rends pas optionnels car c'est nettement plus complexe à traiter) : 
* Site : c’est un mot qui permet d’identifier le site que l’on va utiliser. Si c'est un nombre alors il représente la position du site dans la liste décrite à l'initialisation. Ainsi 3 sera toujours le site bebepassion. Si c'est un nom alors il doit identifier de façon unique un site. Ainsi ‘aujourdhui’ sélectionne le premier site mais maman est une erreur car ce mot existe dans le 2 eme, el 4 eme et dans le 7 eme site. Vous ne pouvez pas énumérer les noms possibles, car je peux autant utiliser min que quel ou guide (ces mots marchent).
* Lettre : La lettre qui doit commencer le prénom, elle peut être écrite en majuscule ou en minuscule et il en faut qu’une seule (ex: x est légal mais bc ou 8 sont des erreurs 
* Genre : Le genre du prénom soit garcon (pas de cédille) ou fille soit boy ou girl. 
* Debut : le numéro de page de début ou le mot 'no' ou le tiret (‘-‘). Le mot no ou le tiret permet notamment d'ignorer ce paramètre s'il n'est pas indispensable à la requête. La page pourrait commencer avec l’index 0.
* Fin : le numéro de page de fin ou le mot 'no' ou le tiret (‘-‘). Le mot no ou le tiret permet notamment d'ignorer ce paramètre s'il n'est pas indispensable à la requête. 


* Si début est un nombre et fin un tiret alors fin = début et si fin est un nombre et début un tiret alors debut = fin et si debut et fin ont un tiret c’est normal, il est probable que le site ne demande pas de numéro de page.


Une procédure de vérification des contraintes doit être écrite et elle doit arrêter l’exécution en signalant dans un langage compréhensible (français ou anglais) toutes les erreurs de saisie pour la règle (il peut y avoir plusieurs erreurs pour la même règle). 
Détails concernant le téléchargement 
L’option téléchargement va analyser chaque règle dans la campagne. Il va avoir une combinaison intelligente entre les URL prédéfinie et les différents arguments de la règle. Si la page cible existe déjà dans le répertoire de téléchargement alors elle n'est pas téléchargée à nouveau (on suppose que le contenu n’évolue pas), à contrario, si la page n’est pas dans le répertoire de téléchargement elle est téléchargée en utilisant curl. Ce comportement pose le problème de coder un nom unique à un fichier sur disque mais différent pour chaque page téléchargeable.


Attention, le filtrage ne se fait pas sur la règle complète mais sur chaque URL, en effet une ancienne règle pourrait dire de charger les pages 11 à 13, règle que vous avez utilisée pour commencer une campagne, mais par la suite vous décider d’étendre la règle pour télécharger de la page 10 à 14. Dans ce cas, la page 10 est téléchargée, pas les pages 11 à 13 mais la 14 est également téléchargée car absente.


Les esprits perspicaces auront noté que pour par exemple l’URL 1 le genre n’est pas précisé dans la requête web mais pourtant il est spécifié dans les paramètres de la campagne. C’est triste, le site ne propose pas la sélection du genre dans son URL mais le résultat (page téléchargée) permet de faire le filtrage dans la phase d’exploitation des fichiers téléchargés.
Le téléchargement d’une page consiste à utiliser curl pour chercher la page des sorties de la semaine des sites demandés et à stocker cette page sur le disque. Pour faire le téléchargement il faudra utiliser curl ou curl voire curl.
Remarques importantes :
* 2 sites (www.mamanpourlavie.com et www.bebepassion.com) utilisent garcon et fille pour faire la différence entre les genres.
* 1 site (www.naissance.fr) utilise boy et girl pour faire la différence entre les genres.
* 3 sites (www.prenoms.com, www.magicmaman.com et quelprenom.com) code le genre avec 1 pour garcon et 2 pour fille.
* 1 site (prenoms.aujourdhui.com) ignore le genre dans la requête mais la page donnera le genre de chaque prénom voire mixte.
* 1 site (nominis.cef.fr) ignore le genre dans la requête et la page ne donnera pas le genre.
* Tous les sites utilisent l’alphabet pour la lettre de début de mot sauf www.magicmaman.com qui utilise la position de la lettre dans l’alphabet (a -> 1, m ->13, z -> 26).
Détails concernant l’analyse
Il faudra trouver une solution ‘simple’ pour savoir quelles sont les pages à analyser. La campagne ne donne plus les bonnes informations et les commandes précédentes ont été oubliées sauf si vous laissez des traces sur le disque. Maintenant, juste la présence des fichiers téléchargés sur le disque peut largement suffire.


La page web est sur l’ordinateur (si vous codez habilement le nom du fichier vous avez même tous les éléments de la règle qui lui a donné naissance). Il faut la scanner pour y découvrir les prénoms (et les genres) et en extraire un fichier qui, au plus simple, pourrait n’avoir que deux colonnes : genre et prénom.
Il est illusoire de créer un script universel qui analyse toutes pages en faisant abstraction de leurs provenances. Il faudra écrire un script dédié à l'analyse de la page provenant d'un site particulier, d'où l'intérêt de coder habilement le nom de fichier pour retrouver les éléments pertinents.
Remarques importantes :
* souvent les caractères accentués posent des problèmes aux filtres basés sur sed/grep/etc. Pas grave si vous perdez des prénoms. Passer la variable LANG à "C" simplifie également les choses (LANG="C").
* Certains sites vont préciser le genre dans le fichier de résultat (attention à la langue et l’orthographe), des fois il y a un troisième genre qui peut exister (mixte). Vous pouvez le traiter ou pas.
Selon l’origine de la page voici quelques conseils : 
prenoms.aujourdhui.com 
La liste des prénoms est du côté de la 1000 ème ligne. Le genre est décrit par trois images bb-boy-16 et bbgirl-16 et bb-mixt-16 qui sont utilisées quelques lignes avant le prénom. Si le fait de passer la variable LANG à "C" n’est pas suffisant, une astuce simple pour effectuer l'assemblage du genre et du prénom sera décrite en life dans la deuxième séance de TP. Inutile de vous casser la tête dessus traitez l’autre sites en attendant. 
www.mamanpourlavie.com 
La liste des prénoms est dans le bloc <ul id= ls-firstname>. Le genre n’est pas/plus précisé car spécifié dans la requête.
www.bebepassion.com 
La liste des prénoms est du côté de la ligne 150. Deux cellules td sur une ligne avec un align comme clé de sélection. Le genre est rappelé dans la deuxième colonne (M ou F).
www.prenoms.com 
La liste des prénoms est du côté de la ligne 600 après la balise avec une class top-prenom Le genre est précisé entre parenthèse (Masculin ou Feminin) dans le titre du lien.
nominis.cef.fr 
La liste des prénoms est du côté de la ligne 136 sur une seule ligne … pas cool. Si vous insérez (avec sed) un saut de ligne avant un <a et après le </a> ça devient tout de suite plus facile. Une tristesse, le genre n’est pas dans la requête et pas dans le fichier téléchargé, il faudra donc supposer qu’ils sont tous du genre souhaité. 
quelprenom.com 
La liste des prénoms est du côté de la ligne 136 sur une seule ligne … pas cool, y a une class=’bn-list’ qui peut servir de repère. Si vous insérez (avec sed) un saut de ligne avant un <a et après le </a> ca devient tout de suite plus facile. Le genre est codé par boy, girl ou mixed, à vous de récupérer les bons prénoms. Enfin comme l’initial de prénom n’est pas utilisé dans la requête il peut y avoir des prénoms erronés qui polluent votre recherche (faudrait les éliminer).
www.magicmaman.com
La liste des prénoms est du côté de la ligne 870 avec une belle class bien unique. Un genre est précisé mais il ne servirait que pour les prénoms mixtes. Dans notre cas, il semble inutile. Le lien est parfait.
www.naissance.fr
La liste des prénoms est du côté de la ligne 800 avec un beau h2. Le lien est parfait.
Détails concernant la fusion 
Il suffit de prendre tous les fichiers de résultats d'analyse et de les rassembler trier sur les prénoms en 1 seul en éliminant les doublons (il faut peut-être tenir compte de l’initial en majuscule ou pas). Le résultat se trouve impérativement à la racine de votre répertoire sous le nom donné en argument.
Déroulement type 
On peut imaginer un premier usage de cette commande : 
./prenoms.bsh -i 
./prenoms.bsh -c 
./prenoms.bsh -a 1 A boy - - 
./prenoms.bsh -a mamanpourlavie a girl no no 
./prenoms.bsh -a bebepassion A garcon - no 
./prenoms.bsh –d -e
./prenoms.bsh -a 4 a fille 4 5 -a 4 a garcon 8 - 
./prenoms.bsh -a liste Q boy 1 - -a quelprenom r boy - 5 
./prenoms.bsh -a gic b fille 12 - 
./prenoms.bsh -a nai c garcon 3 - -a nai d boy - 2 
./prenoms.bsh -d -e -m result.txt


L'usage ci-dessus est un exemple et ne reflète en aucune façon les campagnes que je vais mener. 
Répartition
Et oui 8 sites à télécharger puis à exploiter c’est long et fastidieux aussi vous ne traiterez l’analyse que pour 2 des sites proposés selon la répartition ci-dessous.


Il faut donc savoir ajouter tous les sites d’une campagne. Pr contre en dernier lieu (juste avant de télécharger réellement le site) vous mettez en place la neutralisation de la commande. Il est important que vous ne téléchargez que vos sites, cela évitera la détection d’un ‘pilleur’ de ressources web (activité condamnable) par des accès répétés et agressifs provenant toujours d’une même adresse IP.


Concernant l’analyse, il faut mettre en place une stratégie simple qui permet de ne devoir traiter les sites qui ne sont pas nécessaire mais offre la possibilité d’ajouter rapidement une implémentation. En bref, si je vous demande de coder dans les 15 minutes restant un des sites que vous n’avez pas faits, vous pouvez ‘insérer’ ce code en moins de 30 secondes.


Mon script de test est universel et suppose l’implémentation de tous les 8 sites vous devrez donc trouver une solution simple pour sans signaler aucun échec et ne pas traiter l’analyse des sites que vous ne devez pas.
