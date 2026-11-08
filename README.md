# Detonate — prototype V3.5

## Contenu
| Fichier | Rôle |
|---|---|
| `index.html` | Le jeu complet (solo contre bots et client en ligne). C'est aussi le code que le serveur exécute pour simuler les parties. |
| `server.js` | Serveur des parties privées (WebSocket, salons par code, simulation à 60 Hz). |
| `game_sim.js` | Lance la simulation du jeu (le dernier `<script>` de `index.html`) côté serveur, sans affichage. |
| `package.json` | Dépendance unique : `ws`. |
| `Dockerfile` | Image Node 20 pour Fly.io. |
| `fly.toml` | Application Fly.io `detonate-prototype` (régions cdg, lax, iad, sin). |

## Nouveau en V3.5 : mode Zombie (étape 1)
Carte **Forêt · mode Zombie** dans le menu : 5 équipes de 2, carte sombre, poignard + briquet (3 min),
une vague de zombies par minute (20 vagues), la dernière équipe en vie gagne.
Étape 2 : sac de 3 emplacements, butin aléatoire (armes, munitions, ressources), fouille des voitures (4 s),
pistolet et fusil d'assaut (le bruit attire les zombies), bandages, sac laissé au sol à la mort.
Étape 3 : faim et soif (6 et 9 min ; sous 20 % on ralentit, à zéro on perd 1 PV toutes les 2 s), saignement après une morsure
(bandage, aussi sur un coéquipier en 5 s), animaux (poulet, cochon, vache : viande et cuir), boire et pêcher aux étangs,
**essence** pour recharger le briquet (+1:30), **torche** fabriquée avec 1 bois au feu de camp (grande lumière 2 min),
menu d'artisanat (touche C ou bouton 🔨) : feu de camp, torche, raviver, cuire, lance, canne à pêche, barricades (les zombies
les frappent, les brutes les cassent vite ; E maintenu pour démonter), cocktail Molotov, bandages, sac en cuir (arme de rechange).
Sons du mode Zombie refaits : voix des zombies (grognements selon le type, râle avant de frapper, morsure, cri du hurleur, mort et chute),
pas des zombies proches, coup de poignard et de lance (souffle + impact dans la chair), briquet, objets, feux qui crépitent, ambiance de forêt (vent, grillons, chouette). Début de vague : deux coups de cloche au loin (plus discrets que l'ancienne corne).
Touches PC du mode Zombie : objets é " ' (et ( pour la 4e case), A = changer d'arme (maintenir : la lâcher), V = grenade / Molotov, W = alliance, X = refuser une alliance,
G = briquet, E = ramasser / fouiller / boire, B = bandage, C = artisanat, H = aide. Canne à pêche : touche de son emplacement (ou E au bord
de l'eau quand on n'a pas soif), 4 s sans bouger, elle casse au 2e poisson. Sac en cuir (1 cuir) : à utiliser, +1 case dans le sac.
Étape 4 : à zéro PV, on reste **à terre** 30 s (on rampe) si un coéquipier ou un allié debout peut nous **relever** (E maintenu 4 s, on repart avec 35 PV) ;
**alliances** entre deux équipes (touche W ou bouton 🤝 : proposer, accepter, rompre) : pas de dégâts entre alliés, on se voit dans le noir et on peut se relever ;
quand il ne reste que deux équipes alliées, l'alliance prend fin après 10 s. Après la vague 20, **brouillard toxique** : la zone sûre se resserre par paliers
(dégâts croissants dehors). Bots : déplacements par la grille de navigation (ils ne tournent plus autour des maisons), ils suivent le joueur humain, relèvent leurs alliés.
Emotes d'alliance au-dessus des personnages : 🤝? (doré) une équipe propose une alliance, 💭 elle réfléchit à ta proposition,
🤝✔ (vert) alliance conclue, ✖ refus ; en t'approchant des bots : 🤝 (contour vert) ils accepteraient, ⚔️ ils n'en veulent pas. Refuser : touche X (bouton ✖).
Tir allié : pendant une alliance, on peut blesser ses alliés ; toucher un allié (balle, poignard, lance) est une **trahison** : l'alliance est rompue
sur-le-champ, 🗡️ au-dessus des traîtres, et les bots trahis deviennent hostiles. Les bots ne tirent pas quand un ami est dans la ligne de tir et trahissent
parfois un allié affaibli en fin de partie. Panneau des commandes (H) refait : toutes les touches, textes ajustés.
Étape 5 (bots et équilibrage) : les bots lancent leurs grenades sur les groupes, gardent leurs balles (poignard sur les rôdeurs isolés),
fabriquent une lance quand leur arme est vide, font un feu de camp pour cuire la viande, remplissent leurs bouteilles, défendent puis relèvent
leurs coéquipiers, et ne fuient pas quand leur joueur humain est à terre. Équilibrage : plus de munitions (butin, voitures, et sur les zombies :
14 % ; brutes 70 %), brute 280 PV et un peu plus rare, vagues un peu moins nombreuses, horde plafonnée selon le nombre d'équipes en vie,
saignement qui s'arrête après 25 s, faim 11 min, soif 7 min, à terre 40 s. Sur 12 parties de bots : vague finale moyenne 18,5 (avant : 13,9).
Manger et boire sur place : près d'une nourriture ou d'une eau au sol, appui court sur E = la ramasser, maintenir E (0,4 s) = la manger / la boire
tout de suite sans passer par le sac (sac plein : un simple appui suffit) ; les bots font de même quand ils ont faim ou soif.
Manger et boire prennent 2 s (on est ralenti ; un coup reçu ou une attaque interrompt, la portion prise au sol y est reposée) ; la cuisson prend
5 s par aliment près du feu. Entraide : un bot donne à son coéquipier (toi ou un autre bot) un bandage, de l'eau ou de la nourriture dès qu'il en a
besoin (🎁 au-dessus du bot), en gardant le minimum pour lui.
Correctifs du mode Zombie : les bots ne restent plus bloqués à ramasser puis lâcher une bouteille vide (sac plein), ne reprennent plus
une arme à feu vide à la place de leur lance, ni le bandage ou la nourriture qu'ils viennent de poser à tes pieds quand ton sac est plein
(le cadeau t'est réservé 60 s). Bandage : on se soigne soi-même quand on a moins de vie que son coéquipier (avant, on soignait parfois
l'autre). Le saignement, la faim ou la soif à zéro et le brouillard n'interrompent plus le repas (2 s) ni la pêche. À terre ou mort pendant
un repas : la portion prise au sol y retombe. Maintenir E près d'une nourriture qu'on ne peut pas manger ne bloque plus les autres actions
(boire à l'étang, ramasser). Un joueur à terre ne saigne plus après une morsure. Briquet vide : un message s'affiche. Bouton tactile :
« BOIRE » pour l'eau au sol. À terre : plus de coup ni de tir affiché côté client ; le menu d'artisanat se ferme. « Cuire » explique
pourquoi c'est impossible pendant un repas. Menu d'artisanat et tableau des équipes plus opaques (les annonces ne passent plus au travers).
**Arbalète** (artisanat : 2 bois + 2 métal) : arme de tir rapide (un carreau toutes les 0,3 s, 38 dégâts, précise), presque silencieuse
(les zombies ne l'entendent qu'à 160 px, contre 600 pour le pistolet), sans rechargement ; 60 carreaux, puis elle casse. Elle se prend en main
à la fabrication (l'arme tenue tombe au sol) ; posée au sol, son nom indique les carreaux restants. Les bots en fabriquent une quand ils n'ont plus
d'arme de tir utilisable. Sons « personnels » (manger, boire, ramasser, soin, artisanat, toux, à terre, alliance) : ils étaient muets, ils sont rétablis.
Mini-carte de la Forêt : 20 % plus petite, sans les voitures.
Bots : ils se soignent plus tôt (au calme dès qu'il leur manque 35 PV, et même avec des zombies à distance quand leur vie est très basse),
vont chercher un bandage au sol quand ils sont blessés, et prennent ce que leur coéquipier humain leur pose (bandage, nourriture, eau, munitions,
grenade) même avec un sac plein : ils lâchent alors ce qui leur sert le moins (pierre, bois, bouteille...). Carte du coéquipier sous la mini-carte :
nom, vie (ou « À TERRE »), saignement, faim et soif ; une icône clignote au-dessus de lui quand il a faim ou soif. Sac : la molette ou un clic
choisit un objet (clic sur l'objet déjà choisi : l'utiliser) ; touche T : lâcher 1 objet choisi (munitions : 10), maintenir T : toute la pile.
Plus de munitions : boîtes plus fréquentes dans les chalets, les voitures et la forêt (+50 à +70 %) et plus grosses (24 à 42 balles),
fouille de voiture 30 à 45 balles, zombies 21 % (10 à 19 balles), brutes 80 % (18 à 30). Sur 10 parties de bots : temps sans munitions
6 % (avant 23 %), vague finale moyenne 23,6 (avant 20,5).
Bot coéquipier : il boit vraiment au lac (il s'arrêtait 4 px trop loin de l'eau, puis abandonnait l'étang pour toute la partie), boit en passant
quand il est au bord de l'eau, et sa soif presque vide passe avant un combat à distance ; face à une horde, il se replie vers toi au lieu de
s'enfuir ailleurs et ne s'éloigne plus à plus de 240 px en combattant ; il t'aide à achever un animal que tu viens de frapper ; il fouille les
voitures proches de toi (il n'en fouillait jamais, et ceux qui essayaient restaient parfois trop loin de la voiture). À terre : on est à plat ventre,
tourné dans le sens où l'on rampe (plus vers la visée), bras qui tirent et jambes qui poussent en alternance ; on rampe un peu plus vite (45 px/s) ;
un bot à terre rampe vers son coéquipier pour être relevé plus vite.
Fouille des voitures par les bots : ils ne choisissent plus une voiture que quelqu'un fouille déjà, se placent sur un point libre à portée de
fouille, et s'arrêtent dès que la voiture est à portée (avant, ils pouvaient faire du surplace contre la carrosserie sans jamais fouiller).
Éclair de tir : il s'éteint maintenant 0,06 s après le coup (en mode Zombie il restait allumé en permanence au bout de l'arme).
Bandage au sol : comme la nourriture, on peut s'en servir sans le mettre dans le sac (maintenir E ; sac plein : un simple appui) ; interrompu,
il retombe au sol. Les bots blessés font de même. Carte de la Forêt : 6 chalets de plus (19 au total), placés dans les plus grands vides (coins et
bords de la carte, loin des points de départ), porte tournée vers la route la plus proche, clairière dégagée autour ; le reste de la carte est
identique ; un peu plus d'objets de départ dans les chalets (même quantité par chalet).
Mobile (mode Zombie) : barre de vie (ATH) et inventaire deux fois plus petits.
- **ATH mobile** : barre de vie et inventaire agrandis de 15 % ; l'inventaire a la même largeur que la barre de vie et il est centré dessus ; la carte du coéquipier est centrée sous la mini-carte ; mini-carte mobile réduite de 15 %.

- **Chiffres de l'inventaire sur mobile** : taille minimale de police, pastille sombre derrière les quantités et les munitions ; l'état du briquet et le texte « fabriquer » sont masqués sur mobile pour libérer la place.

- **Case de l'arme sur mobile** : nouvelle mise en page, avec l'icône à gauche et les munitions « chargeur/réserve » à droite, sans chevauchement. La taille du texte s'adapte. L'icône du poignard se retire quand la place manque.

- **Son** : les tirs, les impacts et les sons du joueur sont prioritaires. Les grognements, les pas et l'ambiance ne peuvent plus leur prendre leur place. Le nombre de sons simultanés est limité, plus fortement sur mobile. Les voix des zombies sont plus légères sur mobile (2 formants) et il y a moins de bruits de pas. Les sons sont planifiés un peu en avance, ce qui évite les clics. Sur mobile, le tampon audio est plus grand. Le son reprend après une interruption (appel, autre application, relâchement du doigt sur l'écran).
- **Brouillard de fin de partie** : sur la mini-carte, il reste dans le cadre. Il est hachuré, avec un liseré lumineux et la prochaine zone en pointillés qui défilent. À l'écran, le cercle est calculé sur la partie visible de la carte. Cela corrige l'écran entièrement vert au début du brouillard, et le « trou » de la zone sûre qui n'apparaissait pas sur la mini-carte. Le brouillard a maintenant des volutes qui dérivent, un halo le long du bord et un mur de brume animé.
- **Munitions** : environ 45 % de munitions en moins sur la carte. Poids : maisons 48→30, véhicules 46→30, forêt 24→14, voitures 36→26. Quantités : au sol 18 à 32, voitures fouillées 22 à 34.

- **Sac du joueur mort** : quand un joueur meurt, ses armes (avec leurs munitions) et tout son sac restent dans un sac au sol, à son nom. On l'ouvre avec E (PC) ou le bouton OUVRIR LE SAC (mobile) pour voir son contenu. On touche un objet pour le prendre, ou TOUT PRENDRE. Une arme prise remplace celle qu'on tient, qui va dans le sac. Sac plein : on touche l'objet voulu puis la case de son sac à échanger. Les bots fouillent aussi ces sacs.
- **Manger** : il faut maintenir l'action pour manger ou boire au sol (0,8 s ; 1,5 s pour la viande ou le poisson crus). Un appui court ne fait plus manger, même sac plein. La viande et le poisson crus pris dans le sac demandent un deuxième appui pour confirmer. Appuyer de nouveau sur E (ou sur ANNULER) pendant un repas l'arrête, sans perdre l'aliment.
- **Échanger un objet** : sac plein, sur PC, E prend l'objet au sol à la place de la case choisie (molette ou clic) ; l'ancien objet reste au sol. Sur mobile, le bouton ÉCHANGER puis un toucher sur la case à remplacer.
- **Munitions** : +20 % d'apparition (maisons 30→36, véhicules 30→36, forêt 14→17, voitures 26→31).

- **Équipes du mode Zombie** : ALPHA (bleu), DELTA (orange), SIERRA (violet), COBRA (jaune) et GHOST (vert). Avant : Loups, Renards, Corbeaux, Lynx, Vipères. Les bots s'appellent « Delta 1 », « Sierra 2 », etc.

- **Rangs d'équipe (mode Zombie)** : le rang dépend des zombies tués par l'équipe. I Bronze : 20 ; II Argent : 60 ; III Or : 120 ; IV Diamant : 210. Chaque palier demande plus que le précédent (+20, +40, +60, +90). Bonus pour toute l'équipe : I : +8 % vie, +5 % cadence de tir, +5 % vitesse ; II : +12 %, +8 %, +8 % ; III : +15 %, +15 %, +15 % ; IV : +20 %, +20 %, +20 %. La vie gagnée s'ajoute tout de suite. Chaque rang a sa forme d'écusson : I Bronze un bouclier arrondi (sans brillance), II Argent un écusson à pointe, III Or un hexagone, IV Diamant un losange. Argent, Or et Diamant brillent, sans halo autour : reflet en haut et éclat lumineux qui les traverse de temps en temps. L'insigne du rang est visible à gauche de la barre de vie de chaque joueur, à côté de la vie dans l'ATH, sur les pastilles d'équipe et dans le tableau (TAB). La montée de rang est annoncée.
- **ATH du haut refait** : une première carte centrée avec la vague, les zombies en vie au centre, le minuteur et les 5 équipes (joueurs en vie + insigne de rang). Une deuxième carte montre le rang de l'équipe, la progression vers le rang suivant et les bonus (vie, cadence de tir, vitesse) ; sans rang, ce sont ceux du rang I, en gris. Sur mobile, l'ATH du haut est réduit d'environ 25 % (236 px de large au lieu de 300, 89 px de haut au lieu de 108). Avec les bots, on atteint Bronze vers la vague 6, Argent vers la 11, Or vers la 16 et Diamant vers la 21.

- **AR (mode Zombie)** : le fusil d'assaut est remplacé par l'AR de la carte Neige. Même modèle (corps sombre, repère rouge, canon rond), raccourci d'environ un tiers, en main et au sol, avec une nouvelle icône. Il a les effets de tir de la Neige : tracé orange lumineux, éclair au bout du canon, petits nuages d'impact. Il fait 32 dégâts par balle au lieu de 22, donc plus que le pistolet (28), car il est plus rare. Cadence et chargeur inchangés.

- **Moins de texte à l'écran (mode Zombie)** : plus de messages pour la zone qui se resserre (« Brouillard toxique », « La zone se resserre »), ni pour les alliances (propositions, alliance conclue, fin, trahison). Les sons sont gardés, et le bouton d'alliance s'allume toujours sur mobile. Seul l'avertissement « HORS DE LA ZONE ! » reste, et seulement quand on est dehors.

- **AR** : +15 % d'apparition (poids 8→9,2 dans les maisons, 5→5,75 dans les voitures).
- **Visée tactile (mobile)** : portée plus longue, 680 px avec le pistolet ou l'AR (460 avant) et 620 px avec l'arbalète. Un ennemi (joueur) dans la direction visée devient la cible principale, devant les zombies.
- **Fin de partie** (vague 20, 2e resserrement du brouillard, ou 3 équipes au plus) : les bots attaquent. Ils voient plus loin, ne fuient plus les joueurs, tirent sur les ennemis avant les zombies, sauf un zombie collé, et vont chercher l'équipe ennemie la plus proche dans la zone sûre. Sur 8 parties entre bots : morts tués par un joueur 21 → 61, morts dans le brouillard 26 → 2, parties d'environ 21 min au lieu de 24.
- **Essence au sol** : maintenir l'action recharge le briquet (+1:30) sans passer par le sac ; un appui court la ramasse.

- **Vagues toutes les 50 s** (au lieu de 60). **Bois** : +15 % d'apparition en forêt (poids 30 → 34,5).

- **Fusil à pompe (mode Zombie)** : nouvelle arme. 8 plombs en éventail, 18 dégâts chacun à bout portant, 50 % en bout de course (portée d'environ 290 px). Chaque plomb traverse un premier zombie, ce qui en fait une arme de zone contre la horde : un rôdeur meurt en un coup de près. Contre un joueur, les plombs font 60 % de leurs dégâts. 6 cartouches (les munitions normales), un tir toutes les 0,8 s, rechargement en 2,4 s, très bruyant. Il a son modèle en main et au sol, son icône et un son de pompe. On le trouve dans les maisons (poids 5, environ 4 %) et les voitures (poids 4, environ 3,6 %). Les bots le prennent et s'approchent pour tirer.
- **Une arme en réserve dans le sac** : en ramassant une arme alors qu'on en tient déjà une, celle-ci va dans une case libre du sac au lieu de tomber au sol. Une seule arme peut y être rangée à la fois, sinon elle tombe comme avant. Toucher la case (ou sa touche sur PC) échange l'arme rangée avec celle en main. Lâcher la case pose l'arme entière au sol, avec ses balles.
- **Chargeur étendu** : nouvelle recette pour 2 métal. Il s'applique à l'arme en main (pistolet, AR ou fusil à pompe), une seule fois par arme, et donne +50 % de balles (pistolet 18, AR 45, fusil à pompe 9). Il reste sur l'arme au sol, dans le sac et dans le sac d'un joueur mort. Une pastille jaune « + » le signale sur l'arme, et l'étiquette au sol indique « chargeur étendu ». Les bots le fabriquent quand ils ont 2 métal.
- **Largage de ravitaillement (rare)** : à partir de la vague 5, 9 % de chance par vague, 5 vagues d'écart au moins, 2 par partie au plus. Sur 8 parties entre bots, il y en a eu environ 0,75 par partie. Un avion passe avec ses feux de position, et une caisse descend en parachute en 16 s, avec son ombre et le compte à rebours au point d'atterrissage. Elle tombe sur un sol libre, dans la prochaine zone sûre si le brouillard est là. Elle est repérée sur la mini-carte et par une flèche au bord de l'écran, et une fusée rouge l'éclaire. Elle s'ouvre comme un sac (« RAVITAILLEMENT ») et contient un AR ou un fusil à pompe chargé (une fois sur deux avec chargeur étendu), 40 à 60 munitions, 2 ou 3 bandages, 1 ou 2 grenades, 2 métal, de l'eau ou des snacks, parfois de l'essence. Son atterrissage attire les zombies. Les bots vont la chercher.

- **Base Polaire de nuit : reflet des lampes sur les objets** : les lampadaires, projecteurs, portes, fenêtres et tentes éclairent maintenant les arbres (aiguilles, écorce, neige sur les branches), les rochers, les murs, les véhicules, les caisses et les poteaux, et pas seulement le sol. Chaque objet lit la carte de lumière du sol juste devant ses faces : la face tournée vers une lampe prend sa couleur (orangée pour les lampadaires, blanche pour les projecteurs), la face opposée reste dans l'ombre portée de l'objet, et un objet caché derrière un autre reste sombre. La lumière baisse avec la hauteur, et les toits ne la prennent pas. Il n'y a aucune lumière en plus : une seule texture est lue dans le shader. La carte de jour et les autres cartes ne changent pas.

- **Forêt : deux voitures qui se chevauchaient** (au centre de la carte, vers x 3500-3600) : la deuxième est déplacée à côté, sur un sol libre (3770, 2945). Les collisions, la fouille et le butin suivent.
- **Bouton arme (mobile, mode Zombie)** : un nouveau bouton à côté de R passe à l'arme suivante : arme en main → arme de rechange → arme rangée dans le sac → poignard. Son icône montre l'arme qu'on va prendre. La touche Q sur PC suit le même cycle et inclut maintenant l'arme rangée dans le sac.
- **Boutons mobiles réorganisés en arc autour de R** : arme à gauche, grenade en diagonale, action (ramasser, fouiller…) au-dessus, briquet un peu plus loin, alliance au-dessus de la grenade. Les boutons sont espacés pour que le pouce les atteigne sans en toucher deux.

- **Base Polaire en 6 contre 6** (jour et nuit, mode Code Rouge) au lieu de 5 contre 5, pour mieux équilibrer l'attaque et la défense. Il y a 12 places par salon et une apparition de plus par camp. L'ATH (pastilles A1 à A5 / E1 à E6), le tableau des scores, le lobby et le menu affichent 6 contre 6. Contre l'IA, chaque équipe a aussi 6 joueurs. `server.js` change : `MAP_SIZE.neige` et `MAP_SIZE.neigeN` passent de 5 à 6, il faut le redéployer.

- **V3.6 · étape 1 : nouvelle carte de guerre « FRONT » (8 contre 8)**. La carte est symétrique : les deux camps sont exactement équivalents.
  - Une rivière en S traverse la carte du nord au sud. On ne peut pas la franchir à pied, mais les balles et la vue passent au-dessus.
  - Il y a 3 ponts routiers (nord, centre, sud), qui seront destructibles au C4 à l'étape 2, et 2 passerelles en bois pour l'infanterie.
  - Une ligne de tranchées en zigzag protège chaque camp, avec un parapet en sacs de sable côté ennemi et des caillebotis. Des tranchées avancées longent la rivière.
  - Le no man's land contient des herissons tchèques et des barbelés (on ne les franchit pas, les balles passent), des cratères d'obus, des arbres brûlés, des épaves de chars et de camions en feu, et des villages en ruine.
  - Chaque camp a un QG en béton, des tentes, des caisses de munitions, des fûts, des camions, des miradors, des canons sous filet de camouflage et des drapeaux aux couleurs de l'équipe (ACIER contre CENDRE).
  - L'ambiance est chaotique : obus qui tombent au loin (décor, sans dégâts), fumées, feux, rafales et canons au loin.
  - Les emplacements des cibles (lance-missiles et antennes satellite), du tank et du 4x4 sont déjà réservés pour l'étape 2.
  - En attendant le mode Escorte, la carte se joue en match à mort à 8 contre 8 (premier à 80).
  - `server.js` change : `MAP_SIZE.guerre = 8` (16 places).

- **V3.6 · étape 2 : mode « FRONT » (escorte du tank), 8 contre 8**. Contre l'IA comme en ligne, la carte Front se joue maintenant dans ce mode (plus en match à mort).
  - **But** : chaque équipe escorte son tank jusqu'au camp adverse pour détruire au canon les 4 cibles ennemies. Les cibles sont des stations d'antenne satellite (A1, A2…) et des lance-missiles (B1, B2…). Elles sont tirées au sort à chaque manche, dans 4 des 8 emplacements du camp, avec un mélange au hasard de A et de B. Il faut 6 obus pour détruire une cible (6 barres de vie).
  - **Manche** : elle dure 6 min et la première équipe à 4 manches gagne. Détruire les 4 cibles adverses donne la manche tout de suite. Au chrono, l'équipe qui a détruit le plus de cibles gagne ; en cas d'égalité, celle qui a fait le plus de dégâts, puis celle qui a atteint ce score en premier. On réapparaît 5 s après sa mort.
  - **Tank** (piloté librement par un joueur) :
    - il est lent et plus grand, avec un nouveau modèle détaillé (tourelle, long canon, jupes, trappe du chef) ;
    - son conducteur est invincible et le tank ne craint pas les grenades ;
    - le tireur, debout dans la trappe, peut être visé et tué ;
    - le canon tire des obus explosifs et recharge en 8 s ;
    - le tank écrase les ennemis à pied et pousse un 4x4 qui lui barre la route.
  - **4x4 blindé**, façon Humvee : une mitrailleuse en tourelle, un tireur exposé, une barre de vie visible au-dessus. Il résiste à 4 grenades, un obus en enlève 2 et le C4 le détruit. Il réapparaît au garage 20 s après sa destruction.
  - **Commandes** :
    - **E**, ou le bouton d'action sur mobile : monter, passer au canon, descendre.
    - **E maintenu** : poser le C4.
    - Si un bot occupe une place, le joueur la prend : le bot passe au canon ou descend.
  - **Équipement** : 1 C4 et 1 grenade par vie.
    - **3 C4 sur un grand pont (1 sur une passerelle)** le font exploser 8 s plus tard (au départ il en fallait 4). Le pont reste détruit jusqu'à la fin de la manche : l'eau le remplace et plus personne ne passe. Il faut donc s'organiser en équipe.
    - **C4 sur le tank ennemi** : il est immobilisé 4 s. Ajout d'équilibrage : on ne peut pas reposer de C4 sur ce tank pendant les 3 s qui suivent, pour éviter qu'il reste bloqué en permanence.
  - **La carte Front est plus petite** : 4200 × 2800 (elle faisait 7600 × 4800 à l'étape 1), donc on atteint le camp adverse beaucoup plus vite. Elle est aussi plus chargée :
    - des hameaux en ruine ;
    - des rangées de blocs de béton ;
    - plus d'épaves, de hérissons, de barbelés, de nids de sacs de sable et de dépôts de caisses ;
    - des camps mieux remplis.
    Les ponts routiers sont élargis pour le tank. Le générateur vérifie que le tank et le 4x4 peuvent rejoindre tous les ponts et toutes les cibles adverses.
  - **ATH** :
    - sous le score, l'état des 4 cibles de chaque camp avec leurs points de vie ;
    - au-dessus des cibles et des véhicules, leur nom, leur vie et leur état ;
    - au bord de l'écran, des flèches vers les cibles ennemies et vers votre tank ;
    - sur les ponts, les C4 posés et le compte à rebours ;
    - un panneau du véhicule (rechargement du canon, blindage du 4x4), le compteur de C4, des bandeaux pour les événements (cible détruite, pont détruit…) et une carte de fin de manche ;
    - sur la mini-carte : les cibles, les véhicules et les ponts minés ou détruits.
  - **Bots** : ils ont des rôles (équipage du tank et du 4x4, escorte, défense). Le tank bot choisit un pont différent de celui du tank ennemi, s'arrête à portée pour tirer et contourne les blocages. Les défenseurs vont au-devant du tank ennemi pour tuer le tireur et poser le C4.
  - L'état du mode passe par le réseau : cibles, véhicules, obus, ponts, C4 et effets.
  - **Textures et décor retravaillés** :
    - **Ponts routiers** : tablier en dalles de béton avec joints, chaussée usée (traces de pneus, ligne centrale effacée, rustines, fissures, flaques), bordures, et un vrai treillis d'acier riveté et rouillé avec des contreventements au-dessus. Les piles sont tachées par l'eau.
    - **Passerelles** : planches de bois veinées et clouées, avec des fentes, sur des pilotis.
    - **Pont détruit** : poutres tordues et chaussée effondrée dans l'eau.
    - **Station d'antenne** : refaite en version militaire, voir plus bas (« Station d'antenne satellite plus militaire »).
    - **Lance-missiles** : roues sculptées avec jantes, châssis en acier, réservoirs, cabine vitrée avec calandre, vérins de calage déployés, rampe de lancement levée par deux vérins. Le missile porte des repères de roulis, des inscriptions et une ogive.
    - **Peinture des véhicules** : lignes de panneaux, rivets, coulures et boue en bas de caisse.
    - **Sol** : texture de détail beaucoup plus fine (cailloux en relief, touffes d'herbe sèche, boue sèche fissurée, brindilles) sur deux échelles pour casser la répétition. Plaques d'herbe sèche et de terre humide, traces de chenilles dans les camps.
    - **Arbres morts** : environ 90 grands arbres secs aux branches nues (parfois cassés), en plus des souches brûlées.
  - **Mitrailleuse du 4x4** : nouveau son de mitrailleuse lourde, plus grave et plus puissant. Il combine le claquement du départ, le souffle de la bouche, un coup sourd dans les basses, le bruit métallique de la culasse, la queue qui résonne et, de temps en temps, une douille qui tinte. Il s'assourdit avec la distance et se place à gauche ou à droite. En ligne, les autres joueurs l'entendent aussi (seules les balles de mitrailleuse portent un marqueur en plus).
  - **Ruines variées** : 6 styles de murs (brique rouge, brique jaune, pierre, crépi écaillé, parpaings, béton). Chaque hameau garde son style. Les murs portent de la suie, des impacts, des fissures et des traces d'humidité, et leurs arêtes cassées sont noircies. Des poutres calcinées et des tuiles tombées traînent dans les gravats.
  - **Camps près du spawn** :
    - tentes en 3 toiles (olive, kaki, vert-gris) avec pignons, plancher, mât et haubans ;
    - caisses en bois et caisses de munitions marquées, sous des bâches ;
    - fûts peints et rouillés avec couvercles ;
    - camions militaires détaillés : cabine vitrée, calandre, roues à jantes, bâche tendue sur des arceaux ;
    - miradors en rondins avec toit en tôle ;
    - bunker en béton banché avec meurtrière et porte blindée ;
    - canons d'artillerie.
  - **Bots plus malins face aux véhicules** :
    - **Esquive** :
      - quand un tank fonce sur eux, les bots s'écartent d'un pas de côté ; s'il est encore loin, ils fuient en diagonale (le tank, plus lent, doit tourner) ;
      - ils réagissent parfois en retard, donc on peut encore en écraser ;
      - pour poser le C4, ils abordent le tank ennemi par le flanc ou par l'arrière, plus par l'avant ;
      - le tank n'écrase plus qu'avec l'avant de la caisse : un soldat frôlé sur le côté est seulement bousculé.
    - **Conduite** :
      - les trajectoires sont tendues, sans zigzag ;
      - le tank va se placer face à sa cible, avec une vue dégagée, au lieu de foncer vers son centre, et ralentit à l'approche ;
      - les fausses marches arrière au départ et à la sortie des ponts ont disparu ;
      - le 4x4 roule de concert avec le tank et le contourne au lieu de le bloquer.
    - **Ligne de tir** :
      - le mitrailleur du 4x4 et le canon du tank ne tirent plus si un véhicule (allié ou ennemi) ou un mur se trouve entre eux et leur cible : ils changent de cible ou attendent ;
      - les véhicules bloquent aussi la vue : on peut se cacher derrière un tank.
    - **Mesures sur des parties entre bots** :
      - tirs de mitrailleuse bloqués par un obstacle : de 85 % à 13 % ;
      - manœuvres de dégagement des véhicules : de 34 à 0 en 6 minutes ;
      - quand un tank chasse les bots : 9 fois moins d'écrasements.
  - **Étape 3 : lance-missiles et antenne satellite** (chaque camp a au moins une antenne et un lance-missiles) :
    - **Lance-missiles (B)** :
      - E près de son lanceur ouvre le poste de tir ; on vise un véhicule ennemi (cadre jaune « VERROUILLÉ », ou flèche au bord de l'écran s'il est loin), puis un clic lance un missile guidé ;
      - sur le tank, le missile l'immobilise 8 s ; sur le 4x4, il fait les dégâts d'un obus (2 points de blindage) ;
      - un seul missile pour toute l'équipe toutes les 1 min 30 ; le missile est visible sur la rampe quand il est prêt ;
      - le missile monte, vire vers sa cible en laissant une traînée de fumée, puis plonge dessus.
    - **Antenne (A)** :
      - E près de son antenne révèle tous les ennemis pendant 5 s : losanges rouges à l'écran (même derrière les murs) et cercles sur la mini-carte ;
      - une onde radar part de l'antenne ; la recharge est de 1 min 30.
    - **ATH et sons** :
      - sous le score, l'état du missile et de l'antenne de son camp (prêt ou compte à rebours) et l'alerte « REPÉRÉS » quand l'ennemi utilise son antenne ;
      - au-dessus de ses propres cibles, « MISSILE PRÊT » ou « ANTENNE PRÊTE » ;
      - l'équipe visée reçoit l'alerte « MISSILE EN APPROCHE » avec des bips, un ⚠ au-dessus du véhicule et une flèche vers le missile ;
      - sons : départ du missile, impact, impulsion radar.
    - **Bots** : un défenseur va tirer le missile quand le tank ennemi passe la rivière ou approche des cibles ; un autre active l'antenne quand des ennemis approchent.
    - **En ligne et sur mobile** :
      - missiles, recharges et révélation passent par le réseau ;
      - sur mobile, le bouton d'action devient MISSILE, ANTENNE ou QUITTER, et on tire avec le bouton de tir.
  - **Toits des véhicules** (plus de surfaces plates unies) :
    - **4x4** :
      - toit en tôles camouflées avec coutures, rangées de boulons, bandes antidérapantes, trappe arrière et numéro peint ;
      - capot à persiennes ;
      - couronne de tourelle en acier boulonnée, rampe de feux orange et rails de toit ;
      - plancher arrière en tôle larmée avec jerricans, bâche roulée et caisse.
    - **Tank** :
      - dessus de caisse et de tourelle en tôles soudées et boulonnées, avec antidérapant, poussière et rayures ;
      - câbles de remorquage, pelle et pioche, jerricans, coffre arrière et bâche roulée sur la tourelle.
    - **Lance-missiles** : toit de cabine avec trappe, climatiseur et gyrophare.
    - **Camions du camp** :
      - toit de cabine avec coutures, boulons, anneau de trappe et gyrophare ;
      - plateau en tôle larmée.
    - **Épaves de camions et de chars** : tôles calcinées (rouille, cloques, trous d'éclats, cendre), capot arraché et trappe ouverte.
  - **Garde-corps des ponts** :
    - les véhicules ne passent plus à travers : la collision suit maintenant le contour de la caisse, coins compris, face à l'eau et aux garde-corps ;
    - les garde-corps sont solides sur toute la longueur des ponts, y compris au-dessus des berges ;
    - testé en fonçant dans les garde-corps sous 6 angles : la caisse ne dépasse plus (0 px).
  - **Missile contre les cibles** :
    - le lance-missiles peut aussi viser les antennes et les lance-missiles ennemis : un missile leur retire 1 case de vie ;
    - le défenseur concerné reçoit l'alerte « MISSILE EN APPROCHE : votre B1 ! » ;
    - quand aucun véhicule ne menace, les bots s'en servent sur la cible ennemie la plus abîmée, un lance-missiles de préférence.
  - **Ponts** :
    - il faut maintenant 3 C4 pour un grand pont et 1 seul pour une passerelle (affichage « C4 n/3 » ou « n/1 ») ;
    - les bots font sauter :
      - le grand pont près duquel ils repèrent le tank ennemi ;
      - en début de manche (7 fois sur 10), un grand pont que leur propre tank n'emprunte pas ;
      - une passerelle empruntée par des ennemis à pied ;
      - un pont déjà en partie miné, qu'ils terminent.
    - Ils ne détruisent jamais le dernier grand pont intact, ni celui que leur tank va prendre.
    - Ils quittent un pont piégé, et le tank bot change d'itinéraire si son pont est piégé.
  - **Véhicules ennemis repérés** :
    - le tank et le 4x4 ennemis n'apparaissent sur la mini-carte que si un allié les voit directement (centre ou coin de la caisse en vue), s'ils viennent de tirer, ou pendant la révélation de l'antenne ;
    - même règle pour le verrouillage du missile et pour les décisions des bots ;
    - en ligne, le serveur envoie à chaque équipe ce qu'elle a repéré.
  - **Lance-missiles tournés vers l'ennemi** : ils sont toujours posés dans le sens de la longueur, rampe et missile pointés vers le camp adverse (plus aucun lanceur tourné vers le sud ou le nord). Le missile part dans cette direction.
  - **Fusil d'assaut rangé dans les véhicules** :
    - dans le tank, le 4x4 et au poste du lance-missiles, le fusil du soldat ne s'affiche plus : il ne dépasse plus de la tourelle ni de la mitrailleuse ;
    - l'éclair de tir apparaît maintenant à la bouche de la mitrailleuse du 4x4.
  - **Second 4x4 blindé** par camp :
    - il est garé de l'autre côté du tank ;
    - le premier 4x4 escorte le tank ; le second défend les cibles : il intercepte les véhicules ennemis repérés et les soldats ennemis qui approchent, sinon il patrouille autour des cibles, sans traverser la rivière (il choisit un point de patrouille qu'il peut atteindre, il ne reste plus bloqué au garage) ;
    - les bots lui affectent un conducteur et un tireur (après le premier défenseur) ;
    - les 4x4 contournent le véhicule le plus proche pour ne pas se bloquer entre eux.
  - **Sons des véhicules** (refaits ; continus, spatialisés : volume selon la distance, gauche ou droite selon la position) :
    - **Moteurs** : le bruit est construit explosion de cylindre par explosion de cylindre, comme un vrai moteur, au lieu d'un simple bourdonnement. Deux enregistrements de synthèse par moteur (ralenti et haut régime) sont mélangés selon le régime ; le son s'ouvre (plus de mordant) quand on accélère.
    - **Tank** : diesel 6 cylindres lent et lourd (ralenti vers 700 tr/min, 2000 au maximum), sifflement du turbo qui monte avec le régime, grondement grave en roulant, moteur électrique de la tourelle. Plus aucun bruit de chenilles (ni claquement des patins, ni grincement en virage).
    - **4x4 blindé** : moteur 8 cylindres en V au battement irrégulier, plus nerveux (jusqu'à 3300 tr/min), passages de rapports, roulement des pneus et crissement du gravier, moteur de tourelle.
    - **Démarrage et extinction** : démarreur plus discret quand un conducteur s'installe ; le moteur s'éteint doucement quand il descend ; ralenti à l'arrêt.
    - **Chocs** : un véhicule qui percute un obstacle à bonne vitesse fait un choc de tôle (moins fort qu'avant), avec un peu de poussière.
    - **Volume nettement baissé** (mesuré par rapport à une rafale de mitrailleuse du 4x4) :
      - tank qui roule tout près : environ 13 dB en dessous de la mitrailleuse (au début : 6 dB au-dessus) ;
      - tank au ralenti : 22 dB en dessous ; 4x4 qui passe tout près : 12 dB en dessous ;
      - on entend un tank jusqu'à 1400 px et un 4x4 jusqu'à 1100 px, le son baisse vite avec la distance ;
      - à l'intérieur d'un véhicule, son propre moteur est un peu atténué.
    - **Réglages** :
      - le timbre est réglé pour rester audible aussi sur les haut-parleurs de téléphone (peu d'infrabasses) ;
      - les sons se coupent quand l'onglet est caché ou hors du mode Front.
  - **Explosion des véhicules détruits** :
    - **4x4** : son et souffle puissants. Dégâts à tous ceux qui sont proches, alliés compris, jusqu'à 150 px du bord du véhicule : environ 90 PV au contact, 65 à 90 px, 37 à 140 px.
    - **Carburant du 4x4** : il se rallume 1 à 2 s plus tard (seconde explosion plus petite, jusqu'à 60 PV). Il vaut mieux s'éloigner de l'épave.
    - **Lance-missiles détruit** : souffle jusqu'à 190 px (130 PV au maximum), puis le carburant du missile explose une seconde fois.
    - **Antenne détruite** : souffle jusqu'à 150 px (100 PV au maximum).
    - **Protection** : un mur ou un véhicule entre l'explosion et soi protège ; les conducteurs restent à l'abri du blindage ; un 4x4 tout proche est abîmé.
    - **Le kill** est attribué à celui qui a détruit le véhicule.
  - **Traces de pneus et de chenilles** :
    - **Tank** : deux larges bandes de terre tassée, avec un bourrelet sur les bords et la marque des patins tous les quelques pixels ; dans les virages, les traces suivent la courbe.
    - **4x4 blindé** : même système que les camionnettes de la map Neige, en couleurs de terre :
      - une trace par roue : les roues arrière marquent franchement, les roues avant plus légèrement ; en virage, les traces avant s'écartent un peu des traces arrière ;
      - creux tassé aux bords adoucis, sculptures en chevrons qui se raccordent d'un bout de trace à l'autre, fine crête de terre repoussée sur les bords ;
      - en marche arrière, les chevrons gardent leur sens par rapport au 4x4.
    - **Résolution** : sur PC, les traces sont dessinées en pleine résolution (les sculptures restent nettes) ; sur téléphone, en demi-résolution pour économiser la mémoire.
    - Les traces disparaissent en 10 s au plus : elles restent nettes quelques secondes, puis s'effacent en fondu ; tout est effacé à chaque nouvelle manche.
    - Pas de traces sur les ponts (tablier en dur) ; marche arrière comprise.
  - **Essence des 4x4 blindés** (contre le camping) :
    - 1 min 30 d'essence à plein régime. Elle baisse dès que quelqu'un est à bord ; à l'arrêt, le moteur au ralenti consomme 40 % (s'arrêter économise l'essence).
    - Jauge en secondes dans le panneau du conducteur et du tireur (« presque à sec » sous 22 s) ; petite jauge jaune sous la barre de vie des 4x4 alliés.
    - À sec : le moteur tousse et cale, la mitrailleuse n'a plus de courant. L'équipage descend au bout d'une seconde (joueurs compris) et personne ne peut plus y remonter : on combat à pied.
    - Le 4x4 vide reste sur place (« ⛽ À SEC · garage dans N s » au-dessus, gris sur la mini-carte) ; le tank peut le pousser.
    - Pas de plein sur le terrain : **45 s après la panne, le 4x4 réapparaît à son garage avec le plein** (un 4x4 détruit revient, lui, au bout de 20 s), et chaque manche repart avec le plein.
    - Le tank n'a pas d'essence.
  - **Réglages** :
    - rechargement du canon du tank : 8 s (au lieu de 4) ;
    - antennes et lance-missiles : 6 barres de vie, donc 6 obus pour les détruire (au lieu de 4) ; le missile en enlève toujours une ;
    - essence du 4x4 : 1 min 30 à plein régime (au ralenti : 40 % de la consommation).
    - balles 20 % plus rapides (fusils et mitrailleuse du 4x4 : 1152 px/s au lieu de 960), portée inchangée ; vaut pour toutes les cartes.
    - Le tank bot reste en position tant qu'il tire (il ne repart que si rien n'a été tiré depuis un rechargement + 3 s).
  - **Faire descendre les ennemis d'un véhicule** :
    - à pied, contre un véhicule ennemi (tank ou 4x4) arrêté ou presque (moins de 40 px/s) qui a un équipage : **maintenir X 2 s** fait descendre tout l'équipage, joueurs comme bots ;
    - ils ne peuvent pas remonter dans ce véhicule pendant 8 s (message pour le joueur). Pendant les 2 s, on est exposé au tireur sur le toit, et un véhicule qui repart annule tout ;
    - sur téléphone : maintenir le bouton d'action quand on n'a plus de C4 (avec un C4, le bouton pose le C4) ;
    - les bots le font aussi : sans C4, ils viennent sur le flanc d'un véhicule ennemi arrêté et font descendre l'équipage ; les défenseurs le font contre un tank ennemi arrêté près des cibles.
  - **Barrières de protection des camps** :
    - une rangée de « dents de dragon » (pyramides de béton) ferme chaque zone d'apparition (600 px depuis le bord de la carte, de y 1060 à 1740) : les soldats passent entre les blocs, **aucun véhicule ne passe** ;
    - trois portails, là où sortent les routes principales : la barrière se lève pour les véhicules du camp et reste fermée aux véhicules ennemis ;
    - les bots en tiennent compte dans leurs trajets ; barrières et portails sont dessinés sur la mini-carte (portails verts chez soi, rouges chez l'ennemi).
  - **Anti-camping des joueurs (touche X)** :
    - dans un véhicule (tank ou 4x4), **X** fait descendre tout de suite, quelle que soit la place ;
    - à pied, **maintenir X 1,2 s** près d'un véhicule allié où se trouve un joueur fait descendre les joueurs à bord (les bots ne sont pas concernés : on les remplace déjà en montant) ; ils ne peuvent pas remonter dans ce véhicule pendant 15 s et un message les prévient ;
    - sur téléphone (pas de touche X) : maintenir le bouton d'action près d'un véhicule allié plein dont un joueur occupe une place ;
    - aide à l'écran (« Maintiens X · faire sortir le joueur du 4x4 »), barre de progression, et « X : sortir » dans le panneau du siège.
  - **4x4 40 % plus lent** : 159 px/s au lieu de 265 (accélération réduite d'autant).
  - **4x4 bots économes** : ils s'arrêtent au lieu de tourner en rond.
    - Le 4x4 de défense ne tourne plus autour de la menace : il prend un poste fixe avec vue sur elle (le tireur arrose) et n'en change que si la menace se déplace de plus de 250 px, si la vue est bouchée ou au bout de 20 s.
    - Postes de patrouille tenus 12 s (au lieu de 2,5), postes de raid 10 s (et tant que le tireur tire), postes de l'éclaireur 8 s.
    - Tank qui tire : l'escorte reste sur un flanc au lieu de faire des allers-retours toutes les 9 s.
    - Garde-fou : un 4x4 qui a roulé plus de 700 px en 12 s sans s'éloigner de plus de 200 px s'arrête 6 s et change de but.
  - **Bots : routes, escorte, défense** (mesures sur des parties de bots de 6 min) :
    - **Routes** : les véhicules suivent une carte de conduite où les routes principales et les grands ponts coûtent 1, le bord de route 1,7 et le hors-piste 3 (tank 2,4) ; un raccourci en ligne droite n'est pris que s'il ne sort pas de la route. Part du trajet sur la route : 4x4 environ 63 % → 73 %, tank environ 82 % → 90 %.
    - **Moins de zigzags** : cap lissé, but gardé tant qu'il bouge de moins de 110 px, contournement seulement du véhicule qui est devant. Inversions du volant : 4x4 environ −30 %, tank environ −60 %.
    - **Variantes d'escorte**, tirées pour chaque équipe à chaque manche et rappelées sur la carte d'intro (« Escorte des bots… ») ; le rôle s'affiche sur les 4x4 alliés (TÊTE, QUEUE, FLANC, ÉCLAIREUR, DÉFENSE) :
      - convoi : un 4x4 devant le tank, sur la route qu'il va prendre (il le double par le côté libre au départ), un derrière, exactement sur sa trace ;
      - éclaireur : un devant, un qui part en repérage ;
      - garde : un sur le flanc, un qui défend les cibles ;
      - reco : un en repérage, un derrière.
    - **Éclaireur** : il file vers le camp ennemi par les routes (sortie des grands ponts), puis observe les cibles ennemies de loin. Il repère les ennemis jusqu'à 1150 px : les véhicules apparaissent sur la mini-carte de son équipe, et les bots connaissent la position des soldats.
    - Un 4x4 qui se trouve dans le passage du tank se range sur le côté (avant, le tank pouvait rester coincé derrière son propre 4x4). Tank arrêté parce qu'il est bloqué (et pas pour tirer) : l'escorte garde sa place sur la route.
    - **Défense** :
      - deux défenseurs dès le départ (au lieu d'un), postés face à la rivière (d'où viennent les ennemis) et regardant dans cette direction ;
      - l'équipage du 2e 4x4, une fois son 4x4 hors d'usage, défend les cibles ;
      - **menace** = tank ennemi repéré ou entendu de notre côté à moins de 1500 px d'une cible, ou soldats ennemis connus à moins de 1000 px. Les bots libres de notre côté les plus proches viennent en renfort : 4 contre un tank, jusqu'à 5 en tout ;
      - contre le tank : avec un C4, on arrive par l'arrière ; sinon on se place à 300 px, de biais, pour abattre le tireur sur le toit. Contre les soldats : on va sur leur dernière position connue ;
      - banc d'essai (tank ennemi en position de tir près d'une cible) : défenseurs autour du tank en moyenne 0,6 à 3,8 → 2,4 à 3,6 ; C4 posés sur le tank en 75 s : 0 à 5 → 5 à 6.
  - **Conduite des bots** (mesurée sur des parties de bots de 8 min) :
    - **4x4 immobiles** : un 4x4 occupé par des bots restait arrêté 57 à 75 % du temps (à attendre à côté d'un tank sans équipage, planté à son point de patrouille, ou sans chemin quand le tank barrait la route). C'est maintenant moins de 3 %.
    - **Escorte fluide** : le 4x4 roule à la vitesse du tank avec un rappel doux vers sa place, et garde la même place tant qu'elle est libre (plus de saut d'un flanc à l'autre). Les à-coups sont divisés par 3 environ, les coups de volant par 4.
    - **Tank arrêté (il tire)** : le 4x4 se met sur son flanc, hors de la ligne du canon, et change de position toutes les 9 s.
    - **Tank sans conducteur** (équipage mort) : l'escorte n'attend plus à côté, elle part harceler les défenseurs près des cibles ennemies.
    - **4x4 de défense** : patrouille de point en point autour de nos cibles. Quand un ennemi approche, il tourne autour de lui (le tireur arrose) au lieu de rester arrêté. Il ignore les ennemis de l'autre côté de la rivière.
    - **Route barrée** (tank sur un pont, véhicule en travers) : le 4x4 prend un autre chemin ou change de but. Demi-tour en marche avant quand il y a la place, au lieu de longues marches arrière. S'il n'approche plus de son but pendant 5 s, il en change.
    - **Conducteur seul** : il va chercher son tireur qui arrive à pied.
    - **Tank** : un bot seul au canon reprend le volant quand il n'a plus rien tiré depuis 6 s, puis cherche une autre position. Le tank garde sa cible jusqu'au bout (plus d'allers-retours entre deux cibles).
    - **Dernier recours contre le camping** : un véhicule de bots qui n'a ni bougé ni tiré depuis 25 s change de position ; au bout de 3 fois, les bots descendent et ne remontent pas avant 20 s.
  - **4x4 à sec : retour au garage en 45 s** :
    - quand un 4x4 n'a plus d'essence, l'équipage descend comme avant ; 45 s après la panne, le 4x4 disparaît et réapparaît à son garage, plein fait et réparé, prêt à repartir ;
    - au-dessus du 4x4 allié à sec : « ⛽ À SEC · garage dans N s » ; si la place du garage est occupée, il réessaie toutes les 2 s ; détruit pendant l'attente, il revient comme un 4x4 détruit (20 s).
  - **4x4 encore 15 % plus lent** : 135 px/s au lieu de 159 (accélération 214 au lieu de 252).
  - **Station d'antenne satellite plus militaire** (même emprise de 150 × 150, mêmes règles) :
    - enceinte de bastions HESCO (gabions de géotextile sable dans leur grillage) coiffée de barbelés concertina, entrée côté ennemi avec barrière levante rouge et blanche ;
    - **grande parabole SATCOM** vert olive de 112 de diamètre (au lieu de 106, blanche) sur une vraie tourelle : socle béton, tourelle d'azimut, fourche d'élévation avec moteur et contrepoids, structure arrière à 12 nervures, cornet d'alimentation et sous-réflecteur Cassegrain sur 4 bras ;
    - **pylône en treillis de 140** (au lieu d'un mât de 96) : trois jambes qui s'affinent, croisillons, deux plateformes, trois grandes antennes secteurs, deux faisceaux hertziens, balises rouges, et **un radar de veille qui tourne au sommet** (il s'arrête quand la station est détruite) ;
    - **radome géodésique** sur son socle ;
    - **abri de transmissions** (conteneur militaire nervuré « SATCOM ») sous un **filet de camouflage**, avec une antenne log-périodique HF en arête de poisson sur son toit et un fouet VHF ;
    - groupe électrogène derrière des sacs de sable, fûts de carburant, chemins de câbles entre les équipements ; la bande de couleur de l'équipe court sur le bord de la dalle, autour de l'abri et du radome.
  - **Interface du Front simplifiée** (pour qu'un nouveau joueur s'y retrouve) :
    - **couleurs relatives partout** : bleu = votre camp, rouge = l'ennemi (étiquettes, mini-carte, flèches, cartes des joueurs ennemis en haut), quel que soit votre camp (ACIER ou CENDRE) ; les bandes de couleur des antennes et lance-missiles suivent la même règle ;
    - **contour au sol** autour de chaque cible : rouge et pulsé autour des cibles ennemies, bleu autour des vôtres ; il disparaît quand la cible est détruite ;
    - **étiquettes en clair** au lieu des codes A1 / B2 : « VOTRE ANTENNE », « ANTENNE ENNEMIE », « VOTRE LANCE-MISSILES », « LANCE-MISSILES ENNEMI », avec une icône (parabole, missile), une barre de vie et, chez vous, l'état (« MISSILE PRÊT », « antenne dans 0:45 ») ; les cibles ennemies ont des repères de visée autour de l'icône ;
    - **véhicules** : vos tank et 4x4 ont une petite étiquette (icône et deux ronds pour les places : plein = occupé), le 4x4 garde sa barre de blindage et sa jauge d'essence ; les véhicules ennemis ont un chevron rouge (et la barre de blindage du 4x4) ; le rôle des 4x4 bots (TÊTE, QUEUE…) n'est plus affiché ;
    - **bandeau sous le score** : vos 4 cibles (« À DÉFENDRE ») face aux 4 cibles ennemies (« À DÉTRUIRE 0/4 »), en icônes avec leur vie, une croix quand elles sont détruites ; dessous, l'état de votre missile et de votre antenne, et « REPÉRÉS ! » quand l'antenne ennemie vous révèle ;
    - flèches au bord de l'écran avec l'icône de la cible ou du tank au lieu d'un code ; mini-carte avec icônes ;
    - **intro de manche** réduite à l'essentiel : l'objectif en une phrase, une légende (votre tank en bleu, antenne et lance-missiles ennemis en rouge) et trois commandes (E : monter, E maintenu : C4, X : sortir) ; les détails (essence, escorte des bots, durées) sont retirés ;
    - **panneau des commandes** du Front réduit à 9 lignes courtes (il débordait) ; panneau du véhicule réorganisé (Blindage, Essence, Canon prêt / Rechargement alignés) ;
    - messages en clair : « Antenne ennemie détruite ! », « MISSILE EN APPROCHE : votre lance-missiles ! » ; pas d'étiquettes pendant le décompte (elles passaient sur la carte d'intro), et elles s'estompent quand elles passent sous le haut de l'écran.
  - **10 contre 10** (au lieu de 8 contre 8) :
    - deux points d'apparition de plus par camp (entre les apparitions et le tank) ; 20 soldats, étiquettes et grenades prévus pour 20 ;
    - bots de chaque camp : équipages du tank et des deux 4x4, 2 défenseurs, 2 « poseurs de C4 » qui partent vers les cibles ennemies ;
    - **`server.js` a changé** (taille du salon Front : 10 par camp) : il faut le redéployer avec `index.html`.
  - **C4 sur les antennes et les lance-missiles** (la défense compte davantage) :
    - au bord d'une antenne ou d'un lance-missiles ennemi, **maintenir E 2 s** (téléphone : bouton d'action « C4 ») pose le C4 ; il explose 3 s plus tard : **1 case de vie en moins** et dégâts aux ennemis tout autour ;
    - toujours 1 C4 par vie ; on ne peut pas poser plus de C4 en attente que la cible n'a de cases de vie ;
    - un compte à rebours s'affiche sur le C4 (jaune sur les cibles ennemies, rouge sur les vôtres) ; quand l'ennemi en pose un sur votre cible : message « C4 posé sur votre antenne ! », bip, cible qui clignote sur la mini-carte et flèche au bord de l'écran ;
    - les bots « poseurs de C4 » vont poser le leur sur les cibles ennemies, et les autres bots à pied le font quand une cible ennemie est à moins de 550 px ; les défenseurs abattent les ennemis qui approchent de leurs cibles.
  - **Lance-missiles et antenne : recharge de 45 s** (au lieu de 1 min 30) ; **le missile retire 2 cases** à une antenne ou un lance-missiles (au lieu d'une ; le tank reste immobilisé 8 s).
  - **Plus de cartes de joueurs sous le tableau des scores** dans le Front : le bandeau des cibles remonte juste sous le score.
  - **Corrections (parties en ligne)** :
    - **sons du Front rétablis** : depuis la refonte de l'interface, plus aucun son propre au Front ne sortait (départ et impact des missiles, coup de canon du tank, bips du C4 et du pont piégé, impulsion de l'antenne, moteurs du tank et des 4x4). Un nom de couleur de l'interface masquait celui du système audio ; c'est réparé, en solo comme en ligne ;
    - **les messages à l'écran ont un signal sonore** : deux notes montantes pour une bonne nouvelle (cible ennemie détruite…), descendantes pour une mauvaise (votre 4x4 détruit, C4 sur votre cible…), une note pour une info neutre ; pas de doublon quand un bip est déjà joué ;
    - **choix de l'équipe CENDRE** : le jeu est en 10 contre 10, mais un serveur resté en 8 contre 8 (ancien `server.js` sur Fly) plaçait le joueur choisi chez CENDRE dans la place n° 9, que le jeu attribue à ACIER. Désormais le serveur lit la taille d'équipe directement dans `index.html` (le même fichier que sa simulation) : les deux ne peuvent plus diverger. Et si le serveur n'est pas à jour, le salon l'affiche en rouge (« Serveur pas à jour… redéploie server.js et index.html ») ;
    - le salon du Front affiche « Escorte du tank · premier à 4 manches » (au lieu de « Bombe · manches »).
  - **Camps ALLIÉS et AXE, couleurs fixes** :
    - les camps s'appellent **ALLIÉS** (bleu, ex-ACIER) et **AXE** (marron-orangé, ex-CENDRE) : salon, score, fin de manche, carte d'intro (« Votre camp : ALLIÉS · ennemi : AXE ») ;
    - les couleurs ne dépendent plus du joueur : tout ce qui appartient aux ALLIÉS est **bleu** (bandes et contour au sol des antennes et lance-missiles, étiquettes, barres de vie, icônes du bandeau, flèches, mini-carte, tank et 4x4 avec un camouflage gris-bleu et des bandes bleues) ; tout ce qui appartient à l'AXE est **marron-orangé** (camouflage sable, bandes, contours, étiquettes, icônes) ;
    - le texte reste relatif (« VOTRE ANTENNE » / « ANTENNE ENNEMIE ») et les cibles ennemies gardent leurs repères de visée et leur contour qui pulse ; les alertes (C4 sur votre cible, missile en approche, repérés) restent en rouge.
  - **Drapeaux de la seconde guerre mondiale** (décor) : 3 mâts par camp (au QG et près des portails nord et sud), avec socle en béton, boule dorée et drisse ; le drapeau flotte presque à plat dans le vent pour être lisible vu d'en haut :
    - ALLIÉS : États-Unis (version à 48 étoiles, 1912-1959), Royaume-Uni (Union Jack), URSS (version 1936) ;
    - AXE : Allemagne (drapeau noir-blanc-rouge officiel de 1933 à 1935 ; pas de croix gammée, symbole interdit dans plusieurs pays), Royaume d'Italie (tricolore à l'écu de Savoie couronné), Japon (Hinomaru de 1870) ;
    - chaque mât montre les drapeaux du camp qui occupe ce côté de la carte (les camps changent de côté d'une manche à l'autre).
  - **Drapeaux améliorés, 25 % plus petits, mâts plus hauts** :
    - tissu de 28,5 de haut (au lieu de 38) ; mât de 165 (au lieu de 136) : le drapeau flotte bien au-dessus du camp ;
    - textures deux fois plus fines (étoiles, Union Jack contre-changé, faucille et marteau en croissant, couronne d'Italie à fleurons et perles), trame du tissu, gaine de drisse blanche à œillets côté mât, bout libre un peu effiloché ;
    - le vent fait courir une vague vers le bout libre, qui s'affaisse un peu, avec des rafales ; mât plus fin, pomme et collerette dorées, drisse claire.
  - **Drapeaux vus depuis le camp de l'AXE** : la caméra du camp de l'est est retournée d'un demi-tour, et les drapeaux apparaissaient de dos, à l'envers et écrasés. Le tissu et la drisse tournent maintenant avec la caméra : chaque joueur voit les drapeaux de face, mât à gauche, dans le bon sens, quel que soit son camp.
  - **Menu** : le texte du mode Front indiquait encore « 8 contre 8 » ; il affiche « 10 contre 10 ».
  - **Serveur allégé en mode Front** (moins de latence) :
    - les grilles de conduite des véhicules sont préparées pendant le décompte de début de manche (avant : tout se calculait au premier instant de jeu, d'où un à-coup de 50 ms et plus) ;
    - les pilotes bots décident 20 fois par seconde au lieu de 60, et les tests de collision écartent d'abord les véhicules et barrières trop loin ;
    - le code des bots et des véhicules n'est plus recompilé sans cesse par le moteur JavaScript (objets de forme stable, cache de chemins sans désoptimisation) ;
    - mesure sur une partie de bots : **environ 32 % de calcul en moins** par seconde de jeu, et le pas de simulation le plus long passe de 77 ms à 22 ms. Important sur Fly en `shared-cpu-1x` : la machine n'a droit qu'à environ 6 % d'un processeur en continu, et le serveur est freiné quand il dépasse.
  - **Conduite des bots corrigée** (tank et 4x4) :
    - **freinage** : un véhicule qui en a un autre devant lui, dans sa voie, ralentit puis s'arrête à distance ; s'il suit un véhicule qui roule dans le même sens, il cale sa vitesse sur la sienne. Les collisions entre véhicules passent d'environ 4 par minute à presque zéro ;
    - **face à face** sur une route ou un pont : le moins prioritaire (le 4x4 devant un tank, sinon le numéro le plus haut) recule un court instant et passe ailleurs ; un tank qui cède devant un véhicule ennemi prend un autre pont ;
    - **véhicule arrêté devant** : après 2,5 s d'attente (6 s si c'est un allié avec équipage, qui va repartir), le bot le contourne, seulement si le détour reste raisonnable ;
    - **plus de longues marches arrière** : un but derrière le véhicule fait faire demi-tour en marche avant (le tank pivote sur place) ; il ne reste que de courtes manœuvres de dégagement d'une seconde, jamais vers un véhicule tout proche ;
    - **plus de toupie** : un 4x4 ne choisit plus à chaque instant quel véhicule contourner (son itinéraire basculait d'un côté à l'autre et il tournait sur place) ; l'escorte du tank garde sa place au moins 1,5 s ; un véhicule qui tourne quand même sur lui-même abandonne son but et marque une pause ;
    - **épave du 4x4** : quand l'autre 4x4 est garé sur son garage, il réapparaît juste à côté (avant, la réapparition était reportée sans fin et l'épave redevenait solide une seconde sur deux au milieu de la route).
  - **Bots qui restent dans un véhicule à l'arrêt** :
    - un véhicule bot qui ne progresse plus (moins de 100 px en 11 s, sans tirer) change de but, puis au 2e constat l'équipage descend. Avant, le compteur repartait à zéro juste après chaque constat, et l'équipage ne descendait jamais ;
    - 3 blocages rapprochés (attente sans fin, toupie, impasse) font aussi descendre l'équipage ;
    - les bots descendus vont aider à pied pendant 30 s : C4 sur les cibles ennemies, défense, attaque ; le véhicule abandonné n'est pas repris tout de suite ;
    - jamais de descente forcée quand un joueur humain est à bord.
  - **Soldats bots coincés contre un véhicule** : celui qui a un véhicule juste devant lui le longe comme un mur, du côté qui va vers son but, et le pas de côté quand il est coincé tient compte des véhicules. Le temps passé coincé passe d'environ 27 s à moins d'une seconde par minute de jeu.
  - Mesures sur 9 parties de bots de 5 minutes, avant → après : collisions entre véhicules 3,9 → 0,02 par minute ; chocs 3,3 → 0,2 ; toupies 2 → 0,6 ; marche arrière 19 s → 4 s par minute (plus aucune de plus de 1,5 s) ; soldats coincés 4 → 0,4 par minute. Matchs complets joués jusqu'à la fin sans erreur.
  - Seul `index.html` change pour cette étape (le serveur lit sa simulation dans `index.html`) : à remplacer sur GitHub et sur Fly.
  - **Bots groupés autour du tank** (ils le laissaient vide les deux tiers du temps) :
    - **reprise du tank** : dès qu'une place du tank est vide, les bots libres les plus proches (son équipage d'abord, jamais un défenseur loin de chez lui) y montent, même en plein combat quand la voie est libre ; le tank attend jusqu'à 6 s l'équipier qui arrive ;
    - **le tank n'est plus abandonné** : son équipage ne descend plus quand il est bloqué, le tank change de position à la place ;
    - **défense du tank** : quand des soldats ennemis s'approchent à moins de 450 px du tank, les 3 bots libres les plus proches vont les chasser ;
    - **attaque concentrée** : toute l'équipe frappe la même cible (celle que vise le tank, sinon la plus abîmée). Les attaquants accompagnent le tank tant qu'il en est loin, puis se postent autour de la cible ; les poseurs de C4 et le canon des bots la visent en priorité ;
    - mesures sur 10 parties de bots de 5 minutes, avant → après : tank sans équipage 52 % → 20 % du temps, tank avec ses deux places occupées 29 % → 49 %, cibles détruites +40 %.
  - **Canon du tank** :
    - **l'obus passe au-dessus des obstacles** (bâtiments, murs, son propre camp) et touche la première cible, le premier véhicule ou le premier soldat ennemi sur sa trajectoire ; il vole en cloche ;
    - **portée réduite** de 1 150 à 850 ; les bots se mettent en position de tir entre 440 et 630 px de leur cible ;
    - **viseur** quand on est au canon : réticule au point d'impact, avec le nom de ce qui sera touché (« LANCE-MISSILES B2 », « ANTENNE A1 », « 4x4 ENNEMI », « TANK ENNEMI », « SOLDAT ENNEMI » ou « RIEN EN VUE · portée max »), la vie de la cible et l'état du canon (prêt, rechargement, immobilisé). Un petit cercle montre où la tourelle va se tourner.
  - **Visée automatique sur mobile** : un conducteur à l'abri dans son véhicule n'est plus visé ; seul un tireur sur le toit l'est (on ne vide plus son chargeur sur un véhicule vide en haut).
  - **Noms des cibles** : antennes A1, A2… et lance-missiles B1, B2… sur les étiquettes (« A1 · ANTENNE ENNEMIE »), les flèches au bord de l'écran, la mini-carte, le bandeau sous le score (icône + numéro) et les messages (« Lance-missiles ennemi B2 détruit ! », « C4 posé sur votre antenne A1 ! »).
  - **Nouvelle commande du lance-missiles** : près de son lance-missiles, missile prêt, E (ou le bouton MISSILE) ouvre une **carte de tir** de toute la carte :
    - tout y apparaît : antennes, lance-missiles, tank et 4x4 ennemis (même non repérés), avec leurs noms ; vos propres cibles et véhicules en retrait, votre lance-missiles marqué « VOUS » ;
    - **toucher (ou cliquer) une cible** : le missile part, guidé sur elle ; à la souris, la cible survolée s'allume et la trajectoire s'affiche, avec l'effet du tir en en-tête ;
    - **toucher un autre point de la carte** : un viseur « TIRER ICI » apparaît, il faut le toucher pour confirmer (pas de missile perdu par erreur) ; le missile fait des dégâts autour du point et touche la cible ou le tank qui s'y trouve ;
    - **FERMER** (ou E, ou le bouton QUITTER sur mobile) quitte le lance-missiles ;
    - marche en solo et en ligne (le choix part au serveur avec les commandes du joueur).
  - Seul `index.html` change pour cette étape aussi : à remplacer sur GitHub et sur Fly.
  - **Tanks sur des routes séparées : ils ne se croisent plus** :
    - **la cause** : chaque tank tirait un pont au sort, mais il visait la sortie de ce pont de l'autre côté de la rivière, et le plus court chemin vers cette sortie passait presque toujours par le pont du milieu. Les deux tanks y allaient et restaient face à face ;
    - chaque tank va maintenant d'abord à l'**entrée** de son pont, de son côté, puis à la sortie, et garde son pont jusqu'à l'autre rive ;
    - le **pont du tank ennemi lui est interdit** (le pont qu'il va prendre, et tout grand pont près duquel il se trouve) ; s'il est bloqué, il prend un autre pont, jamais le plus court chemin par celui de l'ennemi ; un tank ennemi conduit par un joueur qui s'engage sur son pont le fait aussi changer ;
    - une fois passé, il ne revient plus de l'autre côté : sa cible et sa position de tir se choisissent au plus court **par les routes** (avant, à vol d'oiseau, il refaisait parfois tout le tour par sa base) ;
    - **variété** : 3 fois sur 4, chaque tank prend un autre pont qu'à la manche précédente ;
    - le tank démarre 150 px en retrait de la porte de sa base : il peut tourner vers les sorties nord et sud au lieu de rester coincé dans la porte ; s'il n'a pas la place de faire demi-tour, il recule un peu (1,6 s au plus) ;
    - **conducteur seul** : quand le tireur est tué et que des ennemis sont tout près, le conducteur bot ne monte plus au canon (il y serait exposé) ; il se replie vers la sortie de son pont et y attend son tireur ;
    - mesures, avant → après : tanks qui se rencontrent 4 manches sur 6 → 0 sur plus de 40 manches ; sortie de la base en 20 s → 12 s en moyenne ; tank sans équipage environ 20 % du temps dans les deux cas ; cibles détruites +30 %.
  - **4x4 : une seule escorte au plus, des missions variées** :
    - 11 combinaisons tirées au sort à chaque manche (jamais la même que la manche d'avant, rôles des deux 4x4 échangés au hasard) : tête, queue, flanc, attaque, garde, éclaireur, pointe, sentinelle, commando, patrouille, veille. **Jamais deux escortes** ; dans 4 combinaisons sur 11, personne n'escorte ;
    - **attaque** (nouveau) : le 4x4 choisit une antenne ou un lance-missiles qui n'est pas la cible du tank ni celle de l'autre 4x4, y va par un autre pont que celui de son tank ; tout près, l'équipage bot qui a du C4 descend le poser (le tireur sans C4 reste au canon et couvre), puis remonte. Sans C4, il tourne autour de la cible et son tireur arrose les défenseurs ;
    - **défense** et **repérage** comme avant (l'éclaireur passe par le pont de son tank en dernier) ;
    - la mission change en cours de manche (toutes les 1 min 20 à 2 min 30, et au retour au garage, 6 fois sur 10), toujours sans deuxième escorte ;
    - un 4x4 arrêté sur lequel son tank attend (poste de défense dans une sortie de la base) lui laisse la place ;
    - mesures : deux escortes en même temps 12 % du temps → 0 %.
  - **Plus de toupie** : un 4x4 qui tourne en rond (plus d'un demi-tour sans s'éloigner) s'arrête un instant et change de point au lieu de continuer à tourner ; un 4x4 d'escorte dont la place est juste derrière lui reste où il est au lieu de faire demi-tour sur place. Toupies 0,8 → 0,36 par minute de jeu.
  - **Son du 4x4 moins répétitif** :
    - boucles du moteur de 4,8 s et 2,2 s au lieu de 1,8 s et 0,55 s, avec une force d'explosion qui varie d'un cycle à l'autre ; le régime flotte un peu (plus au ralenti) ;
    - passages de rapport variés : seuils propres à chaque 4x4, sans va-et-vient entre deux rapports, coupure des gaz au passage, coup de gaz en rétrogradant ;
    - lever de pied (le moteur retombe, petites détonations à l'échappement), cliquetis de caisse sur les bosses, gravier bien plus présent hors de la route, pneus qui frottent dans les virages serrés, sifflement de transmission qui suit la vitesse ;
    - chaque 4x4 a sa propre voix (moteur un peu plus grave ou plus aigu) ;
    - la répétition mesurée en croisière passe de 0,46 à 0,12 (corrélation du son avec lui-même au bout d'une boucle).
  - Seul `index.html` change pour cette étape aussi : à remplacer sur GitHub et sur Fly.
  - **Conduite des véhicules par le joueur, plus simple** : les commandes suivent le véhicule, plus l'écran. Haut (Z / W / ↑) = avancer, bas (S / ↓) = reculer, gauche / droite = tourner le véhicule sur lui-même vers sa gauche / sa droite, quelle que soit sa direction à l'écran. Plus de demi-tour automatique : avant, le véhicule tournait pour se mettre dans la direction de la touche et faisait demi-tour tout seul quand on appuyait à l'opposé. Même chose au joystick sur mobile (haut = avancer, bas = reculer, côtés = tourner). À pied, rien ne change.
  - **Bots : plus de demi-tour forcé pour les 4x4** : quand leur chemin repart derrière eux, ils tournent normalement vers lui ; ils ne reculent un peu que si la caisse ne peut pas tourner (passage étroit). Le tank garde son demi-tour (il pivote sur place).
  - **Les deux 4x4 d'une équipe ne prennent jamais la même route** :
    - chaque 4x4 qui sort (attaque, repérage) a son propre grand pont, jamais celui de l'autre 4x4 ni, si possible, celui du tank (que suit l'escorte) ; au départ, chacun prend le pont du côté de la base où il est garé ;
    - le pont de l'autre 4x4 est fermé dans sa carte de conduite : il ne l'emprunte pas non plus pour revenir ou pour aller plus loin ;
    - l'éclaireur passe par son pont puis observe les cibles de ce côté ; le 4x4 en raid (tank sans conducteur) passe par le pont du tank ;
    - si les deux 4x4 se retrouvent quand même à moins de 300 px hors de la base, le moins prioritaire (défense < repérage < attaque < escorte) s'écarte ; un poste de défense n'est jamais pris à côté de l'autre 4x4 ; jamais deux 4x4 en défense ;
    - mesures sur 16 parties de bots de 5 minutes, avant → après : passages de rivière par le même pont que l'autre 4x4 3 sur 51 → 1 sur 46 en moyenne ; les deux 4x4 à moins de 350 px l'un de l'autre hors de la base 10 % → 4 % du temps (surtout des croisements brefs pendant un combat).
  - **Menu** : la version affichée est V3.5.
  - Seul `index.html` change pour cette étape aussi : à remplacer sur GitHub et sur Fly.
  - **Base de départ refaite (carte Front, les deux camps)** :
    - **le gros bunker en béton est retiré**, ainsi que le bric-à-brac du milieu de la base (sacs de sable, arbres morts, caisses, trous d'obus) ;
    - **aire de stationnement** en dalles de béton (joints, taches d'huile, traces de pneus) avec les emplacements peints en jaune : « TANK » au centre, « 4x4 » au nord et au sud, flèches vers la porte ;
    - **spawn reculé** : le tank part 280 px plus au fond (x 300 au lieu de 580, dans la porte avant), les 4x4 à côté de lui, les soldats tout au fond de la base. Rien ne gêne le tank dans un cercle de 120 px : il pivote sur place sans rien toucher, vers la porte est ou vers les sorties nord et sud ;
    - contre le fond : tentes, caisses de munitions et fûts, drapeau du camp et antenne radio ;
    - **deux tours de guet**, une à chaque bout de la base : quatre pieds en rondins contreventés, plate-forme à 100 de haut, parapet de sacs de sable, toit à deux pans en tôle ondulée, échelle, antenne, et un **projecteur qui balaie lentement l'avant de la base** (cône de lumière) ;
    - **grillage de guerre** à la place des dents de dragon : poteaux d'acier sur socles en béton, treillis en losanges rouillé, bavolets inclinés vers l'extérieur avec trois fils de barbelés, concertina de barbelés au pied côté extérieur, bande de terre retournée. Il arrête maintenant aussi les soldats (les balles et la vue passent au travers) : on sort par les portails ;
    - **portails coulissants** sur les trois sorties : poteaux rayés jaune et noir avec gyrophares, rail au sol, vantail grillagé sur roulettes avec un panneau STOP. Le vantail glisse le long de la clôture quand un véhicule du camp arrive à moins de 280 px ou qu'un soldat passe, puis **se referme derrière** (environ 1 s, gyrophares qui clignotent et bruit du moteur). Les véhicules ennemis ne passent toujours pas ;
    - au départ, le 4x4 d'escorte suit le tank tant qu'il n'est pas sorti de la base (il ne cherche plus à le doubler ou à se mettre sur son flanc dans la porte) ;
    - mesures : sortie du tank en 10 à 17 s (médiane selon les séries), les soldats quittent la base en 4 s, aucun soldat coincé.
  - **Commandes** : le panneau rappelle la conduite (« ↑ ↓ ← → · au volant : avancer · reculer · tourner »).
  - Seul `index.html` change pour cette étape aussi : à remplacer sur GitHub et sur Fly.
  - **Base sans dalle de béton** : l'aire de stationnement en béton est retirée ; le sol de la base est une cour en terre battue (bords adoucis, gravier, traces).
  - **Bouts des routes** : les routes nord et sud ne s'arrêtent plus net au milieu de la base, elles rejoignent la route centrale dans la cour (simple décor, la conduite des bots ne change pas) ; les routes ont des bouts et des virages arrondis, et les pistes qui finissent en pleine campagne s'élargissent et s'effacent dans la terre.
  - Seul `index.html` change pour cette étape aussi : à remplacer sur GitHub et sur Fly.
  - **Marquages jaunes au sol** : les emplacements du tank (« TANK ») et des deux 4x4 (« 4x4 ») et les flèches vers la porte sont peints à même la terre de la base, peinture usée, sans dalle.
  - **Lance-missiles dans le grillage** : les deux emplacements de cibles collés à la clôture de chaque base sont 60 px plus loin ; un lance-missiles (et son missile) ne touche plus le grillage.
  - Seul `index.html` change pour cette étape aussi : à remplacer sur GitHub et sur Fly.
  - **Départ des véhicules (Front)** : plus de tank ni de 4x4 bloqués au début de la manche.
    - le tank sort d'abord ; les 4x4 restent garés à leur place jusqu'à ce qu'il ait quitté la base (il ne les attend plus : un 4x4 garé qui attend son tour est contourné tout de suite), puis ils partent un par un ;
    - **chaque 4x4 sort par le portail de son pont** (nord, est ou sud) : l'attaque et le repérage prennent un pont du côté de la base où ils sont garés (celui du nord ne traverse plus la base devant le tank), l'escorte passe par le portail du tank, la défense par un portail que n'utilisent ni l'autre 4x4 ni le tank ;
    - 4x4 de défense : plus de halte devant un portail ni sur une route près de la base (il barrait la sortie du tank) ;
    - un véhicule coincé tout contre le grillage, dans une case où la conduite ne passe pas, se croyait arrivé à son but et ne bougeait plus : il ne s'arrête maintenant que s'il est vraiment arrivé ;
    - mesures sur 60 débuts de manche, avant → après : véhicules bloqués plus de 6 s près de la base 23 sur 40 débuts → 3 sur 60 (des haltes de défense brèves, hors de la base) ; 4x4 jamais sortis de la base en 60 s 5 → 0 ; sortie du tank 10 s → 8 s, des 4x4 12 s → 11 s ; les deux 4x4 d'une équipe par la même sortie 15 sur 80 → 2 sur 120, ensemble sur la même route 3 à 11 % du temps → 1 %.
  - Seul `index.html` change pour cette étape aussi : à remplacer sur GitHub et sur Fly.
  - **Épaves des 4x4 (Front)** : la carcasse d'un 4x4 détruit disparaît 5 s après l'explosion, dans une bouffée de fumée (elle ne bloque plus le passage ni les tirs).
  - **Chrono de retour du 4x4** : à sa place de garage dans la base, un anneau jaune se remplit avec les secondes restantes et « RETOUR DU 4x4 » (vert dans les 3 dernières secondes) ; le même chrono s'affiche dans le bandeau du haut (« 4x4 dans 12 s ») et sur la mini-carte, à l'emplacement du 4x4 dans la base.
  - **Passage barré par un véhicule** : un bot au volant qui reste 6 s immobile derrière un autre véhicule (ou coincé contre lui) change d'itinéraire.
    - à 6 s, il contourne le véhicule même si le détour est long, et pivote vers le détour au lieu d'attendre arrêté ;
    - s'il n'y a pas de détour (pont barré) ou si rien n'a bougé 2 s plus tard, il prend un autre chemin : le tank change de pont (un pont barré par un véhicule vide ou allié est évité 45 s, même si le seul autre pont est près de l'axe du tank ennemi), un 4x4 d'attaque ou de repérage prend un autre pont ;
    - un 4x4 qui cède le passage s'écarte sur le côté du chemin du véhicule qui attend, au lieu de reculer devant lui ; une escorte qui gêne son tank change de place ;
    - mesures (un 4x4 vide garé en travers de la route juste devant le véhicule), avant → après : tank immobile au plus 12,5 s → 6,5 s (médiane), plus de 15 s dans 2 cas sur 6 → 1, tank qui finit par passer ailleurs 4 sur 6 → 5 sur 6 ; 4x4 immobile au plus 12,5 s → 6,3 s, plus de 15 s 3 cas sur 8 → 0 ; conduite générale : toupies 0,38 → 0,22 par minute, marches arrière 4,3 → 3,7, équipages immobiles 1,3 → 0,8 ; les tanks ne se croisent toujours pas.
  - **Traductions anglaises** : menu (❄️ SNOW, 🧟 FOREST · ZOMBIE MODE, ⚔️ FRONT · WAR 10 VS 10), noms de cartes du salon (POLAR BASE · DAY · 6 vs 6, NEON GRID · 4 vs 4, FRONT · 10 vs 10…), équipes du Front (ALLIES / AXIS), « Code Red · first to 4 rounds », fins de manche du Front (Targets destroyed, Time up, Draw), « 6 players ready. », « READY ✓ ».
  - Seul `index.html` change pour cette étape aussi : à remplacer sur GitHub et sur Fly.
  - **Bots plus forts et plus malins (Front)**
    - **Missiles** : le bot le plus proche du lance-missiles tire dès que le missile est prêt (avant : seulement après 20 s d'attente sans menace) ; cible : le véhicule ennemi menaçant, sinon la cible ennemie qu'on peut achever, un lance-missiles ennemi ou la cible attaquée par notre tank. Un défenseur y va plutôt qu'un attaquant qui vient de réapparaître, et il ne reste plus planté à 50 px du lanceur sans s'en servir.
    - **Antenne** : activée aussi pour préparer l'assaut (notre tank ou deux bots près d'une cible ennemie : les défenseurs sont révélés).
    - **C4 sur les antennes et lance-missiles** : 3 attaquants à pied par équipe (au lieu de 2) ; loin de la cible, ils font route avec notre tank au lieu de traverser seuls ; près du point de pose, ils avancent en tirant au lieu de s'arrêter à chaque duel ; ils gardent leur C4 pour les cibles (plus de ponts à faire sauter pour eux, plus de C4 sur un véhicule de passage, sauf s'il leur barre le chemin ou menace nos cibles) ; quand notre tank est près de la cible commune, les bots libres à moins de 1400 px viennent la miner ; davantage de 4x4 en mission d'attaque au C4, et tout équipage de 4x4 qui passe à moins de 340 px d'une cible ennemie descend la miner.
    - **Tir** : réaction plus rapide, cadence et précision meilleures (mode Front seulement) ; pas de tir dans le grillage de la base ennemie.
    - mesures (20 manches entre bots), avant → après : missiles sur les cibles 0,32 → 0,80 par minute, missile prêt mais inutilisé 32 → 10 s par minute, C4 posés sur les cibles 0,17 → 0,48 par minute (près de 3 fois plus), antennes activées 1,0 → 2,0 par minute, cibles détruites par manche 1,8 → 2,05.
  - **Grillage et portails de la base** : ils arrêtent les balles des ennemis (étincelle et son métallique) ; les balles de l'équipe de la base passent. Aucun ennemi ne peut entrer dans la base : les portails sont fermés pour les soldats ennemis (c'était déjà le cas pour leurs véhicules) et ne s'ouvrent plus à leur approche. Les bots ne visent plus et ne poursuivent plus un ennemi derrière ce grillage.
  - **Véhicules +15 %** : tank 76 → 87, 4x4 135 → 155.
  - **Véhicule bloqué sur un pont** : après un blocage, il ne pivote plus sur le tablier (il se coinçait en travers contre le parapet) : il se remet dans l'axe du pont et roule ou recule tout droit jusqu'à en être sorti ; coincé sans pouvoir pivoter, il avance au pas pour retrouver de la place. Un pont barré par un véhicule fait choisir un autre pont, mais jamais celui du tank ennemi.
  - **Anglais** : « Allié 4 » restait en français dans le fil des éliminations (« Ally 4 »).
  - Seul `index.html` change pour cette étape aussi : à remplacer sur GitHub et sur Fly.
  - **Carte du Front : la longue ligne de tranchée en zigzag disparaît** (avec ses murets de sacs de sable, des deux côtés) ; le reste de la carte ne change pas.
  - **Tranchées creusées** : les tranchées près de la rivière (mêmes emplacements) sont de vraies fosses en 3D : sol découpé, parois boisées, fond boueux, caillebotis, poteaux de soutènement, terre retournée et mottes tout autour. Un soldat qui y descend apparaît plus bas ; les véhicules ne roulent plus dedans (les bots les contournent).
  - **Rivière** : berges au contour irrégulier (boue humide sombre en bord d'eau, terre sèche au-dessus, cailloux), eau plus claire sur les bords et plus sombre au milieu, courant, stries d'écume, écume le long des berges, quelques reflets.
  - **Sol** : texture de détail à trois échelles (plus de motif qui se répète), flaques d'eau de pluie, touffes d'herbe sèche et cailloux en relief (hors routes, rivière, tranchées et bases).
  - **Traces de pas** : chaque soldat à pied laisse des empreintes de bottes qui s'effacent en 16 s (pas sur les ponts ni dans l'eau) ; d'anciennes traces légères partent des bases et des tranchées.
  - Seul `index.html` change pour cette étape aussi : à remplacer sur GitHub et sur Fly.
  - **Traces de pas** : elles s'effacent maintenant au bout de 5 s (nettes 2,5 s puis fondu) ; plus d'anciennes traces permanentes.
  - **Grenades des bots (Front)** : les bots à pied lancent leur grenade (1 par vie) sur un ennemi qui pose du C4 à portée, sur un ennemi repéré à couvert (hors de vue) ou dans une tranchée, ou sur au moins deux ennemis groupés, entre 150 et 420 px, jamais près d'un allié ; avant, ils ne la lançaient presque jamais (1 grenade en 20 min de parties simulées → 157).
  - Seul `index.html` change pour cette étape aussi : à remplacer sur GitHub et sur Fly.
  - **Rivière animée** : le temps de l'eau n'était jamais mis à jour (une ligne du code était passée derrière un commentaire), le courant restait figé. L'eau bouge maintenant : courant, stries d'écume et écume des berges qui défilent, vaguelettes éclairées qui descendent le courant avec quelques reflets du soleil, et débris flottants (branches, feuilles, plaques d'écume) emportés par le courant.
  - Seul `index.html` change pour cette étape aussi : à remplacer sur GitHub et sur Fly.
  - **Lance-missiles et antennes écartés** : devant chaque base, deux emplacements de cibles voisins (au nord et au sud) étaient à 34 px l'un de l'autre, un lance-missiles touchait presque l'antenne d'à côté ; ces deux places sont déplacées de 50 à 60 px, il y a maintenant au moins 110 px entre deux cibles (un soldat passe entre elles). Le petit décor et les cratères sous les nouvelles places sont retirés ; même chose en miroir chez l'adversaire.
  - Seul `index.html` change pour cette étape aussi : à remplacer sur GitHub et sur Fly.
  - **Salon de partie privée refait** :
    - **Salon permanent** : le code reste le même partie après partie ; un salon disparaît seulement après 15 minutes sans aucun joueur.
    - **Vote de la carte et du mode** dans le salon : chacun vote (Nuit, Désert, Neon, Neige jour/nuit, Forêt, Front, puis Objectif ou Match à mort quand la carte le permet). La carte la plus votée devient celle du salon (encadré jaune), en cas d'égalité c'est le choix de l'hôte. Plus besoin de recréer un code pour changer de mode. Une carte trop petite pour le nombre de joueurs n'est pas votable.
    - **Bouton « Je suis prêt »** : les joueurs prêts ont un ✓ PRÊT dans la liste, l'hôte voit « Lancer la partie · 2/3 prêts » et « Tout le monde est prêt ! ».
    - **Retour au salon à la fin de chaque partie** : 15 s après l'écran de fin, tout le monde revient dans le salon (compte à rebours affiché). Le bouton « SALON » y va tout de suite ; celui de l'hôte y ramène tout le monde.
    - **Quitter et revenir quand on veut** : chaque joueur reçoit un jeton. En revenant, il reprend sa place, son pseudo et son vote, dans le salon ou dans la partie en cours (l'IA joue à sa place pendant son absence). On peut aussi rejoindre une partie déjà lancée avec le code.
    - **Déconnexion non voulue** : le jeu se reconnecte tout seul (jusqu'à 8 essais) et le joueur reprend la partie là où elle en est.
    - **Salons récents** dans « Partie privée » : la liste des derniers salons rejoints, avec « Rejoindre » et ✕ pour les retirer. Un bouton « ↩ Revenir dans le salon » apparaît aussi sur le menu principal pendant 15 minutes. Le pseudo est retenu.
    - Les joueurs déconnectés apparaissent « · déconnecté » dans le salon ; si le même compte rejoint depuis un autre onglet, l'ancien onglet revient au menu.
    - Un clic sur le petit texte à l'intérieur d'un bouton du menu (ex. « CDG » sous un serveur) ne se perd plus. Les messages d'erreur de l'écran « Partie privée » s'affichent enfin (avant, ils étaient cachés dans le salon).
  - **Cette fois `server.js`, `game_sim.js`, `index.html` et `fly.toml` changent tous les quatre** : les remplacer sur GitHub, puis redéployer sur Fly (`fly deploy`). `fly.toml` passe à `auto_stop_machines = 'suspend'` : sans ça, Fly arrête la machine quand plus personne n'est connecté et les salons sont perdus avant les 15 minutes.

`server.js` a changé à l'étape 1 (salons de 10 places en 5 équipes) : il faut le redéployer avec `index.html`.

## Tester en solo sur GitHub Pages
1. Pousse ces fichiers sur un dépôt GitHub.
2. Settings → Pages → Source : branche `main`, dossier `/ (root)`.
3. Ouvre `https://<ton-compte>.github.io/<dépôt>/` puis « Jouer contre l'IA ».

## Tester en ligne (parties privées)
Le jeu ouvert depuis GitHub Pages se connecte à `wss://detonate-prototype.fly.dev`
(constante `DEFAULT_SERVER` dans `index.html`). Déploie le serveur prototype :

```
fly deploy --config fly.toml
```

Pour viser un autre serveur sans modifier le fichier : ajoute `?server=wss://autre-app.fly.dev` à l'adresse du jeu.

Le serveur doit toujours être déployé avec **le même `index.html`** que celui de GitHub Pages
(il exécute la même logique de jeu).
