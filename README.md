This repository contains the code for the paper "Find the Funding: Entity Linking with Incomplete Funding Knowledge Bases".
Authors:
* Gizem Aydin, Radboud University
* Seyed Amin Tabatabaei, Elsevier B.V.
* Georgios Tsatsaronis, Elsevier B.V.
* Faegheh Hasibi, Radboud University

## Domain Adaptation of BERT, $BERT_{TAPT}$
Run the notebook `Domain Adaptation of BERT/BERT_TAPT.ipynb`. 

## Named Entity Recognition for Funding Organizations, $BERT_{TAPT}^{MD}$

1. Training $BERT_{TAPT}^{MD}$:
    * Run `Named Entity Recognition/Train_BERT_TAPT_MD.ipynb`.
2. Obtain predictions:
    * Run `Named Entity Recognition/NER_Predictions.ipynb`.
3. Evaluate the results:
    * Run `Named Entity Recognition/Evaluate_NER.ipynb`.

In the first cell of each notebook, you may find the instructions on how to provide the inputs. After the inputs are provided, you can run the code from top to bottom. The code also has support for grant mentions if they are available.

## Entiy Disambiguation for Funding Organizations, FunD

1. Training FunD:
    * Training of the Biencoder.
        1. In the first round, train with random negatives:
            *  Run `Entity Disambiguation/BiEncoder RandomNegative Training.ipynb`.
        2. The rest of the training will be with both hard and random negatives. For this purpose, the predictions on Training and Val sets should be obtained:
            * Compute entity embeddings: Run `Entity Disambiguation/Compute Entity Embeddings.ipynb`.
            * Run the notebook `Entity Disambiguation/Hard Negative Mining.ipynb` twice. Once for Training set, and a second time for Val set.
            * Run the notebook `Entity Disambiguation/Number Random Negatives.ipynb` to see how many random negatives per mention will be sampled for the next round. This notebook also shows some statistics on the number of hard negatives found.
        3. Train with hard negatives:
            *  Run `Entity Disambiguation/BiEncoder HardNegative Training.ipynb`.
        4. Repeat steps (2-3) for the amount of hard negative training rounds. In the original research, these steps are repeated 3 times.
    * Training of GBM$_{F5}$:
        1. Run the notebook `Entity Disambiguation/Prediction with Biencoder.ipynb` twice. Once for Traning set, and a second time for the Val set. This notebook retrieves the candidate entities for these datasets, which are later used for training.
        2. Run the notebook `Entity Disambiguation/Train GBM F5.ipynb`.
2. Obtain Predictions:
    * Run `Entity Disambiguation/Neural Entity Disambiguation Predictions.ipynb`. 
3. Evaluate the results:
    * Run `Entity Disambiguation/Evalute ED Model.ipynb`. 

## Evaluation for Entity Linking

1. Obtain end-to-end predictions:
    * Run `Entity Linking/Neural Entity Linking Predictions.ipynb`.
2. Evaluate the results:
    * Code for evaluation: 
        * Import `Evaluate_End2End` function from one of the following files, depending on the evaluation mode: 
            * `EvaluationPoolStrict.py`: Strict matching, ``Normal`` setting.
            * `EvaluationPoolStrictEE.py`: Strict matching, ``EE`` setting.
            * `EvaluationPoolStrictInKB.py`: Strict matching, ``InKB`` setting.
        * Usage:
            * Inputs:
                * `all_gold_ann`: List of lists. The length of the main list is equal to the number of documents. For each document, a list stores the correct annotations. In this list, each annotation is indicated with another list of 3 elements. The first element is the start index of mention, the second element is the length of the mention, and the third element is the correct entity ID. Example correct annotation list for a document:
                ```python
                [
                    [5,10,"Entity_A"],
                    [25,3,None]
                ]
                ```
                According to the example, there are two mentions. One starts at character index 5 and has a length of 10. The correct link for this mention is `"Entity_A"`. The other mention starts at index 25 and has a length of 3. This is a NIL mention.
                * `all_preds`: Similar to `all_gold_ann`. Only difference is that it contains the predicted annotations instead of the gold ones.
                * `entity_pool`: A dictionary where keys are entity IDs and the values are Python sets only containing those entity IDs.
            * Output: Prints Micro and Macro averaged Precision, Recall and F1 scores. Returns Micro averaged ones.

## Libraries and Versions
Python==3.7.9
* [annoy](https://github.com/spotify/annoy)==1.17.0
* [datasets](https://pypi.org/project/datasets/)==1.4.1
* [fuzzywuzzy](https://github.com/seatgeek/fuzzywuzzy)==0.18.0
* [lightgbm](https://github.com/microsoft/LightGBM)==2.3.0
* [numpy](https://numpy.org/)==1.19.2
* [pandas](https://pandas.pydata.org/)==1.1.3
* [scipy](https://www.scipy.org/)==1.5.2
* [seqeval](https://github.com/chakki-works/seqeval)==1.2.2
* [torch](https://pytorch.org/)==1.7.1
* [tqdm](https://github.com/tqdm/tqdm)==4.49.0
* [transformers](https://huggingface.co/transformers/)==3.5.1