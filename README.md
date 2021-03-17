# Train SpaCy v3 models with CoNLL-2003 data

## Data

CoNLL-2003 datasets include corpus in two languages: English and German
* The English data was obtained through [the official channel](https://www.clips.uantwerpen.be/conll2003/ner/) (requested from NIST and labeled with the bin file provided by the shared-task orgnizers). It will not be uploaded since the data is not publicly available, thgouh there are plenty of other sources/versions available on GitHub.
* The original German corpus (the Frankfurter Rundschau) is no longer available on LDC, the project uses a publicly available dataset from [here](https://github.com/MaviccPRP/ger_ner_evals/tree/master/corpora/conll2003).

## Models

Two SpaCy v3 models are trained: an English model and a German model (with respective data)
* The English model is trained with the CoNLL-2003 English data, the model was trained on Google Colab (not recommended). 

    _Go to [eng]()_

* The German model is trained with the CoNLL-2003 German data, the model was trained locally, also built into a SpaCy project. (Everything takes significantly longer time on Colab, running on local machine is recommended in this case.)

    _Go to [deu]()_



<sub>The [fe]() folder includes the report of a first experiment of trianing an English model with data from unoffical source, in which the annotations differ from the official data. </sub>