## German NER model report

* Data

The German CoNLL-2003 data are non longer available on LDC. The data used for experiments were found [here](https://github.com/MaviccPRP/ger_ner_evals/tree/master/corpora/conll2003).<br>

German data require converting to utf-8 first for spacy to process.<br>
_tool: [subtitletools](https://subtitletools.com/convert-text-files-to-utf8-online)<br>_

##### convert CoNLL-2003 to spaCy format
`python -m spacy convert -c ner -b de_core_news_sm -n 10 ./data/train.txt ./corpus`<br>
```
ℹ Auto-detected token-per-line NER format
ℹ Grouping every 10 sentences into a document.
✔ Generated output file (1271 documents): corpus/train.spacy
```
`python -m spacy convert -c ner -b de_core_news_sm -n 10 ./data/testa.txt ./corpus`<br>
```
ℹ Auto-detected token-per-line NER format
ℹ Grouping every 10 sentences into a document.
✔ Generated output file (307 documents): corpus/testa.spacy
```
`python -m spacy convert -c ner -b de_core_news_sm -n 10 ./data/testb.txt ./corpus`<br>
```
ℹ Auto-detected token-per-line NER format
ℹ Grouping every 10 sentences into a document.
✔ Generated output file (316 documents): corpus/testb.spacy
```

#### configuration

Download base config from [here](https://spacy.io/usage/training) 

*The following shows how defualt configuration file was created.*

* auto-fill defaults<br>
`python -m spacy init fill-config ./base_config.cfg ./default_config.cfg`

* debug config<br>
`python -m spacy debug config ./configs/default_config.cfg -F -V`<br>
```

============================= Config validation =============================

===================== Config validation for [initialize] =====================

====================== Config validation for [training] ======================
✔ Config is valid

=============================== Variables (0) ===============================

Variable                                   Value                             
-----------------------------------------  ----------------------------------


========================= Registered functions (12) =========================
ℹ [components.ner.model]
Registry   @architectures                
Name       spacy.TransitionBasedParser.v2
Module     spacy.ml.models.parser        
File       /Users/xujinghua/spcv3/lib/python3.7/site-packages/spacy/ml/models/parser.py (line 34)
ℹ [components.ner.model.tok2vec]
Registry   @architectures                
Name       spacy.Tok2VecListener.v1      
Module     spacy.ml.models.tok2vec       
File       /Users/xujinghua/spcv3/lib/python3.7/site-packages/spacy/ml/models/tok2vec.py (line 17)
ℹ [components.tok2vec.model]
Registry   @architectures                
Name       spacy.Tok2Vec.v2              
Module     spacy.ml.models.tok2vec       
File       /Users/xujinghua/spcv3/lib/python3.7/site-packages/spacy/ml/models/tok2vec.py (line 90)
ℹ [components.tok2vec.model.embed]
Registry   @architectures                
Name       spacy.MultiHashEmbed.v1       
Module     spacy.ml.models.tok2vec       
File       /Users/xujinghua/spcv3/lib/python3.7/site-packages/spacy/ml/models/tok2vec.py (line 111)
ℹ [components.tok2vec.model.encode]
Registry   @architectures                
Name       spacy.MaxoutWindowEncoder.v2  
Module     spacy.ml.models.tok2vec       
File       /Users/xujinghua/spcv3/lib/python3.7/site-packages/spacy/ml/models/tok2vec.py (line 258)
ℹ [corpora.dev]
Registry   @readers                      
Name       spacy.Corpus.v1               
Module     spacy.training.corpus         
File       /Users/xujinghua/spcv3/lib/python3.7/site-packages/spacy/training/corpus.py (line 21)
ℹ [corpora.train]
Registry   @readers                      
Name       spacy.Corpus.v1               
Module     spacy.training.corpus         
File       /Users/xujinghua/spcv3/lib/python3.7/site-packages/spacy/training/corpus.py (line 21)
ℹ [nlp.tokenizer]
Registry   @tokenizers                   
Name       spacy.Tokenizer.v1            
Module     spacy.language                
File       /Users/xujinghua/spcv3/lib/python3.7/site-packages/spacy/language.py (line 67)
ℹ [training.batcher]
Registry   @batchers                     
Name       spacy.batch_by_words.v1       
Module     spacy.training.batchers       
File       /Users/xujinghua/spcv3/lib/python3.7/site-packages/spacy/training/batchers.py (line 49)
ℹ [training.batcher.size]
Registry   @schedules                    
Name       compounding.v1                
Module     thinc.schedules               
File       /Users/xujinghua/spcv3/lib/python3.7/site-packages/thinc/schedules.py (line 43)
ℹ [training.logger]
Registry   @loggers                      
Name       spacy.ConsoleLogger.v1        
Module     spacy.training.loggers        
File       /Users/xujinghua/spcv3/lib/python3.7/site-packages/spacy/training/loggers.py (line 27)
ℹ [training.optimizer]
Registry   @optimizers                   
Name       Adam.v1                       
Module     thinc.optimizers              
File       /Users/xujinghua/spcv3/lib/python3.7/site-packages/thinc/optimizers.py (line 58)
```

* debug data
`python -m spacy debug data ./configs/default_config.cfg`

```
============================ Data file validation ============================
✔ Corpus is loadable
✔ Pipeline can be initialized with data

=============================== Training stats ===============================
Language: de
Training pipeline: tok2vec, ner
1271 training docs
307 evaluation docs
✔ No overlap between training and evaluation data
⚠ Low number of examples to train a new pipeline (1271)

============================== Vocab & Vectors ==============================
ℹ 207484 total word(s) in the data (32933 unique)
ℹ No word vectors present in the package

========================== Named Entity Recognition ==========================
ℹ 0 new label(s), 4 existing label(s)
0 missing value(s) (tokens with '-' label)
⚠ 6 entity span(s) with punctuation
✔ Good amount of examples for all labels
✔ Examples without occurrences available for all labels
✔ No entities consisting of or starting/ending with whitespace
Entity spans consisting of or starting/ending with punctuation can not be
trained with a noise level > 0.

================================== Summary ==================================
✔ 6 checks passed
⚠ 2 warnings
```

* train
_transformer model_ 
<br>
_I ran the experiment on a Mac device on which `cuda` cannot be installed. The transformer model was trained on CPU, which is strongly suggested against. Tranformer models shoudl be trained on GPU, it can take a significantly long time to train on CPU, in the case of no access to GPU, non-transformer models are recommended._
<br>

`python -m spacy train ./configs/config_transformer.cfg -o training/config_transformer --gpu-id -1`

```
✔ Created output directory: training/config_transformer
ℹ Using CPU

=========================== Initializing pipeline ===========================
[2021-03-20 18:46:24,521] [INFO] Set up nlp object from config
[2021-03-20 18:46:24,530] [INFO] Pipeline: ['transformer', 'ner']
[2021-03-20 18:46:24,533] [INFO] Created vocabulary
[2021-03-20 18:46:24,533] [INFO] Finished initializing nlp object
[2021-03-20 18:46:31,834] [INFO] Initialized pipeline components: ['transformer', 'ner']
✔ Initialized pipeline

============================= Training pipeline =============================
ℹ Pipeline: ['transformer', 'ner']
ℹ Initial learn rate: 0.0
E    #       LOSS TRANS...  LOSS NER  ENTS_F  ENTS_P  ENTS_R  SCORE 
---  ------  -------------  --------  ------  ------  ------  ------
  0       0        9099.78    551.50    3.08    2.02    6.46    0.03
  1     200      510644.36  58904.13   80.95   85.78   76.64    0.81
  3     400        4739.14   6542.93   84.22   84.72   83.74    0.84
  5     600        1812.76   2932.69   86.19   88.65   83.86    0.86
  7     800         851.33   1662.55   86.44   85.13   87.79    0.86
  8    1000         457.73    904.67   85.52   86.71   84.36    0.86
  10    1200         384.72    735.41   84.70   86.60   82.89    0.85
 12    1400         250.01    536.14   86.63   88.45   84.90    0.87
 14    1600         210.19    476.37   87.22   88.28   86.18    0.87
 16    1800         167.71    410.33   86.75   87.73   85.79    0.87
 17    2000         186.82    402.48   86.25   87.19   85.33    0.86
 19    2200         165.87    363.69   86.02   87.40   84.69    0.86
 21    2400         132.80    298.93   85.28   86.31   84.27    0.85
 23    2600         102.26    269.53   84.66   85.42   83.90    0.85
 25    2800         101.38    262.47   83.93   84.83   83.05    0.84
 26    3000         138.70    304.55   86.18   86.61   85.76    0.86
 28    3200         120.62    262.51   85.68   87.06   84.34    0.86
✔ Saved pipeline to output directory
training/config_transformer/model-last
```

* evaluate


`python -m spacy evaluate ./training/config_transformer/model-best ./corpus/testb.spacy --output ./metrics/config_transformer.json --gold-preproc`

```
ℹ Using CPU

================================== Results ==================================

TOK     100.00
NER P   84.10 
NER R   82.49 
NER F   83.29 
SPEED   117   


=============================== NER (per type) ===============================

           P       R       F
LOC    84.41   83.19   83.80
ORG    79.59   71.15   75.14
PER    92.93   93.56   93.24
MISC   72.71   74.78   73.73

✔ Saved results to metrics/config_transformer.json
```

the alternative 

`python -m spacy evaluate ./training/config_transformer/model-best ./corpus/testb.spacy -dp ./displacy`

```
ℹ Using CPU

================================== Results ==================================

TOK     -    
NER P   85.08
NER R   82.30
NER F   83.67
SPEED   197  


=============================== NER (per type) ===============================

           P       R       F
LOC    86.19   82.61   84.36
ORG    77.38   70.38   73.71
PER    91.78   95.31   93.51
MISC   78.61   72.39   75.37

✔ Generated 25 parses as HTML
displacy
```