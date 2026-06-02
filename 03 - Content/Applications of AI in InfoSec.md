---
creation date: June 2nd 2026
last modified date: June 2nd 2026
aliases: []
tags:
---

Primary Categories: { Add link(s) [[]] back to related PRIMARY categories }
Secondary Categories:  { Add link(s) [[]] back to related SECONDARY categories }
Links: {Add link(s) [[]] to related terms}
Search Tag: #📕  

# [[Applications of AI in InfoSec]]  
___

## Description:  

# Applications of AI in InfoSec — Practical ML Notes

## Big Picture

Most of the module is the same machine learning workflow repeated across different security use cases.

The basic flow is:

```text
Raw Data → Feature Extraction → Model Training → Prediction → Evaluation → Save Model
```

A model does not understand raw text, packets, executables, or logs directly. The data has to be converted into features first.

## Core Terms

### Feature

A measurable input used by the model.

Examples:

- Words in an email
    
- Packet count
    
- Byte count
    
- Flow duration
    
- API call counts
    
- File size
    
- Entropy
    
- Number of failed logins
    

### Label

The answer the model is trying to learn.

Examples:

- `spam` or `not spam`
    
- `1` positive or `0` negative
    
- `malicious` or `benign`
    
- `normal` or `anomaly`
    

### X and y

In sklearn examples:

```python
X = input features
y = labels / answers
```

Example:

```python
X_train = train["text"]
y_train = train["label"]
```

## Classification

Classification means the model predicts a category.

Examples:

- Spam vs not spam
    
- Positive vs negative sentiment
    
- Malware vs benign
    
- Normal vs malicious traffic
    

General classification flow:

```text
Input → Features → Classifier → Label
```

## Text Classification

Text must be converted into numbers before a model can use it.

A common method is TF-IDF.

TF-IDF gives higher importance to words that are useful for distinguishing documents.

Examples:

- Spam classifier: email text → spam / ham
    
- IMDB classifier: review text → positive / negative
    
- Phishing classifier: message text → phishing / benign
    

Reusable text classifier:

```python
import pandas as pd
import joblib

from sklearn.pipeline import Pipeline
from sklearn.feature_extraction.text import TfidfVectorizer
from sklearn.linear_model import LogisticRegression
from sklearn.metrics import accuracy_score, classification_report

train = pd.read_json("train.json")
test = pd.read_json("test.json")

X_train = train["text"]
y_train = train["label"]

X_test = test["text"]
y_test = test["label"]

model = Pipeline([
    ("vectorizer", TfidfVectorizer(
        lowercase=True,
        ngram_range=(1, 2),
        max_features=100000,
        sublinear_tf=True
    )),
    ("classifier", LogisticRegression(
        max_iter=2000,
        C=2.0
    ))
])

model.fit(X_train, y_train)

predictions = model.predict(X_test)

print("Accuracy:", accuracy_score(y_test, predictions))
print(classification_report(y_test, predictions))

joblib.dump(model, "model.joblib", protocol=4)
```

Important: saving the full pipeline lets the model accept raw text directly.

## Numeric Classification

For tabular security data, the input is usually rows and columns.

Examples:

```text
packet_count, byte_count, duration, failed_logins, label
```

Reusable numeric classifier:

```python
import pandas as pd
import joblib

from sklearn.pipeline import Pipeline
from sklearn.preprocessing import StandardScaler
from sklearn.ensemble import RandomForestClassifier
from sklearn.metrics import accuracy_score, classification_report

train = pd.read_csv("train.csv")
test = pd.read_csv("test.csv")

X_train = train.drop(columns=["label"])
y_train = train["label"]

X_test = test.drop(columns=["label"])
y_test = test["label"]

model = Pipeline([
    ("scaler", StandardScaler()),
    ("classifier", RandomForestClassifier(
        n_estimators=200,
        random_state=42
    ))
])

model.fit(X_train, y_train)

predictions = model.predict(X_test)

print("Accuracy:", accuracy_score(y_test, predictions))
print(classification_report(y_test, predictions))

joblib.dump(model, "model.joblib", protocol=4)
```

