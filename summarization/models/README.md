This directory contains several finetuned models for abstractive summarization. Each separate model is made up of:
* Model file (`<model_name>.pt`) - the checkpoint that can be loaded using pytorch for inference
* Raw and preprocessed dataset - contains both the raw representations of the dataset (`*.{source|target}`) and the preprocessed representations, using BPE (`*.bpe.{source|target}`)
* Binarized dataset - contains the binarized representation of the dataset, using fairseq preprocess.py
* Summaries - contains the inference output on both the train and test sets
* Any additional relevant files

# Models

## Best model

Currently, the model with the best performance is `bart_finetuned_terms_labels_v7`

Candidates:
* `bart_finetuned_terms_labels_v9`
* `bart_finetuned_terms_labels_sentences_v2`
* `bart_finetuned_terms_labels_sentences_ngrams_v1`
* !!! `bart_finetuned_terms_labels_ngrams_v2`
* !!! `bart_finetuned_terms_labels_v11`
* `bart_finetuned_terms_labels_v12`

## Terms-to-labels models

In the case of terms-labels models, the source is a "pseudo-sentence" formed by concatenating the topic terms. For each topic, this pseudo-sentence is mapped to all the target labels, forcing the fine-tuned model to learn a middle-ground label that best encapsulates all the duplicate information. This, however, means that there is a single label for each topic, as selecting other topic terms as source would not make sense.

1. `bart_finetuned_terms_labels_v1`
    * Pre-trained model: regular large BART
    * Source: **30** topic terms
    * Target: **10** separate labels, first 10 from the new (prob-based) candidate generation; no ranking (supervised or unsupervised) is done on these
    * Corpus: english (0.25 coherence threshold), selected by indices
    * Dataset split: 80% train, 20% test
    
    Fine-tuning hyperparameters:
	* Number of epochs: **4** (last epoch trained)
	* Update frequency: **2**
	* Warmup updates: **3**
	* Learning rate: **3e-04**
	* Max tokens: **2048**
	
	The results of the models (summaries) are obtained using the following snippet:
    ```
    bart.sample(slines, beam=6, lenpen=2.0, max_len_b=60, min_len=3, no_repeat_ngram_size=5, length_penalty=1.0)
	```

2. `bart_finetuned_terms_labels_v2`
    * Pre-trained model: regular large BART
    * Source: **20** topic terms
    * Target: **10** separate labels, top-10 prob-based (unsupervised) ranked candidates from the new (prob-based) candidate generation
    * Corpus: english (0.25 coherence threshold), selected by indices
    * Dataset split: 80% train, 20% test

    Fine-tuning hyperparameters:
	* Number of epochs: **12** (last epoch trained, progress can still be made on both the train and test set)
	* Update frequency: **4**
	* Warmup updates: **5**
	* Learning rate: **3e-05**
	* Max tokens: **2048**
	
    The results of the models (summaries) are obtained using the following snippet:
    ```
    bart.sample(slines, beam=6, lenpen=2.0, max_len_b=60, min_len=3, no_repeat_ngram_size=5, length_penalty=1.0)
    ```

3. `bart_finetuned_terms_labels_v3`
    * Pre-trained model: regular large BART
    * Source: **20** topic terms
    * Target: variable number of **manual** labels
    * Corpus: english (0.25 coherence threshold), selected by indices
    * Dataset split: 80% train, 20% test

    Fine-tuning hyperparameters:	
	* Number of epochs: **17** (out of 30 total epochs)
	* Update frequency: **4**
	* Warmup updates: **10**
	* Learning rate: **3e-05**
	* Max tokens: **2048**
	
    The results of the models (summaries) are obtained using the following snippet:
    ```
    bart.sample(slines, beam=10, lenpen=2.0, max_len_b=60, min_len=1, no_repeat_ngram_size=10, length_penalty=1.0)
    ```
   
