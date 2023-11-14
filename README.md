# Notebook-DUO
Essai dépôt notebook DUO
# Analyse exploratoire des données de métabolomique sur *Cakile maritima*

### Utilisation des données normalisées des intensités de pics des métabolites détectés par GC-MS.

**Autrice : Delphine Bonnin**

---

Les données regroupent 3 conditions différentes parfois combinées :

| Durée (jours) | Niveau foliaire | Concentation saline (mM) |
|:------:|:-----:|:-----:|
|10 | 1 | 0 |
| 20 | 3 | 100 |
|  | 5 | 200 |
|  |  | 400 |



## 1 Génération de composantes en analyses principales :

- **Installation des librairies nécessaires**

```install.packages("FactoMineR")
install.packages("ggcorrplot")
installed.packages("ggfortify")

library(FactoMineR)
library(ggcorrplot)
library(plotly)
library(ggfortify)
```

- **Récupération des données :**

```cols = scan("Resultats bruts nombre transposes.csv", nlines=1, sep=";", what="char")[-1]
Mydata<- read.csv("Resultats bruts nombre transposes.csv",sep=";",dec = ",", fill = TRUE, row.names=1)
colnames(Mydata) = cols
```

- **Modification du dataframe pour l'analyse :**

```Mydata_n <- as.data.frame(sapply(Mydata[,4:48], function(x) as.numeric(x)))
Mydata_n$Day<- Mydata[,2]
Mydata_n$Leaf<- Mydata[,1]
Mydata_n$NaCl<- Mydata[,3]
```

- **Création de sous-dataframes conditionnés**
  
```Mydata_n_10<-Mydata_n[Mydata_n$Day==10,]
Mydata_n_20<-Mydata_n[Mydata_n$Day==20,]
Mydata_n_0<-Mydata_n[Mydata_n$NaCl==0,]
Mydata_n_100<-Mydata_n[Mydata_n$NaCl==100,]
Mydata_n_200<-Mydata_n[Mydata_n$NaCl==200,]
Mydata_n_400<-Mydata_n[Mydata_n$NaCl==400,]
Mydata_n_1<-Mydata_n[Mydata_n$Leaf==1,]
Mydata_n_3<-Mydata_n[Mydata_n$Leaf==3,]
Mydata_n_5<-Mydata_n[Mydata_n$Leaf==5,]
Mydata_n_3_10<-Mydata_n[Mydata_n$Leaf==3 & Mydata_n$Day==10,]
Mydata_n_3_20<-Mydata_n[Mydata_n$Leaf==3 & Mydata_n$Day==20,]
```
![newplot (13)](https://github.com/DelphineB86/Notebook-DUO/assets/133370331/ff3ee952-4fd1-449e-9529-4dc9a7d817c7)

### Réalisation de PCAs par conditions :

- **PCA en sélectionnant les jours (préciser 10 ou 20) :**

```data10=Mydata_n_10[1:45]
pca <- prcomp(data10, scale. = TRUE)
p10 <- autoplot(pca,Mydata_n_10,frame = TRUE, colour="NaCl")+
  geom_text(vjust=-1, label=rownames(Mydata_n_10))+
  stat_ellipse()+
  ggtitle("10 Jours")
ggplotly(p10)
```
![newplot (14)](https://github.com/DelphineB86/Notebook-DUO/assets/133370331/1a74913c-df46-4159-932b-2bdd712efde2)

-**Exemple de code pour une autre condition :**

```data=Mydata_n_400[1:45]
pca <- prcomp(data, scale. = TRUE)

p400 <- autoplot(pca,Mydata_n_400,frame = TRUE,label=TRUE, colour="Day")+
  ggtitle("400mM NaCl")

ggplotly(p400)
```

![newplot (15)](https://github.com/DelphineB86/Notebook-DUO/assets/133370331/69abbac9-dfb0-4443-8ab0-e9b40db865dc)

- **Puis, groupement par deux conditionnements :**

```data=Mydata_n_3_20[1:45]
pca <- prcomp(data, scale. = TRUE)

p3_20 <- autoplot(pca,Mydata_n_3_20,frame = TRUE,label=TRUE, colour="NaCl")+
  ggtitle("Leaf 3 day 20")

ggplotly(p3_20)
```
![newplot (16)](https://github.com/DelphineB86/Notebook-DUO/assets/133370331/442a8ee1-0346-4d7c-af66-72ee3aab9d9a)

# Merci pour cette année !!!


```sessionInfo()
```