## Anomaly Detection

Anomaly detection is used when I may not have labels for malicious activity.

Instead of learning:

```text
good vs bad
```

the model learns:

```text
normal behavior
```

Then it flags things that are unusual.

Common model:

```python
IsolationForest
```

Reusable anomaly detection template:

```python
import pandas as pd
import joblib

from sklearn.pipeline import Pipeline
from sklearn.preprocessing import StandardScaler
from sklearn.ensemble import IsolationForest

data = pd.read_csv("network_logs.csv")

X = data.drop(columns=["label"], errors="ignore")

model = Pipeline([
    ("scaler", StandardScaler()),
    ("detector", IsolationForest(
        contamination=0.05,
        random_state=42
    ))
])

model.fit(X)

predictions = model.predict(X)

# 1 = normal
# -1 = anomaly
data["prediction"] = predictions

print(data["prediction"].value_counts())

joblib.dump(model, "anomaly_model.joblib", protocol=4)
```

## Malware Classification

Malware classification is usually just tabular classification.

Possible features:

- File size
    
- Entropy
    
- Imported DLLs
    
- API calls
    
- Section count
    
- Strings count
    
- Permissions
    
- PE header values
    

The model does not understand the executable directly unless features are extracted first.

General flow:

```text
Executable → Feature Extraction → Classifier → Benign/Malicious
```

## Evaluation Metrics

### Accuracy

Overall percent correct.

Good when classes are balanced.

### Precision

Of everything predicted malicious, how many were actually malicious?

High precision means fewer false positives.

### Recall

Of everything actually malicious, how many did the model catch?

High recall means fewer false negatives.

### F1-score

Balances precision and recall.

Useful when I care about both false positives and false negatives.

### Confusion Matrix

Shows correct and incorrect predictions by class.

For binary classification:

```text
True Negative   False Positive
False Negative  True Positive
```

## Saving Models

HTB often asks for a `.joblib` file.

Use:

```python
import joblib

joblib.dump(model, "model.joblib", protocol=4)
```

If preprocessing is needed, save the whole pipeline, not just the classifier.

Good:

```python
Pipeline([
    ("vectorizer", TfidfVectorizer()),
    ("classifier", LogisticRegression())
])
```

Bad:

```python
joblib.dump(classifier_only, "model.joblib")
```

The classifier alone may not know how to handle raw input.

## Mental Checklist for Future Assessments

1. What is the input?
    
    - Text?
        
    - Numeric rows?
        
    - Files?
        
    - Logs?
        
2. What is the label?
    
    - Spam/not spam?
        
    - Positive/negative?
        
    - Benign/malicious?
        
    - Normal/anomaly?
        
3. What is `X`?
    
    - The input features.
        
4. What is `y`?
    
    - The answer column.
        
5. What preprocessing is needed?
    
    - Text → TF-IDF
        
    - Numeric → scaling
        
    - Categorical → encoding
        
6. What model makes sense?
    
    - Text classification → Logistic Regression
        
    - Tabular classification → Random Forest
        
    - Anomaly detection → Isolation Forest
        
7. How do I evaluate it?
    
    - Accuracy
        
    - Classification report
        
    - Confusion matrix
        
8. How do I save it?
    
    - `joblib.dump(model, "model.joblib", protocol=4)`
        

## Key Takeaway

The IMDB assessment was the spam classifier again.

Spam:

```text
Email text → TF-IDF → Classifier → spam/ham
```

IMDB:

```text
Review text → TF-IDF → Classifier → positive/negative
```

Same machine learning pattern. Different dataset.


___

## Resources:

| Hyperlink | Info |
| --------- | ---- |


Created Date: June 2nd 2026 (07:04 pm)
Last Modified Date: June 2nd 2026 (07:04 pm)
