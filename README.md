<!-- <h1 align="center">
  <img src="images/dadmatech.jpeg"  width="150"  />
   Dadmatools
</h1> -->


<h2 align="center">Dadmatools: A Python NLP Library for Persian</h2>

<div align="center">
  <a href="https://pypi.org/project/dadmatools/"><img src="https://img.shields.io/pypi/v/dadmatools.svg"></a>
  <!-- <a href="https://travis-ci.com/dadmatech/dadmatools"><img src="https://travis-ci.com/dadmatech/dadmatools.svg?branch=master"></a> -->
  <!-- <a href="https://coveralls.io/github/alexandrainst/danlp?branch=master"><img src="https://coveralls.io/repos/github/alexandrainst/danlp/badge.svg?branch=master"></a> -->
  <a href=""><img src="https://img.shields.io/badge/license-Apache%202-blue.svg"></a>
  <!-- <a href=''><img src='https://readthedocs.org/projects/danlp-alexandra/badge/?version=latest' alt='Documentation Status' /></a> -->
</div>
<div align="center">
  <h5>
      Named Entity Recognition
    <span> | </span>
      Part of Speech Tagging
    <span> | </span>
      Dependency Parsing
  </h5>
  <h5>
      Constituency Parsing
    <span> | </span>
      Chunking
  </h5>
  <h5>
      Tokenizer
    <span> | </span>
      Lemmatizer
  </h5>
  <h5>
    <!-- <a href="https://github.com/alexandrainst/danlp/tree/master/examples/tutorials">
      Tutorials
    </a> -->
  </h5>
</div>


# **DadmaTools**
Dadmatools is a repository for Natural Language Processing resources for the Persian Language. 
The aim is to make it easier and more applicable to practitioners in the industry to use 
Persian NLP and hence this project is licensed to allow commercial use. 
The project features code examples on how to use the models in popular 
NLP frameworks such as spaCy and Transformers as well as Deep Learning frameworks 
such as PyTorch. 
for more details about how to use this tool read the instruction below. 


## Installation

To get started using DadmaTools in your python project simply install the pip package. Note that installing the default pip package 
will not install all NLP libraries because we want you to have the freedom to limit the dependency on what you use. Instead we provide you with an installation option if you want to install all the required dependencies. 

### Install with pip

To get started using DadmaTools simply install the project with pip:

```bash
pip install dadmatools 
```

Note that the default installation of DadmaTools does install other NLP libraries such as SpaCy and supar.

You can check the `requirements.txt` file to see what version the packages has been tested with.

### Install from github
Alternatively you can install the latest version from github using:
```bash
pip install git+https://github.com/Dadmatech/dadmatools.git
```

## NLP Models

Natural Language Processing is an active area of research and it consists of many different tasks. 
The Dadmatools repository provides an overview of Persian models for some of the most common NLP tasks (and is continuously evolving). 

Here is the list of NLP tasks we currently cover in the repository.
-  Named Entity Recognition
-  Part of speech tagging
-  Dependency parsing
-  Constituency parsing
-  Chunking
-  Lemmatizing
-  Tokenizing
-  Normalizing

### Use Case

These NLP tasks are defined as pipelines. Therefore, a pipeline list must be created and passed through the model. This will allow the user to choose the only task needed without loading others. 
Each task has its abbreviation as following:
-  ```ner```: Part of speech tagging
-  ```pos```: Part of speech tagging
-  ```dep```: Dependency parsing
-  ```cons```: Constituency parsing
-  ```chunk```: Chunking
-  ```lem```: Lemmatizing
-  ```tok```: Tokenizing

Note that the normalizer can be used outside of the pipeline as there are several configs (the default confing is in the pipeline with the name of def-norm).
Note that if no pipeline is passed to the model the tokenizer will be load as default.

### Normalizer
cleaning text and unify characters.

