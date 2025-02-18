# JapaneseEmbeddingEval

* JSTS/JSICK: Spearman's rank correlation coefficient
   * Cosine similarity was used to calculate the similarity of sentence pairs.
* MIRACL: top30 recall

| Model                                           |   JSTS valid-v1.1 |   JSICK test |   MIRACL dev |   Average |
|:------------------------------------------------|------------------:|-------------:|-------------:|----------:|
| MU-Kindai/SBERT-JSNLI-base                      |             0.766 |        0.652 |        0.326 |     0.581 |
| MU-Kindai/SBERT-JSNLI-large                     |             0.774 |        0.677 |        0.278 |     0.576 |
| cl-nagoya/sup-simcse-ja-base                    |             0.809 |        <ins>0.827</ins> |        0.527 |     0.721 |
| cl-nagoya/sup-simcse-ja-large                   |             **0.831** |        **0.831** |        0.507 |     0.723 |
| cl-nagoya/unsup-simcse-ja-base                  |             0.789 |        0.790 |        0.487 |     0.689 |
| cl-nagoya/unsup-simcse-ja-large                 |             0.814 |        0.796 |        0.485 |     0.699 |
| colorfulscoop/sbert-base-ja                     |             0.742 |        0.657 |        0.254 |     0.551 |
| intfloat/multilingual-e5-base                   |             0.796 |        0.806 |        0.845 |     0.816 |
| intfloat/multilingual-e5-large                  |             <ins>0.819</ins> |        0.794 |        **0.883** |     **0.832** |
| intfloat/multilingual-e5-small                  |             0.789 |        0.814 |        <ins>0.847</ins> |     <ins>0.817</ins> |
| pkshatech/GLuCoSE-base-ja                       |             0.818 |        0.757 |        0.692 |     0.755 |
| pkshatech/simcse-ja-bert-base-clcmlp            |             0.801 |        0.735 |        0.544 |     0.693 |
| oshizo/sbert-jsnli-luke-japanese-base-lite      |             0.811 |        0.726 |        0.497 |     0.678 |
| sonoisa/sentence-bert-base-ja-mean-tokens-v2    |             0.809 |        0.768 |              |           |
| text-embedding-ada-002                          |             0.790 |        0.789 |        0.723[^1] |     0.768 |
| universal-sentence-encoder-multilingual-3       |             0.790 |        0.800 |              |           |
| universal-sentence-encoder-multilingual-large-3 |             0.801 |        0.823 |              |           |

[^1]: Evaluate only the first 100 queries out of 860 queries

## Datasets

* JSTS valid-v1.1
    * https://github.com/yahoojapan/JGLUE
    * 1,457 sentence pairs

* JSICK test
    * https://github.com/verypluming/JSICK
    * 4,927 sentence pairs

* MIRACL dev
    * https://huggingface.co/datasets/miracl/miracl
    * 860 japanese queries
    * From the 6,953,614 japanese data in miracl/miracl-corpus, the sentences to be searched were selected as follows to reduce computation time.
        1. positive passage for each query
        2. 300 hard negatives for each query
        * Hard negative mining was performed using intfloat/multilingual-e5-base
        * Scores for models other than intfloat/multilingual-e5-base are calculated higher only in the following case, but we believe that they are almost unaffected.
            * A negative that is ranked lower than the top 300 by intfloat/multilingual-e5-base is ranked within the top 30 by that model, which pushes the positive into the top 30 or lower.
