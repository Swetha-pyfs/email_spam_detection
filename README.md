# Email Spam Detection using Machine Learning

A machine learning project that classifies text messages as **spam** or **ham** (not spam) using Natural Language Processing (NLP) and a Random Forest classifier, with a simple Flask web interface for real-time predictions.

---

## Table of Contents

- [Overview](#overview)
- [Features](#features)
- [Tech Stack](#tech-stack)
- [Dataset](#dataset)
- [Project Workflow](#project-workflow)
- [Model Performance](#model-performance)
- [Project Structure](#project-structure)
- [Installation](#installation)
- [Usage](#usage)
- [How the Web App Works](#how-the-web-app-works)
- [Limitations and Future Improvements](#limitations-and-future-improvements)
- [Author](#author)

---

## Overview

Spam messages waste time and are often used for scams and phishing. This project builds a model that reads a message and predicts whether it is spam or not. The trained model is served through a Flask web app, where a user can paste a message and instantly see the result with a confidence percentage.

## Features

- Text preprocessing with NLTK (stopword removal and stemming)
- Bag of Words feature extraction with 4000 features
- Comparison of two classifiers: Random Forest and Decision Tree
- Model evaluation with accuracy, confusion matrix and classification report
- Trained model and vectorizer saved with Pickle
- Clean, responsive Flask web interface

## Tech Stack

| Area | Tools |
|---|---|
| Language | Python |
| NLP | NLTK |
| Machine learning | Scikit-learn |
| Data handling and plots | Pandas, NumPy, Matplotlib |
| Web app | Flask, HTML, CSS, JavaScript |
| Development | Jupyter Notebook |

## Dataset

The project uses the **SMS Spam Collection** dataset (`spam.csv`).

| Class | Messages | Share |
|---|---|---|
| Ham | 4825 | 86.6% |
| Spam | 747 | 13.4% |
| **Total** | **5572** | |

- The dataset is **moderately imbalanced**, roughly 6.5 ham messages for every spam message.
- After removing duplicate records, 5169 messages remain (4516 ham and 653 spam, about 87:13).
- The file contains special characters, so it must be read with `encoding="ISO-8859-1"`.
- Only two columns are used: `v1` (label) and `v2` (message), renamed to `label` and `message`.

## Project Workflow

```
Load data -> Clean text -> Bag of Words -> Split data -> Train models -> Evaluate -> Save model -> Flask app
```

1. **Load and prepare the data:** read `spam.csv` and keep the label and message columns.
2. **Clean the text:** remove non-letter characters, convert to lowercase, remove English stopwords and apply Porter stemming (for example, "winning" becomes "win").
3. **Vectorize:** convert the cleaned text to numbers with `CountVectorizer(max_features=4000)`.
4. **Encode labels:** spam is 1 and ham is 0.
5. **Split:** 80% training and 20% testing (`random_state=42`).
6. **Train:** Random Forest and Decision Tree classifiers.
7. **Evaluate:** accuracy, confusion matrix, precision, recall and F1-score.
8. **Save:** the chosen model (`RFC.pkl`) and the vectorizer (`cv.pkl`) with Pickle.

## Model Performance

Results on the 20% test set (1115 messages):

| Model | Accuracy |
|---|---|
| **Random Forest** | **97.58%** |
| Decision Tree | 96.77% |

**Random Forest classification report**

| Class | Precision | Recall | F1-score |
|---|---|---|---|
| Ham | 0.97 | 1.00 | 0.99 |
| Spam | 0.99 | 0.83 | 0.90 |

**Confusion matrix (Random Forest)**

|  | Predicted ham | Predicted spam |
|---|---|---|
| **Actual ham** | 964 | 1 |
| **Actual spam** | 26 | 124 |

**Final model: Random Forest.** It had the highest accuracy and a spam precision of 0.99, meaning it almost never marks a genuine message as spam.

> Because the dataset is imbalanced, accuracy alone can be misleading. A model that always predicts "ham" would already score about 87%. For that reason, spam precision and recall are reported as well. Random Forest results can vary slightly between runs unless `random_state` is set.

## Project Structure

```
email-spam-detection/
├── app.py                          # Flask backend
├── EMAIL_SPAM_DETECTION.ipynb      # Training and evaluation notebook
├── spam.csv                        # Dataset
├── RFC.pkl                         # Trained Random Forest model
├── cv.pkl                          # Saved CountVectorizer
├── templates/
│   └── index.html                  # Web interface
└── README.md
```

## Installation

1. **Clone the repository**
   ```bash
   git clone https://github.com/Swetha-pyfs/email-spam-detection.git
   cd email_spam_detection
   ```

2. **Create a virtual environment (recommended)**
   ```bash
   python -m venv .venv
   # Windows
   .venv\Scripts\activate
   # macOS / Linux
   source .venv/bin/activate
   ```

3. **Install the dependencies**
   ```bash
   pip install nltk pandas numpy matplotlib scikit-learn flask
   ```

## Usage

### 1. Train the model (optional)

Open the notebook and run all cells. This creates `RFC.pkl` and `cv.pkl`.

```bash
jupyter notebook EMAIL_SPAM_DETECTION.ipynb
```

> Always save the model and the vectorizer from the **same training run**. A model paired with a different vectorizer gives wrong predictions.

### 2. Run the web app

```bash
python app.py
```

Then open **http://127.0.0.1:5000** in your browser, paste a message and click **Check Message**.

### Example messages

| Message | Expected result |
|---|---|
| `Congratulations! You have won a FREE prize. Call now to claim your reward!` | Spam |
| `Hey, are we still meeting for lunch tomorrow?` | Ham |

## How the Web App Works

1. The user enters a message in the web page.
2. JavaScript sends it to the Flask route `/predict` as JSON.
3. Flask applies the same cleaning steps used in training.
4. The saved vectorizer (`cv.pkl`) converts the message into 4000 features.
5. The saved model (`RFC.pkl`) predicts spam or ham and returns a confidence score.
6. The page shows the result.

## Limitations and Future Improvements

- The model is trained on an older SMS dataset, so modern phishing messages may be missed.
- Bag of Words counts words but does not understand meaning or context.
- Spam recall (0.83) can be improved for the imbalanced data with a stratified split, `class_weight='balanced'` or resampling techniques such as SMOTE.
- Possible upgrades: TF-IDF features, additional models (for example Naive Bayes), and deep learning approaches.

## Author

**Your Name**
GitHub: [Swetha](https://github.com/Swetha-pyfs)
