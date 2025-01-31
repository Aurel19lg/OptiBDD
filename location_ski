1️⃣ Liste des clients (toutes les informations) dont le nom commence par un D
```sql
SELECT * FROM clients 
WHERE nom LIKE 'D%';
```

2️⃣ Nom et prénom de tous les clients
```sql
SELECT prenom, nom
FROM clients;
```

3️⃣ Liste des fiches (n°, état) pour les clients (nom, prénom) qui habitent en Loire Atlantique (44)
```sql
SELECT f.noFic, f.etat, c.nom, c.prenom
FROM fiches f
JOIN clients c ON f.noCli = c.noCli
WHERE c.cpo LIKE '44%';
```

4️⃣ Détail de la fiche n°1002
```sql
SELECT 
    f.noFic,
    c.nom,
    c.prenom,
    a.refArt,
    a.designation,
    lf.depart,
    lf.retour,
    t.prixJour,
    (DATEDIFF(COALESCE(lf.retour, NOW()), lf.depart) + 1) * t.prixJour AS montant
FROM fiches f
JOIN clients c ON f.noCli = c.noCli
JOIN lignesFic lf ON f.noFic = lf.noFic
JOIN articles a ON lf.refArt = a.refArt
JOIN grilleTarifs gt ON a.codeGam = gt.codeGam AND a.codeCate = gt.codeCate
JOIN tarifs t ON gt.codeTarif = t.codeTarif
WHERE f.noFic = 1002;
```

5️⃣ Prix journalier moyen de location par gamme
```sql
SELECT 
    gammes.libelle AS Gamme,
    AVG(tarifs.prixJour) AS "tarif journalier moyen"
FROM grilleTarifs gt
JOIN tarifs ON gt.codeTarif = tarifs.codeTarif
JOIN gammes ON gt.codeGam = gammes.codeGam
GROUP BY gammes.libelle;
```

6️⃣ Détail de la fiche n°1002 avec le total
```sql
SELECT 
    f.noFic,
    c.nom,
    c.prenom,
    a.refArt,
    a.designation,
    lf.depart,
    lf.retour,
    t.prixJour,
    (DATEDIFF(lf.retour, lf.depart) + 1) * t.prixJour AS Montant,
    SUM((DATEDIFF(lf.retour, lf.depart) + 1) * t.prixJour) OVER (PARTITION BY f.noFic) AS Total
FROM fiches f
JOIN clients c ON f.noCli = c.noCli
JOIN lignesFic lf ON f.noFic = lf.noFic
JOIN articles a ON lf.refArt = a.refArt
JOIN grilleTarifs gt ON a.codeGam = gt.codeGam AND a.codeCate = gt.codeCate
JOIN tarifs t ON gt.codeTarif = t.codeTarif
WHERE f.noFic = 1002;
```


7️⃣ Grille des tarifs
```sql
SELECT 
    c.libelle AS `Catégorie`,
    g.libelle AS `Gamme`,
    t.libelle AS `Tarif`,
    t.prixJour AS `Prix`
FROM grilleTarifs gt
JOIN tarifs t ON gt.codeTarif = t.codeTarif
JOIN gammes g ON gt.codeGam = g.codeGam
JOIN categories c ON gt.codeCate = c.codeCate
ORDER BY c.libelle, g.libelle, t.libelle;
```


8️⃣ Liste des locations de la catégorie SURF
```sql
SELECT 
    a.refArt,
    a.designation,
    COUNT(lf.noLig) AS nbLocation
FROM articles a
JOIN categories c ON a.codeCate = c.codeCate
JOIN lignesFic lf ON a.refArt = lf.refArt
WHERE c.libelle = 'SURF'
GROUP BY a.refArt, a.designation;
```

9️⃣ Calcul du nombre moyen d’articles loués par fiche de location
```sql
SELECT 
    ROUND(AVG(nb_lignes_par_fiche), 6) AS nb_lignes_moyen_par_fiche
FROM (
    SELECT 
        f.noFic,
        COUNT(lf.noLig) AS nb_lignes_par_fiche
    FROM fiches f
    JOIN lignesFic lf ON f.noFic = lf.noFic
    GROUP BY f.noFic
) AS nb_lignes;
```


10 - Calcul du nombre de fiches de location établies pour les catégories de location Ski alpin, Surf et Patinette
```sql
SELECT 
    c.libelle AS `catégorie`,
    COUNT(lf.refArt) AS `nombre de location`
FROM lignesFic lf
JOIN articles a ON lf.refArt = a.refArt
JOIN categories c ON a.codeCate = c.codeCate
WHERE c.libelle IN ('Ski alpin', 'Surf', 'Patinette')
GROUP BY c.libelle;
```


11 Calcul du montant moyen des fiches de location
```sql
SELECT 
    ROUND(AVG(montant_fiche), 4) AS `montant moyen d'une fiche de location`
FROM (
    SELECT 
        f.noFic,
        SUM((DATEDIFF(COALESCE(lf.retour, NOW()), lf.depart) + 1) * t.prixJour) AS montant_fiche
    FROM fiches f
    JOIN lignesFic lf ON f.noFic = lf.noFic
    JOIN articles a ON lf.refArt = a.refArt
    JOIN grilleTarifs gt ON a.codeGam = gt.codeGam AND a.codeCate = gt.codeCate
    JOIN tarifs t ON gt.codeTarif = t.codeTarif
    GROUP BY f.noFic
) AS total_montants;
```
