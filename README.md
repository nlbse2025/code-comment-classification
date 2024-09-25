## NLBSE'25 Tool Competition: Code Comment Classification

This repository contains the data and results for the baseline classifiers the [NLBSE’25 tool competition](https://nlbse2025.github.io/tools/) on code comment classification.

Participants of the competition must use the provided data to train/test their classifiers, which should outperform the baselines.

Details on how to participate in the competition are found [here](https://colab.research.google.com/drive/1GhpyzTYcRs8SGzOMH3Xb6rLfdFVUBN0P?usp=sharing).

## Contents of this package
---
- [NLBSE'25 Tool Competition: Code Comment Classification](#nlbse25-tool-competition-code-comment-classification)
- [Contents of this package](#contents-of-this-package)
- [Citing Related Work](#citing-related-work)
- [Folder structure](#folder-structure)
- [Data for classification](#data-for-classification)
- [Dataset Preparation](#dataset-preparation)
- [Software Projects](#software-projects)
- [Baseline Results](#baseline-results)

## Citing Related Work
Since you will be using our dataset (and possibly one of our notebooks) as well as the original work behind the dataset, please cite the following references in your paper:
```
@inproceedings{nlbse2025,
  author={Colavito, Giuseppe and Al-Kaswan, Ali and Stulova, Nataliia and Rani, Pooja},
  title={The NLBSE'25 Tool Competition},
  booktitle={Proceedings of The 4th International Workshop on Natural Language-based Software Engineering (NLBSE'25)},
  year={2025}
}
```
```
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
```
@inproceedings{pascarella2017,
  title={Classifying code comments in Java open-source software systems},
  author={Pascarella, Luca and Bacchelli, Alberto},
  booktitle={2017 IEEE/ACM 14th International Conference on Mining Software Repositories (MSR)},
  year={2017},
  organization={IEEE}
}
```

## Folder structure
- ### Java
    - `input`: The CSV files of the sentences for each category (within a training and testing split). **These are the main files used for classification**. See the format of these files below. 
    - `project_classes`: CSV files with the list of classes for each software project and corresponding code comments.
- ### Pharo
  Same structure as Java.
- ### Python 
  Same structure as Java.

## Data for classification

We provide a CSV file for each programming language (in the `input` folder) where each row represents a sentence (aka an instance) and each sentence contains six columns as follows:
- `comment_sentence_id` is the unique sentence ID;
- `class` is the class name referring to the source code file where the sentence comes from;
- `comment_sentence` is the actual sentence string, which is part of a (multi-line) class comment;
- `partition` is the dataset split in training and testing; 0 identifies training instances and 1 identifies testing instances, respectively;
- `instance_type` specifies if an instance actually belongs to the given category or not: 0 for negative and 1 for positive instances;
- `category` is the ground-truth or oracle category.


## Dataset Preparation

- **Preprocessing**. Before splitting, the manually tagged class comments were preprocessed as follows:
    - We changed the sentences to lowercase, reduced multiple line endings to one, and removed special characters except for  `a-z0-9,.@#&^%!? \n`  since different languages can have different meanings for the symbols. For example, `$,:{}!!` are markup symbols in Pharo, while in Java it is `‘/* */ <p>`, and `#,`  in Python. For simplicity reasons, we removed all such special character meanings.
    - We replaced periods in numbers, "e.g.", "i.e.", etc, so that comment sentences are not split incorrectly. 
    - We removed extra spaces before and after comments or lines. 

- **Splitting sentences**.
    - Since the classification is sentence-based, we split the comments into sentences. 
    - We use the NEON tool to split the text into sentences. It splits the sentences based on selected characters `(\\n|:)`. This is another reason to remove some of the special characters to avoid unnecessary splitting. 
    - Note: the sentences may not be complete. Sometimes, the annotators classify a relevant phrase a sentence into a category. 

- **Partition selection**.  
    - After splitting comments into  sentences, we split the sentence dataset in an 80/20 training-testing split. 
    - The partitions are determined based on an algorithm in which we first determine the stratum of each class comment. The original paper gives more details on strata distribution. 
    - Then, we follow a round-robin approach to fill training and testing partitions from the strata. We select a stratum, select the category with a minimum number of instances in it to achieve the best balancing and assign it to the train or test partition based on the required proportions. 

## Software Projects
We extracted the class comments from selected projects.

- ### Java 
     Details of six Java projects. 
    - Eclipse:  The version of the project referred to extracted class comments is available as [Raw Dataset](https://doi.org/10.5281/zenodo.4311839) on Zenodo. More detail about the project is available on GitHub [Eclipse](https://github.com/eclipse).
    
    - Guava: The version of the project referred to extracted class comments is available as [Raw Dataset](https://doi.org/10.5281/zenodo.4311839) on Zenodo. More detail about the project is available on GitHub [Guava](https://github.com/google/guava).
    
    - Guice: The version of the project referred to extracted class comments is available as [Raw Dataset](https://doi.org/10.5281/zenodo.4311839) on Zenodo. More detail about the project is available on GitHub [Guice](https://github.com/google/guice).
    
    - Hadoop:  The version of the project referred to extracted class comments is available as [Raw Dataset](https://doi.org/10.5281/zenodo.4311839) on Zenodo. More detail about the project is available on GitHub [Apache Hadoop](https://github.com/apache/hadoop)
    
    - Spark.csv: The version of the project referred to extracted class comments is available as [Raw Dataset](https://doi.org/10.5281/zenodo.4311839) on Zenodo. More detail about the project is available on GitHub [Apache Spark](https://github.com/apache/spark)
    
    - Vaadin: The version of the project referred to extracted class comments is available as [Raw Dataset](https://doi.org/10.5281/zenodo.4311839) on Zenodo. More detail about the project is available on GitHub [Vaadin](https://github.com/vaadin/framework)
   
- ### Pharo
     Contains the details of seven Pharo projects.     
    - GToolkit: The version of the project referred to extracted class comments is available as [Raw Dataset](https://doi.org/10.5281/zenodo.4311839) on Zenodo.  
     
    - Moose: The version of the project referred to extracted class comments is available as [Raw Dataset](https://doi.org/10.5281/zenodo.4311839) on Zenodo. 
     
    - PetitParser: The version of the project referred to extracted class comments is available as [Raw Dataset](https://doi.org/10.5281/zenodo.4311839) on Zenodo.
    
    - Pillar: The version of the project referred to extracted class comments is available as [Raw Dataset](https://doi.org/10.5281/zenodo.4311839) on Zenodo.
    
    - PolyMath: The version of the project referred to extracted class comments is available as [Raw Dataset](https://doi.org/10.5281/zenodo.4311839) on Zenodo.
    
    - Roassal2: The version of the project referred to extracted class comments is available as [Raw Dataset](https://doi.org/10.5281/zenodo.4311839) on Zenodo.
    
    - Seaside: The version of the project referred to extracted class comments is available as [Raw Dataset](https://doi.org/10.5281/zenodo.4311839) on Zenodo.

- ### Python
     Details of the extracted class comments of seven Python projects. 
    - Django: The version of the project referred to extract class comments is available as [Raw Dataset](https://doi.org/10.5281/zenodo.4311839) on Zenodo. More detail about the project is available on GitHub [Django](https://github.com/django)
    
    - IPython: The version of the project referred to extract class comments is available as [Raw Dataset](https://doi.org/10.5281/zenodo.4311839) on Zenodo. More detail about the project is available on GitHub[IPython](https://github.com/ipython/ipython)
    
    - Mailpile: The version of the project referred to extract class comments is available as [Raw Dataset](https://doi.org/10.5281/zenodo.4311839) on Zenodo. More detail about the project is available on GitHub [Mailpile](https://github.com/mailpile/Mailpile)
        
    - Pandas: The version of the project referred to extract class comments is available as [Raw Dataset](https://doi.org/10.5281/zenodo.4311839) on Zenodo. More detail about the project is available on GitHub [pandas](https://github.com/pandas-dev/pandas)
        
    - Pipenv: The version of the project referred to extract class comments is available as [Raw Dataset](https://doi.org/10.5281/zenodo.4311839) on Zenodo. More detail about the project is available on GitHub [Pipenv](https://github.com/pypa/pipenv)
        
    - Pytorch: The version of the project referred to extract class comments is available as [Raw Dataset](https://doi.org/10.5281/zenodo.4311839) on Zenodo. More detail about the project is available on GitHub [PyTorch](https://github.com/pytorch/pytorch)
        
    - Requests: The version of the project referred to extract class comments is available as [Raw Dataset](https://doi.org/10.5281/zenodo.4311839) on Zenodo. More detail about the project is available on GitHub [Requests](https://github.com/psf/requests/)

## Baseline Results
[To be updted]
We trained and tested 19 binary classifiers (one for each category) using the Sentence Transformer architecture on the provided training and test sets.

The baseline classifiers are coined as STACC and proposed by [Al-Kaswan et al.](https://arxiv.org/abs/2302.13681)

The summary of the baseline results is found in `baseline_results_summary.xlsx`.

We provide a notebook to [train our baseline classifiers](STACC_baseline.ipynb) and to [run the evaluations](https://colab.research.google.com/drive/1lvXuzdl_vSwMTCGIEfqTyQC1nzl22WCy?usp=sharing).
