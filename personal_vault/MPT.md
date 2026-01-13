
 Structure technique des phases :

### Phase 1 : Identification des Étudiants Éligibles
On crée une vue pour filtrer les étudiants selon tes critères de notes et de modules non valides: la moyenne doit être supérieure a 12 et l’étudiant ne doit pas avoir de modules non valide en premiere année.

```sql
CREATE OR REPLACE VIEW ETUDIANTS_ELIGIBLES AS
SELECT matricule, nom, prenom, code_filiere, moy_gen
FROM ETUDIANT
WHERE moy_gen > 12 
-- ET nb_nv_annee1 = 0  <-- Condition 0 modules non validés
;
```

### Phase 2 : Vérification de la Compatibilité des Vœux
Ici, on croise les vœux de l'étudiant avec la table `COMPATIBLE` pour s'assurer que l'offre choisie correspond à sa filière.

```sql
CREATE OR REPLACE VIEW VOEUX_VALIDES AS
SELECT v.matricule, v.id_offre, v.rang_preference
FROM Vouer v
JOIN ETUDIANT e ON v.matricule = e.matricule
JOIN COMPATIBLE c ON v.id_offre = c.id_offre AND e.code_filiere = c.code_filiere
WHERE v.etat = 'En attente'; -- On ne prend que les vœux non encore traités
```

### Phase 3 : Calcul du Score et Classement (FIFO)
On applique ta formule de calcul de score.
Pour des raison de simplicité on a mit le score égale a cette relation `(MoyenneAnneeDerniere * MoyenneS3) / 2`.

```sql
CREATE OR REPLACE VIEW CLASSEMENT_SELECTION AS
SELECT 
    v.matricule, 
    v.id_offre,
    v.rang_preference,
    -- Ta formule : (Moyenne * Moyenne) / 2
    ((e.moy_gen * e.moy_gen) / 2) AS score_selection, 
    -- On crée un rang par offre, trié par score puis par date (FIFO)
    ROW_NUMBER() OVER(PARTITION BY v.id_offre ORDER BY ((e.moy_gen * e.moy_gen) / 2) DESC, v.date_saisie ASC) as rang_classement
FROM VOEUX_VALIDES v
JOIN ETUDIANT e ON v.matricule = e.matricule;
```

### Phase 4 : Vérification de la Capacité Financière
On vérifie si l'étudiant a déposé un document de type 'FINANCE' qui a été validé par un administrateur.

```sql
CREATE OR REPLACE VIEW ETUDIANTS_FINANCIEREMENT_PRETS AS
SELECT DISTINCT d.matricule
FROM DOCUMENT d
JOIN VALIDE v ON d.id_doc = v.id_doc
WHERE d.type_doc = 'CAPACITE_FINANCIERE' 
AND d.statut = 'Valide';
```

### Phase 5 : Attribution Finale (Le MPT complet)
Cette requête finale réunit tout : elle prend les candidats classés, vérifie s'ils ont l'argent, et compare leur rang au nombre de places disponibles dans l'offre.

```sql
SELECT 
    cs.matricule,
    e.nom,
    e.prenom,
    o.id_offre,
    p.nom as partenaire,
    cs.score_selection,
    CASE 
        WHEN cs.rang_classement <= CAST(o.Nombre_place AS INT) 
             AND efp.matricule IS NOT NULL THEN 'ACCEPTÉ DÉFINITIF'
        WHEN cs.rang_classement <= CAST(o.Nombre_place AS INT) 
             AND efp.matricule IS NULL THEN 'EN ATTENTE (MANQUE JUSTIFICATIF FINANCIER)'
        ELSE 'LISTE D''ATTENTE (RANG TROP ÉLEVÉ)'
    END AS STATUT_FINAL
FROM CLASSEMENT_SELECTION cs
JOIN OFFRE o ON cs.id_offre = o.id_offre
JOIN PARTENAIRE p ON o.id_partenaire = p.id_partenaire
JOIN ETUDIANT e ON cs.matricule = e.matricule
LEFT JOIN ETUDIANTS_FINANCIEREMENT_PRETS efp ON cs.matricule = efp.matricule
ORDER BY o.id_offre, cs.rang_classement;
```

