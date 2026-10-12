# Évaluation et preuves

Le barème porte sur ce qui est **démontré et expliqué**. Une suite non exécutée ou une capture absente n’est pas un succès implicite. Les choix internes peuvent différer du corrigé si le contrat et les critères sont respectés.

| Critère | Points / 20 | Preuve attendue |
| --- | --- | --- |
| Protocole reproductible | 3 | Commit, build, machine et conditions enregistrés avant modification. |
| Recherche et pagination correctes | 4 | Contrat respecté et remise à page 1 testée. |
| Réponses tardives et annulation | 4 | A/B inversées, démontage et erreurs d’annulation vérifiés. |
| Mesures interprétées honnêtement | 3 | LCP/INP/CLS distingués ; valeurs brutes et limites terrain présentes. |
| Correction et non-régression | 3 | Une hypothèse étayée, mêmes scénarios et clavier préservé. |
| Bilan et traces | 3 | Avant/après reproductible, conclusion mesurée et limites explicites. |

## Questions d’explication

1. Que résout un délai de stabilisation que l’annulation ne résout pas ?
2. Pourquoi faut-il aussi protéger l’identité de la demande ?
3. Pourquoi une page 3 peut-elle devenir incohérente après filtrage ?
4. Peut-on annoncer un INP terrain avec trois essais locaux ?
5. Quelle preuve justifierait l’ajout de memo ou de lazy ?

## Format de preuve

Pour chaque contrôle : commit, environnement, commande ou action, résultat attendu, résultat observé, interprétation et lien vers le rapport/capture. Séparer backend, frontend, navigateur et éventuelle CI distante. Pour une mesure : conserver aussi protocole et valeurs brutes.

Un invariant critique non respecté reste non acquis même si les points de présentation sont obtenus. L’évaluation est formative ; elle n’atteste ni une mise en production, ni un niveau senior, ni la sécurité d’un service public.
