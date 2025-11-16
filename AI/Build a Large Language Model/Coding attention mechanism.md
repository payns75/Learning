MOC: [[BUILD A LARGE LANGUAGE MODEL - FROM SCRATCH]]
Source: Build a Large Language Model - From Scratch
Auteur: Sebastian Raschka
Date: 2025-11-11

---
## Coding attention mechanism

### Simplified self-attention

On imagine partir avec le input suivant:

$$ 
\mathtt{imputs} =
\begin{bmatrix}
0.43 & 0.15 & 0.89 \\
0.55 & 0.87 & 0.66 \\
0.57 & 0.85 & 0.64 \\
0.22 & 0.58 & 0.33 \\
0.77 & 0.25 & 0.10 \\
0.05 & 0.80 & 0.55
\end{bmatrix}
\in\mathbb{R}^{6\times 3}
$$

Chaque ligne correspond à un mot de la phrase

> Your journey starts with one step

Qui correspond au code **Python** suivant:

```python
inputs = torch.tensor(
  [[0.43, 0.15, 0.89], # Your     (x^1)
   [0.55, 0.87, 0.66], # journey  (x^2)
   [0.57, 0.85, 0.64], # starts   (x^3)
   [0.22, 0.58, 0.33], # with     (x^4)
   [0.77, 0.25, 0.10], # one      (x^5)
   [0.05, 0.80, 0.55]] # step     (x^6)
)
```

#### Compute attention scores

##### Exemple de calcul du score d'attention sur un mot

Calculer le score d'attention d'un mot, c'est l'idée de prendre ce mot et de faire la multiplication sur l'ensemble des mots.

Par exemple, si dans notre input, on souhaite calculer le score d'attention du mot **journey**, on va avoir à faire:

$$
\underbrace{
\begin{bmatrix}
\color{blue}0.55 & \color{blue}0.87 & \color{blue}0.66
\end{bmatrix}
}_{\text{query } q = \text{inputs}^{(2)}}
\;
\cdot
\;
\underbrace{
\begin{bmatrix}
\color{green}0.43 & \color{green}0.15 & \color{green}0.89 \\
\color{blue}0.55 & \color{blue}0.87 & \color{blue}0.66 \\
0.57 & 0.85 & 0.64 \\
0.22 & 0.58 & 0.33 \\
0.77 & 0.25 & 0.10 \\
0.05 & 0.80 & 0.55
\end{bmatrix}
}_{\text{inputs}}
=
\begin{bmatrix}
\color{red}0.9544 \\
1.4950 \\
1.4754 \\
0.8434 \\
0.7070 \\
1.0865
\end{bmatrix}
$$

Donc si par exemple, on veut calculer la première ligne de la matrice, de l'exemple précédent, on peut faire:

$$
\text{score}(\text{Your}, \text{Journey})
=
\begin{bmatrix}
\color{green}0.43 \\ \color{green}0.15 \\ \color{green}0.89
\end{bmatrix}
\cdot
\begin{bmatrix}
\color{blue}0.55 \\ \color{blue}0.87 \\ \color{blue}0.66
\end{bmatrix}
= \; \color{green}0.43\color{blue}(0.55) \\ \;+\; \color{green}0.15\color{blue}(0.87) \\ \;+\; \color{green}0.89\color{blue}(0.66)
$$

$$
= \; 0.2365 \;+\; 0.1305 \;+\; 0.5874 
= \color{red}0.9544
$$

##### Calcul du score d'attention sur l'ensemble des mots

Pour faire le calcul du score d'attention sur l'ensemble des mots, il serait possible de faire en Python, une boucle dans une boucle et de ressortir un tableau de 6X6.

Il est possible de faire la même chose en utilisant le transposé de la matrice. Comme le montre l'exemple ci-dessous, on va multiplier la matrice d'origine avec sont transposé pour avoir le résultat final attendu.


$$

