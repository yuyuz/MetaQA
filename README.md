Download
========

The entire MetaQA dataset can be downloaded from [here](https://drive.google.com/drive/folders/0B-36Uca2AvwhTWVFSUZqRXVtbUE?resourcekey=0-kdv6ho5KcpEXdI2aUdLn_g&usp=sharing).

License
=======

This data is released under the Creative Commons Public License. A copy is included with the data.


Introduction
============

This document explains the contents of the MetaQA dataset, which is introduced in the following paper:

* Yuyu Zhang\*, Hanjun Dai\* (equal contribution), Zornitsa Kozareva, Alexander J. Smola and Le Song, *Variational Reasoning for Question Answering with Knowledge Graph*, in AAAI 2018. [[arXiv](https://arxiv.org/abs/1709.04071)]

```
@inproceedings{zhang2017variational,
  title={Variational Reasoning for Question Answering with Knowledge Graph},
  author={Zhang, Yuyu and Dai, Hanjun and Kozareva, Zornitsa and Smola, Alexander J and Song, Le},
  booktitle={AAAI},
  year={2018}
}
```

MetaQA stands for MoviE Text Audio QA. It contains three main components:

* Vanilla text data: There are three datasets in total: 1-hop, 2-hop and 3-hop. The 1-hop dataset is derived from the `wiki_entities` branch of the Facebook MovieQA (a.k.a. WikiMovies) dataset (https://research.fb.com/downloads/babi/). Questions with ambiguous entity are removed, making our Vanilla 1-hop text dataset slightly smaller than MovieQA. The 2-hop and 3-hop datasets are generated from the same knowledge base which will be described shortly. We design 21 question types in 2-hop data, and 15 question types in 3-hop data, with 10 text templates for each type. A full list of question types and examples can be found in our paper.

* NTM text data (paraphrased by neural translation model): With the help of neural translation model, more variations of questions can be introduced automatically. We translate each question in the vanilla datasets to French, and then translate it back to English with beam search to get a paraphrased question. Entities are guaranteed to be kept in the paraphrased question.

* Audio data: We use Google text-to-speech API to read all questions in Vanilla datasets and save the audio as mp3 files. For users' convenience, we also provide extracted MFCC features for each question.

We organize datasets in the number of hops first, so folders named `1-hop`, `2-hop` and `3-hop` are in the root directory. In each of them, we have `vanilla`, `ntm` and `audio` folders for the components described above. The questions in each component are of the same order. For example, the 5th question in `vanilla/qa_train.txt` corresponds to the 5th question in `ntm/qa_train.txt` and the 5th question in `audio/qa_train.npz`.


Knowledge base
==============

All questions in MetaQA are generated from the movie knowledge base in MovieQA. We provide the knowledge base in `kb.txt` in the root directory. Each line is one knowledge triple, and the schema is `subject|relation|object`.

For example, `Kismet|directed_by|William Dieterle` shows that the movie `Kismet` is directed by the director `William Dieterle`.


Train / dev / test split
========================

We provide train / dev / test split for 1-hop, 2-hop and 3-hop datasets. All components share the same split. The counts of questions are listed below:

|       |  1-hop |   2-hop |   3-hop |
|-------|-------:|--------:|--------:|
| Train | 96,106 | 118,980 | 114,196 |
| Dev   |  9,992 |  14,872 |  14,274 |
| Test  |  9,947 |  14,872 |  14,274 |


Question type files
===================

In each hop folders, we provide question type files for train / dev / test data. These question type files are generally for the evaluation of QA systems by decomposing the performance into different question types. Question type files follow the same order of the text and audio questions.

For example, the 3rd line in `1-hop/qa_test_qtype.txt` identifies the question type of the 3rd question in `1-hop/vanilla/qa_test.txt`, `1-hop/ntm/qa_test.txt` and `1-hop/audio/qa_test.npz`.


Text data
=========

For text data in `txt` files, including Vanilla and NTM datasets, each line is a question and its corresponding answer(s). Question and answer(s) are separated by `\t` in the same line. Questions may have multiple answers. If so, answer entities are separated by `|`. Entities in questions are highlighted with a pair of square brackets in the text.

For example, in the question `[Joe Thomas] appears in which movies`, the topic entity `Joe Thomas` is highlighted.


Audio data
==========

For audio data, we provide original mp3 files, such as `1-hop/audio/audio_mp3.tar.gz`. Each question in the Vanilla 1-hop data has a mp3 file in this archive file, and the file name follows the line index (0-based) of vanilla questions. The answers can be found in the text data, as described before.

Since it takes time to process tens of thousands of audio files, we also provide extracted MFCC features for each question in `npz` files. They can be loaded by the `numpy` library. For example, load in MFCC data by `mfccs = np.load(qa_train.npz)` and get the MFCC features for the 0th question by `mfccs['qa_train-0']`.

Besides the audio of questions, we also provide audio of all entities in the knowledge base in the `entity` folder in the root directory. In this folder, the original mp3 files are archived in `entity_mp3.tar.gz`, which follows the entity index (0-based) in `kb_entity_dict.txt`. Again, we provide extracted MFCC features for each entity in `kb_entity.npz`.