4. `bart_finetuned_terms_labels_v4`
    * Pre-trained model: regular large BART
    * Source: **20** topic terms
    * Target: **100** separate labels from the new (prob-based) candidate generation; no ranking (supervised or unsupervised) is done on these
    * Corpus: english (0.25 coherence threshold), selected by indices
    * Dataset split: 80% train, 20% test

    Fine-tuning hyperparameters:
	* Number of epochs: **3** (out of 33 total epochs)
	* Update frequency: **4**
	* Warmup updates: **20**
	* Learning rate: **3e-05**
	* Max tokens: **2048**
	
    The results of the models (summaries) are obtained using the following snippet:
    ```
    bart.sample(slines, beam=10, lenpen=2.0, max_len_b=60, min_len=1, no_repeat_ngram_size=10, length_penalty=1.0)
    ```

5. `bart_finetuned_terms_labels_v5`
    * Pre-trained model: regular large BART
    * Source: **20** topic terms
    * Target: **100** separate labels from the new (prob-based) candidate generation; no ranking (supervised or unsupervised) is done on these
    * Corpus: all stackexchange corpora (english - 0.25 coherence threshold; biology, economics, law, photo - 0.35 coherence threshold), NOT selected by indices
    * Dataset split: 80% train, 20% test
    
    Fine-tuning hyperparameters:
	* Number of epochs: **2** (out of 3 total epochs)
	* Update frequency: **4**
	* Warmup updates: **100**
	* Learning rate: **3e-05**
	* Max tokens: **2048**
	
    The results of the models (summaries) are obtained using the following snippet:
    ```
    bart.sample(slines, beam=10, lenpen=2.0, max_len_b=60, min_len=1, no_repeat_ngram_size=10, length_penalty=1.0)
    ```

6. `bart_finetuned_terms_labels_v6`
    * Pre-trained model: regular large BART
    * Source: **20** topic terms
    * Target: **variable number** (max. 100) of separate labels from the new (prob-based) candidate generation, where labels that are single-word stopwords are removed; no ranking (supervised or unsupervised) is done on these
    * Corpus: all stackexchange corpora (english - 0.25 coherence threshold; biology, economics, law, photo - 0.35 coherence threshold), NOT selected by indices
    * Dataset split: 80% train, 20% test
    
    Fine-tuning hyperparameters:
    * Number of epochs: **2** (out of 4 total epochs)
    * Update frequency: **4**
    * Warmup updates: **100**
    * Learning rate: **3e-05**
    * Max tokens: **2048**
    
    The results of the models (summaries) are obtained using the following snippet:
    ```
    bart.sample(slines, beam=10, lenpen=2.0, max_len_b=60, min_len=1, no_repeat_ngram_size=10, length_penalty=1.0)
    ```

7. `bart_finetuned_terms_labels_v7`
    * Pre-trained model: regular large BART
    * Source: **20** topic terms
    * Target: **variable number** (max. 100) of separate labels from the new (prob-based) candidate generation, where labels that only contain stopwords are removed; no ranking (supervised or unsupervised) is done on these
    * Corpus: all stackexchange corpora (english - 0.25 coherence threshold; biology, economics, law, photo - 0.35 coherence threshold), NOT selected by indices
    * Dataset split: 80% train, 20% test
    
    Fine-tuning hyperparameters:
    * Number of epochs: **2** (out of 3 total epochs)
    * Update frequency: **4**
    * Warmup updates: **100**
    * Learning rate: **3e-05**
    * Max tokens: **2048**
    
    The results of the models (summaries) are obtained using the following snippet:
    ```
    bart.sample(slines, beam=10, lenpen=2.0, max_len_b=60, min_len=1, no_repeat_ngram_size=10, length_penalty=1.0)
    ```
   
