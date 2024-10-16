### Fiche Mémo : SQL (Structured Query Language)

Le **SQL (Structured Query Language)** est un langage standard utilisé pour interagir avec des bases de données relationnelles. Il permet d'effectuer différentes opérations, comme interroger (SELECT), insérer (INSERT), mettre à jour (UPDATE), et supprimer des données (DELETE).

Voici une fiche mémo détaillée avec des concepts de base et avancés du SQL, accompagnée d'exemples en français.

---

## 1. **Création d'une base de données et de tables**

### Syntaxe :
```sql
CREATE DATABASE nom_base_donnees;

CREATE TABLE nom_table (
  colonne1 TYPE_DONNEE,
  colonne2 TYPE_DONNEE,
  colonne3 TYPE_DONNEE
);
```

### Exemple :
Créons une base de données `Bibliotheque` et une table `Livres`.

```sql
CREATE DATABASE Bibliotheque;

USE Bibliotheque;

CREATE TABLE Livres (
  id INT PRIMARY KEY AUTO_INCREMENT,
  titre VARCHAR(100),
  auteur VARCHAR(100),
  annee_publication INT,
  genre VARCHAR(50)
);
```

### Résultat :
Une base de données nommée **Bibliotheque** et une table **Livres** avec les colonnes :
- `id`: identifiant unique pour chaque livre.
- `titre`: titre du livre.
- `auteur`: nom de l’auteur.
- `annee_publication`: année de publication.
- `genre`: genre du livre.

---

## 2. **Insertion de données**

### Syntaxe :
```sql
INSERT INTO nom_table (colonne1, colonne2, colonne3)
VALUES (valeur1, valeur2, valeur3);
```

### Exemple :
Ajoutons des livres à la table **Livres**.

```sql
INSERT INTO Livres (titre, auteur, annee_publication, genre)
VALUES ('Le Petit Prince', 'Antoine de Saint-Exupéry', 1943, 'Fiction'),
       ('Les Misérables', 'Victor Hugo', 1862, 'Roman'),
       ('La Peste', 'Albert Camus', 1947, 'Philosophie');
```

### Résultat :
La table **Livres** contient désormais trois enregistrements.

| id  | titre            | auteur                     | annee_publication | genre        |
| --- | ---------------- | -------------------------- | ----------------- | ------------ |
| 1   | Le Petit Prince   | Antoine de Saint-Exupéry    | 1943              | Fiction      |
| 2   | Les Misérables    | Victor Hugo                | 1862              | Roman        |
| 3   | La Peste          | Albert Camus               | 1947              | Philosophie  |

---

## 3. **Sélection de données (SELECT)**

### Syntaxe :
```sql
SELECT colonne1, colonne2 FROM nom_table
WHERE condition
ORDER BY colonne ASC/DESC;
```

### Exemple :
Recherchons tous les livres de genre **Fiction** et trions-les par année de publication décroissante.

```sql
SELECT titre, auteur, annee_publication
FROM Livres
WHERE genre = 'Fiction'
ORDER BY annee_publication DESC;
```

### Résultat :
| titre           | auteur                  | annee_publication |
| --------------- | ----------------------- | ----------------- |
| Le Petit Prince | Antoine de Saint-Exupéry | 1943              |

---

## 4. **Mise à jour des données (UPDATE)**

### Syntaxe :
```sql
UPDATE nom_table
SET colonne1 = nouvelle_valeur
WHERE condition;
```

### Exemple :
Modifions l'année de publication de **La Peste**.

```sql
UPDATE Livres
SET annee_publication = 1948
WHERE titre = 'La Peste';
```

### Résultat :
La table **Livres** sera mise à jour comme suit :

| id  | titre          | auteur        | annee_publication | genre        |
| --- | -------------- | ------------- | ----------------- | ------------ |
| 3   | La Peste       | Albert Camus  | 1948              | Philosophie  |

---

## 5. **Suppression de données (DELETE)**

### Syntaxe :
```sql
DELETE FROM nom_table
WHERE condition;
```

### Exemple :
Supprimons le livre **Les Misérables** de la table.

```sql
DELETE FROM Livres
WHERE titre = 'Les Misérables';
```

### Résultat :
La table **Livres** après la suppression :

