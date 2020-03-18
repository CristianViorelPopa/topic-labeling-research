# Notes:
- the process of getting the labels for a topic consists of 2 steps:
	* candidate generation, where a certain number candidate labels (before, 19, right now, 100) are selected based on doc2vec and word2vec similarity
	* supervised/unsupervised ranking of the candidate labels
- statistics about the number of common candidate labels between initial and tweaked version (100 candidates each):
	* min: 16
	* max: 92
	* mean: 57.42
	* stddev: 17.97
- the files in the 'interesting_cases' directory have txt files that showcase some topics where the labeling results vary a lot between different versions
	* each version name is described below ('Versions' section)
	* the format for comparison is:
		* 1st row: top 10 topic terms
		* 2nd row: top 10 topic labels 1st version
		* 3rd row: top 10 topic labels 2nd version
	* comparison order is based on the filename, e.g. in 'interesting_cases_before_19_vs_before.txt' the 1st version is 'before_19', while the 2nd version is 'before'
	* cases are conveniently partitioned in 'good', 'neutral' or 'bad' based on the performance of the 2nd version compared to 1st


# Corpus components
- csv: contains only the top-N (currently, 10) topic terms, no order
- json: contains the top-N (currently, 100) topic terms and their respective probabilities in the topic model


# Corpus versions
- initial: LDA output is 10-topic terms csv and 10-topic terms json; topic coherence was used from the beginnning (coherence threshold: 0.25) --> NOT currently tested on
- new: LDA output is 10-topic terms csv and 100-topic terms json; as soon as this was used, then the number of candidate labels was raised to 100, instead of 19 (coherence threshold: 0.25)
- new-0.4: LDA output is 10-topic terms csv and 100-topic terms json; number of candidate labels is 100 (coherence threshold: 0.4) --> NOT currently tested on


# Versions
- before: no prob-based ranking on the candidate generation or unsupervised labeling
- before-19: same as 'before', but uses only the first 19 candidates (for ranking purposes)
- after: prob-based ranking for both the candidate generation and unsupervised ranking --> NOT currently logged, bad labels that were way too focused on the first few terms
- after-experimental: same as 'after', but the first 5 topic terms in each topic have their probs equalized (during the candidate generation)
- prob-based-ranking: initial candidate generation, prob-based unsupervised ranking
- prob-based-cand-gen: prob-based candidate generation, initial unsupervised ranking


# Prob-based Ranking
- unsupervised
	- tri-grams of the topic terms are computed with probs of the word instead of raw occurences
	- score of the labels is weighted by the number of times they are found as candidates for other topics (to aid discrimination)
- supervised
	- tri-grams of the topic terms are computed with probs of the word instead of raw occurences
	- rest of the features are not modified
