# Train spaCy v3.0 models with CoNLL-2003 data

I trained a series of spaCy v3.0 (English and German) NER models with different configurations in order to achieve a best possible F-score during training.

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

* The English models were trained with the CoNLL-2003 English data, the models were trained on local machine on CPU. Part of the experiment was also performed on Google Colab (the best model `cnn_glove_small` was also trained on Colab, though training on Colab is not recommended).

The model with the best performance during training was `cnn_glove_small`, achieved F-score as good as 0.9281870165.<br>

```
TOK     -    
NER P   87.49
NER R   86.70
NER F   87.10
SPEED   13993
```


    _Go to [eng](https://github.com/JINHXu/CoNLL03_SpaCy_v3/tree/main/eng)_

* The German models were trained with the CoNLL-2003 German data, the models were trained locally on CPU, though for transformer models, trianing on CPU is suggested against. Training transformer models on GPU can be 3-4X faster.

The model with the best performance during training was `transformer`, achieved F-score as good as 0.8721599832.<br>
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
