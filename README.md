# NLBSE'25 Tool Competition: Code Comment Classification

This repository contains the data and results of the baseline classifiers for the [NLBSE’25 tool competition](https://nlbse2025.github.io/tools/) on code comment classification.

The competition participants must use the provided data to train/test their classifiers, which should outperform the baselines.

Details on how to participate in the competition are available in our [Google Colab notebook](https://colab.research.google.com/drive/1GhpyzTYcRs8SGzOMH3Xb6rLfdFVUBN0P?usp=sharing).

- [Citing Related Work](#citing-related-work)
- [Data for classification](#data-for-classification)
- [Dataset Preparation](#dataset-preparation)
- [Software Projects](#software-projects)
- [Baseline Results](#baseline-results)

## Citing Related Work

Since you will be using our dataset (and possibly one of our notebooks) as well as the original work behind the dataset, please cite the following references in your paper:

```bibtex
@inproceedings{nlbse2025,
  author={Colavito, Giuseppe and Al-Kaswan, Ali and Stulova, Nataliia and Rani, Pooja},
  title={The NLBSE'25 Tool Competition},
  booktitle={Proceedings of The 4th International Workshop on Natural Language-based Software Engineering (NLBSE'25)},
  year={2025}
}
```

```bibtex
@article{rani2021,
  title={How to identify class comment types? A multi-language approach for class comment classification},
  author={Rani, Pooja and Panichella, Sebastiano and Leuenberger, Manuel and Di Sorbo, Andrea and Nierstrasz, Oscar},
  journal={Journal of systems and software},
  volume={181},
  pages={111047},
  year={2021},
  publisher={Elsevier}
}
```

```bibtex
@inproceedings{pascarella2017,
  title={Classifying code comments in Java open-source software systems},
  author={Pascarella, Luca and Bacchelli, Alberto},
  booktitle={2017 IEEE/ACM 14th International Conference on Mining Software Repositories (MSR)},
  year={2017},
  organization={IEEE}
}
```

```bibtex
@inproceedings{alkaswan2023stacc,
  title={Stacc: Code comment classification using sentencetransformers},
  author={Al-Kaswan, Ali and Izadi, Maliheh and Van Deursen, Arie},
  booktitle={2023 IEEE/ACM 2nd International Workshop on Natural Language-Based Software Engineering (NLBSE)},
  pages={28--31},
  year={2023},
  organization={IEEE}
}
```

## Data for Classification

We provide a [HF Dataset](https://huggingface.co/datasets/NLBSE/nlbse25-code-comment-classification) with the competition data. The Dataset has six splits two (train and test) per language. Each row represents a sentence (aka an instance), and each sentence contains five columns as follows:
- `class` is the class name referring to the source code file where the sentence comes from;
- `comment_sentence` is the actual sentence string, which is part of a (multi-line) class comment;
- `partition` is the dataset split in training and testing; `0` identifies training instances, and `1` identifies testing instances, respectively;
- `combo` is the class name appended to the sentence string, used to train the baselines; 
- `labels` is the ground-truth category, it is a binary list that a single sample belongs to. Each sample belongs one or more categories. 

The list of labels is defined for each of as follows:
- `java`: [`summary`, `Ownership`, `Expand`, `usage`, `Pointer`, `deprecation`, `rational`],
- `python`: [`Usage`, `Parameters`, `DevelopmentNotes`, `Expand`, `Summary`],
- `pharo`: [`Keyimplementationpoints`, `Example`, `Responsibilities`, `Classreferences`, `Intent`, `Keymessages`, `Collaborators`]


## Dataset Preparation

- **Preprocessing**. Before splitting, the manually tagged class comments were preprocessed as follows:
    - changed the sentences to lowercase, reduced multiple line endings to one, and removed special characters except for  `a-z0-9,.@#&^%!? \n`  since different languages can have different meanings for the symbols. For example, `$,:{}!!` are markup symbols in Pharo, while in Java it is `‘/* */ <p>`, and `#,`  in Python. For simplicity reasons, we removed all such special character meanings.
    - replaced periods in numbers and in Latin contractions such as `e.g.`, `i.e.`, `etc.`, so that comment sentences are not split incorrectly. 
    - removed extra whitespace before and after comments or lines. 

- **Splitting sentences**:
    - Since the classification is sentence-based, we split the comments into sentences. 
    - We use the [NEON](https://github.com/adisorbo/NEON_tool) tool to split the text into sentences. It splits the sentences based on selected characters `(\\n|:)`. This is another reason to remove some of the special characters to avoid unnecessary splitting. 
    - Note: the sentences may not be complete. Sometimes, the annotators classify a relevant phrase of a sentence into a category. 

- **Partition selection**:
    - After splitting comments into  sentences, we split the sentence dataset in an 80/20 training-testing split. 
    - The partitions are determined based on an algorithm in which we first determine the stratum of each class comment. The [original paper](https://www.sciencedirect.com/science/article/pii/S0164121221001448) by _Rani et al._ gives more details on strata distribution. 
    - Then, we follow a round-robin approach to fill training and testing partitions from the strata. We select a stratum, select the category with a minimum number of instances in it to achieve the best balancing and assign it to the train or test partition based on the required proportions. 

## Software Projects

We extracted the class comments from selected projects into a joint [dataset](https://doi.org/10.5281/zenodo.4311839) available on Zenodo.

| Language | Project | Project Homepage |
|-|-|-|
| Java | Eclipse | [github.com/eclipse](https://github.com/eclipse) |
| Java | Guava   | [github.com/google/guava](https://github.com/google/guava) |
| Java | Guice   | [github.com/google/guice](https://github.com/google/guice) |
| Java | Hadoop  | [github.com/apache/hadoop](https://github.com/apache/hadoop) |
| Java | Spark   | [github.com/apache/spark](https://github.com/apache/spark) |
| Java | Vaadin  | [github.com/vaadin/framework](https://github.com/vaadin/framework) |
| Pharo | GToolkit    | [github.com/feenkcom/gtoolkit](https://github.com/feenkcom/gtoolkit) |
| Pharo | Moose       | [github.com/moosetechnology/Moose](https://github.com/moosetechnology/Moose) |
| Pharo | PetitParser | [github.com/moosetechnology/PetitParser](https://github.com/moosetechnology/PetitParser) |
| Pharo | Pillar      | [github.com/pillar-markup/pillar](https://github.com/pillar-markup/pillar) |
| Pharo | PolyMath    | [github.com/PolyMathOrg/PolyMath](https://github.com/PolyMathOrg/PolyMath) |
| Pharo | Roassal2    | [github.com/ObjectProfile/Roassal2](https://github.com/ObjectProfile/Roassal2) |
| Pharo | Seaside     | [github.com/SeasideSt/Seaside](https://github.com/SeasideSt/Seaside) |
| Python | Django   | [github.com/django](https://github.com/django) |
| Python | IPython  | [github.com/ipython/ipython](https://github.com/ipython/ipython) |
| Python | Mailpile | [github.com/mailpile/Mailpile](https://github.com/mailpile/Mailpile) |
| Python | Pandas   | [github.com/pandas-dev/pandas](https://github.com/pandas-dev/pandas) |
| Python | Pipenv   | [github.com/pypa/pipenv](https://github.com/pypa/pipenv) |
| Python | Pytorch  | [github.com/pytorch/pytorch](https://github.com/pytorch/pytorch) |
| Python | Requests | [github.com/psf/requests/](https://github.com/psf/requests/) |

## Baseline Results

|    | Language    | Category                     |   precision |   recall |       f1 |
|---:|:-------|:------------------------|------------:|---------:|---------:|
|  0 | java   | summary                 |    0.873385 | 0.829448 | 0.85085  |
|  1 | java   | Ownership               |    1        | 1        | 1        |
|  2 | java   | Expand                  |    0.323529 | 0.444444 | 0.374468 |
|  3 | java   | usage                   |    0.911043 | 0.818182 | 0.862119 |
|  4 | java   | Pointer                 |    0.738255 | 0.940171 | 0.827068 |
|  5 | java   | deprecation             |    0.818182 | 0.6      | 0.692308 |
|  6 | java   | rational                |    0.162162 | 0.295082 | 0.209302 |
|  7 | python | Usage                   |    0.700787 | 0.735537 | 0.717742 |
|  8 | python | Parameters              |    0.793893 | 0.8125   | 0.803089 |
|  9 | python | DevelopmentNotes        |    0.243902 | 0.487805 | 0.325203 |
| 10 | python | Expand                  |    0.433628 | 0.765625 | 0.553672 |
| 11 | python | Summary                 |    0.648649 | 0.585366 | 0.615385 |
| 12 | pharo  | Keyimplementationpoints |    0.636364 | 0.651163 | 0.643678 |
| 13 | pharo  | Example                 |    0.872881 | 0.903509 | 0.887931 |
| 14 | pharo  | Responsibilities        |    0.596154 | 0.596154 | 0.596154 |
| 15 | pharo  | Classreferences         |    0.2      | 0.5      | 0.285714 |
| 16 | pharo  | Intent                  |    0.71875  | 0.766667 | 0.741935 |
| 17 | pharo  | Keymessages             |    0.68     | 0.790698 | 0.731183 |
| 18 | pharo  | Collaborators           |    0.26087  | 0.6      | 0.363636 |

We trained and tested 3 multi-class classifiers (one for each language) based on [Al-Kaswan et al.](https://arxiv.org/abs/2302.13681) on the provided training and test sets. The models are available on the [HuggingFace Hub](https://huggingface.co/collections/NLBSE/nlbse25-code-comment-classification-competition-670914985f4ed2ff1f0ddb03).

The summary of the baseline results is provided in `baseline_results_summary.csv`.

We provide a notebook to train our baseline classifiers and to run the evaluations [locally](SetFit_baseline.ipynb) and on [Google Colab](https://colab.research.google.com/drive/1RULzasjO_nrqiXLrGze-PznFlHKtGQ4s?usp=sharing) (note that the final evaluation and score calculation needs to be performed on Google Colab).
