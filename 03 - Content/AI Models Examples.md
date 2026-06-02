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

# [[AI Models Examples]]  
___
environment JupyterLab Setup
This is good when you want to experiment cell-by-cell.

### Create the environment

```
conda create -n htb-ai python=3.10 -y
conda activate htb-ai
```

### Install the packages

Use `python -m pip` so packages install into the active environment.

```
python -m pip install --upgrade pip  
python -m pip install pandas scikit-learn==1.3.2 joblib jupyterlab ipykernel
```

### Add this environment as a Jupyter kernel

```
python -m ipykernel install --user --name htb-ai --display-name "Python (htb-ai)"
```

### Launch JupyterLab

```
jupyter lab
```

Inside JupyterLab, select the kernel:

```
Python (htb-ai)
```

Then run this in the first cell to verify it is using the right environment:

```
import sys  
import sklearn  
import joblib  
  
print(sys.executable)  
print(sklearn.__version__)  
print(joblib.__version__)
```

You should see something like:

```
...\miniconda3\envs\htb-ai\python.exe
```

The important part is:

```
envs\htb-ai
```

## Option 2: Easier setup — plain Python script



Make a folder like:

```
new-model/  
│  
├── train.json  
├── test.json  
└── train_model.py
```

Then in PowerShell:

```
conda activate htb-ai  
cd path\to\imdb-assessment  
python train_model.py
```

No Jupyter confusion. No kernel weirdness. It just runs the file.
## Description:  

Ask yourself:

**1. Is the input text?**

Use:

```
TfidfVectorizer + LogisticRegression
```

**2. Is the input numeric rows and you have labels?**

Use:

```
StandardScaler + RandomForestClassifier
```

**3. Is the input numeric rows but you only know what normal looks like?**

Use:

```
StandardScaler + IsolationForest
```

**4. Does the challenge ask for a `.joblib`?**

Save the whole pipeline:

```
joblib.dump(model, "model.joblib", protocol=4)
```

Not just part of it.

## Reusable template 1: Text classification

Use this for:

- spam
- phishing emails
- movie review sentiment
- toxic text
- ticket classification
- alert descriptions

```
import pandas as pd
import joblib

from sklearn.pipeline import Pipeline
from sklearn.feature_extraction.text import TfidfVectorizer
from sklearn.linear_model import LogisticRegression
from sklearn.metrics import accuracy_score, classification_report, confusion_matrix

# Load data
train = pd.read_json("train.json")
test = pd.read_json("test.json")

# Pick input and label columns
X_train = train["text"]
y_train = train["label"]

X_test = test["text"]
y_test = test["label"]

# Text model pipeline
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

# Train
model.fit(X_train, y_train)

# Evaluate
predictions = model.predict(X_test)

print("Accuracy:", accuracy_score(y_test, predictions))
print(classification_report(y_test, predictions))
print(confusion_matrix(y_test, predictions))

# Save
joblib.dump(model, "model.joblib", protocol=4)
```

The important part is this:

```
Pipeline([
    ("vectorizer", TfidfVectorizer()),
    ("classifier", LogisticRegression())
])
```

That means the saved model can accept raw text directly.

You can test it by creating another file called test_model.py
```
import joblib

model = joblib.load("model.joblib")

reviews = [
    "This movie was amazing. I loved the acting and the story.",
    "This movie was terrible. It was boring and way too long.",
    "The film was okay, not great but not awful either."
]

predictions = model.predict(reviews)

for review, prediction in zip(reviews, predictions):
    label = "Positive" if prediction == 1 else "Negative"

    print("Review:", review)
    print("Prediction:", prediction, "-", label)
    print()
```

The run it in powershell: `python test_model.py`


## Reusable template 2: Normal classification with numeric data

Use this for:

- malware classification with numeric features
- network traffic classification
- login risk scoring
- suspicious behavior classification

Example CSV:

```
packet_count,byte_count,duration,label
100,5000,3.2,0
9000,200000,40.5,1
```

Code:

```
import pandas as pd
import joblib

from sklearn.pipeline import Pipeline
from sklearn.preprocessing import StandardScaler
from sklearn.ensemble import RandomForestClassifier
from sklearn.metrics import accuracy_score, classification_report

train = pd.read_csv("train.csv")
test = pd.read_csv("test.csv")

# Everything except label is input
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

For numeric data, the big thing is:

```
X = all columns except label
y = label column
```

Python to test the model
```
import pandas as pd
import joblib

model = joblib.load("model.joblib")

sample = pd.DataFrame([
    {
        "duration": 10.5,
        "packet_count": 300,
        "byte_count": 15000
    }
])

prediction = model.predict(sample)

print(prediction)
```

## Reusable template 3: Anomaly detection

Use this when you may not have good malicious labels.

Instead of teaching the model:

```
this is bad
this is good
```

you teach it:

```
this is what normal usually looks like
```

Then it flags weird stuff.

```
import pandas as pd
import joblib

from sklearn.pipeline import Pipeline
from sklearn.preprocessing import StandardScaler
from sklearn.ensemble import IsolationForest

data = pd.read_csv("network_logs.csv")

# Use only feature columns
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

# IsolationForest returns:
#  1 = normal
# -1 = anomaly
data["prediction"] = predictions

print(data["prediction"].value_counts())

joblib.dump(model, "anomaly_model.joblib", protocol=4)
```

This is different from normal classification because there might not be a real `y_train`.

Test it with this
```
import pandas as pd
import joblib

model = joblib.load("model.joblib")

sample = pd.DataFrame([
    {
        "duration": 10.5,
        "packet_count": 300,
        "byte_count": 15000
    }
])

prediction = model.predict(sample)

print(prediction)
```

___

## Resources:

| Hyperlink | Info |
| --------- | ---- |


Created Date: June 2nd 2026 (07:08 pm)
Last Modified Date: June 2nd 2026 (07:08 pm)
