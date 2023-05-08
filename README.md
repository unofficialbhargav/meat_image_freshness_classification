Meat Freshness Prediction <!-- omit from toc -->
==============================
- [Summary](#summary)
- [Model Prediction Results](#model-prediction-results)
- [Model Evaluation](#model-evaluation)
  - [Misclassification Cost (MCC) Matrix](#misclassification-cost-mcc-matrix)
  - [Evaluation on (MCC)](#evaluation-on-mcc)
- [Project Organization](#project-organization)
- [Team Composition](#team-composition)


Summary
------------
i. Trained ResNet models to detect rotting meat in supermarkets based on their image. Used the expected value framework to determine which model will be the best for deployment base on the cost of misclassifying items

ii. Experimented with Image Segmentation (UNet) and a Dense Net model to classify images based on the image segments detected by UNet model and passing these as input features to the Dense Net model. 

Full details published in [arXiv](https://arxiv.org/abs/2305.00986).

Model Prediction Results
------------
FE: Feature Extraction, FT: Fine-tuning<br>
Calculated on test data
| Model        |   Accuracy |  Precision |     Recall |
| :----------- | ---------: | ---------: | ---------: |
| ResNet 18-FE |     82.71% |     84.66% |    83.71% |
| ResNet 18-FT | **93.13%** | **93.97%** | **92.85%** |
| ResNet 50-FE |     88.03% |     88.35% |     88.86% |
| ResNet 50-FT |     84.70% |     84.50% |     86.14% |


Model Evaluation 
------------
### Misclassification Cost (MCC) Matrix ###

|                      | True Spoiled | True Half-Fresh | True Fresh |
| -------------------- | :----------: | :-------------: | :--------: |
| Predicted Spoiled    |      0       |      $4.50      |   $9.00    |
| Predicted Half-Fresh |   $499.75    |        0        |   $4.00    |
| Predicted Fresh      |    $99.90    |      $3.50      |     0      |

### Evaluation on (MCC) ###
| Model        |   Accuracy |      MCC |
| ------------ | ---------: | -------: |
| ResNet 18-FE |     82.71% |     $886 |
| ResNet 18-FT | **93.13%** |   $5,076 |
| ResNet 50-FE |     88.03% | **$242** |
| ResNet 50-FT |     84.70% |     $316 |

Best model to deploy is ResNet 50-FE

### Experiment Results on UNet and Dense Net ###
| Accuracy     |  Precision |   Recall |
| ------------ | ---------: | -------: |
| 35.35%       |  100%      |   35.25% |

Unfortunately this combination did not yield a good performance and the resulting MCC was $89,411.

Project Organization
------------

    ├── LICENSE
    ├── Makefile           <- Makefile with commands like `make data` or `make train`
    ├── README.md          <- The top-level README for developers using this project.
    ├── data
    │   ├── external       <- Data from third party sources.
    │   ├── interim        <- Intermediate data that has been transformed.
    │   ├── processed      <- The final, canonical data sets for modeling.
    │   └── raw            <- The original, immutable data dump.
    │
    ├── docs               <- A default Sphinx project; see sphinx-doc.org for details
    │
    ├── flask_demo         <- A web demo for this project created using flask
    │   ├── models         <- Contains resnet 50-fe model binary
    │   ├── screenshots    <- Screenshots of demo web pages
    │   ├── static         <- css and static resources
    │   └── Procfile       <- Process file to run flask server
    │   └── README.md      <- Description of flask demo
    │   └── app.yaml       <- Config file for environment setup
    │   └── main.py        <- Main flask file for demo
    │   └── requirements.txt     <- Package installation
    │
    ├── models             <- (Empty)
    │
    ├── notebooks          <- Jupyter notebooks used for modelling, predictions and experiments
    │   └── 1.0-EDA-and-preprocessing.ipynb
    │   └── 2.0-image-segmentation.ipynb
    │   └── 3.0-resnet_model_evaluation.ipynb
    │   └── 4.0-resnet-LIME-Explanation.ipynb
    │
    ├── references         <- Data dictionaries, manuals, and all other explanatory materials.
    │
    ├── reports            <- Generated analysis as HTML, PDF, LaTeX, etc.
    │   └── figures        <- Generated graphics and figures to be used in reporting
    │
    ├── requirements.txt   <- The requirements file for reproducing the analysis environment, e.g.
    │                         generated with `pip freeze > requirements.txt`
    │
    ├── setup.py           <- makes project pip installable (pip install -e .) so src can be imported
    ├── src                <- Source code for use in this project.
    │   ├── __init__.py    <- Makes src a Python module
    │   │
    │   ├── data           <- Scripts to download or generate data
    │   │   └── make_dataset.py
    │   ├── evaluation           <- Scripts for model evaluation
    │   │   └── misclassification_cost.py
    │   │
    │   ├── features       <- Scripts to turn raw data into features for modeling
    │   │   └── build_features.py
    │   │
    │   ├── models         <- Scripts to train models and then use trained models to make
    │   │   │                 predictions
    │   │   └── train_resnet.py
    │   │   └── predict_resnet.py
    │   │   └── resnet_pipe.sh
    │   │   └── resnet_predict.sh 
    │   │
    │   └── utils  <- Scripts for utilitity functions
    │   │   └── manage_constants.py
    │   │
    │   └── visualization  <- Scripts to create exploratory and results oriented visualizations
    │
    └── tox.ini            <- tox file with settings for running tox; see tox.readthedocs.io


--------

Team Members 
------------
Sagiraju Bhargav <br>
Nathaniel Nartea Casanova <br>
Manan Lohia <br>
Lam Ivan Chuen Chun <br>
Toshinori Yoshiyasu <br>

All Rights Reserved.

<p><small>Project based on the <a target="_blank" href="https://drivendata.github.io/cookiecutter-data-science/">cookiecutter data science project template</a>. #cookiecutterdatascience</small></p>