Note: None means no action! 
```python
from dadmatools.models.normalizer import Normalizer

normalizer = Normalizer(
    full_cleaning=False,
    unify_chars=True,
    refine_punc_spacing=True,
    remove_extra_space=True,
    remove_puncs=False,
    remove_html=False,
    remove_stop_word=False,
    replace_email_with="<EMAIL>",
    replace_number_with=None,
    replace_url_with="",
    replace_mobile_number_with=None,
    replace_emoji_with=None,
    replace_home_number_with=None
)

text = """
<p>
دادماتولز اولین نسخش سال ۱۴۰۰ منتشر شده. 
امیدواریم که این تولز بتونه کار با متن رو براتون شیرین‌تر و راحت‌تر کنه
لطفا با ایمیل dadmatools@dadmatech.ir با ما در ارتباط باشید
آدرس گیت‌هاب هم که خب معرف حضور مبارک هست:
 https://github.com/Dadmatech/DadmaTools
</p>
"""
normalized_text = normalizer.normalize(text)
#<p> دادماتولز اولین نسخش سال 1400 منتشر شده. امیدواریم که این تولز بتونه کار با متن رو براتون شیرین‌تر و راحت‌تر کنه لطفا با ایمیل <EMAIL> با ما در ارتباط باشید آدرس گیت‌هاب هم که خب معرف حضور مبارک هست: </p>

#full cleaning
normalizer = Normalizer(full_cleaning=True)
normalized_text = normalizer.normalize(text)
#دادماتولز نسخش سال منتشر تولز بتونه کار متن براتون شیرین‌تر راحت‌تر کنه ایمیل ارتباط آدرس گیت‌هاب معرف حضور مبارک

```

