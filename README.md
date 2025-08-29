# FadeIT shared task

This repository contains useful materials related to the [FadeIT shared task](https://sites.google.com/fbk.eu/fadeit2026) at [EVALITA 2026](https://www.evalita.it/campaigns/evalita-2026/). You can find more information about how to participate, how data looks like, and which subtasks are available on the [shared task website](https://sites.google.com/fbk.eu/fadeit2026).

- :page_with_curl: **[Get the data](#page_with_curl-get-the-data)**
- :triangular_ruler: **[Data format](#triangular_ruler-data-format)**
- :rocket: **[Submission requirements](#rocket-submission-requirements)**
- :bar_chart: **[Evaluation and scorer](#bar_chart-evaluation-and-scorer)**
- :pushpin: **[Baselines](#pushpin-baselines)**
- :alarm_clock: **[Important dates](#alarm_clock-important-dates)**
- :paperclip: **[How to cite](#citation)**


### :page_with_curl: Get the data

If you are interested in participating in the FadeIT shared task, **please write to us at fadeit2026[at]gmail[dot]com indicating provisional team members and the subtasks of potential interest**. We will send to you the link to join the dedicated Google Group, where you can keep in touch with us and ask any shared task-related questions. You will also find information about how to obtain the data (from September 22th, 2025).


### :triangular_ruler: Data format

We provide `train-dev.tsv` and `train-dev.conll` for **Subtask A** and **Subtask B**, respectively, following the format described below. Test data for both subtasks (`test.tsv` and `test.conll`) will be available during the evaluation window (i.e., November 3-17, 2025).

#### Subtask A: *Coarse-grained* fallacy detection

The data format is in a tab-separated format and contains a header line. Each line consists of information about each post (i.e., id, date, topic, text, labels). Post-level annotations by each annotator are provided in separate columns and multiple annotations for the same post and annotator are separated by a pipe (|). Specifically, each post is represented as shown below:

```
$POST_ID      $POST_DATE      $POST_TOPIC_KEYWORDS      $POST_TEXT      $LABELS_BY_ANN_A      $LABELS_BY_ANN_B
```

where:
- `$POST_ID`: the identifier of the post (*integer*);
- `$POST_DATE`: the date of the post (*YYYY-MM*);
- `$POST_TOPIC_KEYWORDS`: the set in which the keyword that led to the post selection belongs (*migration*, *climate change*, or *public health*);
- `$POST_TEXT`: the text of the post (anonymized with [USER], [URL], [EMAIL], and [PHONE] placeholders);
- `$LABELS_BY_ANN_j`: the fallacy label(s) assigned by annotator *j* for the post (e.g., "Vagueness", "Strawman"). In the case where multiple labels for the post are assigned by the same annotator *j*, these are separated by a pipe (|) and ordered lexicographically, e.g., "Strawman|Vagueness". In the case where no labels for the post are assigned by the same annotator j, the label is empty.

An example of a post for subtask A is available on the [FadeIT shared task website](https://sites.google.com/fbk.eu/fadeit2026/data).

**Note**: `test.tsv` will follow the same format of `train-dev.tsv` but with no gold label values (i.e., `$LABELS_BY_ANN_j` column values will be empty and must be predicted).

#### Subtask B: *Fine-grained* fallacy detection

The data format is based on the CoNLL format. Each post is separated by a blank line and consists of a header with post information, followed by each token in the text (with tab-separated information) separated by newlines. Token annotations follow the BIO scheme (i.e., B: *begin*, I: *inside*, O: *outside*) and multiple annotations for the same token and annotator are separated by a pipe (|). Specifically, each post is represented as shown below:

```
# post_id = $POST_ID
# post_date = $POST_DATE
# post_topic_keywords = $POST_TOPIC
# post_text = $POST_TEXT
$TOKEN_1      $TOKEN_1_TEXT      $TOKEN_1_LABELS_BY_ANN_A      $TOKEN_1_LABELS_BY_ANN_B
...
$TOKEN_N      $TOKEN_N_TEXT      $TOKEN_N_LABELS_BY_ANN_A      $TOKEN_N_LABELS_BY_ANN_B
```

where:
- `$POST_ID`: the identifier of the post (*integer*);
- `$POST_DATE`: the date of the post (*YYYY-MM*);
- `$POST_TOPIC_KEYWORDS`: the set in which the keyword that led to the post selection belongs (*migration*, *climate change*, or *public health*);
- `$POST_TEXT`: the text of the post (anonymized with [USER], [URL], [EMAIL], and [PHONE] placeholders);
- `$TOKEN_i`: the index of the token within the post (*incremental integer*);
- `$TOKEN_i_TEXT`: the text of the *i*-th token within the post;
- `$TOKEN_i_LABELS_BY_ANN_j`: the fallacy label(s) assigned by annotator *j* for the *i*-th token within the post. Each label follows the format `$BIO`-`$LABEL`, where `$BIO` is the BIO tag and `$LABEL` is the fallacy label (e.g., "Vagueness", "Strawman"), e.g., "B-Vagueness", "I-Strawman", and "O". In the case where multiple labels for the *i*-th token are assigned by the same annotator *j*, these are separated by a pipe (|) and ordered lexicographically by `$LABEL`, e.g., "I-Strawman|B-Vagueness". In the case where no labels for the i-th token are assigned by the same annotator j, the label is "O".

An example of a post for subtask B is available on the [FadeIT shared task website](https://sites.google.com/fbk.eu/fadeit2026/data).

**Note**: `test.conll` will follow the same format of `train-dev.conll` but with no gold label values (i.e., `$TOKEN_i_LABELS_BY_ANN_j` column values will be empty and must be predicted).


### :rocket: Submission requirements

Test data to be used for coarse-grained or fine-grained subtasks (`test.tsv` and `test.conll`, respectively) will be made available on **November 3, 2025** and participants can submit their predictions during the evaluation window (i.e., **November 3-17, 2025**). Test set results and the overall systems' ranking will be communicated to participants by November 20th, 2025.

We allow participants to submit **up to 3 runs for each subtask** (i.e., a team participating in all subtasks will be able to submit up to a total of 6 runs, of which up to 3 for each subtask). Different runs can reflect e.g., different solutions or different configurations of the same system.

***Note**: Participants are allowed to use external resources in addition to (or in place of) the data provided by the organizers to train their models, e.g., pre-trained models, existing fallacy detection datasets, and newly annotated data. Participants are also encouraged to leverage time and topic metadata associated with the posts for designing their solutions. In case of doubt, you can write to us through the Google Group.*

#### Submission format

Prediction files must be formatted **the same way as train/dev data** (i.e., practically, just by filling the empty gold label columns on the test data file). The order of the posts in the test data must be preserved.

##### Subtask A: *Coarse-grained* fallacy detection

A tab-separated file with a social media post per line and the first line as header. Only the **last two columns** will be used for evaluation.

```
post_id  post_date    post_topic_keywords  post_text                              labels_a1                                                                           labels_a2
658      2021-06      public health        Studio americano: la mutazione...      Appeal-to-authority|Evading-the-burden-of-proof|Hasty-generalization|Vagueness      Appeal-to-authority|Doubt|Vagueness
...
```

If your system produces a single prediction for each post (either using or not using individual/disaggregated annotators' labels during training), just write the same predictions on both columns. Otherwise, print the different predictions to the corresponding columns.

*Note: the first column of the test set starts at id 57, the snippet above is just an example of the submission format.*

##### Subtask B: *Fine-grained* fallacy detection

A CoNLL-like file where each social media post is separated by a blank line and consists of a header with post information, followed by each token in the text (with tab-separated information) separated by newlines. Only the **last two columns** for each token will be used for evaluation.

```
# post_id = 658
# post_date = 2021-06
# post_topic_keywords = public health
# post_text = Studio americano: la mutazione...
1      Studio      B-Appeal-to-authority|B-Vagueness      B-Appeal-to-aothority|B-Vagueness
2      americano   I-Appeal-to-authority|I-Vagueness      I-Appeal-to-authority|I-Vagueness
3      :           I-Appeal-to-authority                  I-Appeal-to-authority
4      la          B-Evading-the-burden-of-proof          O
5      mutazione   I-Evading-the-burden-of-proof          O
...
```

If your system produces a single prediction for each token (either using or not using individual/disaggregated annotators' labels during training), just write the same predictions on both columns. Otherwise, print the different predictions to the corresponding columns.

*Note: the first column of the test set starts at id 57, the snippet above is just an example of the submission format.*

#### How to submit your runs

**[1]** Name your submission file(s) following the naming convention **`TEAM.SUBTASK.RUN.[tsv|conll]`**, where:
- **TEAM**: the name of your team;
- **SUBTASK**: the identifier of the subtask, i.e., either `A` or `B`;
- **RUN**: an incremental identifier starting from 1 denoting your subtask run (e.g., `1`, `2`, or `3`).

*For instance, if you are participating in both the subtasks and send a single run for both and your team is named "Pippo", your run names would be "Pippo.A.1.tsv" and "Pippo.B.1.conll"*.

**[2]** Compress all your run files as a ZIP file named `TEAM.zip` (where TEAM is the name of your team).

**[3]** Send an email to fadeit2026@gmail.com with subject **FadeIT: TEAM submission** attaching `TEAM.zip`.


### :bar_chart: Evaluation and scorer

Predictions will be evaluated according to micro **F1 score** (Subtask A) and micro **F1 score (*soft evaluation*)** (Subtask B, see details on the evaluation on the [FadeIT shared task website](https://sites.google.com/fbk.eu/fadeit2026/task-description). We will provide participants with a scorer (i.e., `eval.py`) on **September 22, 2025**. The usage is the following:

```
python eval.py -S $SUBTASK -G $GOLD_FILEPATH -P $PRED_FILEPATH
```
- `$SUBTASK`: The subtask the run refers to. Choices: ['`A`', '`B`'].
- `$GOLD_FILEPATH`: Path to the file that contains the gold standard(s) for the subtask.
- `$PRED_FILEPATH`: Path to the file that contains the predictions for the subtask, with the same format as `$GOLD_FILEPATH`.


### :pushpin: Baselines

Baselines for both *coarse-grained* and *fine-grained fallacy detection* subtasks will be provided to participants on **September 22, 2025** – i.e., when data (train/dev) and evaluation scorer will be released.


### :alarm_clock: Important dates

- **September 22, 2025** – Data (train/dev) and evaluation scorer are provided to participants
- **November 3-17, 2025** – Evaluation window: participants submit predictions on the test data
- **December 1, 2025** – Submission deadline for system description papers by participants
- **December 15, 2025** – Notification of acceptance of system description papers to participants
- **January 12, 2026** – Camera-ready version of system description papers due
- **February 26-27, 2026** – [EVALITA 2026 Workshop](https://www.evalita.it/campaigns/evalita-2026/) in Bari, Italy

### Citation

If you refer to the FadeIT shared task in your work, please cite our paper as follows:

```
@inproceedings{ramponi-tonelli-2026-fadeit,
    title={{F}ade{IT} at {EVALITA} 2026: Overview of the Fallacy Detection in Italian Social Media Texts Task},
    author={Ramponi, Alan and Tonelli, Sara},
    booktitle={Proceedings of the Ninth Evaluation Campaign of Natural Language Processing and Speech Tools for Italian. Final Workshop (EVALITA 2026)},
    publisher = {CEUR.org},
    year = {2026},
    month = {February},
    address = {Bari, Italy}
}
```

If you refer, use or build on top of the dataset used in the context of the shared task (i.e., FAINA), please cite the following:

```
@inproceedings{ramponi-etal-2025-fine,
    title = "Fine-grained Fallacy Detection with Human Label Variation",
    author = "Ramponi, Alan  and
      Daffara, Agnese  and
      Tonelli, Sara",
    editor = "Chiruzzo, Luis  and
      Ritter, Alan  and
      Wang, Lu",
    booktitle = "Proceedings of the 2025 Conference of the Nations of the Americas Chapter of the Association for Computational Linguistics: Human Language Technologies (Volume 1: Long Papers)",
    month = apr,
    year = "2025",
    address = "Albuquerque, New Mexico",
    publisher = "Association for Computational Linguistics",
    url = "https://aclanthology.org/2025.naacl-long.34/",
    doi = "10.18653/v1/2025.naacl-long.34",
    pages = "762--784",
    ISBN = "979-8-89176-189-6"
}
```