\text{attn\_scores} = \text{inputs} \cdot \text{inputs}^\top
=
\begin{bmatrix}
0.43 & 0.15 & 0.89 \\
\color{blue}{0.55} & \color{blue}{0.87} & \color{blue}{0.66} \\
0.57 & 0.85 & 0.64 \\
0.22 & 0.58 & 0.33 \\
0.77 & 0.25 & 0.10 \\
0.05 & 0.80 & 0.55
\end{bmatrix}
\cdot
\begin{bmatrix}
\color{green}0.43 & \color{blue}{0.55} & 0.57 & 0.22 & 0.77 & 0.05 \\
\color{green}0.15 & \color{blue}{0.87} & 0.85 & 0.58 & 0.25 & 0.80 \\
\color{green}0.89 & \color{blue}{0.66} & 0.64 & 0.33 & 0.10 & 0.55
\end{bmatrix}
=
\begin{bmatrix}
\color{red}0.9544 & 1.4950 & 1.4754 & 0.8434 & 0.7070 & 1.0865 \\
1.4950 & 1.9940 & 1.9348 & 1.1394 & 0.8858 & 1.4886 \\
1.4754 & 1.9348 & 1.9790 & 1.1394 & 0.8626 & 1.4648 \\
0.8434 & 1.1394 & 1.1394 & 0.4529 & 0.2924 & 0.7378 \\
0.7070 & 0.8858 & 0.8626 & 0.2924 & 0.6354 & 0.4855 \\
1.0865 & 1.4886 & 1.4648 & 0.7378 & 0.4855 & 1.1506
\end{bmatrix}

$$


En python, le code finale sera simplement celui-ci:

```python
# Calcul du score d'attention d'une matrice avec sont transposé
# @ En python correspond à la multiplication matricielle
attn_scores = inputs @ inputs.T
```


####  Computing attention weights

Calculer les poids d’attention consiste à normaliser les scores d’attention de manière à obtenir une distribution qui somme à 1. 

##### Exemple de calcul du poids d'attention sur un mot simplifié

Dans cet exemple simplifié, la normalisation se fait en divisant chaque score par la somme des scores.

En python on écrirait le code suivant pour calculer le poids d'attention du mot **journey**.

```python
attn_scores_2 = attn_scores[:, 1] # On récupère la première colonne de la matrice pour démontrer l'exemple ci-après.
attn_weights_2_tmp = attn_scores_2 / attn_scores_2.sum()
```


Ce qui correspond à:

Avec S, la somme des éléments de la matrice.
$$
S
=
\color{red}{0.9544} \\
+
1.4950
+
1.4754
+
0.8434
+
0.7070
+
1.0865
$$

$$
\text{attn\_weights}_2
=
\frac{1}{S}
\begin{bmatrix}
\color{red}{0.9544} \\
1.4950 \\
1.4754 \\
0.8434 \\
0.7070 \\
1.0865
\end{bmatrix}
=
\begin{bmatrix}
0.1454 \\
0.2279 \\
0.2249 \\
0.1285 \\
0.1077 \\
0.1656
\end{bmatrix}
$$

À noter que la somme du résultat est de 1

$$
\sum_{i=1}^{6} \text{attn\_weights}_2(i)
=
0.1454
+ 0.2279
+ 0.2249
+ 0.1285
+ 0.1077
+ 0.1656
= 1
$$
##### Exemple de calcul d'attention en utilisant la fonction *Softmax*

Dans la vraie vie, on utilise généralement la fonction _softmax_ pour transformer nos scores d’attention en probabilités, car elle amplifie les différences entre les scores, garantit que tous les poids sont positifs et que leur somme vaut exactement 1, ce qui permet au modèle de se concentrer davantage sur les mots les plus pertinents.


$$
\text{attn\_weights}_2
=
\text{softmax}(\text{attn\_scores}_2)
=
\begin{bmatrix}
0.1485 \\
0.2571 \\
0.2514 \\
0.1335 \\
0.1142 \\
0.0954
\end{bmatrix}
\qquad
\text{avec}
\qquad
\sum_i \alpha_i = 1
$$


En python, cela se traduit par:

```python
# Note: dim=0 signifie que l'on applique softmax sur les colonnes. dim=1 serait sur les lignes.
attn_weights_2 = torch.softmax(attn_scores_2, dim=0)
print("Attention weights:", attn_weights_2)
print("Sum:", attn_weights_2.sum()) # devra donner 1
```

##### Calcul du poids d'attention sur l'ensemble des mots

Pour calculer le poids d'attention sur l'ensemble des mots, on va partir de la matrice de score d'attention. On va appliquer la fonction *softmax* à l'ensemble de la matrice d'entrée:

