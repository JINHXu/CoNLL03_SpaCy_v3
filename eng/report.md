# Train an English SpaCy v3 model with CoNLL-2003 datasets

## data
The datasets were obtained through the official channel: request `rcv1.tar.xz` from NIST and annotate by <br>
`cd ner`<br>
`bin/make.eng.2016`<br>
```
extracting corpus files for eng.train and eng.testa...
removing all xml codes...
tokenizing...
extracting corpus files for eng.testb...
removing all xml codes...
tokenizing...
```

## model
I tried training the model on Google Colab and on my local machine. Training this model on Colab turned out to be an inadvisable option, since it takes significantly longer time. 

* [Colab notebook]()

* [local training process]()


---

### model training on local machine report

* convert data to SpaCy format<br>
`python -m spacy convert -c ner -b en_core_web_sm -n 10 ./ner/train.txt ./corpus`<br>
```
ℹ Auto-detected token-per-line NER format
⚠ Document delimiters found, automatic document segmentation with `-n`
disabled.
✔ Generated output file (946 documents): corpus/train.spacy
```
`python -m spacy convert -c ner -b en_core_web_sm -n 10 ./ner/dev.txt ./corpus`<br>
```
ℹ Auto-detected token-per-line NER format
⚠ Document delimiters found, automatic document segmentation with `-n`
disabled.
✔ Generated output file (216 documents): corpus/dev.spacy
```
`python -m spacy convert -c ner -b en_core_web_sm -n 10 ./ner/test.txt ./corpus`<br>
```
ℹ Auto-detected token-per-line NER format
ℹ Grouping every 10 sentences into a document.
✔ Generated output file (369 documents): corpus/test.spacy
```
* debug data<br>
`python -m spacy debug data configs/cnn_glove_small.cfg`<br>
```

============================ Data file validation ============================
✔ Corpus is loadable
✔ Pipeline can be initialized with data

=============================== Training stats ===============================
Language: en
Training pipeline: ner
14041 training docs
3250 evaluation docs
⚠ 129 training examples also in evaluation data

============================== Vocab & Vectors ==============================
ℹ 203621 total word(s) in the data (23623 unique)
ℹ 2196017 vectors (2196016 unique keys, 300 dimensions)
⚠ 3910 words in training data without vectors (0.02%)

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

* pre-training step:

    * prepare assets: 
        * `vectors.zip`<br>
        source: http://nlp.stanford.edu/data/glove.840B.300d.zip
        * `orth_variants.json`<br>
        a file containing orth variants for data augmentation

    * configuration file:

    * install dependencies <br>
        `python -m pip install -r requirements.txt`

    * convert, truncate and prune the vectors <br>
    `python -m spacy init vectors en assets/vectors.zip corpus/en_vectors -n en_glove840b_vectors_md`

* train 
`python -m spacy train configs/cnn_glove_small.cfg -o training/cnn_glove_small --gpu-id -1`<br>
```
ℹ Using CPU

=========================== Initializing pipeline ===========================
[2021-03-17 19:06:43,710] [INFO] Set up nlp object from config
[2021-03-17 19:06:43,719] [INFO] Pipeline: ['ner']
[2021-03-17 19:06:43,723] [INFO] Created vocabulary
[2021-03-17 19:07:07,928] [INFO] Added vectors: corpus/en_vectors
[2021-03-17 19:07:07,930] [INFO] Finished initializing nlp object
[2021-03-17 19:07:25,262] [INFO] Initialized pipeline components: ['ner']
✔ Initialized pipeline

