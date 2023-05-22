# Seahorse dataset

Seahorse is a dataset for multilingual, multifaceted summarization evaluation.
It contains 96K summaries with human ratings along 6 quality dimensions: comprehensibility, repetition, grammar, attribution, main ideas, and conciseness, covering 6 languages, 9 systems and 4 datasets.

You can download the Seahorse dataset here: https://storage.googleapis.com/seahorse-public/seahorse_data.zip

## Dataset description

The dataset is split into 3 .tsv files: the train, validation, and test sets.

Each file contains the following information:
* `gem_id` The ID corresponding to the article that was used to generate the summary (see [Retrieving articles from GEM](https://github.com/google-research-datasets/seahorse/edit/main/README.md#retrieving-articles-from-gem) for more details)
* `worker_lang` The language ID (de, es-ES, en-US, ru, tr, vi) 
* `summary` The generated summary
* `model` The source of the summary (either reference or the summarization model)
* `question1-6` 6 columns with annotator ratings, corresponding to the 6 dimensions of quality (comprehensibility, repetition, grammar, attribution, main idea(s), and conciseness). If `question1`= No, then there will be no ratings for the remaining questions.

Here is an example entry:
```
xlsum_english-validation-6416	en-US	Schools in England, Wales and Scotland are being urged to bring back overseas exchange trips.	t5_base	Yes	Yes	Yes	Yes	Yes	Yes
```

There is also a directory called `duplicates`, which contains the items that received multiple annotations. Note that this data should NOT be used for training metrics, as there may be overlap between the train/dev/test sets.

## Retrieving articles from GEM

The xsum, mlsum, and xlsum articles can all be retrieved using the GEMv2 on HuggingFace.

The wikilingua articles must be retrieved using GEMv1.

## Contact

Please email eaclark@google.com if you have any questions about the dataset.
