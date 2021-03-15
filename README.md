# Train SpaCy v3 models with CoNLL-2003 data

## Obtain CoNLL-2003 DATA
Follow the instructions on the [share-task website](https://www.clips.uantwerpen.be/conll2003/ner/) to obtain train, valid and test data. <BR>
*(Note: the original German data is no longer available on LDC.)*<br>
*All data files are local since the data are not available publicly.*<br>
  `cd ner`<br>
  `bin/make.eng.2016`<br>
  `bin/make.deu.2016`<br>


## Experiments start here

0. install SpaCy v3 in a new Python enviroment (not advisible to update the old version, also, one might consider turning off VPN)

`python -m spacy download en_core_web_trf` (the option for accuracy)<br> 
`python -m spacy download de_dep_news_trf` (the option for accuracy)

### English model

1. Convert CoNLL-03 to SpaCy format<br>

`python -m spacy convert -c ner -b en_core_web_trf -n 10 assets/conll2003/train.txt corpus`<br>
CLI opt example (converted training data): 
```
ℹ Auto-detected token-per-line NER format
ℹ Grouping every 10 sentences into a document.
✔ Generated output file (1499 documents): corpus/train.spacy
```
```
ℹ Auto-detected token-per-line NER format
ℹ Grouping every 10 sentences into a document.

✔ Generated output file (369 documents): corpus/test.spacy
```
```
ℹ Auto-detected token-per-line NER format
ℹ Grouping every 10 sentences into a document.
✔ Generated output file (347 documents): corpus/valid.spacy
```
<br>
repeat for test and valid data.<br>

3. Prepare the configuration file
5. Debug data
`python -m spacy debug data configs/cnn_glove_small.cfg`<br>
```
============================ Data file validation ============================
✔ Corpus is loadable
✔ Pipeline can be initialized with data

=============================== Training stats ===============================
Language: en
Training pipeline: ner
14987 training docs
3466 evaluation docs
⚠ 130 training examples also in evaluation data

============================== Vocab & Vectors ==============================
ℹ 204567 total word(s) in the data (23624 unique)
ℹ 2196017 vectors (2196016 unique keys, 300 dimensions)
⚠ 4856 words in training data without vectors (0.02%)

========================== Named Entity Recognition ==========================
ℹ 0 new label(s), 4 existing label(s)
0 missing value(s) (tokens with '-' label)
⚠ 15 entity span(s) with punctuation
✔ Good amount of examples for all labels
✔ Examples without occurrences available for all labels
✔ No entities consisting of or starting/ending with whitespace
Entity spans consisting of or starting/ending with punctuation can not be
trained with a noise level > 0.

================================== Summary ==================================
✔ 5 checks passed
⚠ 3 warnings
```
7. Data overview
8. Model training
`python -m spacy train configs/cnn_glove_small.cfg -o training/cnn_glove_small --gpu-id -1`
```
ℹ Using CPU

=========================== Initializing pipeline ===========================
[2021-03-14 19:22:53,075] [INFO] Set up nlp object from config
[2021-03-14 19:22:53,083] [INFO] Pipeline: ['ner']
[2021-03-14 19:22:53,086] [INFO] Created vocabulary
[2021-03-14 19:23:04,758] [INFO] Added vectors: corpus/en_vectors
[2021-03-14 19:23:04,762] [INFO] Finished initializing nlp object
[2021-03-14 19:23:11,593] [INFO] Initialized pipeline components: ['ner']
✔ Initialized pipeline

============================= Training pipeline =============================
ℹ Pipeline: ['ner']
ℹ Initial learn rate: 0.001
E    #       LOSS NER  ENTS_F  ENTS_P  ENTS_R  SCORE 
---  ------  --------  ------  ------  ------  ------
  0       0     50.06    0.13    0.18    0.10    0.00
  0     200   2725.22   76.50   77.73   75.31    0.77                                                                                                                                                               
  0     400   1536.00   83.48   83.18   83.79    0.83                                                                                                                                                               
  0     600   1349.86   87.21   87.27   87.14    0.87                                                                                                                                                               
  0     800   1300.49   88.84   88.60   89.08    0.89                                                                                                                                                               
  0    1000   1372.97   89.93   89.87   89.99    0.90                                                                                                                                                               
  1    1200   1347.53   90.88   90.91   90.86    0.91                                                                                                                                                               
  1    1400   1191.25   91.52   91.55   91.50    0.92                                                                                                                                                               
  1    1600   1322.71   91.61   91.68   91.55    0.92                                                                                                                                                               
  2    1800   1409.99   92.09   92.00   92.17    0.92                                                                                                                                                               
  3    2000   1497.47   92.14   92.13   92.16    0.92                                                                                                                                                               
  3    2200   1260.66   92.50   92.50   92.51    0.93                                                                                                                                                               
  4    2400   1120.31   92.74   92.69   92.80    0.93                                                                                                                                                               
  5    2600    929.63   92.71   92.68   92.73    0.93                                                                                                                                                               
  6    2800    895.65   92.65   92.65   92.66    0.93                                                                                                                                                               
  7    3000    814.22   92.75   92.78   92.73    0.93                                                                                                                                                               
  8    3200    771.40   92.88   92.95   92.81    0.93                                                                                                                                                               
  9    3400    646.83   92.76   92.87   92.66    0.93                                                                                                                                                               
 10    3600    613.22   93.02   93.13   92.90    0.93                                                                                                                                                               
 11    3800    584.29   92.96   93.05   92.86    0.93                                                                                                                                                               
 12    4000    494.00   92.76   92.82   92.70    0.93                                                                                                                                                               
 13    4200    571.51   92.68   92.72   92.63    0.93                                                                                                                                                               
 14    4400    523.94   92.72   92.71   92.73    0.93                                                                                                                                                               
 15    4600    352.71   92.68   92.74   92.63    0.93                                                                                                                                                               
 16    4800    369.45   92.81   92.77   92.85    0.93                                                                                                                                                               
 17    5000    372.69   92.75   92.70   92.80    0.93                                                                                                                                                               
 18    5200    462.73   92.83   92.82   92.85    0.93                                                                                                                                                               
 19    5400    356.32   92.75   92.78   92.73    0.93                                                                                                                                                               
 20    5600    360.94   92.76   92.75   92.76    0.93                                                                                                                                                               
 21    5800    321.63   92.59   92.71   92.48    0.93                                                                                                                                                               
 22    6000    355.62   92.57   92.69   92.44    0.93                                                                                                                                                               
 23    6200    290.30   92.54   92.65   92.44    0.93                                                                                                                                                               
 24    6400    400.99   92.48   92.55   92.41    0.92                                                                                                                                                               
 25    6600    323.93   92.48   92.55   92.41    0.92                                                                                                                                                               
 25    6800    310.79   92.51   92.61   92.41    0.93                                                                                                                                                               
 26    7000    267.43   92.44   92.53   92.34    0.92                                                                                                                                                               
 27    7200    261.17   92.50   92.58   92.43    0.93                                                                                                                                                               
 28    7400    288.59   92.65   92.75   92.54    0.93                                                                                                                                                               
 29    7600    278.45   92.64   92.76   92.53    0.93                                                                                                                                                               
 30    7800    216.87   92.54   92.65   92.44    0.93                                                                                                                                                               
 31    8000    261.97   92.59   92.71   92.48    0.93                                                                                                                                                               
 32    8200    260.65   92.52   92.59   92.46    0.93                                                                                                                                                               
 33    8400    256.20   92.64   92.67   92.61    0.93                                                                                                                                                               
 34    8600    211.15   92.60   92.58   92.61    0.93                                                                                                                                                               
 35    8800    223.18   92.68   92.66   92.70    0.93                                                                                                                                                               
 36    9000    165.30   92.77   92.76   92.78    0.93                                                                                                                                                               
 37    9200    200.80   92.71   92.73   92.70    0.93                                                                                                                                                               
 38    9400    176.98   92.64   92.64   92.63    0.93                                                                                                                                                               
 39    9600    202.34   92.72   92.68   92.75    0.93                                                                                                                                                               
 40    9800    178.60   92.64   92.62   92.66    0.93                                                                                                                                                               
 41   10000    173.16   92.69   92.68   92.70    0.93                                                                                                                                                               
 42   10200    194.73   92.75   92.76   92.73    0.93                                                                                                                                                               
 43   10400    286.18   92.64   92.67   92.61    0.93                                                                                                                                                               
 44   10600    147.20   92.58   92.61   92.54    0.93                                                                                                                                                               
 45   10800    213.68   92.64   92.72   92.56    0.93                                                                                                                                                               
 46   11000    211.87   92.63   92.70   92.56    0.93                                                                                                                                                               
 47   11200    181.26   92.59   92.68   92.49    0.93                                                                                                                                                               
 48   11400    169.10   92.64   92.72   92.56    0.93                                                                                                                                                               
 49   11600    243.23   92.61   92.67   92.54    0.93                                                                                                                                                               
Epoch 50:   0%|                                                                                                                                                                             | 0/200 [00:00<?, ?it/s]✔ Saved pipeline to output directory
training/cnn_glove_small/model-last
```
9. Evaluation
`python -m spacy evaluate ./training/cnn_glove_small/model-best ./corpus/test.spacy --output ./metrics/cnn_glove_small.json --gpu-id -1 --gold-preproc`<br>
```
ℹ Using CPU

================================== Results ==================================

TOK     100.00
NER P   88.92 
NER R   88.69 
NER F   88.80 
SPEED   13594 


=============================== NER (per type) ===============================

           P       R       F
LOC    91.23   92.93   92.07
PER    93.76   92.95   93.35
MISC   78.57   78.35   78.46
ORG    86.20   84.65   85.42

✔ Saved results to metrics/cnn_glove_small.json
```

### German model

1. Convert CoNLL-03 to SpaCy format
3. Prepare the configuration file
5. Debug data
6. Data overview
7. Model trainiing
8. Evaluation

*The experimenting process is also [here]() on Google Colab.*<br>
*[paper]()*

### project
`python -m spacy project assets project_dir`<br>
`python -m spacy project run subcommand project_dir --force --dry`

