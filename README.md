# PORORO: Platform Of neuRal mOdels for natuRal language prOcessing

<p align="center">
  <a href="https://github.com/kakaobrain/pororo/releases"><img alt="GitHub release" src="https://img.shields.io/github/release/kakaobrain/pororo.svg" /></a>
  <a href="https://github.com/kakaobrain/pororo/blob/master/LICENSE"><img alt="Apache 2.0" src="https://img.shields.io/badge/license-Apache%202.0-blue.svg" /></a>
  <a href="https://kakaobrain.github.io/pororo/"><img alt="Docs" src="https://img.shields.io/badge/docs-passing-success.svg" /></a>
  <a href="https://github.com/kakaobrain/pororo/issues"><img alt="Issues" src="https://img.shields.io/github/issues/kakaobrain/pororo" /></a>
</p>

<br>

![](assets/usage.gif)

`pororo` performs Natural Language Processing and Speech-related tasks.

It is easy to solve various subtasks in the natural language and speech processing field by simply passing the task name.

<br>

## Installation

- `pororo` is based on `torch=1.6(cuda 10.1)` and `python>=3.6`

- You can install a package through the command below:

```console
pip install pororo
```

- Or you can install it **locally**:

```console
git clone https://github.com/kakaobrain/pororo.git
cd pororo
pip install -e .
```

- For library installation for specific tasks other than the **common modules**, please refer to [INSTALL.md](INSTALL.md)

