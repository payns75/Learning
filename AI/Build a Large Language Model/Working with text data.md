MOC: [[BUILD A LARGE LANGUAGE MODEL - FROM SCRATCH]]
Source: Build a Large Language Model - From Scratch
Auteur: Sebastian Raschka
Date: 2025-11-11

---
## Objectif

L'idée ici est de convertir un texte en tokens lisibles par un LLM.  On va simplement "splitter" notre texte pour en extraire les mots et les symbole. Chaque mot sera ensuite rapproché d'un dictionnaire d'Ids. Des tokens spéciaux peuvent être ajoutés dans le cas par exemple de mots qui n'existent pas dans le dictionnaire ou d'identifiants de fin de texte. Les mots inconnu peuvent aussi être découpés en syllabes ou en lettre.



