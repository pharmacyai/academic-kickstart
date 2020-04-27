---
# Documentation: https://sourcethemes.com/academic/docs/managing-content/

title: "Computational methods for detecting drug-drug interactions (DDIs)"
subtitle: ""
summary: ""
authors: []
tags: []
categories: []
date: 2020-04-27T22:10:49+02:00
lastmod: 2020-04-27T22:10:49+02:00
featured: false
draft: false

# Featured image
# To use, add an image named `featured.jpg/png` to your page's folder.
# Focal points: Smart, Center, TopLeft, Top, TopRight, Left, Right, BottomLeft, Bottom, BottomRight.
image:
  caption: ""
  focal_point: ""
  preview_only: false

# Projects (optional).
#   Associate this post with one or more of your projects.
#   Simply enter your project's folder or file name without extension.
#   E.g. `projects = ["internal-project"]` references `content/project/deep-learning/index.md`.
#   Otherwise, set `projects = []`.
# projects: []
---

A drug interaction is a reaction between drug and other drugs, food, or supplements. These interactions may make drug less effective, cause unexpected side effects, or increase the action of a drug. On one side, combinations of drug therapies are showing promising results for several diseases, however on the other side drug interaction side effects can lead to adverse drug events (ADEs) or adverse drug reactions (ADRs). 

Drug-drug interactions (DDI) refers to the efficacy change of drug in case of co-administration with another drug. It is estimated that over 30% of all adverse drug reactions are due to drug interactions, [1, 2]. Due to complexity of this problem, it unfeasible to identify all potential DDIs using experimentational methods. We are also learning about a lot of interactions from clinical trials and post-marketing drug surveillance (PMS). 

Therefore, there is a pressure need, and huge opportunity, for advancements in computational methods for detecting DDIs. There are two main categories from which we can approach:
* **classification methods**: using various drug information detect potential DDI, drug pairs, during the drug development process or find new DDIs from existing set of drugs
* **knowledge-based methods**: from corpus of medical content (scientific literature, blogs, etc.) extract information about drug interaction

In this post, we will give quick high-level overview of classification method, and data sources from which we will start exploring the solution space. Before we dig in into a bit more technical conversations, here is a great TED talk by Russ Altman (an American professor of bioengineering, genetics, medicine, and biomedical data science and past chairman of the bioengineering department at Stanford University) about what happens when you mix medications.

{{< youtube avuhY7D71sQ >}}

## Classification methods approch

[Classification represents](https://en.wikipedia.org/wiki/Statistical_classification) a problem of identifying to which category, or set of categories, new object belongs. In case of DDI, we are looking at drug pairs and we want to predict if given pair of drugs will lead to adverse drug reaction. This is called binary classification problem because we have two classes. In case if we are interested in type of reaction, problem moves to be multiclass classification area but for purpose of this post let us consider only two classes: DDI and non-DDI.

Usually, classification problem requires us to train a model, a function, which maps object to category (or probability for each of the possible categories). For us to find the optimal model parameters, we need dataset containing example objects for both classes. For example, creating a model for classification of profile images to male and female requires us to have a dataset with example profile images of both male and female.

{{< figure src="classification.png" title="DDI classification approch: mapping two drugs into probability that they are DDI pair" numbered="true" lightbox="true" >}}
 
DDI is an example of a [one-class classification problem](https://en.wikipedia.org/wiki/One-class_classification). We have set of samples from one class, DDI in our case which we will call positive class, and unlabeled samples. For DDI, unlabeled samples represent all the drug pairs for which we do not have information that they might have interaction in case of coadministration. This means that we do not have information about instances from unlabeled dataset, they could be positive or negative. In order to apply classification approach, we need instances of both positive and negative classes.  There are various techniques how can reliable negative samples be extracted from unlabeled dataset.

{{< figure src="one_class_classification.jpg" title="One-class classification DDI overview" numbered="true" lightbox="true" >}}

Let us look at the [DrugBank](https://www.drugbank.ca) [3], one of the online available drug data sources. Database contains various information about drugs, like chemical structure or drug targets, and for each drug it contains list of drugs with which there is an interaction. This is example of dataset we can use for training the DDI classification models.

Drugs are complex objects, and in order to apply computational methods over them we need to represent them in vector form, as list of numbers. As noted, drug has multiple different properties, such as chemical structure, targets, indications, pathways, substituents etc. Each of the component tells us different drug property, but they are also correlated to an extent.

{{< figure src="drug_vector_example.PNG" title="Example of, arguably a bit odd, vector representation in $R^5$, vector of 5 numbers" numbered="true" lightbox="true" >}}

For example, that a look at information about [Acetaminophen (paracetamol)](https://www.drugbank.ca/drugs/DB00316) from DrugBank, where you can find most of the drug properties mentioned here. After having a drug representation in a vector form, we can create a drug pair representation in different ways, like concatenating these drug vectors or applying pairwise sum.

In the next post we will be looking at different ways to represent drug in vector form.

## References

[1] Zheng Y, Peng H, Zhang X, Zhao Z, Gao X, Li J, DDI-PULearn: a positive-unlabeled learning method for large-scale prediction of drug-drug interactions, BMC Bioinformatics. 2019;20(19):661 
https://bmcbioinformatics.biomedcentral.com/articles/10.1186/s12859-019-3214-6

[2] Bucşa C, Farcas A, Cazacu I, Daniel-Corneliu L, Achimas A, Mogosan C, Bojita M, How many potential drug-drug interactions cause adverse drug reactions in hospitalized patients?, European journal of internal medicine. 2012; 24. 10.1016
https://www.ejinme.com/article/S0953-6205(12)00249-X/pdf

[3] Wishart DS, Feunang YD, Guo AC, Lo EJ, Marcu A, Grant JR, Sajed T, Johnson D, Li C, Sayeeda Z, Assempour N, Iynkkaran I, Liu Y, Maciejewski A, Gale N, Wilson A, Chin L, Cummings R, Le D, Pon A, Knox C, Wilson M. DrugBank 5.0: a major update to the DrugBank database for 2018. Nucleic Acids Res. 2017 Nov 8. doi: 10.1093/nar/gkx1037. 
