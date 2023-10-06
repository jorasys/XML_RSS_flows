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
