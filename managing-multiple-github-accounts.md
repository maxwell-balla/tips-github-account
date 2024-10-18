# Guide pour configurer plusieurs comptes GitHub avec SSH

Lorsque vous avez plusieurs comptes GitHub (par exemple un compte personnel et un compte de travail), vous devez utiliser des clés SSH différentes pour chaque compte. Ce guide vous explique comment configurer cela facilement.

## Étape 1 : Générer des clés SSH distinctes

Vous devez d'abord générer une clé SSH pour chaque compte GitHub.

### Générer une clé pour votre compte **personnel** :
1. Ouvrez un terminal.
2. Tapez la commande suivante (remplacez `votre_email_personnel@example.com` par l'email associé à votre compte GitHub personnel) :
   ```bash
   ssh-keygen -t rsa -C "votre_email_personnel@example.com"
   ```
3. Lorsque le terminal demande où sauvegarder la clé, entrez ce chemin pour bien distinguer les comptes :
   ```bash
   ~/.ssh/id_rsa_personal
   ```

### Générer une clé pour votre compte **travail** :
Répétez le même processus pour le compte travail :
1. Tapez la commande suivante en changeant l'email par celui de votre compte GitHub professionnel :
   ```bash
   ssh-keygen -t rsa -C "votre_email_travail@example.com"
   ```
2. Pour le chemin, entrez cette fois :
   ```bash
   ~/.ssh/id_rsa_work
   ```

## Étape 2 : Ajouter les clés SSH à l'agent SSH

L'agent SSH est un programme qui garde vos clés en mémoire, de sorte que vous n'avez pas besoin de les recharger manuellement à chaque fois.

### Ajouter la clé **personnelle** à l'agent :
1. Tapez cette commande :
   ```bash
   ssh-add ~/.ssh/id_rsa_personal
   ```

### Ajouter la clé **travail** à l'agent :
1. Ensuite, ajoutez la clé de travail de la même manière :
   ```bash
   ssh-add ~/.ssh/id_rsa_work
   ```

## Étape 3 : Configurer le fichier `~/.ssh/config`

Le fichier `config` dans le dossier `.ssh` permet de dire à Git quel compte utiliser pour chaque projet GitHub.

1. Ouvrez le fichier `~/.ssh/config` (ou créez-le s'il n'existe pas encore) :
   ```bash
   nano ~/.ssh/config
   ```
2. Ajoutez les configurations suivantes pour vos deux comptes GitHub :

    - Pour le compte **personnel** :
      ```bash
      Host github-personal
        HostName github.com
        User git
        IdentityFile ~/.ssh/id_rsa_personal
      ```

    - Pour le compte **travail** :
      ```bash
      Host github-work
        HostName github.com
        User git
        IdentityFile ~/.ssh/id_rsa_work
      ```

NB: l'alias que vous allez utiliser ici est le Host

## Étape 4 : Ajouter vos clés publiques à GitHub

Chaque clé SSH a une partie publique et une partie privée. Vous devez copier la partie publique de chaque clé et l’ajouter sur GitHub pour qu’il vous reconnaisse.

### Pour le compte **personnel** :
1. Copiez la clé publique :
   ```bash
   cat ~/.ssh/id_rsa_personal.pub
   ```
2. Allez dans [les paramètres GitHub](https://github.com/settings/keys) de votre compte personnel, et ajoutez une nouvelle clé SSH avec le contenu que vous venez de copier.

### Pour le compte **travail** :
1. Copiez la clé publique de la même manière :
   ```bash
   cat ~/.ssh/id_rsa_work.pub
   ```
2. Ajoutez cette clé sur votre compte GitHub professionnel en suivant le même processus.

## Étape 5 : Tester la connexion SSH

Pour vous assurer que tout fonctionne, testez la connexion à GitHub avec les alias que vous avez créés.

### Test pour le compte **personnel** :
```bash
ssh -T git@github-personal
```

### Test pour le compte **travail** :
```bash
ssh -T git@github-work
```

## Étape 6 : Configurer l'URL distante de vos dépôts

Maintenant que vos clés sont en place, vous devez configurer chaque dépôt pour qu’il utilise le bon compte GitHub (personnel ou travail).

### Pour un projet personnel :
1. Allez dans le dossier de votre projet et tapez cette commande pour associer le dépôt à votre compte personnel :
   ```bash
   git remote set-url origin git@github-personal:votre_username_personnel/repo.git
   ```

### Pour un projet travail :
1. Pour un projet professionnel, utilisez cette commande :
   ```bash
   git remote set-url origin git@github-work:votre_username_travail/repo.git
   ```

Si tout est correctement configuré, vous devriez voir un message de réussite.

🙏