# XML_RSS_flows
## Mécanismes cryptographiques
[ ] Protocole de communication:
<details>
<summary>Utiliser des protocoles de chiffrement et d'authentification.</summary>
Il est nécessaire d’utiliser uniquement des protocoles pouvant chiffrer et authentifier les communications.
</details>
[ ]  Version des protocoles SSL/TLS:
<details>
<summary>Choisir de TLSv1.2 ou TLSv1.3.</summary>
Il est recommandé de choisir les protocoles TLSv1.2 ou TLSv1.3 qui sont considérés comme étant sécurisés.
</details>
[ ] Suites cryptographiques robustes:
<details>
<summary>Utiliser une cryptographie robuste.</summary>
Il est nécessaire d’utiliser des suites cryptographiques robustes (cf guide de développement sécurisé python pour le détail).
</details>
[ ] Certificat X509:
<details>
<summary>Utiliser un certificat X509 pour SSL/TLS.</summary>
Pour établir une connexion SSL/TLS non vulnérable aux attaques « Man-In-The-Middle », il est essentiel de s’assurer que le serveur présente un certificat X509 valide.
</details>
[ ] Générateurs aléatoires:
<details>
<summary>Utiliser des valeurs rigoureusement aléatoires uniques et secrètes.</summary>
- Utiliser uniquement des générateurs de nombre aléatoire (CSPRNG) : secrets() en python.
- Utiliser les valeurs aléatoires une seule fois
- Ne pas exposer les valeurs aléatoires générées
</details>
[ ] Fonctions de hachage
<details>
<summary>Utiliser des fonctions de hachage robustes.</summary>
Il est recommandé d’utiliser les algorithmes suivants : SHA-256, SHA-512, SHA-3 or bcrypt.</details>
<details>
<summary>Utiliser un sel.</summary>
- Utiliser des fonctions de hash qui génèrent leur propre sel, ou générer un sel aléatoire d’au moins 32 octets.
-  Le sel doit être au moins aussi long que la longueur du hash retournée.
- Le sel peut être stocké à côté du hash, pour les futures opérations de validations du hash (cas du mot de passe).
</details>
## Interactions avec le système d'exploitation
[ ] Fichiers temporaires
<details><summary>Utiliser des librairies sécurisées.</summary> Il convient d’utiliser des librairies de création de fichier temporaire sécurisée (ex : NamedTemporaryFile).</details>
[ ] Positionnement des droits
<details>
<summary>Gérer les droits rigouresement (position, clarté, accès):</summary>
- D’utiliser un dossier dédié à l’application, avec des droits correctement positionnés.
- Utiliser lorsque cela est possible des fichiers avec des noms imprédictibles.
- Le fichier temporaire n’est accessible en lecture et écriture que par l’ID de l’utilisateur qui l’a créé.
- Le « file descriptor » ne doit pas être hérité par les processus enfants.
- Supprimer les fichiers temporaires dès qu’ils sont fermés.
</details>
[ ] Gestion des processus
<details>
<summary>Contrôler l'envoi de signaux: </summary>
- Contrôler qu’un utilisateur est autorisé à envoyer un signal, par exemple en l’interdisant si l’utilisateur n’est pas maitre du processus.
- Configurer le processus qui envoie le signal avec des permissions minimales.
</details>
[ ] REGEX
<details>
<summary>Eviter la répétition d'un groupe de capture:</summary>
- Utilisez si possible une bibliothèque qui n'est pas vulnérable aux attaques Redos comme Google Re2. 
- Sinon, éviter la répétition d’un groupe de capture ayant lui-même une répétition ou une alternative avec un recouvrement, comme :
	- (a+)+ : répétition d’un groupe ayant lui-même une répétition
	- (a|aa)+ : répétition d’un groupe ayant lui-même une alternative avec recouvrement
