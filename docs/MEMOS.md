# Mémos — React · Mesurer et améliorer le catalogue

À relire pour expliquer les décisions ; les mémos ne remplacent pas les preuves de [l’évaluation](EVALUATION.md).

## Mesure, hypothèse et correction

Une optimisation commence par un symptôme et un protocole. La comparaison garde constants le scénario et l’environnement, puis modifie un élément identifiable. Un résultat peut contredire l’hypothèse initiale ; il reste utile. Déplacer le coût vers une autre interaction ou supprimer une fonctionnalité ne démontre pas que le même parcours est meilleur.

## Les trois Core Web Vitals

LCP concerne le rendu du contenu principal, INP la réactivité aux interactions observées et CLS les déplacements visuels inattendus. Les valeurs de référence servent à interpréter une expérience utilisateur avec sa population et ses conditions. L’atelier est local : il produit des indices et des traces, sans données réelles d’utilisateurs. Il emploie INP ; FID est un ancien indicateur et ne doit pas être présenté comme le troisième Core Web Vital actuel.

## Laboratoire et terrain

Un build local, un profil réseau simulé et quelques interactions permettent une comparaison contrôlée. Ils ne reproduisent ni la diversité des appareils ni tous les usages d’un site réel. Une métrique absente se note non mesurée. Un score Lighthouse agrège des diagnostics de chargement ; il ne vaut pas une observation complète d’INP sur une visite interactive. Une approximation affichée dans l’application conserve son étiquette d’approximation.

## Délai, annulation et identité

Le délai de stabilisation retarde le déclenchement. AbortController peut interrompre la lecture et ses ressources associées. La garde d’identité empêche une ancienne demande d’écrire le nouvel état. Ces trois mécanismes répondent à des problèmes différents. Pour un POST de réservation, abandonner l’attente ne retire pas l’effet déjà appliqué au serveur ; l’idempotence demeure nécessaire.

## Pagination et état courant

Le serveur connaît le total filtré ; la page n’est qu’un extrait. Changer la définition de l’ensemble doit remettre la pagination dans un état cohérent. Les filtres sont une intention courante ; les résultats peuvent appartenir temporairement à une demande précédente. Nommer cette différence évite un écran qui présente d’anciens produits comme s’ils correspondaient déjà au nouveau filtre.

## Optimisations React

La mémoïsation conserve un calcul ou évite certains rendus dans des conditions précises. Elle ne compense pas une architecture incohérente ni un effet qui boucle. Le chargement différé peut réduire le travail initial et en ajouter plus tard. Une petite liste de trente produits peut fonctionner correctement sans optimisation supplémentaire ; une décision documentée de ne pas complexifier constitue un résultat professionnel recevable.
