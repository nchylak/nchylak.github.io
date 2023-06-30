---
layout: default
title: Measure of performance of classifiers
parent: Machine learning algorithms
grand_parent: Data science notes
permalink: /docs/data-science-notes/ml-algorithms/6performance_measure_classifiers/
nav_order: 6
---

# Measure of performance of classifiers

## Binary single-output classifiers

### Cross-validation

#### By hand

```python
from sklearn.model_selection import StratifiedKFold
from sklearn.base import clone

skfolds = StratifiedKFold(n_splits=3, random_state=42)
for train_index, test_index in skfolds.split(X_train, y_train_5):
clone_clf = clone(sgd_clf)
X_train_folds = X_train[train_index]
y_train_folds = (y_train_5[train_index])
X_test_fold = X_train[test_index]
y_test_fold = (y_train_5[test_index])

clone_clf.fit(X_train_folds, y_train_folds)
y_pred = clone_clf.predict(X_test_fold)
n_correct = sum(y_pred == y_test_fold)
print(n_correct / len(y_pred)) # prints 0.9502, 0.96565 and 0.96495
```

#### Using `cross_val_score()`

```python
from sklearn.model_selection import cross_val_score
cross_val_score(sgd_clf, X_train, y_train_5, cv=3, scoring="accuracy")

# >> array([ 0.9502 , 0.96565, 0.96495])
```

### Confusion matrix

```python
from sklearn.metrics import confusion_matrix
confusion_matrix(y_train, y_train_pred)
```

### Precision and recall

$$precision = \frac{TP}{TP+FP}$$

$$recall = \frac{TP}{TP+FN}$$

Precision and recall are usually looked at jointly (otherwise one could just make one good positive prediction and there would be no false negatives and therefore a precision of a 100%).

```python
from sklearn.metrics import precision_score, recall_score
precision_score(y_train, y_pred) # when predicting the positive class, it is correct only X% of the time
recall_score(y_train, y_train_pred) # it only detects X% of the positive class
```

### F1-score

It is often convenient to combine precision and recall into a single metric called the F1 score (harmonic mean of precision and recall), in particular if you need a simple way to compare two classifiers.

```python
from sklearn.metrics import f1_score
f1_score(y_train_5, y_pred)
```

This is when you do not care about precision more than recall or vice versa. You can’t have it both ways: increasing precision reduces recall, and vice versa. This is called the precision/recall tradeoff.

### Curves

#### Change the decision threshold

Instead of using the `predict()` function of scikit-learn, use the function `decision_function()` that return the score based on which the prediction is made.

```python
y_score = clf.decision_function(instance_of_X)
y_some_digit_pred = (y_score > custom_threshold)
```

How can you decide which threshold to use? For this you will first need to get the scores of all instances in the training set using the `cross_val_predict()` function.

```python
y_scores = cross_val_predict(sgd_clf, X_train, y_train, cv=3, method = "decision_function")
```

#### Precision-recall curve

Now with these scores you can compute precision and recall for all possible thresholds
using the `precision_recall_curve()` function.

```python
from sklearn.metrics import precision_recall_curve
precisions, recalls, thresholds = precision_recall_curve(y_train, y_scores)

def plot_precision_recall_vs_threshold(precisions, recalls, thresholds):
    plt.plot(thresholds, precisions[:-1], "b--", label="Precision")
    plt.plot(thresholds, recalls[:-1], "g-", label="Recall")
    plt.xlabel("Threshold")
    plt.legend(loc="upper left")
    plt.ylim([0, 1])
plot_precision_recall_vs_threshold(precisions, recalls, thresholds)
plt.show()
```

#### ROC curve

The ROC curve plots the true positive rate (another name for recall) against the false positive rate. The dotted line represents the ROC curve of a purely random classifier; a good classifier stays as far away from that line as possible (toward the top-left corner).

```python
from sklearn.metrics import roc_curve
fpr, tpr, thresholds = roc_curve(y_train_5, y_scores)

def plot_roc_curve(fpr, tpr, label=None):
    plt.plot(fpr, tpr, linewidth=2, label=label)
    plt.plot([0, 1], [0, 1], 'k--')
    plt.axis([0, 1, 0, 1])
    plt.xlabel('False Positive Rate')
    plt.ylabel('True Positive Rate')
plot_roc_curve(fpr, tpr)
plt.show()
```

One way to compare classifiers is to measure the area under the curve (AUC). A perfect
classifier will have a ROC AUC equal to 1, whereas a purely random classifier will
have a ROC AUC equal to 0.5.

```python
from sklearn.metrics import roc_auc_score
roc_auc_score(y_train_5, y_scores)
```

## Multilabel classifiers

Multilabel classifiers are classifiers outputting multiple binary labels for each instance, e.g. recognize the presence of multiple objects on a picture. The output is the therefore an array, such as [0,1,1].

### F1-score

```python
from sklearn.neighbors import KNeighborsClassifier

y_train_large = (y_train > = 7)
y_train_odd = (y_train % 2 = = 1)
y_multilabel = np.c_[ y_train_large, y_train_odd]

knn_clf = KNeighborsClassifier()
knn_clf.fit( X_train, y_multilabel)

y_train_knn_pred = cross_val_predict(knn_clf, X_train, y_multilabel, cv = 3)

f1_score( y_multilabel, y_train_knn_pred, average =" macro") # "macro" for the simple average across all labels (if they are equally important) or "weighted" for weighting in accordance with the number of instances of a label.
```

## Multiclass classifiers

Multiclass classifiers are classifiers outputting one multiclass (i.e. not binary, more than two options) label for each instance (e.g. good, bad, neutral).
