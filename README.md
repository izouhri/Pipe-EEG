# Pipe-EEG
Mesure de la charge mentale grâce à un bandeau EEG (MUSE) lors de la réalisation d'un exercice de reflexion

# Analyse de variance à deux facteur

Etude de l'influence de X1 et X2 sur Y avec :
  X1: La musique stimulante(Qualitative),
  X2: La musique sédative (Qualitative),
  Y : La charge mentale lors de la réalisation d'une tâche de réflexion ( Quantitative)
 
## Etape 1 : Lecture du jeu de données

importer le jeu de données

Utiliser le jeu de données :
Utiliser le jeu de données :
```c
attach(data)
```
```c
Y = quantitative
X1 = qualitative
X2 = qualitative
```
## Etape 2 : Description des données 
Visulisation des données (boxplots)
```c
boxplot(Y~X1+X2)
```
## Etape 3 : vérifcation des conditions d'application
##### Condition 1 : indépendance des données
Le jeu de données doit avoir été fait de tel que toutes les observations sont indépendantes les unes des autres

##### Condition 2 : normalité des distributions
Création de sous-groupes de Y pour les toutes les combinaisons de modalités :
```c
mod1a2a = Y[X1=="Modalite1a" & X2=="Modalite2a"]
mod1a2b = Y[X1=="Modalite1a" & X2=="Modalite2b"]
mod1b2a = Y[X1=="Modalite1b" & X2=="Modalite2a"]
mod1b2b = Y[X1=="Modalite1b" & X2=="Modalite2b"]
```

Hypothèses de normalité :  
**H0 :** *les données suivent une loi normale*  
**H1 :** *les données ne suivent pas une loi normale*  

Vérification de la normalité de la distribution de Y dans chaque modalité de X :
```c
shapiro.test(mod1a2a)
shapiro.test(mod1a2b)
shapiro.test(mod1b2a)
shapiro.test(mod1b2b)
```
Conclusions au seuil 5% :  
p-value < 0,05 pour chaque sous-groupe → rejet de *H0* → les données ne suivent pas une loi normale  
p-value > 0,05 pour chaque sous-groupe → non rejet de *H0* → les données suivent une loi normale → **condition OK**

##### Condition 3 : homoscédasticité ou égalité des variances
Hypothèses d'homoscédasticité :  
**H0 :** *pas de différence entre les variances des différents sous-groupes*  
**H1 :** *différence entre les variances des différents sous-groupes*  

Test de Bartlett :
```c
bartlett.test(Y,paste(X1,X2))
```
Conclusion au seuil 5% :  
p-value < 0,05 → rejet de *H0* → au moins une différence entre les variances  
p-value > 0,05 → non rejet de *H0* → homoscédasticité → **condition OK**


### Choix du test
Si toutes les conditions sont vérifiées → test paramétrique  
Si au moins une des conditions n'est pas vérifiée → test non paramétrique


## Étape 4 : réalisation du test
### Test paramétrique
Hypothèses :  
Première partie  
**H0 :** *pas d'effet du facteur X1 sur Y*  
**H1 :** *effet du facteur X1 sur Y*  

Deuxième partie  
**H0 :** *pas d'effet du facteur X2 sur Y*  
**H1 :** *effet du facteur X2 sur Y*  

Troisième partie  
**H0 :** *pas d'effet de l'interaction des facteurs X1 et X2 sur Y*  
**H1 :** *effet de l'interaction des facteurs X1 et X2 sur Y*  

Calcul de l'ANOVA :
```c
anova = aov(Y~X1+X2+X1*X2)
```

Affichage des résultats :
```c
summary(anova)
```
Conclusions au seuil 5% :  
Première partie  
p-value < 0,05 → rejet de *H0* → effet de X1 sur Y  
p-value > 0,05 → non rejet de *H0* → pas d'effet de X1 sur Y  

Deuxième partie  
p-value < 0,05 → rejet de *H0* → effet de X2 sur Y  
p-value > 0,05 → non rejet de *H0* → pas d'effet de X2 sur Y  

Troisième partie  
p-value < 0,05 → rejet de *H0* → effet de l'interaction de X1 et X2 sur Y  
p-value > 0,05 → non rejet de *H0* → pas d'effet de l'interaction de X1 et X2 sur Y  


## Étape 5 : conclusion finale
Si on ne peut pas rejeter *H0* → analyse terminée  
Si on rejette *H0* → comparaison des modalités 2 à 2  

Comparaison des modalités 2 à 2 :
```c
pairwise.t.test(Y,paste(X1,X2),p.adjust.method = "bonferroni")
```
Remarque : *l'ajustement de la p-value est nécessaire quand on a plus de 2 modalités*

Conclusion au seuil 5% :  
p-value < 0,05 → rejet de *H0* → différence significative de Y dans les 2 modalités  
p-value > 0,05 → non rejet de *H0* → pas de différence significative de Y dans les 2 modalités  

  
  
