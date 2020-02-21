# INSA 2SU 2020

### 2/- A quoi sert la chaîne de confiance? Pourquoi est-elle nécessaire?

Le concept de chaîne de confiance prend tout son sens dans l’implémentation de secure boot connus sous le nom de Trusted boot ou verified boot. Ils permettent de valider de façon cryptographique et à partir d’une root of trust matérielle chaque composant que le système charge quand il commence à exécuter du code. 
Les processeurs ARM actuels implémente TrustZone qui fournit un TEE (trusted environment execution) capable d’isoler les applications dont la sécurité est critique du reste du code.
Celui dit chaîne de confiance fournit divers services de cryptographie, de gardiennage des accès privilégiés aux matériels et peut être utilisé pour vérifier l’intégrité du système d’exploitation principa

### 3- Décrire la méthode pour aborder la sécurité sur un produit embarqué. Pourquoi établir un modèle d'attaquant est-il important?

Il est impossible de « sécuriser un système » dans l’absolu. Pour aborder sereinement la question de la sécurité, il est nécessaire d’organiser une réflexion sur les menaces et les enjeux. La question par laquelle on doit commencer est de définir la nature de la cible à protéger, ayant bien définis le contexte de notre cible; l’étape suivantes consiste à définir les informations sensibles que contient notre système (info sur le constructeur, info sur l’utilisateur, log …) ainsi que les victimes potentiels si ces informations sont diffusées.
Par la suite il est primordiale de définir les différents chemin d’attaques/accès sur notre système, on citant les différents systèmes dont notre système cible a accès, ainsi que tous les chemins d’accès possible pour notre cible (accès physique, accès par réseau …).
L’étape suivantes consiste à identifier notre adversaire car les moyens de sécurité à déployer ne sont pas les mêmes selon le niveau supposé des adversaires : adversaire occasionnel ( peu compétent, peu de moyen , Il suffit de mettre des barrières standards dans son chemin pour le bloquer), adversaire expert (très compétent, gros moyens à sa disposition, Il est extrêmement difficile de se protéger de ce genre d’adversaires).

### 4- Trouver un moyen rapide de faire du debug embarqué (par exemple sur cible ARM)? Expliquer les avantages :

Le débogage est une tâche que tout développeur veut éviter, mais c'est malheureusement un mal nécessaire au développement logiciel. En fait, comme l'a révélé l'étude intégrée d'EDN, les projets de développement intégrés consacrent en moyenne plus de 20% de leur effort total au seul débogage. 
Prenant l’exemple d’une cible ARM, il existe un projet  DAPLink(un projet d'ARM) visant à développer une sonde de débogage de cortex open source, qui fournit un ensemble d'interfaces pour simplifier la programmation et le débogage et peut être implémenté sur presque n'importe quel matériel compatible USB. DAPLink fournit un ensemble de points de terminaison, y compris une interface CMSIS-DAP pour le débogage, un disque USB pour la programmation par glisser-déposer et un port série en option pour la communication avec le système cible. Cette approche de disque USB est utile pour la programmation d'appareils sur le terrain car elle ne nécessite aucun logiciel supplémentaire, mais n'est pas toujours fiable pour une utilisation en développement.

### 5-  Lister les catégories de bug possibles et comment les exploiter et les défendre ?

Parmi les bugs possibles on trouve :
- Dump / injection de commande : 
Une injection de commande est une classe de vulnérabilités où l'attaquant peut contrôler une ou plusieurs commandes en cours d'exécution sur un système. 
Afin d'empêcher un attaquant d'exploiter une application web vulnérable et d'insérer des caractères spéciaux dans la commande du système d'exploitation, on doit généralement éviter les appels système dans la mesure du possible. En toutes circonstances, évitez toute intervention de l'utilisateur, sauf si cela est absolument nécessaire. 

- Elévation de privilèges ( Get root) :
On utilise cette vulnérabilité pour gagner des accès hauts dans le système, un pirate ou un utilisateur pourra modifier la configuration système ou implanter une backdoor.
Il y a trois remèdes simples pour réduire les attaques d'escalade : 
  - Faites en sorte que vos utilisateurs ou clients utilisent la meilleure méthode d'authentification possible.
  - Scannez vos applications Web pour identifier les vulnérabilités connues afin de minimiser les attaques par exploit.
  - Validez les données dans tous les formulaires de soumission utilisés sur votre site web. 

- Security bypass : 

- Shellcode (prise de contrôle) : 
Est un petit morceau de code utilisé comme charge utile dans l'exploitation d'un logiciel de vulnérabilité . Elle est appelée « shellcode » parce qu'il commence généralement un shell de commande à partir de laquelle l'attaquant peut contrôler la machine compromise.

### 6- Quelles idées pour améliorer la sécurité en embarqué? (IA, Anti-debug, Obfuscation, Crypto ...) Choisissez une idée, chercher si elle existe et développer en quelques phrases quel avantage elle apporte et ses limites :

L'anti-débogage est un ensemble de techniques utilisées dans le code d'une application pour détecter et empêcher l'acte de débogage. Cela empêche les attaquants d'exécuter des applications de manière dynamique, essayant de comprendre leur fonctionnement et modifiant le comportement de certaines fonctionnalités ou vérifications au sein de l'application. Les techniques anti-débogage incluent l'observation et la détection de petites différences dans la mémoire, le système d'exploitation, les informations de processus et la latence qui surviennent lorsqu'un débogueur est connecté à une application et comparées à l'absence de débogueur.

Bien qu'il existe plusieurs méthodes différentes d'anti-débogage, l'une des principales méthodes utilisées est appelée anti-débogage de code modifié. Cette technique insère du code à plusieurs endroits de l'application pour rechercher activement des points d'arrêt, des débogueurs ou des techniques de débogage. La détection de ces points d'arrêt peut être obtenue en analysant l'ensemble du processus de fonctionnement de l'application pour les modifications du code tout en le comparant aux valeurs ou normes attendues. Les écarts détectés déclenchent une alarme qui peut entraîner une enquête plus approfondie. 
Ces vérifications tiennent compte des aspects uniques de la plate-forme cible et des indicateurs spécifiques qui peuvent identifier la présence d'un débogueur. Lorsqu'un débogueur ou une activité suspecte est détectée, plusieurs actions de défense peuvent être effectuées, telles que la corruption de l'état du programme pour bloquer l'application, ou même configurer votre application pour exécuter une fonction de rappel personnalisée définie dans le code source. Vous pouvez même choisir si l'état du programme doit être corrompu ou si l'application doit être laissée en cours d'exécution.
 Les vérifications anti-débogage utilisent des appels système du noyau, contournant ainsi les hooks en mode utilisateur que le pirate informatique peut avoir insérés, forçant l'attaquant à modifier son noyau, ce qui diminue considérablement les chances d'une attaque réussie.