### Pipeline - Tokenizer, Lemmatizer, POS Tagger, Dependancy Parser, Constituency Parser
```python
import dadmatools.pipeline.language as language

# here lemmatizer and pos tagger will be loaded
# as tokenizer is the default tool, it will be loaded as well even without calling
pips = 'tok,lem,pos,dep,cons' 
nlp = language.Pipeline('lem')

# you can see the pipeline with this code
print(nlp.analyze_pipes(pretty=True))

# doc is an SpaCy object
doc = nlp('از قصهٔ کودکیشان که می‌گفت، گاهی حرص می‌خورد!')
```
[```doc```](https://spacy.io/api/doc) object has different extensions. First, there is ```sentences``` in ```doc``` which is the list of the list of [```Token```](https://spacy.io/api/token). Each [```Token```](https://spacy.io/api/token) also has its own extentions. Note that we defined our own extention as well in DadmaTools. If any pipeline related to the that specific extentions is not called, that extention will have no value.

To better see the results you can use this code:

```python

dictionary = language.to_json(pips, doc)
print(dictionary)
```

```python
[[{'id': 1, 'text': 'از', 'lemma': 'از', 'pos': 'ADP', 'rel': 'case', 'root': 2}, {'id': 2, 'text': 'قصهٔ', 'lemma': 'قصه', 'pos': 'NOUN', 'rel': 'obl', 'root': 10}, {'id': 3, 'text': 'کودکی', 'lemma': 'کودکی', 'pos': 'NOUN', 'rel': 'nmod', 'root': 2}, {'id': 4, 'text': 'شان', 'lemma': 'آنها', 'pos': 'PRON', 'rel': 'nmod', 'root': 3}, {'id': 5, 'text': 'که', 'lemma': 'که', 'pos': 'SCONJ', 'rel': 'mark', 'root': 6}, {'id': 6, 'text': 'می\u200cگفت', 'lemma': 'گفت#گو', 'pos': 'VERB', 'rel': 'acl', 'root': 2}, {'id': 7, 'text': '،', 'lemma': '،', 'pos': 'PUNCT', 'rel': 'punct', 'root': 6}, {'id': 8, 'text': 'گاهی', 'lemma': 'گاه', 'pos': 'NOUN', 'rel': 'obl', 'root': 10}, {'id': 9, 'text': 'حرص', 'lemma': 'حرص', 'pos': 'NOUN', 'rel': 'compound:lvc', 'root': 10}, {'id': 10, 'text': 'می\u200cخورد', 'lemma': 'خورد#خور', 'pos': 'VERB', 'rel': 'root', 'root': 0}, {'id': 11, 'text': '!', 'lemma': '!', 'pos': 'PUNCT', 'rel': 'punct', 'root': 10}]]

```

```python
sentences = doc._.sentences
for sentence in sentences:
    text = sentence.text
    for token in sentences:
        token_text = token.text
        lemma = token.lemma_ ## this has value only if lem is called
        pos_tag = token.pos_ ## this has value only if pos is called
        dep = token.dep_ ## this has value only if dep is called
        dep_arc = token._.dep_arc ## this has value only if dep is called
sent_constituency = doc._.constituency ## this has value only if cons is called
sent_chunks = doc._.chunks ## this has value only if cons is called
ners = doc._.ners ## this has value only if ner is called
```

Note that ```_.constituency``` and ```_.chunks``` are the object of [SuPar](https://parser.yzhang.site/en/latest/) class.

### Loading Persian NLP Datasets
We provide an easy-to-use way to load some popular persian nlp datasets

Here is the list of supported datasets.

   |    Dataset             | Task 
|      ----------------                | ----------- 
   |    PersianNER           |   NER   | 
   |       Arman             |   NER
   |       Peyma             | NER
  |       FarTail           | Entailment
 |        FaSpell           | Spell checking
  |      PersianNews        | Text classification
  |       PerUDT            | Universal Dependency
  |      PnSummary          | Text Summarization
  |    SnappfoodSentiment   | Sentiment Classification
  |           TEP           | Text Translation(eng-fa)
| Wikipedia               | Corpus

get dataset info:
```python

from dadmatools.datasets import get_all_datasets_info

get_all_datasets_info().keys()
#dict_keys(['Persian-NEWS', 'fa-wiki', 'faspell', 'PnSummary', 'TEP', 'PerUDT', 'FarsTail', 'Peyma', 'snappfoodSentiment', 'Persian-NER', 'Arman', 'PerSent'])

#specify task
get_all_datasets_info(tasks=['NER'])
```
the output will be:

```json
{"Arman": {"description": "",
           "filenames": ["train_fold1.txt",
                         "train_fold2.txt",
                         "train_fold3.txt",
                         "test_fold1.txt",
                         "test_fold2.txt",
                         "test_fold3.txt"],
           "name": "Arman",
           "size": {"test": 7680, "train": 15361},
           "splits": ["train", "test"],
           "task": "NER",
           "version": "1.0.0"},
 "Persian-NER": {"description": "source: "
                                "https://github.com/Text-Mining/Persian-NER",
                 "filenames": ["Persian-NER-part1.txt",
                               "Persian-NER-part2.txt",
                               "Persian-NER-part3.txt",
                               "Persian-NER-part4.txt",
                               "Persian-NER-part5.txt"],
                 "name": "Persian-NER",
                 "size": 1000000,
                 "splits": [],
                 "task": "NER",
                 "version": "1.0.0"},
 "Peyma": {"description": "source: "
                          "http://nsurl.org/2019-2/tasks/task-7-named-entity-recognition-ner-for-farsi/",
           "filenames": ["peyma/600K", "peyma/300K"],
           "name": "Peyma",
           "size": 1000000,
           "splits": [],
           "task": "NER",
           "version": "1.0.0"}}
```
all datasets are iterator and can be used like below:
```python
from dadmatools.datasets import FarsTail
from dadmatools.datasets import SnappfoodSentiment
from dadmatools.datasets import get_all_datasets_info
from dadmatools.datasets import Peyma
from dadmatools.datasets import PerUDT

farstail = FarsTail()

#len of dataset
len(farstail['train'])

#each dataset is a iterator
next(farstail['train'])

#loop over dataset
snpfood_sa = SnappfoodSentiment()
train_dataset_info = snpfood_sa['train'].info
for i, item in enumerate(snpfood_sa['test']):
    print(item['comment'], item['label'])

perudt = PerUDT()
for token_list in perudt['dev']:
    print(token_list[0]['lemma'])

peyma = Peyma()
print(next(peyma)[0]['tag'])
```

### Loading Persian Word Embeddings
download, load and using some pre-trained persian word embeddings

dadmatools supports all glove,fasttext and word2vec formats
```python
from pprint import pprint
from dadmatools.embeddings import get_embedding, get_all_embeddings_info, get_embedding_info

pprint(get_all_embeddings_info())
# word_embeddings = get_embedding('fasttext-commoncrawl-vec')
embedding_info = get_embedding_info('glove-wiki')
word_embeddings = get_embedding('glove-wiki')
vocab = word_embeddings.get_vocab()
print(word_embeddings.word_vector('سلام'))
print(word_embeddings.doesnt_match("دیروز به دانشگاه رفتم"))
print(word_embeddings.similarity('کتب', 'کتاب'))
print(word_embeddings.embedding_text('امروز هوای خوبی بود'))
```

## How to use (Colab)
You can see the codes and the output here.

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/drive/1re_7tr-U6XOmzptkb-s-_lK2H9Kb0Y6l?usp=sharing)

## Cite
Will be added in future.
<!-- 
If you want to cite this project, please use the following BibTeX entry: 

```
@inproceedings{
}
``` -->

<!-- Read the paper here.  -->

