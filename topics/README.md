# Structure

This diretory contains topic-related results, including:
- csv files with the top 10 terms per topic (topics_\<subject\>_new.csv)
- json files with the top 100 terms, and their respective probability, per topic (topics_\<subject\>_new.json)
- multiple txt files with relevant sentences/short paragraphs for each topic, of which:
	* top 10 sentences (topics_\<subject\>_new_sentences_raw.txt)
	* top 2 sentences (topics_\<subject\>_new_sentences.txt)
	* top 2 sentences, with my own modifications (topics_\<subject\>_new_sentences_my_opinion.txt)
- txt file containing indices of topics that are not comprehensible enough (topics_\<subject\>_new_ids.txt) for one (or more) of the following reasons:
	* confusing, even considering the example sentences
	* 2 (or more) different meanings with no clear winner -> answers would be totally different based on the example sentence chosen
	* 1 german topic, 2 spanish/french topics
	* focused on artifacts from text processing (code/some html tokens/latex formatting)
	* duplicates

The `full` directory contains all the initial topics, while the `selected` one contains only the ones that were not removed due to the reasons above.