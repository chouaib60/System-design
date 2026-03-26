# System-design

1 bit est la plus petite unité du data (elle égal soit à 0 ou )
1 byte = 8 bits

KB = 1024 bytes
MB = 1024 KBs
GB = 1024 MBs
TB = 1024 GBs

composant ordinateur:

1. disque dur : pour le stockage donnée , il est lent par rapport à SDD dans la lecture et écriture des données .

2. RAM : c'est une mémoire temporaire de l'ordinateur où il garde les données pendant qu'il travaille.
ces données sont structure de données , variables , les données applicatives en cours d'utilisation .
elle sert exécuter des programmes , ouvrir des applications (chromes, jeux...), garder les données actives en cours d'utilisation

c'est une mémoire volatile car après un redémarrage de l'ordinateur tout s'efface . donc elle nécessite de l'alimentation pour conserver le contenu

3. cache : la mémoire cache est plus petite que la ram mais le temps d'accès à la mémoire cache est beaucoup plus rapide.
son but c'est réduire le temps moyen d'accès aux données pour optimiser les performances du processeur cpu.
![[Pasted image 20260325120831.png]]

le CPU check d'abord  le cache L1 pour récupérer la data , s'il ne la trouve pas il vérifie dans caches L2 et L3 .
finalement s'il ne la trouve pas il passe à la RAM.

4. CPU : c'est le cerveau de l'ordinateur , il récupère les données , décode et exécute les instructions .
si j'ai un code écrit en JAVA , c'est le cpu qui traite les opérations définies dans le programme.
quand je l'exécute le compiler va le traduire en langage de la machine.
une fois compilé le cpu va l'exécuter.

5. la carte mère : c'est le composant qui connecte tous les éléments at assure la communication entre eux


## 
![[Pasted image 20260325174049.png]]


<font color="#92d050">LOAD BALANCER :</font>
- une fois notre application est en production , elle doit gérer un grand nombre de requêtes utilisateur ,
cette gestion sera assurée par LOAD BALANCER qui répartisse les requêtes de manière uniforme sur plusieurs services pour éviter les problèmes en cas de forte charge.

- le serveur de production est distinct du serveur de stockage externe.

<font color="#92d050">logging et monitoring :</font>

- nous disposons d'un système de logging et monitoring (journalisation et surveillance) qui surveillent attentivement chaque micro-interaction, stockant les logs et analysant les données (capturant les erreurs ou les anomalies par exemple)
pour le backend on peut utiliser PM 2 pour logging et monitoring
pour le frontend on peut utiliser Sentry.  

#### <font color="#92d050">bugs</font>
![[Pasted image 20260325180802.png]]

les développeurs cherche (dans les loggins et monitoring ) les anomalies et les schemas qui vont pointer vers la source du problème .
il ne faut jamais débuguer dans l'environnement de production . 
instead les développeurs vont récréer l'anomalie dans un testing environnent ça assure que les utilisateurs de l'application ne sont pas affectés par le debugging process .
une fois le bug est corrigé  on déploie la correction dans la prod.

# what makes a good system design

1. la scalabilité : capacité d'un système à grandir facilement lorsqu'il y a plus d'utilisateurs ou de données.
exemple : Une application qui marche bien pour 100 utilisateurs mais plante à 10000 → pas scalable.

2. maintainability : facilité à modifier ou corriger un logiciel sans tout casser.
code bien documenté , organisé avec tests → facile à maintenir

3. efficiency : capacité d'un logiciel à utiliser le moins de ressource possible (CPU, RAM, Stockage) tout en faisant son travail rapidement.

4. reliability : capacité  d'un système à fonctionner correctement tout le temps, sans erreurs , ni pannes.

# CAP theorem

![[Pasted image 20260326095535.png]]


1. cohérence : la cohérence garantit que tous les noeuds du système distribué possèdent les mêmes données en même temps . si je modifie un noeud, cette modification doit être affiché sur tous le noeuds . c'est comme un document si une personne effectue une modification tout le monde la voit immédiatement. 

![[Pasted image 20260326095446.png]]

2. Availability (disponibilité) : signifie que le système est toujours opérationnel et réactif aux requêtes dans tous les cas  24/24 quelles que soient les perturbations behind the scenes.

![[Pasted image 20260326101636.png]]


3. partition tolerance: est la capacité d'un système distribué  à fonctionner malgré une coupure de communication entre ses noeuds . 

![[Pasted image 20260326095400.png]]

selon le théorème de CAP, un système distribué ne peut atteindre que <u>deux</u> de ces trois propriétés en même temps (voir première image du théorème).


## AVAILABILITY

on  mesure souvent la disponibilité en termes de temps et fonctionnements (up time et down time).

![[Pasted image 20260326104909.png]]


c'est là qu'interviennent :
- SLO : Service Level Objectives : les SLO définissent  des objectifs de performances et de disponibilité pour nos systèmes. par exemple :  un SLO peut mettre une condition que notre serveur web doit répondre aux requêtes en moins de 300 millisecondes et avoir une disponibilité de  99.9 %
- SLA : Service Level Agreem : c'est comme des contrats formels avec nos utilisateurs ou clients , par exemple  si notre SLA  garantit qu'on aura une Availability de 99.9 % mais c'était pas le cas , on doit le rembourser.

## calcul de la vitesse de notre système :

pour cela nous utilisons le débit et la latence.

- <span style="background:#affad1"> le débit : </span>combien de données notre système peut traiter sur une période donnée.
<span style="background:#affad1">pour le serveur , le débit su serveur</span> est mesuré en requêtes par seconde(RPS). RPS(request per second) : signifie combien de requêtes client qu'un serveur peut traiter dans une durée de temps.
une valeur RPS élevé ↑ indique que le serveur peut gérer un plus grand nombre d'utilisateurs simultanés.
le débit de la base de données QPS (query per seconde) représente le nombre de requêtes qu'une base de données peut traiter par seconde .
QPS ↑ is good
- <span style="background:#affad1">Latency :</span> c'est le temps qu'il faut pour qu'une requête reçoit une réponse . Latency ↓ is good
![[Pasted image 20260326112528.png]]
