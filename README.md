# Version 8 — deux exports

Remplacer les fichiers Python du dépôt par ceux de l'archive. Fichier principal : app.py. Modules nécessaires : fusion.py, pnm.py, parametrage.py, trame.py. Installer requirements.txt.

## Fonctionnement
1. Charger FEC initial et PNM analytique (CP850 pour LAUNAY).
2. Vérifier/modifier les correspondances des journaux et comptes 512. Les codes 10,50,51,60,70 sont proposés en OD,BNP,CMB,HA,VT pour le format documenté. Les noms des journaux ne sont pas changés ; les comptes 512 restent inchangés par défaut.
3. Télécharger les deux fichiers : PENNYLANE_IMPORT.csv et ECRITURES_A_VERIFIER.csv.

La numérotation est automatique, sans case à cocher : JOURNAL-REFERENCE_PNM-COMPTEUR_UNIQUE. Le compteur évite les collisions entre exercices, dates et pièces. Si aucune référence unique n'est déterminée, la pièce va en vérification avec un identifiant technique. Libellé de ligne = PieceRef du FEC. Libellé de pièce commun à la pièce. Dates, montants et lettrage source conservés.

PENNYLANE_IMPORT.csv reprend les 15 colonnes de la trame fournie dans leur ordre, plus Poids de la catégorie en dernière colonne. UTF-8 BOM, point-virgule, une ligne d'en-tête. Mapper Identifiant de ligne et Poids de la catégorie pour les ventilations. Les montants comptables sont répétés uniquement pour la ventilation, ils ne doivent pas être sommés sans dédupliquer l'identifiant.

Toute pièce source contenant une ligne problématique est mise EN ENTIER dans ECRITURES_A_VERIFIER.csv : ambiguïté PNM, références contradictoires, dates multiples, déséquilibre, poids invalide, code journal ou compte incompatible. Ce fichier reprend les mêmes colonnes, plus le motif, l'écriture FEC, la référence PNM et les candidats. Ne pas importer ce deuxième fichier avant correction.

Les deux fichiers se complètent : aucune ligne supprimée, aucune ligne source commune aux deux. L'export principal est contrôlé pour l'équilibre de chaque pièce et l'unicité journal/date/libellé. Les contrôles locaux ne peuvent pas vérifier les paramètres propres au dossier Pennylane ou la présence d'écritures déjà importées. Le lettrage source est conservé ; sa compatibilité dépend aussi du plan comptable du dossier.

La normalisation des poids reste un choix explicite. Sans correction, les pièces concernées vont simplement en vérification et ne bloquent pas les autres. Aucun compte 471 ni montant d'équilibrage n'est inventé.

Le FEC original doit être conservé : la trame ne reprend pas toutes ses colonnes, notamment DateLet et les libellés de ligne historiques. Les détails de rapprochement restent visibles dans l'app.

Tests : python -m unittest discover -s . -v
Lancement : python -m streamlit run app.py