```python
# Note: dim=-1, on applique softmax sur la derniere  dimension du tenseur donc sur les colonnes.
attn_weights = torch.softmax(attn_scores, dim=-1)
print(attn_weights)
```


$$
\text{attn\_weights}
=
\text{softmax}(
\text{attn\_scores}
)
=
\text{softmax}(
\begin{bmatrix}
\color{red}0.9544 & 1.4950 & 1.4754 & 0.8434 & 0.7070 & 1.0865 \\
1.4950 & 1.9940 & 1.9348 & 1.1394 & 0.8858 & 1.4886 \\
1.4754 & 1.9348 & 1.9790 & 1.1394 & 0.8626 & 1.4648 \\
0.8434 & 1.1394 & 1.1394 & 0.4529 & 0.2924 & 0.7378 \\
0.7070 & 0.8858 & 0.8626 & 0.2924 & 0.6354 & 0.4855 \\
1.0865 & 1.4886 & 1.4648 & 0.7378 & 0.4855 & 1.1506
\end{bmatrix}
)
$$

$$
\text{attn\_weights}
=
\begin{bmatrix}
0.2098& 0.2006& 0.1981& 0.1242& 0.1220& 0.1452 \\
0.1385& 0.2379& 0.2333& 0.1240& 0.1082& 0.1581 \\
0.1390& 0.2369& 0.2326& 0.1242& 0.1108& 0.1565 \\
0.1435& 0.2074& 0.2046& 0.1462& 0.1263& 0.1720 \\
0.1526& 0.1958& 0.1975& 0.1367& 0.1879& 0.1295 \\
0.1385& 0.2184& 0.2128& 0.1420& 0.0988& 0.1896
\end{bmatrix}
$$
#### Compute context vectors

Finalement, il ne nous reste plus qu'à calculer le vecteur de contexte en multipliant la matrice de poids d'attention avec l'input.

```python
all_context_vecs = attn_weights @ inputs
print(all_context_vecs)
```


$$

\text{all\_context\_vecs} = \text{attn\_weights} \cdot \text{inputs}
=
\begin{bmatrix}
0.2098& 0.2006& 0.1981& 0.1242& 0.1220& 0.1452 \\
0.1385& 0.2379& 0.2333& 0.1240& 0.1082& 0.1581 \\
0.1390& 0.2369& 0.2326& 0.1242& 0.1108& 0.1565 \\
0.1435& 0.2074& 0.2046& 0.1462& 0.1263& 0.1720 \\
0.1526& 0.1958& 0.1975& 0.1367& 0.1879& 0.1295 \\
0.1385& 0.2184& 0.2128& 0.1420& 0.0988& 0.1896
\end{bmatrix}
\cdot
\begin{bmatrix}
0.43 & 0.15 & 0.89 \\
0.55 & 0.87 & 0.66 \\
0.57 & 0.85 & 0.64 \\
0.22 & 0.58 & 0.33 \\
0.77 & 0.25 & 0.10 \\
0.05 & 0.80 & 0.55
\end{bmatrix}
$$

$$
\text{all\_context\_vecs} =
\begin{bmatrix}
0.4421& 0.5931& 0.5790 \\
0.4419& 0.6515& 0.5683 \\
0.4431& 0.6496& 0.5671 \\
0.4304& 0.6298& 0.5510 \\
0.4671& 0.5910& 0.5266 \\
0.4177& 0.6503& 0.5645
\end{bmatrix}
$$
#### Code final

Finalement le code finale pour ces transformation est le suivant:

```python
# Tenseur d'entré:
inputs = torch.tensor(
  [[0.43, 0.15, 0.89], # Your     (x^1)
   [0.55, 0.87, 0.66], # journey  (x^2)
   [0.57, 0.85, 0.64], # starts   (x^3)
   [0.22, 0.58, 0.33], # with     (x^4)
   [0.77, 0.25, 0.10], # one      (x^5)
   [0.05, 0.80, 0.55]] # step     (x^6)
)

# Calcul du score d'attention
attn_scores = inputs @ inputs.T

# Calcul du poids d'attention
attn_weights = torch.softmax(attn_scores, dim=-1)

# Vecteur de contexte
all_context_vecs = attn_weights @ inputs
```

