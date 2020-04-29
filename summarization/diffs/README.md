This directory contains the diff files for labels extracted using different methods.
The diff files directly compare 2 methods, where the **first** one is the name of the directory and the second one is the name of the file itself. They have the following repeating format:
* top-20 terms of the topic
* ("===") common labels between the 2 methods
* ("<<<") labels unique to the first method
* (">>>") labels unique to the second method

# Methods

Right now, the methods are:
* `bart_finetuned_terms_labels_v9` - all the other methods are compared to this one
    * Pre-trained model: regular large BART
    * Source: **20** topic terms
    * Target: **variable number** (max. 100) of separate labels from the NETL new (prob-based) candidate generation, where labels that only contain stopwords are removed; no ranking (supervised or unsupervised) is done on these
    * Corpus: all stackexchange corpora (english, biology, economics, law, photo - 0.30 coherence threshold)

* `bart_finetuned_terms_labels_v11`
    * Pre-trained model: regular large BART
    * Source: **20** topic terms
    * Target:
        * **variable number** (max. 100) of separate labels from the NETL new (prob-based) candidate generation, where labels that only contain stopwords are removed; no ranking (supervised or unsupervised) is done on these
        * **variable number** (max. 5) of separate labels from T-order labeling (see https://hal-lirmm.ccsd.cnrs.fr/lirmm-01910614/document), where labels that only contain stopwords are removed
    * Corpus: all stackexchange corpora (english, biology, economics, law, photo - 0.30 coherence threshold)

* `bart_finetuned_terms_labels_sentences_v3`
    * Pre-trained model: regular large BART
    * Source: **20** topic terms
    * Target:
        * **variable number** (max. 100) of separate labels from the NETL new (prob-based) candidate generation, where labels that only contain stopwords are removed; no ranking (supervised or unsupervised) is done on these
        * **5** separate sentences, top-10 extracted using cos10 algorithm (see https://hal-lirmm.ccsd.cnrs.fr/lirmm-01910614/document)
    * Corpus: all stackexchange corpora (english, biology, economics, law, photo - 0.30 coherence threshold)

* `bart_finetuned_labels_ngrams_v2`
    * Pre-trained model: regular large BART
    * Source: **20** topic terms
    * Target:
        * **variable number** (max. 100) of separate labels from the NETL new (prob-based) candidate generation, where labels that only contain stopwords are removed; no ranking (supervised or unsupervised) is done on these
        * **5** separate n-grams, with the following n distributions: 2 - 0.10, 3 - 0.60, 4 - 0.30
    * Corpus: all stackexchange corpora (english, biology, economics, law, photo - 0.30 coherence threshold)

* `bart_finetuned_terms_labels_sentences_ngrams_v2`
    * Pre-trained model: regular large BART
    * Source: **20** topic terms
    * Target:
        * **variable number** (max. 100) of separate labels from the NETL new (prob-based) candidate generation, where labels that only contain stopwords are removed; no ranking (supervised or unsupervised) is done on these
        * **variable number** (max. 5) of separate labels from T-order labeling (see https://hal-lirmm.ccsd.cnrs.fr/lirmm-01910614/document), where labels that only contain stopwords are removed
        * **5** separate sentences, top-10 extracted using cos10 algorithm (see https://hal-lirmm.ccsd.cnrs.fr/lirmm-01910614/document)
        * **5** separate n-grams, with the following n distributions: 2 - 0.10, 3 - 0.60, 4 - 0.30
    * Corpus: all stackexchange corpora (english, biology, economics, law, photo - 0.30 coherence threshold)

* `netl_unsupervised`
    * original (unmodified) NETL method for unsupervised ranking on the candidate generation

* `netl_supervised`
    * original (unmodified) NETL method for supervised ranking on the candidate generation; the ranker is not retrained for this corpus due to lack of annotator scores