- For the utilization of **Automatic Speech Recognition**, [_wav2letter_](https://github.com/facebookresearch/wav2letter) should be installed separately. For the installation, please run the [asr-install.sh](asr-install.sh)

```console
bash asr-install.sh
```

- For the utilization of **Speech Synthesis**, please run the [tts-install.sh](tts-install.sh)

```console
bash tts-install.sh
```

- **Speech Synthesis** samples can be found [here](https://pororo-tts.github.io/)

<br>

## Usage

- `pororo` can be used as follows:
- First, in order to import `pororo`, you must execute the following snippet

```python
>>> from pororo import Pororo
```

- After the import, you can check the tasks currently supported by the `pororo` through the following commands

```python
>>> from pororo import Pororo
>>> Pororo.available_tasks()
"Available tasks are ['mrc', 'rc', 'qa', 'question_answering', 'machine_reading_comprehension', 'reading_comprehension', 'sentiment', 'sentiment_analysis', 'nli', 'natural_language_inference', 'inference', 'fill', 'fill_in_blank', 'fib', 'para', 'pi', 'cse', 'contextual_subword_embedding', 'similarity', 'sts', 'semantic_textual_similarity', 'sentence_similarity', 'sentvec', 'sentence_embedding', 'sentence_vector', 'se', 'inflection', 'morphological_inflection', 'g2p', 'grapheme_to_phoneme', 'grapheme_to_phoneme_conversion', 'w2v', 'wordvec', 'word2vec', 'word_vector', 'word_embedding', 'tokenize', 'tokenise', 'tokenization', 'tokenisation', 'tok', 'segmentation', 'seg', 'mt', 'machine_translation', 'translation', 'pos', 'tag', 'pos_tagging', 'tagging', 'const', 'constituency', 'constituency_parsing', 'cp', 'pg', 'collocation', 'collocate', 'col', 'word_translation', 'wt', 'summarization', 'summarisation', 'text_summarization', 'text_summarisation', 'summary', 'gec', 'review', 'review_scoring', 'lemmatization', 'lemmatisation', 'lemma', 'ner', 'named_entity_recognition', 'entity_recognition', 'zero-topic', 'dp', 'dep_parse', 'caption', 'captioning', 'asr', 'speech_recognition', 'st', 'speech_translation', 'ocr', 'srl', 'semantic_role_labeling', 'p2g', 'aes', 'essay', 'qg', 'question_generation', 'age_suitability']"
```

- To check which models are supported by each task, you can go through the following process

```python
>>> from pororo import Pororo
>>> Pororo.available_models("collocation")
'Available models for collocation are ([lang]: ko, [model]: kollocate), ([lang]: en, [model]: collocate.en), ([lang]: ja, [model]: collocate.ja), ([lang]: zh, [model]: collocate.zh)'
```

- If you want to perform a specific task, you can put the task name in the `task` argument and the language name in the `lang` argument

```python
>>> from pororo import Pororo
>>> ner = Pororo(task="ner", lang="en")
```

- After object construction, it can be used in a way that passes the input value as follows:

```python
>>> ner("Michael Jeffrey Jordan (born February 17, 1963) is an American businessman and former professional basketball player.")
[('Michael Jeffrey Jordan', 'PERSON'), ('(', 'O'), ('born', 'O'), ('February 17, 1963)', 'DATE'), ('is', 'O'), ('an', 'O'), ('American', 'NORP'), ('businessman', 'O'), ('and', 'O'), ('former', 'O'), ('professional', 'O'), ('basketball', 'O'), ('player', 'O'), ('.', 'O')]
```

- If task supports multiple languages, you can change the `lang` argument to take advantage of models trained in different languages.

```python
>>> ner = Pororo(task="ner", lang="ko")
>>> ner("????????? ????????? ??????(??????: Michael Jeffrey Jordan, 1963??? 2??? 17??? ~ )??? ????????? ????????? ?????? ????????????.")
[('????????? ????????? ??????', 'PERSON'), ('(', 'O'), ('??????', 'CIVILIZATION'), (':', 'O'), (' ', 'O'), ('Michael Jeffrey Jordan', 'PERSON'), (',', 'O'), (' ', 'O'), ('1963??? 2??? 17??? ~', 'DATE'), (' ', 'O'), (')???', 'O'), (' ', 'O'), ('??????', 'LOCATION'), ('???', 'O'), (' ', 'O'), ('?????????', 'O'), (' ', 'O'), ('?????? ??????', 'CIVILIZATION'), ('??????.', 'O')]
>>> ner = Pororo(task="ner", lang="ja")
>>> ner("???????????????????????????????????????????????????????????????????????????????????????????????????????????????")
[('????????????????????????????????????????????????', 'PERSON'), ('???', 'O'), ('????????????????????????', 'O'), ('???', 'O'), ('???', 'O'), ('????????????????????????', 'O'), ('??????', 'O')]
>>> ner = Pororo(task="ner", lang="zh")
>>> ner("????????????????????????????????????????NBA??????????????????????????????????????????????????????????????????????????????????????????")
[('?????????????????????????', 'PERSON'), ('???', 'O'), ('??????', 'GPE'), ('???', 'O'), ('???', 'O'), ('nba', 'ORG'), ('???', 'O'), ('???', 'O'), ('???', 'O'), ('???', 'O'), ('???', 'O'), ('???', 'O'), ('???', 'O'), ('???', 'O'), ('???', 'O'), ('???', 'O'), ('???', 'O'), ('???', 'O'), ('???', 'O'), ('???', 'O'), ('???', 'O'), ('???', 'O'), ('???', 'O'), ('???????????????', 'ORG'), ('???', 'O'), ('???', 'O'), ('???', 'O'), ('???', 'O'), ('???', 'O'), ('???', 'O'), ('???', 'O'), ('???', 'O')]
```

- If the task supports **multiple models**, you can change the `model` argument to use another model.

```python
>>> from pororo import Pororo
>>> mt = Pororo(task="mt", lang="multi", model="transformer.large.multi.mtpg")
>>> fast_mt = Pororo(task="mt", lang="multi", model="transformer.large.multi.fast.mtpg")
```

<br>

## Documentation

For more detailed information, see [full documentation](https://kakaobrain.github.io/pororo/)

If you have any questions or requests, please report [the issue](https://github.com/kakaobrain/pororo/issues).

<br>

## Citation

If you apply this library to any project and research, please cite our code:

```
@misc{pororo,
  author       = {Heo, Hoon and Ko, Hyunwoong and Kim, Soohwan and
                  Han, Gunsoo and Park, Jiwoo and Park, Kyubyong},
  title        = {PORORO: Platform Of neuRal mOdels for natuRal language prOcessing},
  howpublished = {\url{https://github.com/kakaobrain/pororo}},
  year         = {2021},
}
```

<br>

## Contributors

[Hoon Heo](https://github.com/huffon), [Hyunwoong Ko](https://github.com/hyunwoongko), [Soohwan Kim](https://github.com/sooftware), [Gunsoo Han](https://github.com/robinsongh381), [Jiwoo Park](https://github.com/bernardscumm) and [Kyubyong Park](https://github.com/Kyubyong)

<br>

## License

`PORORO` project is licensed under the terms of **the Apache License 2.0**.

Copyright 2021 Kakao Brain Corp. <https://www.kakaobrain.com> All Rights Reserved.