============================= Training pipeline =============================
ℹ Pipeline: ['ner']
ℹ Initial learn rate: 0.001
E    #       LOSS NER  ENTS_F  ENTS_P  ENTS_R  SCORE 
---  ------  --------  ------  ------  ------  ------
  0       0     42.50    0.00    0.00    0.00    0.00
  0     200   2470.77   75.49   75.77   75.21    0.75                                                                                                                            
  0     400   1415.19   84.68   84.63   84.72    0.85                                                                                                                            
  0     600   1287.38   87.51   87.62   87.39    0.88                                                                                                                            
  0     800   1356.82   89.18   89.34   89.03    0.89                                                                                                                            
  0    1000   1374.96   90.28   90.36   90.21    0.90                                                                                                                            
  1    1200   1336.49   91.23   91.15   91.32    0.91                                                                                                                            
  1    1400   1204.59   91.63   91.56   91.70    0.92                                                                                                                            
  1    1600   1523.81   91.98   92.02   91.94    0.92                                                                                                                            
  2    1800   1360.99   92.13   92.25   92.01    0.92                                                                                                                            
  3    2000   1280.38   92.37   92.45   92.29    0.92                                                                                                                            
  3    2200   1201.12   92.52   92.59   92.46    0.93                                                                                                                            
  4    2400   1065.96   92.56   92.58   92.54    0.93                                                                                                                            
  5    2600   1025.46   92.43   92.43   92.43    0.92                                                                                                                            
  6    2800    812.98   92.48   92.51   92.46    0.92                                                                                                                            
  7    3000    797.84   92.55   92.50   92.60    0.93                                                                                                                            
  8    3200    628.66   92.47   92.48   92.46    0.92                                                                                                                            
  9    3400    570.50   92.43   92.46   92.41    0.92                                                                                                                            
 10    3600    620.79   92.34   92.36   92.33    0.92                                                                                                                            
 11    3800    504.93   92.25   92.24   92.26    0.92                                                                                                                            
 12    4000    487.87   92.51   92.47   92.56    0.93                                                                                                                            
 13    4200    497.39   92.65   92.57   92.73    0.93                                                                                                                            
 14    4400    396.64   92.71   92.58   92.83    0.93                                                                                                                            
 15    4600    392.11   92.59   92.48   92.70    0.93                                                                                                                            
 16    4800    459.67   92.57   92.49   92.65    0.93                                                                                                                            
 17    5000    456.51   92.46   92.37   92.54    0.92                                                                                                                            
 18    5200    398.12   92.55   92.47   92.63    0.93                                                                                                                            
 19    5400    365.19   92.53   92.46   92.60    0.93                                                                                                                            
 20    5600    308.26   92.72   92.66   92.78    0.93                                                                                                                            
 21    5800    323.22   92.82   92.76   92.88    0.93                                                                                                                            
 22    6000    315.26   92.80   92.71   92.88    0.93                                                                                                                            
 23    6200    351.79   92.80   92.73   92.88    0.93                                                                                                                            
 24    6400    337.94   92.60   92.58   92.61    0.93                                                                                                                            
 25    6600    342.39   92.50   92.45   92.54    0.92                                                                                                                            
 26    6800    334.87   92.54   92.53   92.54    0.93                                                                                                                            
 27    7000    345.88   92.49   92.49   92.48    0.92                                                                                                                            
 28    7200    285.29   92.43   92.43   92.43    0.92                                                                                                                            
 29    7400    194.50   92.51   92.45   92.56    0.93                                                                                                                            
 29    7600    199.67   92.40   92.38   92.43    0.92                                                                                                                            
 30    7800    266.21   92.39   92.41   92.38    0.92                                                                                                                            
 31    8000    222.40   92.65   92.66   92.65    0.93                                                                                                                            
 32    8200    217.91   92.52   92.53   92.51    0.93                                                                                                                            
 33    8400    226.55   92.52   92.47   92.58    0.93                                                                                                                            
 34    8600    205.71   92.52   92.44   92.60    0.93                                                                                                                            
 35    8800    181.72   92.41   92.34   92.48    0.92                                                                                                                            
 36    9000    235.37   92.40   92.32   92.48    0.92                                                                                                                            
 37    9200    175.94   92.32   92.21   92.43    0.92                                                                                                                            
 38    9400    207.68   92.34   92.27   92.41    0.92                                                                                                                            
 39    9600    157.09   92.37   92.32   92.43    0.92                                                                                                                            
 40    9800    211.78   92.47   92.45   92.49    0.92                                                                                                                            
 41   10000    244.70   92.59   92.54   92.65    0.93                                                                                                                            
 42   10200    288.53   92.56   92.52   92.60    0.93                                                                                                                            
 43   10400    219.29   92.50   92.44   92.56    0.92                                                                                                                            
 44   10600    202.98   92.50   92.48   92.51    0.92                                                                                                                            
 45   10800    247.77   92.44   92.40   92.48    0.92                                                                                                                            
 46   11000    180.41   92.41   92.35   92.46    0.92                                                                                                                            
 47   11200    257.04   92.48   92.49   92.46    0.92                                                                                                                            
 48   11400    202.75   92.39   92.41   92.38    0.92                                                                                                                            
 49   11600    161.73   92.34   92.33   92.36    0.92                                                                                                                            
 50   11800    194.42   92.24   92.21   92.28    0.92                                                                                                                            
 51   12000    185.24   92.09   92.05   92.14    0.92                                                                                                                            
 52   12200    178.36   92.07   92.06   92.09    0.92                                                                                                                            
 53   12400    180.26   92.12   92.12   92.12    0.92                                                                                                                            
 54   12600    238.95   92.30   92.31   92.29    0.92                                                                                                                            
 55   12800    202.22   92.32   92.28   92.36    0.92                                                                                                                            
 56   13000    140.98   92.37   92.35   92.39    0.92                                                                                                                            
 57   13200    191.08   92.29   92.31   92.28    0.92                                                                                                                            
 58   13400    107.82   92.32   92.34   92.31    0.92                                                                                                                            
 59   13600    145.07   92.42   92.41   92.43    0.92                                                                                                                            
 60   13800    123.64   92.37   92.39   92.34    0.92                                                                                                                            
Epoch 61:   0%|                                                                                                                                          | 0/200 [00:00<?, ?it/s]✔ Saved pipeline to output directory
training/cnn_glove_small/model-last
```
* evaluate
`python -m spacy evaluate ./training/cnn_glove_small/model-best ./corpus/test.spacy --output ./metrics/cnn_glove_small.json --gpu-id -1 --gold-preproc`<br>
```
ℹ Using CPU

================================== Results ==================================

TOK     100.00
NER P   89.20 
NER R   89.24 
NER F   89.22 
SPEED   13745 


=============================== NER (per type) ===============================

           P       R       F
LOC    91.16   92.75   91.95
PER    93.45   93.51   93.48
ORG    87.18   85.97   86.57
MISC   79.34   78.77   79.06

✔ Saved results to metrics/cnn_glove_small.json
```

`python -m spacy evaluate ./training/cnn_glove_small/model-best ./corpus/test.spacy -dp ./displacy`<br>
```
ℹ Using CPU

================================== Results ==================================

TOK     -    
NER P   87.49
NER R   86.70
NER F   87.10
SPEED   13993


=============================== NER (per type) ===============================

           P       R       F
LOC    90.24   89.75   89.99
PER    92.10   91.53   91.81
MISC   78.91   78.35   78.63
ORG    83.84   82.48   83.16

✔ Generated 25 parses as HTML
displacy
```

