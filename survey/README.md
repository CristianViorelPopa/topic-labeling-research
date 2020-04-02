# Structure

This diretory contains survey-related results, including:
- csv files with the top 10 terms per topic (`topics_<subject>.csv`)
- txt files with candidate labels for each topic (row i corresponds to topic i), for both the old (before) and new (after) candidate generation methods (`candidates_<subject>_{before|after}.txt`)
- multiple txt files with relevant sentences/short paragraphs for each topic. Before these, there are the top 10 topic terms and a blank line after. The variations of these files are:
	* top 10 sentences (`topics_<subject>_sentences_raw.txt`)
	* top 2 sentences (`topics_<subject>_sentences.txt`)
	* top 2 sentences, with my own modifications (`topics_<subject>_sentences_my_opinion.txt`)
- txt file that contains added labels for each topic, that were not proposed by candidate generation, but will be used in the survey (`added_labels.txt`). The format used is:
    * one line with the top 10 topic terms
    * one line with the human-suggested labels, or '-' if none were suggested
- txt file that aggregates the topics and their labels, adding the previous labels (`survey_helper.txt`). The format is similar to the `added_files.txt`, with one line containing topic terms and one containing labels.
