# Challenges and Insights in Multiword Expressions Processing for NLP Tasks in Portuguese

This repository contains the code and datasets used in the paper "Challenges and Insights in Multiword Expressions Processing for NLP Tasks in Portuguese". This work aims to study two Multiword Expressions (MWE), namely “morrer de X” (to die of X) and “chorar de X” (to cry of X) in Portuguese, which are commonly used metaphorically for intensification, but can also be interpreted literally. The code is a jupyter notebook containing the code to:

- load the data and preprocess it: As mentioned in the article, some labels were inconsistent.
- load the models and infer the labels from the data: We used mmBERT small multilingual sentiment and
  DistilBERT base multilingual cased sentiments for inference.
- compare the results between the models.

## Results

In both expressions, the two models had difficulties to correctly identify the true labels in European and Brazilian Portuguese, getting similar accuracies around 0.57. However we can get some insights by looking at the precision
and recall of both models. Firstly, is important to have well defined both metrics. The precision is measured as

$$\texttt{precision} = \frac{\texttt{\\#True Positives}}{\texttt{\\#True Positives } + \texttt{ \\#False Positives}}$$

Where, given a label $X$, the true positives are the cases where the model correctly predicted $X$ and false positives the cases where the model incorrectly predicted label $X$. It can range from $0$ to $1$, where $1$ indicates that everytime the model tries to predict $X$ it does correctly and $0$ indicates that it predicts $X$ wrongly everytime it tries. The recall measure the other way around

$$\texttt{recall} = \frac{\texttt{\\#True Positives}}{\texttt{\\#True Positives } + \texttt{ \\#False Negatives}}$$

Where, given $X$, the false negatives are the cases where the true label _were_ $X$ and the model didn't predicted it. Again, it ranges from $0$ to $1$ with $1$ indicating that it predicted correctly all the instances of $X$ and $0$ indicanting it predicted no $X$ correctly.

### DistilBERT for pt-BR

<div style="margin-left: auto; margin-right: auto; align=center;">

| Labels | Precision | Recall |
|----------|:----------:|:----------:|
| $\texttt{negative}$ | $0.21$ | $0.75$ |
| $\texttt{neutral}$ | $1.00$ | $0.20$ |
| $\texttt{positive}$ | $0.92$ | $0.63$ |
  
</div>

As we can see, in all cases DistilBERT predicted $\texttt{neutral}$ as the sentence true label it hits correctly ($\texttt{precision}$ is $1$). However it represents only $20\\%$ of the total ($\texttt{recall}$ is $0.20$), i.e., it never tries to predict $\texttt{neutral}$. Also we have a lot of $\texttt{negatives}$ wrongly predicted ($\texttt{precision}$ is $0.21$), but with a good accuracy for the true ones ($\texttt{recall}$ is $0.75$). It indicates that the model predicts a lot of $\texttt{negative}$, probably due to literal interpretation.

### mmBERT for pt-BR

<div style="margin-left: auto; margin-right: auto; align=center;">

| Labels | Precision | Recall |
|----------|:----------:|:----------:|
| $\texttt{negative}$ | $0.50$ | $0.75$ |
| $\texttt{neutral}$ | $0.50$ | $0.40$ |
| $\texttt{positive}$ | $1.00$ | $0.95$ |
  
</div>

mmBERT shows an excelent performance in the $\texttt{positive}$ labels, with both $\texttt{precision}$ and $\texttt{recall}$ high, but it has a worst result for $\texttt{neutral}$ label. It doesn't try $\texttt{neutral}$ so much ($\texttt{recall}$ is low), but when it tries it does a lot of wrong predictions ($\texttt{precision}$ is low). However, in general, it peforms better in Brazilian portuguese than DistilBERT.

## Conclusions

Both models perform similarly in European portuguese, but the difference in Brazilian portuguese is huge, although we had much more European portuguese data. However, in general terms, both models struggled in the presence of MWEs with highly inconsistent labeling. The experiment presents limitations, as it evaluates only pre-trained, openly available models, without engaging in model training. Future work will involve training models with different architectures on the same dataset in order to provide a more robust and reliable comparison.

## Acknowledgements

This study is funded by CAPES/DS (Coordenação de Aperfeiçoamento de Pessoal de Nível Superior/Coordination for the Improvement of Higher Education Personnel). Marcia dos Santos Machado Vieira thanks CNPq (National Council for Scientific and Technological Development, projects 409043/2021-4 and 312423/2022-5) and FAPERJ (Carlos Chagas Filho Foundation for Research Support in the State of Rio de Janeiro, project E-26/201.209/2022 (273339), SEI 260.003/003571/2022) for the research support.

## Authors

- Mariana Gonçalves da Costa - Universidade Federal do Rio de Janeiro, Rio de Janeiro, Brazil (https://github.com/MarianaGCosta)
- Matheus do Ó Santos Tiburcio - Universidade Federal do Rio de Janeiro, Rio de Janeiro, Brazil (https://github.com/Mth0)
- Marcia dos Santos Machado Vieira - Universidade Federal do Rio de Janeiro, Rio de Janeiro, Brazil
- Sérgio Manuel Serra da Cruz - Universidade Federal do Rio de Janeiro, Rio de Janeiro, Brazil
