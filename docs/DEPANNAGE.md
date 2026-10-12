# Dépannage raisonné

Reproduire le symptôme sur un état connu, lire la réponse ou la trace, formuler une hypothèse puis effectuer un contrôle ciblé. Ne pas effacer tout le projet pour faire disparaître l’erreur.

| Symptôme | Hypothèse à vérifier | Contrôle et correction |
| --- | --- | --- |
| Ancienne recherche réapparaît | Résultats acceptés sans identité de demande | Résoudre B puis A dans un test et protéger l’écriture courante. |
| Erreur visible à chaque frappe | AbortError traité comme panne | Distinguer annulation attendue et échec réel. |
| Page vide après filtre | Ancien numéro de page conservé | Réinitialiser page à 1 lors du changement des critères. |
| Comparaison très variable | Cache, réseau ou build différents | Conserver les conditions et les valeurs brutes ; répéter. |
| INP absent de Lighthouse | Audit limité au chargement | Mesurer un parcours interactif adapté ; ne pas rebaptiser TBT. |
| Décalages de mise en page | Zone de résultat sans stabilité | Identifier l’élément déplacé puis réserver un espace cohérent. |
| memo partout sans gain | Optimisation sans symptôme mesuré | Retirer la complexité non justifiée et documenter l’observation. |

Si le blocage concerne une installation manquante du Day 0, le noter séparément du résultat métier. Demander une aide ciblée après avoir inscrit la commande, le statut et le message utile dans le journal.
