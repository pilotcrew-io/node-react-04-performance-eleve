# Tutoriel 04 — React · Mesurer et améliorer le catalogue

Le temps indiqué suppose [les prérequis](PREREQUIS.md) terminés. Le [contrat commun](PREREQUIS.md) est la référence pour les champs, routes et limites. Consigner les résultats au fil de l’eau dans [le journal](../travail/REPONSES.md).

## 1. Construire un protocole avant de modifier — 30 min

Notez le commit, la version du navigateur, la machine, la taille de fenêtre, le profil réseau, le ralentissement CPU et l’état du cache. Préparez deux parcours : chargement initial du catalogue, puis recherche rapide suivie d’un changement de page et d’un ajout au panier. Écrivez les interactions dans l’ordre ; toute comparaison utilisera le même ordre. Fermez les tâches parasites et gardez les mêmes conditions pour les séries avant et après.

Servez un build du frontend, avec un proxy /api qui pointe sur le même backend local. Dans frontend/, après avoir défini les scripts :

```bash
npm run build
npm run preview
```

Le navigateur de mesure ouvre 127.0.0.1:4173. Le serveur Vite de développement sur 5173 reste utile pour coder, mais ses transformations et son mode de développement ne doivent pas être mélangés au protocole du build. La prévisualisation locale ne constitue pas un hébergement de production.

Enregistrez une première trace de chargement et une trace d’interactions. Relevez aussi le nombre de requêtes, le poids des fichiers chargés et le comportement du focus. Les trente produits forment un petit jeu : il se peut qu’aucun coût important ne soit observable. Le signaler est une conclusion valide ; il ne faut ni inventer une lenteur ni augmenter artificiellement une note.

## 2. Brancher recherche, filtres, tri et pages — 55 min

Ajoutez un champ Recherche, un choix de catégorie, un tri et une pagination. Chaque contrôle a un libellé explicite. Décrivez la demande sous forme d’un objet cohérent : q, category, sort, page, pageSize. Construisez l’URL avec URLSearchParams, en omettant category lorsqu’aucun filtre n’est actif. Vous avez déjà validé ces paramètres côté serveur ; les limites visibles doivent être compatibles avec cette validation.

Lorsque la recherche, la catégorie, le tri ou la taille de page changent, repartez à la page 1. Sans cette règle, un utilisateur qui était en page 3 peut voir une page vide après un filtre pourtant riche de résultats. Calculez le nombre de pages depuis total et pageSize ; ne déduisez pas la fin du catalogue du seul nombre d’éléments reçus. Différenciez « aucune correspondance » et une erreur de lecture.

Choisissez une politique de déclenchement : bouton Rechercher ou délai de stabilisation explicite, par exemple 250 ms comme choix pédagogique. La saisie doit rester immédiatement visible même si la lecture réseau est différée. Un délai réduit certaines requêtes ; il ne protège pas à lui seul contre une réponse ancienne. Conservez les contrôles accessibles pendant les lectures et annoncez sobrement le nombre de résultats obtenu.

Vérifiez q avec une casse ou un accent différent, une catégorie précise, un tri de prix et la dernière page. Le tri doit être global sur l’ensemble filtré. Une solution qui télécharge tout puis trie seulement les douze lignes affichées ne suit pas le contrat. Elle doit être corrigée avant d’interpréter les mesures de performance.

## 3. Annuler et ignorer ce qui n’est plus courant — 50 min

Donnez à chaque synchronisation un AbortController et transmettez son signal jusqu’à fetch. Le nettoyage annule la requête quand la demande change ou quand le composant est démonté. Si vous utilisez un délai de stabilisation, nettoyez aussi ce délai. Une annulation attendue ne doit pas afficher une erreur de panne.

Protégez en plus l’application du résultat par une identité de demande ou une garde équivalente. Même une opération dont le signal est annulé peut être proche de sa résolution, et un double de test peut simuler une réponse tardive. L’état courant ne doit recevoir que le résultat qui lui correspond. Cette règle évite qu’une recherche « lampe » tardive efface les résultats de la recherche « bureau » déjà affichés.

Préservez une stratégie visuelle lisible : conserver les anciens résultats avec une indication de mise à jour, ou réserver une zone de chargement stable. Évitez de faire sauter les contrôles au-dessus du contenu. Une zone aria-live ne doit pas répéter trente fiches à chaque frappe ; annoncez un état bref et pertinent.

