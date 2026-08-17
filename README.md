# Tools Obsession — canal de mise à jour

Ce dépôt n'est pas le code source de l'application : il **héberge les fichiers
de mise à jour** que Tools Obsession télécharge automatiquement.

```
manifest.json       liste des fichiers de la version courante, avec leur SHA-256
manifest.json.sig   signature Ed25519 du manifeste
files/              les fichiers eux-mêmes
.nojekyll           empêche GitHub Pages d'ignorer le dossier _internal/
```

## Pourquoi c'est public

L'application de chaque utilisateur doit pouvoir lire ces fichiers en HTTPS
sans authentification. Rien de sensible ne s'y trouve : ce sont les binaires
distribués de toute façon avec l'installeur.

## Ce qui protège la mise à jour

`manifest.json` est signé avec une clé Ed25519 dont la **clé privée n'est pas
ici** — elle ne quitte pas la machine de l'auteur. L'application embarque
uniquement la clé publique correspondante et **refuse tout manifeste dont la
signature ne correspond pas**.

Conséquence : même si ce dépôt était compromis, personne ne pourrait faire
installer un fichier modifié aux utilisateurs. Chaque fichier est en plus
vérifié individuellement par son empreinte SHA-256 avant d'être écrit.

## Ne pas modifier à la main

Le contenu est généré et poussé par `RELEASE.bat` depuis le projet. Une
modification manuelle invaliderait la signature et l'updater refuserait la
mise à jour.
