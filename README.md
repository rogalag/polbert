# Polbert - Polish BERT
Polish version of BERT language model is here! While this is still work in progress, I'm happy to share the first model, similar to a BERT-Base and trained on a large Polish corpus. If you'd like to contribute to this project, please reach out to me!

![PolBERT image](/img/polbert.png)

## Pre-training corpora

Below is the list of corpora used along with the output of `wc` command (counting lines, words and characters). These corpora were divided into sentences with srxsegmenter (see references), concatenated and tokenized with HuggingFace BERT Tokenizer. 

| Tables        | Lines           | Words  | Characters  |
| ------------- |--------------:| -----:| -----:|
| [Polish subset of Open Subtitles](http://opus.nlpl.eu/OpenSubtitles-v2018.php)      | 236635408| 1431199601 | 7628097730 |
| [Polish subset of ParaCrawl](http://opus.nlpl.eu/ParaCrawl.php)     | 8470950      |   176670885 | 1163505275 |
| [Polish Parliamentary Corpus](http://clip.ipipan.waw.pl/PPC) | 9799859      |    121154785 | 938896963 |
| [Polish Wikipedia - Feb 2020](https://dumps.wikimedia.org/plwiki/latest/plwiki-latest-pages-articles.xml.bz2) | 8014206      |    132067986 | 1015849191 |
| Total | 262920423      |    1861093257 | 10746349159 |

## Pre-training details
* Polbert was trained with code provided in Google BERT's github repository (https://github.com/google-research/bert)
* Currently released model follows bert-base-uncased model architecture (12-layer, 768-hidden, 12-heads, 110M parameters)
* Training set-up: 1 million training steps with batches of 256 sequences of length 512 with an initial learning rate 1e-4
* The model was trained on a single Google Cloud TPU v3-8 

## Usage
Polbert is released via [HuggingFace Transformers library](https://huggingface.co/transformers/).
```python
from transformers import AutoTokenizer, AutoModel

tokenizer = AutoTokenizer.from_pretrained("dkleczek/bert-base-polish-uncased-v1")
model = AutoModel.from_pretrained("dkleczek/bert-base-polish-uncased-v1")
```

See the next section for an example usage of Polbert in downstream tasks. 

## Evaluation
I'd love to get some help from the Polish NLP community here! If you feel like evaluating Polbert on some benchmark tasks, it would be great if you can share the results. 

So far, I've compared the performance of Polbert vs Multilingual BERT on PolEmo 2.0 sentiment classification, here are the results. These results are produced via *polemo_eval.py* script in the project repo. 

| PolEmo 2.0 Sentiment Classifcation | Test Accuracy | 
| ------------- |--------------:|
| Multilingual BERT | 0.8158 |
| Polbert | TBD |

## Bias
The data used to train the model is biased. It may reflect stereotypes related to gender, ethnicity etc. Please be careful when using the model for downstream task to consider these biases and mitigate them.  

## Acknowledgements
I'd like to express my gratitude to Google [TensorFlow Research Cloud (TFRC)](https://www.tensorflow.org/tfrc) for providing the free TPU credits - thank you! Also appreciate the help from Timo Möller from [deepset](https://deepset.ai) for sharing tips and scripts based on their experience training German BERT model. Finally, thanks to Rachel Thomas, Jeremy Howard and Sylvain Gugger from [fastai](https://www.fast.ai) for their NLP and Deep Learning courses!

## Author
Dariusz Kłeczek (a.k.a. Darek)

## References
* https://github.com/google-research/bert
* https://github.com/narusemotoki/srx_segmenter
* SRX rules file for sentence splitting in Polish, written by Marcin Miłkowski: https://raw.githubusercontent.com/languagetool-org/languagetool/master/languagetool-core/src/main/resources/org/languagetool/resource/segment.srx
* PolEmo 2.0 Sentiment Analysis Dataset for CoNLL: https://clarin-pl.eu/dspace/handle/11321/710