8. `bart_finetuned_terms_labels_v8`
    * Pre-trained model: regular large BART
    * Source: **20** topic terms
    * Target: **variable number** (max. 100) of separate labels from T-order labeling (see https://hal-lirmm.ccsd.cnrs.fr/lirmm-01910614/document), where labels that only contain stopwords are removed
    * Corpus: all stackexchange corpora (english, biology, economics, law, photo - 0.30 coherence threshold), NOT selected by indices
    * Dataset split: 100% train
    
    Fine-tuning hyperparameters:
    * Number of epochs: **3** (last epoch)
    * Update frequency: **4**
    * Warmup updates: **100**
    * Learning rate: **3e-05**
    * Max tokens: **2048**
    
    The results of the models (summaries) are obtained using the following snippet:
    ```
    bart.sample(slines, beam=10, lenpen=2.0, max_len_b=60, min_len=1, no_repeat_ngram_size=10, length_penalty=1.0)
    ```

9. `bart_finetuned_terms_labels_v9`
    * Pre-trained model: regular large BART
    * Source: **20** topic terms
    * Target: **variable number** (max. 100) of separate labels from the new (prob-based) candidate generation, where labels that only contain stopwords are removed; no ranking (supervised or unsupervised) is done on these
    * Corpus: all stackexchange corpora (english, biology, economics, law, photo - 0.30 coherence threshold), NOT selected by indices
    * Dataset split: 100% train
    
    Fine-tuning hyperparameters:
    * Number of epochs: **4** (out of 8 total epochs)
    * Update frequency: **4**
    * Warmup updates: **100**
    * Learning rate: **3e-05**
    * Max tokens: **2048**
    
    The results of the models (summaries) are obtained using the following snippet:
    ```
    bart.sample(slines, beam=10, lenpen=2.0, max_len_b=60, min_len=1, no_repeat_ngram_size=10, length_penalty=1.0)
    ```
   
10. `bart_finetuned_terms_labels_v10`
    * Pre-trained model: regular large BART
    * Source: **20** topic terms
    * Target: **variable number** (max. 100) of separate labels from T-order labeling (see https://hal-lirmm.ccsd.cnrs.fr/lirmm-01910614/document), where labels that only contain stopwords are removed
    * Corpus: all stackexchange corpora (english, biology, economics, law, photo - 0.30 coherence threshold), NOT selected by indices
    * Dataset split: 100% train

    Fine-tuning hyperparameters:
    * Number of epochs: **2** (last epoch)
    * Update frequency: **4**
    * Warmup updates: **100**
    * Learning rate: **3e-05**
    * Max tokens: **2048**
    
    The results of the models (summaries) are obtained using the following snippet:
    ```
    bart.sample(slines, beam=10, lenpen=2.0, max_len_b=60, min_len=1, no_repeat_ngram_size=10, length_penalty=1.0)
    ```
   
11. `bart_finetuned_terms_labels_v11`
    * Pre-trained model: regular large BART
    * Source: **20** topic terms
    * Target:
        * **variable number** (max. 100) of separate labels from the new (prob-based) candidate generation, where labels that only contain stopwords are removed; no ranking (supervised or unsupervised) is done on these
        * **variable number** (max. 5) of separate labels from T-order labeling (see https://hal-lirmm.ccsd.cnrs.fr/lirmm-01910614/document), where labels that only contain stopwords are removed
    * Corpus: all stackexchange corpora (english, biology, economics, law, photo - 0.30 coherence threshold), NOT selected by indices
    * Dataset split: 100% train
    
    Fine-tuning hyperparameters:
    * Number of epochs: **7** (last epoch)
    * Update frequency: **4**
    * Warmup updates: **100**
    * Learning rate: **3e-05**
    * Max tokens: **2048**
    
    The results of the models (summaries) are obtained using the following snippet:
    ```
    bart.sample(slines, beam=10, lenpen=2.0, max_len_b=60, min_len=1, no_repeat_ngram_size=10, length_penalty=1.0)
    ```
   
12. `bart_finetuned_terms_labels_v12`
    * Pre-trained model: regular large BART
    * Source: **20** topic terms
    * Target: **variable number** (max. 100) of separate labels extracted using `bart_finetuned_terms_labels_v11`
    * Corpus: all stackexchange corpora (english, biology, economics, law, photo - 0.30 coherence threshold), NOT selected by indices
    * Dataset split: 100% train
    
    Fine-tuning hyperparameters:
    * Number of epochs: **4** (out of 6 total epochs)
    * Update frequency: **4**
    * Warmup updates: **100**
    * Learning rate: **3e-05**
    * Max tokens: **2048**
    
    The results of the models (summaries) are obtained using the following snippet:
    ```
    bart.sample(slines, beam=10, lenpen=2.0, max_len_b=60, min_len=1, no_repeat_ngram_size=10, length_penalty=1.0)
    ```
    
## Terms-to-labels-sentences models

Similar to the terms-to-labels models, topic terms are the source, while the targets are labels, in addition to top-relevant topic sentences. This remains a one-to-many mapping.

1. `bart_finetuned_terms_labels_sentences_v1`
    * Pre-trained model: regular large BART
    * Source: **20** topic terms
    * Target:
        * **variable number** (max. 100) of separate labels from the new (prob-based) candidate generation, where labels that only contain stopwords are removed; no ranking (supervised or unsupervised) is done on these
        * **10** separate sentences, top-10 extracted using cos10 algorithm (see https://hal-lirmm.ccsd.cnrs.fr/lirmm-01910614/document)
    * Corpus: all stackexchange corpora (english - 0.25 coherence threshold; biology, economics, law, photo - 0.35 coherence threshold), NOT selected by indices
    * Dataset split: 100% train
    
    Fine-tuning hyperparameters:
	* Number of epochs: **6** (last epoch, low chances of improvement)
	* Update frequency: **4**
	* Warmup updates: **100**
	* Learning rate: **3e-05**
	* Max tokens: **2048**
	
    The results of the models (summaries) are obtained using the following snippet:
    ```
    bart.sample(slines, beam=10, lenpen=2.0, max_len_b=60, min_len=1, no_repeat_ngram_size=10, length_penalty=1.0)
    ```

2. `bart_finetuned_terms_labels_sentences_v2`
    * Pre-trained model: regular large BART
    * Source: **20** topic terms
    * Target:
        * **variable number** (max. 100) of separate labels from the new (prob-based) candidate generation, where labels that only contain stopwords are removed; no ranking (supervised or unsupervised) is done on these
        * **5** separate sentences, top-10 extracted using cos10 algorithm (see https://hal-lirmm.ccsd.cnrs.fr/lirmm-01910614/document)
    * Corpus: all stackexchange corpora (english - 0.25 coherence threshold; biology, economics, law, photo - 0.35 coherence threshold), NOT selected by indices
    * Dataset split: 100% train
    
    Fine-tuning hyperparameters:
	* Number of epochs: **3** (last epoch, low chances of improvement)
	* Update frequency: **4**
	* Warmup updates: **100**
	* Learning rate: **3e-05**
	
	* Max tokens: **2048**
	
    The results of the models (summaries) are obtained using the following snippet:
    ```
    bart.sample(slines, beam=10, lenpen=2.0, max_len_b=60, min_len=1, no_repeat_ngram_size=10, length_penalty=1.0)
    ```


## Terms-to-labels-ngrams models

Similar to the terms-to-labels models, topic terms are the source, while the targets are labels, in addition to (not necessarily consecutive) n-grams extracted from the topic terms, to teach the model that sometimes repeating the input terms is correct. This remains a one-to-many mapping.

1. `bart_finetuned_terms_labels_ngrams_v1`
    * Pre-trained model: regular large BART
    * Source: **20** topic terms
    * Target:
        * **variable number** (max. 100) of separate labels from the new (prob-based) candidate generation, where labels that only contain stopwords are removed; no ranking (supervised or unsupervised) is done on these
        * **10** separate n-grams, with the following n distributions: 2 - 0.10, 3 - 0.60, 4 - 0.30
    * Corpus: all stackexchange corpora (english - 0.25 coherence threshold; biology, economics, law, photo - 0.35 coherence threshold), NOT selected by indices
    * Dataset split: 100% train
    
    Fine-tuning hyperparameters:
	* Number of epochs: **3** (out of 5 total epochs)
	* Update frequency: **4**
	* Warmup updates: **100**
	* Learning rate: **3e-05**
	* Max tokens: **2048**
	
    The results of the models (summaries) are obtained using the following snippet:
    ```
    bart.sample(slines, beam=10, lenpen=2.0, max_len_b=60, min_len=1, no_repeat_ngram_size=10, length_penalty=1.0)
    ```

2. `bart_finetuned_terms_labels_ngrams_v2`
    * Pre-trained model: regular large BART
    * Source: **20** topic terms
    * Target:
        * **variable number** (max. 100) of separate labels from the new (prob-based) candidate generation, where labels that only contain stopwords are removed; no ranking (supervised or unsupervised) is done on these
        * **5** separate n-grams, with the following n distributions: 2 - 0.10, 3 - 0.60, 4 - 0.30
    * Corpus: all stackexchange corpora (english, biology, economics, law, photo - 0.30 coherence threshold), NOT selected by indices
    * Dataset split: 100% train
    
    Fine-tuning hyperparameters:
    * Number of epochs: **1**(!!!) (out of 3 total epochs)
    * Update frequency: **4**
    * Warmup updates: **100**
    * Learning rate: **3e-05**
    * Max tokens: **2048**
    
    The results of the models (summaries) are obtained using the following snippet:
    ```
    bart.sample(slines, beam=10, lenpen=2.0, max_len_b=60, min_len=1, no_repeat_ngram_size=10, length_penalty=1.0)
    ```

## Terms-to-labels-sentences-ngrams models

Similar to the terms-to-labels models, topic terms are the source, while the targets are labels, top relevant sentences to the topic, in addition to (not necessarily consecutive) n-grams extracted from the topic terms, to teach the model that sometimes repeating the input terms is correct. This remains a one-to-many mapping.

1. `bart_finetuned_terms_labels_sentences_ngrams_v1`
    * Pre-trained model: regular large BART
    * Source: **20** topic terms
    * Target:
        * **variable number** (max. 100) of separate labels from the new (prob-based) candidate generation, where labels that only contain stopwords are removed; no ranking (supervised or unsupervised) is done on these
        * **5** separate sentences, top-10 extracted using cos10 algorithm (see https://hal-lirmm.ccsd.cnrs.fr/lirmm-01910614/document)
        * **5** separate n-grams, with the following n distributions: 2 - 0.10, 3 - 0.60, 4 - 0.30
    * Corpus: all stackexchange corpora (english - 0.25 coherence threshold; biology, economics, law, photo - 0.35 coherence threshold), NOT selected by indices
    * Dataset split: 100% train
    
    Fine-tuning hyperparameters:
	* Number of epochs: **3** (last epoch)
	* Update frequency: **4**
	* Warmup updates: **100**
	* Learning rate: **3e-05**
	* Max tokens: **2048**
	
    The results of the models (summaries) are obtained using the following snippet:
    ```
    bart.sample(slines, beam=10, lenpen=2.0, max_len_b=60, min_len=1, no_repeat_ngram_size=10, length_penalty=1.0)
    ```

## Sentences-to-labels models

In the case of sentence-to-labels models, each sentence is a separate entry mapped to all the target labels for the corresponding topic. Thus, the corpus used for a single topic is a join between all the source sentences and all the target labels. This additionally differs from terms-labels models in that there can be multiple labels that make sense for a single topic, by querying for more relevant sentences.

1. `bart_finetuned_sentences_labels_v1`
    * Pre-trained model: regular large BART
    * Source: **10** separate sentences, top-10 extracted using cos10 algorithm (see https://hal-lirmm.ccsd.cnrs.fr/lirmm-01910614/document)
    * Target: **10** separate labels first 10 from the new (prob-based) candidate generation; no ranking (supervised or unsupervised) is done on these
    * Corpus: english (0.25 coherence threshold), selected by indices
    * Dataset split: 80% train, 20% test
    
    Fine-tuning hyperparameters:
    * Number of epochs: **4** (last epoch trained, potentially better results on longer training)
    * Update frequency: **4**
    * Warmup updates: **5**
    * Learning rate: **3e-05**
    * Max tokens: **2048**
    
    The results of the models (summaries) are obtained using the following snippet:
    ```
    bart.sample(slines, beam=10, lenpen=2.0, max_len_b=60, min_len=1, no_repeat_ngram_size=10, length_penalty=1.0)
    ```