</details>
[ ] Archive (tar, zip, gzip..)
<details>
<summary>Contrôler la décompression des archives (chemin, contenu, taille):</summary> 
Les applications qui décompressent des archives doivent vérifier :
- Le chemin où les archives sont développées.
- Ne pas se fier aveuglément au contenu de l'archive.
- Les archives ne doivent pas être décompressées en dehors du répertoire racine où l'archive est censée être décompressée.
- De plus, les applications doivent contrôler la taille des données décompressées pour ne pas être victimes d'une attaque de type "Zip Bomb".
</details>
## Gestion des mots de passe
[ ] Mots de passe utilisateur
<details>
<summary>Inclure une gestion et politique de mots de passe:</summary>
- Longueur de 8 caractères minimum pour un compte utilisateur.
- Longueur de 12 caractères minimum pour un compte à privilège.
- 3 classes de caractères.
- Une comparaison avec une liste des mots de passe les plus communs.
- Un temps d’expiration de 90 jours.
- Une interdiction de réutiliser l’un des 4 derniers mots de passe.
- Le mot de passe doit être modifier à la première utilisation.
</details>
[ ] Mots de passe des services
<details>
<summary>Stocker les mots de passe de façon sécurisée:</summary>
- Les mots de passe sont stockés en dehors du code sources, comme dans un fichier avec des droits restreints ou encore dans une base de gestion des secrets (ex : secret Kubernetes).
- Si un fichier de configuration est utilisé, il ne doit pas être ajouté dans le gestionnaire de version du code source.
</details>
## Gestion des entrées
[ ] Injection SQL
<details><summary>Se prémunir contre les injections SQL:</summary> 
- Il convient d’utiliser des requêtes préparées plutôt que de concaténer des chaines de caractères statiques avec des données incertaines.
- De cette façon, les “charges” potentielles seront échappées correctement et vues comme des chaines de caractères par la base de données.
</details>
[ ] Injection XML
<details><summary>Se prémunir contre les injections XML:</summary> 
- Il est recommandé de désactiver la fonctionnalité « EXTERNAL ou INTERNAL ENTITY » et l'accès au réseau lors de la construction de document XML (ou Xpath).
</details>
[ ] Injection de commande OS
<details><summary>Se prémunir contre les injections de commande OS:</summary> 
S’il est nécessaire d’exécuter des commandes système qui contiennent des entrées non maitrisées, il convient :
- D’utiliser le module de sous-processus sans le shell=true. Dans ce cas, le sous-processus attend un tableau où la commande et ses arguments sont séparés.
- D’échapper les arguments avec shlex.quote.
</details>
[ ] Exécution de code dynamique
<details><summary>L’ajout dynamique de module doit être contrôlé avec une liste blanche de valeurs autorisées.</summary> </details>
[ ] Désérialisation
<details><summary>Garantir la sécurité et l'intégrité des mécanismes de désérialisation:</summary>
- N’utiliser le module PyYAML qu'avec l’option « safe load » configuré.
- Utiliser des formats qui valident l’intégrité des données (JWT...)
</details>
[ ] Injection dans les logs
<details><summary>Contrôler les injections dans les logs (encodage, données sensibles):</summary>
- Reformater ou encoder toutes les entrées avant de les inscrire dans les logs. Cela inclut la vérification de la taille, du contenu, de l’encodage, de la syntaxe, ou la validation en utilisant des listes blanches chaque fois que cela est possible.
- S’assurer que le programme ne puisse pas insérer de données sensibles dans les logs ou dans la sortie standard / d’erreur.
</details>
## Gestion des entrées (application web)
[ ] Gestion des entrées utilisateurs
<details><summary>Contrôler les entrées fournies par les utilisateurs:</summary> 
- Il convient de s’assurer que toutes les entrées utilisateurs correspondent à des choix ou formats attendus.
- Valider les données fournies par l'utilisateur sur la base d'une liste blanche ou d’une vérification du format (alphanumérique, longueur, valeur…) et rejeter les entrées qui ne correspondent pas.
</details>
[ ] Injection XSS
<details><summary>Se prémunir contre les injections XSS:</summary>
- Encoder les données fournies par l'utilisateur qui sont retournées par le serveur. 
- Adapter l'encodage par apport au contexte de sortie.
- Utiliser l'encodage HTML pour le contenu HTML.
- Utiliser l'encodage des attributs HTML pour les attributs.
- Utiliser l'encodage JavaScript pour le JavaScript généré par le serveur.
- Utiliser des méthodes d'échappement de caractère.   
- Utiliser des bibliothèques spécifiquement conçues pour cela.
</details>
[ ] Path traversal
<details><summary>Se prémunir contre un Path traversal:</summary> 
- opter pour une stratégie d'atténuation basée sur la liste blanche des chemins ou des caractères autorisés (ex : suppression des «. » « / »…).
- Utiliser des bibliothèques spécifiquement conçues pour cela.
</details>
[ ] Injection dans les en-têtes HTTP
<details><summary>Se prémunir contre les injections dans les en-têtes HTTP:</summary>
- Valider les données avant de les traiter (données fournies par l'utilisateur pour construire l'en-tête).
- La validation doit être basée sur une liste blanche.
</details>
[ ] Injection LDAP
<details><summary>Explication du controle</summary> 
- Echapper les caractères spéciaux (" ", "[ ], '"', "+", ",", ";","\\","\< ", ">" et null).
- RFC 4514 : Remplacer par ces caractères spéciaux par le caractère backslash '\' suivi des deux chiffres hexadécimaux correspondant au code ASCII du caractère à échapper.
- RFC 4515 : Les filtres de recherche LDAP doivent échapper un ensemble différent de caractères spéciaux ("*", "(", ")", "\" et null).
</details>

[ ] Redirection HTTP
<details><summary>Contrôler les redirections HTTP:</summary> 
- Valider les données fournies par l'utilisateur sur la base d'une liste blanche et rejeter les entrées qui ne correspondent pas.
</details>
## Gestion des sessions et des droits
[ ] Faille CSRF
<details><summary>Explication du controle</summary> Le principe de protection est de s’assurer que l’application requiert une authentification qui n’est pas utilisée automatiquement par le navigateur. Cela consiste typiquement en l’ajout d’un élément aléatoire à la validation de formulaire de telle sorte qu’une requête ne sera considérée comme légitime que si elle contient cet élément aléatoire. En effet, les attaques CSRF ne sont possibles que si les requêtes sont prédictibles.</details>
[ ] Configuration des cookies
<details><summary>Explication du controle</summary> Il convient donc de configurer les cookies avec :</details>

<details><summary>Explication du controle</summary> ·        L’attribut HttpOnly pour la plupart des cookies ; il est obligatoire pour les cookies de session ou sensible.</details>

<details><summary>Explication du controle</summary> ·        L’attribut Secure pour tous les cookies.</details>
[ ] Gestion des privilèges
<details><summary>Explication du controle</summary> Pour toutes requêtes reçues, il convient de vérifier si l’utilisateur possède suffisamment de droits sur l’objet ou la fonctionnalité demandés.</details>
## Configuration des en-têtes http
[ ] Cross origin Resource
<details><summary>Explication du controle</summary> Il convient de définir l'en-tête Access-Control-Allow-Origin avec une liste de domaines utilisés et de confiance uniquement.</details>
## Contrôles additionnels
[ ] Backdoor
<details><summary>Explication du controle</summary> Vérifier l’absence de backdoor dans le code source de l’application. Les backdoors prennent souvent la forme de code obfusqué (longue suite de caractères sans signification) qui correspondent à des payloads encodés ou chiffrés. </details>

<details><summary>Explication du controle</summary> Les ouvertures de connexion réseau vers des IP ou domaine inconnus peuvent également être des sources de menaces.</details>
[ ] Fonctionnalité de DEBUG
<details><summary>Explication du controle</summary> Vérifier l’absence de fonctionnalité de débogage dans les développements avant leur mise en production</details>


