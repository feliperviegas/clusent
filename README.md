# CluSent

## **How to build the CluSent container:**
Word Embedding exploited:
- [FastText](https://fasttext.cc/docs/en/english-vectors.html) (Pre-trained Word Embedding)
- BERT

Build docker container:

```docker build -t cluwords `pwd` ```

Run docker container:

```docker run --rm --name cluwords -v `pwd`:/cluwords -i -t cluwords /bin/bash```

Then, run the code:

```export LANG="C.UTF-8"```

For more information about building and running a docker container, see: https://docs.docker.com/

**Setting Environment:** Before execution the CluSent, you need to create a file `.env` in the root of the project. This file has some settings for the CluSent execution, such as number of threads. It follows an example of this file: 

```bash
# MODE define the structure exploited to store the semantic information.
# There are two structures implemented: "memmap" and "sparse".
MODE="sparse"

# TODO - DESCRIBE
N_THREADS=3

# TODO - DESCRIBE
# CLUSTERING_METHOD="knn_cosine"
CLUSTERING_METHOD="hnsw"

# TODO - DESCRIBE PATHS
MAIN_PATH="/cluwords"
EMBEDDING_RESULTS="fasttext_wiki"
EXPERIMENT_PATH="${MAIN_PATH}/${EMBEDDING_RESULTS}"
PATH_TO_SAVE_EMBEDDINGS="${EXPERIMENT_PATH}/semantic_structure"
PATH_TO_SAVE_DATASETS="${EXPERIMENT_PATH}/datasets"

# File type of the embedding exploited in the method.
# Empty value means text format and True otherwise.
EMBEDDINGS_BIN_TYPE=''

# Define the documents exploited to infer load the information about the embedding
# Empty value means exploit the training set information format, and adding a value (e.g. "true")
# means exploit the dataset information
VOCAB_TYPE=

# "cw", "cw-pos", "cw-tf_al", "cw-pos-tf_al"
CLUWORDS_TYPE="cw"

# Percentage of compression -- NUMBER_OF_FEATURES * SVD_PERC_LANTENTS
SVD_PERC_LANTENTS=0.6
```

## **Exemple of how to execute the CluSent:**

```bash
for dataset in  aisopos_ntua; do \
       python3 main_random_search.py -d ${dataset} -t 2Label/${dataset}/texts_pre.txt \
               -l 2Label/${dataset}/score_pre.txt \
               -s 2Label/${dataset}/representations/split_5.csv \
               -f 5 \
               -e wiki-news-300d-1M.vec \
               -ed 300\
               -le vader_sentiment_lexicon_v2.txt; \
done
```

**Datasets used:**
The datasets are located in the folder `2Label`. Each fold in `2Label` corresponds to a dataset evaluated in the paper. The information of the each dataset are divided in three files:
* text_pre.txt: Raw texts;
* score_pre.txt: Text labals;
* representations/split_5.csv: The split file maps the texts that belongs to train/test following a 5-cross-validation. Each line of the file corresponds to an iteration the cross-validation.