| id  | titre            | auteur                     | annee_publication | genre        |
| --- | ---------------- | -------------------------- | ----------------- | ------------ |
| 1   | Le Petit Prince   | Antoine de Saint-Exupéry    | 1943              | Fiction      |
| 3   | La Peste          | Albert Camus               | 1948              | Philosophie  |

---

## 6. **Jointures (JOINS)**

### Syntaxe :
Les jointures sont utilisées pour récupérer des données à partir de plusieurs tables.

- **INNER JOIN** : Retourne les lignes qui ont des correspondances dans les deux tables.
- **LEFT JOIN** : Retourne toutes les lignes de la table de gauche et les correspondances de la table de droite.
- **RIGHT JOIN** : Retourne toutes les lignes de la table de droite et les correspondances de la table de gauche.

```sql
SELECT colonne1, colonne2
FROM table1
INNER JOIN table2 ON table1.colonne = table2.colonne;
```

### Exemple :
Créons une table **Auteurs** et joignons-la à la table **Livres**.

```sql
CREATE TABLE Auteurs (
  id INT PRIMARY KEY AUTO_INCREMENT,
  nom VARCHAR(100),
  nationalite VARCHAR(50)
);

INSERT INTO Auteurs (nom, nationalite)
VALUES ('Antoine de Saint-Exupéry', 'Française'),
       ('Albert Camus', 'Française');

SELECT Livres.titre, Auteurs.nom, Auteurs.nationalite
FROM Livres
INNER JOIN Auteurs ON Livres.auteur = Auteurs.nom;
```

### Résultat :
| titre           | nom                      | nationalite |
| --------------- | ------------------------ | ----------- |
| Le Petit Prince | Antoine de Saint-Exupéry  | Française   |
| La Peste        | Albert Camus              | Française   |

---

## 7. **Fonctions d'agrégation (COUNT, SUM, AVG, MAX, MIN)**

### Syntaxe :
Les fonctions d'agrégation permettent de calculer des statistiques sur les données.

- `COUNT()` : Compte le nombre de lignes.
- `SUM()` : Calcule la somme.
- `AVG()` : Calcule la moyenne.
- `MAX()` : Trouve la valeur maximale.
- `MIN()` : Trouve la valeur minimale.

```sql
SELECT COUNT(*), SUM(colonne), AVG(colonne), MAX(colonne), MIN(colonne)
FROM nom_table
WHERE condition;
```

### Exemple :
Comptons le nombre de livres et trouvons l'année de publication la plus ancienne.

```sql
SELECT COUNT(*), MIN(annee_publication)
FROM Livres;
```

### Résultat :
| COUNT(*) | MIN(annee_publication) |
| -------- | ---------------------- |
| 2        | 1943                   |

---

## 8. **Groupement des données (GROUP BY)**

### Syntaxe :
Le `GROUP BY` permet de regrouper les lignes ayant des valeurs communes dans une colonne, et d'utiliser des fonctions d'agrégation sur ces groupes.

```sql
SELECT colonne1, COUNT(*)
FROM nom_table
GROUP BY colonne1;
```

### Exemple :
Regroupons les livres par genre et comptons combien il y en a dans chaque catégorie.

```sql
SELECT genre, COUNT(*)
FROM Livres
GROUP BY genre;
```

### Résultat :
| genre        | COUNT(*) |
| ------------ | -------- |
| Fiction      | 1        |
| Philosophie  | 1        |

---

## 9. **Conditions avancées (HAVING)**

### Syntaxe :
Le `HAVING` est utilisé pour appliquer des conditions après un `GROUP BY`, contrairement au `WHERE` qui filtre les lignes avant le regroupement.

```sql
SELECT colonne1, COUNT(*)
FROM nom_table
GROUP BY colonne1
HAVING COUNT(*) > valeur;
```

### Exemple :
Affichons uniquement les genres avec plus de 1 livre.

```sql
SELECT genre, COUNT(*)
FROM Livres
GROUP BY genre
HAVING COUNT(*) > 1;
```

### Résultat :
Aucun résultat ici, car chaque genre n'a qu'un seul livre.

---

Cela constitue une bonne base pour une fiche mémo SQL détaillée. Nous pouvons continuer à l'enrichir avec des concepts plus avancés tels que **les sous-requêtes**, **les index**, **les transactions** ou encore **les vues** selon vos besoins.
