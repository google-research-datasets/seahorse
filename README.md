# Seahorse dataset

Seahorse is a dataset for multilingual, multifaceted summarization evaluation.
It contains 96K summaries with human ratings along 6 quality dimensions: comprehensibility, repetition, grammar, attribution, main ideas, and conciseness, covering 6 languages, 9 systems and 4 datasets.

More details can be found in the [paper](https://arxiv.org/abs/2305.13194), which can be cited as follows:
```
@misc{clark2023seahorse,
      title={SEAHORSE: A Multilingual, Multifaceted Dataset for Summarization Evaluation}, 
      author={Elizabeth Clark and Shruti Rijhwani and Sebastian Gehrmann and Joshua Maynez and Roee Aharoni and Vitaly Nikolaev and Thibault Sellam and Aditya Siddhant and Dipanjan Das and Ankur P. Parikh},
      year={2023},
      eprint={2305.13194},
      archivePrefix={arXiv},
      primaryClass={cs.CL}
}
```
The Seahorse dataset is released under the [CC-BY 4.0](https://creativecommons.org/licenses/by/4.0/) license.

You can download the dataset here: https://storage.googleapis.com/seahorse-public/seahorse_data.zip


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

If you would like to access the articles that the Seahorse summaries are based on, you will need to retrieve them using their GEM ids.

The [xsum](https://huggingface.co/datasets/GEM/xsum), [mlsum](https://huggingface.co/datasets/GEM/mlsum), and [xlsum](https://huggingface.co/datasets/GEM/xlsum) articles can all be retrieved through GEM on HuggingFace. The `gem_id` column points to the article in the GEM datasets.

The wikilingua article ids come from a previous version of the GEM dataset and should be retrieved using [TensorFlow datasets](https://www.tensorflow.org/datasets/catalog/gem). Here's an example of how to load the English wikilingua dataset into a dataframe:

```
import tensorflow_datasets as tfds

lang = 'english_en'
orig_split = 'validation'

ds, info = tfds.load(f'huggingface:gem/wiki_lingua_{lang}', split=orig_split, with_info=True)
hfdf = tfds.as_dataframe(ds,info)
```

## Leaderboard

We are maintaining a leaderboard with official results on our test set.

We ask you to **not** incorporate any part of the Seahorse validation set into the training data, and only use it for validation/hyperparameter tuning as development sets are typically used.

We report results on two metrics: Pearson correlation ($\rho$) and area under the ROC curve (roc).

<table>
  <tr>
    <th></th>
    <th></th>
    <th colspan="2">Q1</th>
    <th colspan="2">Q2</th>
    <th colspan="2">Q3</th>
    <th colspan="2">Q4</th>
    <th colspan="2">Q5</th>
    <th colspan="2">Q6</th>
  </tr>
  <tr>
    <th>Model</th>
    <th>Link</th>
    <th>$\rho$</th>
    <th>roc</th>
    <th>$\rho$</th>
    <th>roc</th>
    <th>$\rho$</th>
    <th>roc</th>
    <th>$\rho$</th>
    <th>roc</th>
    <th>$\rho$</th>
    <th>roc</th>    
    <th>$\rho$</th>
    <th>roc</th>
  </tr>
       <tr>
      <td> mT5-seahorse </td>
         <td> <a href="">[Clark et al. 2023]</a> </td>
         <td><b>0.52</b></td>
         <td><b>0.90</b></td>
         <td><b>0.86</b></td>
         <td><b>0.98</b></td>
         <td><b>0.45</b></td>
         <td><b>0.84</b></td>
         <td><b>0.59</b></td>
         <td><b>0.85</b></td>
         <td><b>0.50</b></td>
         <td><b>0.80</b></td>
         <td><b>0.52</b></td>
         <td><b>0.81</b></td>         
  </tr> 
         <tr>
      <td> mT5-XNLI </td>
           <td> [<a href="https://aclanthology.org/2022.naacl-main.287/">Honovich et al. 2022</a>, <a href="https://aclanthology.org/D18-1269/">Conneau et al. 2018</a>] </td>
         <td>-</td>
         <td>-</td>
         <td>-</td>
         <td>-</td>
         <td>-</td>
         <td>-</td>
         <td>0.43</td>
         <td>0.78</td>
         <td>-</td>
         <td>-</td>
         <td>-</td>
         <td>-</td>         
  </tr>
           <tr>
      <td> ROUGE-L </td>
           <td> [<a href="https://aclanthology.org/2022.naacl-main.287">Lin et al. 2004</a>] </td>
         <td>0.04</td>
         <td>0.54</td>
         <td>0.06</td>
         <td>0.54</td>
         <td>-0.03</td>
         <td>0.43</td>
         <td>0.13</td>
         <td>0.55</td>
         <td>0.03</td>
         <td>0.54</td>
         <td>0.02</td>
         <td>0.54</td>         
  </tr> 
           <tr>
      <td> Majority Class </td>
           <td> - </td>
         <td>-</td>
         <td>0.5</td>
         <td>-</td>
         <td>0.5</td>
         <td>-</td>
         <td>0.5</td>
         <td>-</td>
         <td>0.5</td>
         <td>-</td>
         <td>0.5</td>
         <td>-</td>
         <td>0.5</td>         
  </tr>   
  
</table>

## Leaderboard Submission

If you want to submit to the leaderboard, please send an email to the contact email below with your results.

## Contact

Please email eaclark@google.com if you have any questions about the dataset.
