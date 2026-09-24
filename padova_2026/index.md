---
title: "High frequency </br>doesn’t mean </br>high importance"
subtitle: "Determining the most distinctive features in topic modeling"
author: 
    name: Maciej Eder
    affiliation: Polish Academy of Sciences | University of Tartu
date: 2026-09-25
date-format: "DD/MM/YYYY"
format:
  revealjs:
    theme: pp.scss
    #logo: UT_logo.svg
    logo: csg.png
    css: medium_logo.css
    self_contained: true
    #incremental: true
---



# introduction

## motivation

- Brazlian literature diverse
    - several influences (Spanish, Italian, German, ...)
    - remarkable diversity between the South and the North
    - regional differences
- group of novels described as "urban" 
- group of novels described as "regional"
- Romanticism vs. Modernism being a strong factor, toof
- plus, a general language drift from 19th to 20th century is there


> (work in progress by Pagano, Coneglian & Eder)


## regionalism vs. urbanism

- urbanist novels
    - about life in the city 
    - social relations 
    - stories that unfold in big cities
    - set in Rio de Janeiro, São Paulo, or Porto Alegre 
- regionalist novels
    - life in the countryside
    - people's struggle with climate events 
    - hardships, violence and harsh life
    - set in the countryside e.g. of the state Minas Gerais 



## research plan

- a corpus of 21 Brazilian novels
    - covering both Regionalism and Urbanism
    - written in 19th and 20th centruries
    - belong to Romanticism or Modernism
- train a topic model
    - explore different parameters
- perform classification
- identify the most discriminating features (=topics)



##

* 🍉 José de Alencar: *Lucíola* (1862), *Senhora* (1875)
* 🍉 Manuel Antônio de Almeida: *Memórias de um sargento de milícias* (1853)
* 🍉 Joaquim Manuel Macedo: *A moreninha* (1844), *A luneta mágica* (1869)
* 🍉 Érico Veríssimo: *O tempo e o vento* (1949-1961), *Incidente em Antares* (1971), *Olhai os lírios do campo* (1938)
* 🍉 Jorge Amado: *Capitães da areia* (1937)
* 🥑 Rodolfo Teófilo: *A fome* (1890)
* 🥑 José do Patrocínio: *Os retirantes* (1879)
* 🥑 José de Alencar: *O sertanejo* (1875)
* 🥑 José Américo de Almeida: *A bagaceira* (1928)
* 🥑 Rachel de Queiroz: *O quinze* (1930)
* 🥑 João Miguel: (1932)
* 🥑 José Lins do Rego: *Menino do engenho* (1932), *Usina* (1936)
* 🥑 Graciliano Ramos: *Caetés* (1933), *Vidas secas* (1938)
* 🥑 Jorge Amado: *Cacau* (1933), *Suor* (1934)



##

* 👒 José de Alencar: *Lucíola* (1862), *Senhora* (1875)
* 👒 Manuel Antônio de Almeida: *Memórias de um sargento de milícias* (1853)
* 👒 Joaquim Manuel Macedo: *A moreninha* (1844), *A luneta mágica* (1869)
* 🧢 Érico Veríssimo: *O tempo e o vento* (1949-1961), *Incidente em Antares* (1971), *Olhai os lírios do campo* (1938)
* 🧢 Jorge Amado: *Capitães da areia* (1937)
* 👒 Rodolfo Teófilo: *A fome* (1890)
* 👒 José do Patrocínio: *Os retirantes* (1879)
* 👒 José de Alencar: *O sertanejo* (1875)
* 🧢 José Américo de Almeida: *A bagaceira* (1928)
* 🧢 Rachel de Queiroz: *O quinze* (1930)
* 🧢 João Miguel: (1932)
* 🧢 José Lins do Rego: *Menino do engenho* (1932), *Usina* (1936)
* 🧢 Graciliano Ramos: *Caetés* (1933), *Vidas secas* (1938)
* 🧢 Jorge Amado: *Cacau* (1933), *Suor* (1934)



## setup of choice

- 21 novels chunked into 1,000-word segments
- NLP pre-processed with `udpipe`
    - all lemmatized
    - NER removed
    - allowed: nouns, verbs, adjective, adverbs
    - other grammatical categories removed
- Latent Dirichlet Allocation (LDA)
- 80 topics
- stopwords removed
- low-freq filter etc.