Écrivez un test qui commence la demande A, commence la demande B, résout B puis A, et vérifie que B reste affichée. Un autre test démonte le composant avant la résolution et observe le signal d’annulation. Il ne suffit pas de chercher le mot abort dans le code. Ne réutilisez pas ce mécanisme comme une annulation métier de réservation : abandonner une réponse POST n’annule pas un stock déjà réservé.

## 4. Lire les métriques et choisir une correction — 55 min

Repérez le contenu qui détermine le LCP dans votre trace de chargement. Sur ce catalogue sans images externes, il peut s’agir d’un bloc de texte ; n’imaginez pas un problème d’image absent. Pour la réactivité, observez les interactions réellement effectuées et les tâches longues du thread principal. Pour la stabilité, identifiez les éléments qui se déplacent de façon inattendue au chargement ou pendant une mise à jour.

Les Core Web Vitals actuels sont LCP, INP et CLS. Les repères « bons » sont respectivement au plus 2,5 secondes, 200 millisecondes et 0,1, appréciés au 75e percentile des visites terrain avec segmentation pertinente. Ils ne transforment pas trois essais locaux en verdict terrain. Un audit Lighthouse de chargement ne mesure pas à lui seul l’INP d’un parcours interactif ; TBT est un signal de diagnostic distinct.

Choisissez une seule correction motivée par les traces : éviter un travail synchrone inutile pendant la saisie, réduire un rendu mesuré comme coûteux, stabiliser une zone, ou charger le panier à la demande s’il alourdit réellement le chargement initial. Donnez l’hypothèse, le code concerné et la mesure qui pourrait la réfuter. useMemo, memo ou lazy n’apportent pas automatiquement un gain ; ils ont un coût de complexité et doivent répondre à une observation.

Si aucun problème significatif n’est visible, faites vérifier une propriété de correction et documentez l’absence de gain mesurable. Vous pouvez étudier séparément une fixture amplifiée pour comprendre une limite, mais étiquetez cette expérience et ne la mélangez pas aux résultats du catalogue de trente produits. Aucun envoi de métriques à un service analytique externe n’est nécessaire.

## 5. Comparer sans modifier le protocole — 55 min

Recompilez le frontend après la correction, servez-le de la même manière et rejouez les deux parcours. Effectuez trois répétitions comparables de chaque condition, gardez les valeurs brutes et indiquez médiane ainsi que dispersion observée. Trois mesures pédagogiques ne constituent pas une étude statistique ; elles servent à détecter un résultat instable ou une comparaison truquée par le cache.

Reliez chaque variation à une trace. Si le temps réseau varie tandis que le code concerné ne change pas, n’attribuez pas automatiquement la différence à React. Vérifiez également le coût d’interaction : un chargement plus léger peut déporter du travail vers l’ouverture du panier. Une optimisation acceptable ne dégrade pas discrètement le parcours au clavier ou les messages de chargement.

Exécutez les tests de réponses tardives, d’annulation, de changement de page et de remise à page 1. Vérifiez le backend sur q trop long, paramètre répété et pageSize invalide pour maintenir la frontière du contrat. Puis relancez tests et builds des deux projets. Le protocole doit garder les mêmes prix, stocks initiaux et routes ; une réduction de fonctionnalités n’est pas une preuve d’accélération du même parcours.

Renseignez un tableau avant/après avec des cellules « non mesuré » lorsqu’un outil ne fournit pas la métrique. Un petit panneau d’observations ajouté à l’application peut être utile, mais ne le présentez pas comme un calcul officiel des Core Web Vitals s’il utilise une approximation. Préférez alors la trace des outils ou une instrumentation conforme à la documentation.

## 6. Présenter une conclusion falsifiable — 25 min

Montrez une trace avant, une trace après, la modification unique et les tests qui protègent le comportement. Expliquez une incertitude restante : peu de produits, machine unique, aucune donnée terrain ou faible nombre de répétitions. Une conclusion précise peut être « moins de requêtes inutiles, aucun gain LCP mesurable dans ce protocole ».

Votre binôme doit pouvoir reconstruire les conditions depuis le journal. Il vérifiera aussi que le dernier filtre demandé reste à l’écran malgré une ancienne réponse. Remettez les données brutes, les captures ou traces, les commandes et le commit. Gardez les rapports volumineux hors du code source selon les règles de remise, sans les remplacer par une valeur inventée.
