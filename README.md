# Train spaCy v3.0 models with CoNLL-2003 data

I trained a series of (language-dependent) spaCy v3.0 (English and German) NER models with different configurations in order to achieve the best possible f-score. Among them, the best English NER model (benchmark model) had F-score **89.22**, the best German NER model had F-score **83.29**, both evaluated on the respective testb data. 

[paper](https://github.com/JINHXu/CoNLL03_SpaCy_v3/blob/main/CONLL_2003_SPACY_FINAL.pdf)

<sub>With no access to GPU, all models including the transformer-based model were trained on CPU. However, it is generally suggested against training a transformer-based model on CPU, training on GPU is 3-4X faster.</sub>


## software

```
pip install -U pip setuptools wheel
pip install -U spacy
python -m spacy download en_core_web_sm
python -m spacy download de_core_news_sm
```

## Data

CoNLL-2003 datasets include corpus in two languages: English and German
* The English data was obtained through [the official channel](https://www.clips.uantwerpen.be/conll2003/ner/) (requested from NIST and labeled with the bin file provided by the shared-task orgnizers). It will not be uploaded since the data is not publicly available, thgouh there are plenty of other sources/versions available on GitHub.
* The original German corpus (the Frankfurter Rundschau) is no longer available on LDC, the project uses a publicly available dataset from [here](https://github.com/MaviccPRP/ger_ner_evals/tree/master/corpora/conll2003).

## Models

* The English models were trained with the CoNLL-2003 English data, the models were trained on local machine on CPU. Part of the experiment was also performed on Google Colab (the benchmark model `cnn_glove_small` was trained both on Colab and on my computer, training on Colab is slower so not recommended).<br>

The benchmark model without a doubt showed the highest f-score during training, and the evaluation results:<br>
(the best model I configured in the experiments also presented relatively high f-score during training, around 0.1 smaller than that of the benchmark model )

```
TOK     100.00
NER P   89.20 
NER R   89.24 
NER F   89.22 
SPEED   13745 
```

   _Go to [eng](https://github.com/JINHXu/CoNLL03_SpaCy_v3/tree/main/eng)_

* The German models were trained with the CoNLL-2003 German data, the models were trained locally on CPU, though for transformer models, trianing on CPU is suggested against. Training transformer models on GPU can be 3-4X faster.

The model with the best performance during training was the transformer-based model `transformer`, it achieved F-score as good as 0.8721599832 during training.<br>

Evaluation on testb:
```
TOK     100.00
NER P   84.10 
NER R   82.49 
NER F   83.29 
SPEED   117   
```

   _Go to [deu](https://github.com/JINHXu/CoNLL03_SpaCy_v3/tree/main/deu)_

<sub>The [fe]() folder includes the report of a first experiment of trianing an English model with data from unoffical source, in which the annotations differ from the official data. </sub>