# results (_?_)


## topic 23, topic 44

![](img/topics_23_44.png)




## topic 47, topic 75

![](img/topics_47_75.png)




## distinctive features

- the biggest variance (_cf_ Delta)
- support vectors (_cf_ SVM)
- features that survived penalization (_cf_ NSC)
- loadings of a projected space (_cf_ PCA)
- Shapley values 👈


## SHAP values

- derived from game theory
- aim: fairly distribute the total gains or costs among a group of players who have collaborated
- used to understand feature importance in machine learning
    - can be combined with random forest or XGboost
    - tries to optimize the model in a number of iterations
    - looks at interplay between different (groups of) features



## Regionalism vs. Urbanism -- SHAP values

![](img/U-vs-R_results_for_80_topics.png)



## 19th cent. vs. 20th cent. -- SHAP values

![](img/20-vs-19_results_for_80_topics.png)


## the results problematic

- while the separation is there...
- ... the same features (=topics) deemed distinctive for both groups
    - Reg. vs. Urb.: topics 52, 53, 23, 4, 47, ...
    - 19th vs. 20th: topics 4, 52, 43, 32, 53, ...
- the problem of confound factors


## PCA projection of 80 topics

![](img/PCA.png)



# distributional semantics


## vector models encode semantics

![](img/wordvectors.jpg)



## what's in the original paper

> Somewhat surprisingly, it was found that similarity of word representations goes beyond simple syntactic regularities. Using a word offset technique where simple algebraic operations are performed on the word vectors, it was shown for example that vector(”King”) - vector(”Man”) + vector(”Woman”) results in a vector that is closest to the vector representation of the word Queen.

(Mikolov et al. 2013)


## ‘table’ + ‘breakfast’

![](img/vec_01.png)


## ‘table’ + ‘breakfast’

![](img/vec_02.png)


## ‘table’ + ‘breakfast’

![](img/vec_03.png)


## ‘table’ + ‘breakfast’

![](img/vec_04.png)



# subtracting vectors



## ‘breakfast’ – ‘table’

![](img/vec_01.png)



## ‘breakfast’ – ‘table’

![](img/vec_06.png)


## ‘breakfast’ – ‘table’

![](img/vec_07.png)


## ‘breakfast’ – ‘table’

![](img/vec_08.png)


## ‘breakfast’ – ‘table’

![](img/vec_09.png)



# adding & subtracting vectors


## ‘breakfast’ – ‘table’ + ‘tea’

![](img/vec_10.png)



## ‘breakfast’ – ‘table’ + ‘tea’

![](img/vec_11.png)



## ‘breakfast’ – ‘table’ + ‘tea’

![](img/vec_12.png)



## ‘breakfast’ – ‘table’ + ‘tea’

![](img/vec_13.png)


# ‘woman’ – ‘man’ + ‘king’ = ???

## topics = a semantic representation

- topic is a distribution of words
- document is a distribution of topics
    - a row of probabilities: topic 1, topic 2, ..., topic _n_
    - in fact, it is a vector!
    - consequently, document is a point in _n_-dimensional space
- (btw, there exists `topic2vec`: for a good reason)



# ‘20th cent.’ – ‘19th cent.’ + ???


## cutting off the temporal signal

- add all the vectors `19_R_*` and `19_U_*` together
    - resulting vector: `all_19_vec`
- add all the vectors `20_R_*` and `20_U_*` together
    - resulting vector: `all_20_vec`
- get the difference (i.e. vector) between 20th cent. and 19th cent
    - `all_20_vec - all_19_vec = chrono_diff_vec`
- add the diff vector to all the 19th-cent. samples
    - add `chrono_diff_vec` to `19_R_*` and `19_U_*`


# does it work?


## PCA projection of 80 topics again

![](img/PCA_corected.png)


## Regionalism vs. Urbanism again

![](img/U-vs-R_vectors_corrected.png)



## 19th cent. vs. 20th cent. again

![](img/20-vs-19_results_vectors_corrected.png)



## topic 53: homem, coisa, vida, ...

![](img/t_53.png)



## topic 52: olhar, olho, cabeça, ...

![](img/t_52.png)




## topic 25: mão, sentir, olho ...

![](img/t_25.png)




## conclusions

- final results not spectacular, but:
- vector shifting potentially powerful
- formal evaluation needed




