---
title: "Building an AI Spam Detector"
description: "In this project, I attempt to write a Python script to apply Machine Learning to detect spams emails. First, I will build the model and use a set of data to train the model. Then I will use a test file to test the results.
"
date: "2023-12-21"
headerImage: "https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcS95g-hyDxR6oSbYLhrnpZgNbsguS16HZXLng&s"
tags: ["AI", "Python"]
---

## Machine Learning Pipeline
This post also goes over Machine Learning Pipeline, which includes a series of steps involved in building and deploying a model. These steps are:
1. Data Collection
2. Data Processing
3. Train/Test split dataset
4. Model Training
5. Model Evaluation
6. Test Model

Before getting started with these processes, we need to import required libraries. Usage of these libraries will be explained later.

    import numpy as np
    import pandas as pd
    from sklearn.feature_extraction.text import CountVectorizer
    from sklearn.model_selection import train_test_split
    from sklearn.naive_bayes import MultinomialNB
    from sklearn.metrics import classification_report

### Step 1: Data Collection
We need to collect data and convert them into data frame so they can be processed. In this case, I will use `csv` file.

To collect data, we will use pandas library.

    data = pd.read_csv("spam.csv", encoding="utf-8")
    # Double check the data
    print(data.head())

The output should be similar to this

![pd_output](https://miro.medium.com/v2/resize:fit:720/format:webp/0*Nn0JSH_CNirlgn6O.png)

Convert data into Data Frame for processing.

    df = pd.DataFrame(data)
    print(df)

The output should be similar to the above:

![df_output](https://miro.medium.com/v2/resize:fit:720/format:webp/0*kMRa-CCtiVI_b1wy.png)

## Step 2: Data processing
In order to determine if an email is a spam, my approach is to count the frequency of certain words appearing in spams and based on the probability, the model can make a decision
In this step, I process the data that I've collected using __Feature Extraction__, a technique that convert data into numerical values. By converting the text to numerical values, we can easily determine the frequency of a text. To use this technique, we need the `__from sklearn.feature_extraction.text import CountVectorizer__` library that we imported

    from sklearn.feature_extraction.text import CountVectorizer
    vectorizer = CountVectorizer()
    numerical_data = vectorizer.fit_transform(df['Message'])
    print(numerical_data)

The above code snippet will create a CountVectorizer object `vectorizer`, which then is used to transform all the text in the __Message__ column into numerical values. Printing the output will give us something similiar to this:

![vectorizer_output](https://miro.medium.com/v2/resize:fit:414/format:webp/0*61QdIqJ-qGV_5H6l.png)

## Step 3: Train/Test split data
The reason for splitting the data set is to test the model’s performance on unseen data. By splitting the data, we can train our model on one subset and test its performance on another. To do this, we need `__from sklearn.model_selection import train_test_split__`

    from sklearn.model_selection import train_test_split
    label = df['Classification']
    numerical_data_train, numerical_data_test, label_train, label_test = train_test_split(numerical_data, label, test_size=0.2)

The __train_test_split__ function takes 3 arguments: the numerical data of the text, the labels and the percentage into which the test will be split (80% for training and 20% for testing).
The 4 values that __train_test_split__ are:
- numerical_data_train: the data that will be used for training.
- numerical_data_test: the data that will be used for testing.
- label_train: the labels that will be used for training.
- label_test: the labels that will be used for testing.

## Step 4: Training Model
As explained above, I will determine whether an email is a spam based on the words it contains and how frequently they appear on spams/non-spams. If the email has more words that show up more in spams, it is likely a spam. And vice versa.To use this approach, I need to import `__from sklearn.naive_bayes import MultinomialNB__` to apply the Naive Bayes Classifier model.

    from sklearn.naive_bayes import MultinomialNB
    classifier=MultinomialNB()
    classifier.fit(numerical_data_train,label_train)

When the script calls the `fit` method, the MultinomialNB model goes through the data and learns patterns. In the context of Naive Bayes, it calculates the probabilities and likelihoods of each feature (word/token) being associated with each class (spam/ham). These calculations are based on Bayes' theorem and the assumption of feature independence given the class label.

Once the model has been trained with the fit method, it can be used to make predictions on new, unseen data.

## Step 5: Model Evaluation
To evaluate the model, I need to import `__from sklearn.metrics import classification_report__`, which uses __classification_report__.

    from sklearn.metrics import classification_report
    label_prediction = classifier.predict(numerical_data_test)
    print(classification_report(label_test,label_prediction))

![classifier_report](https://miro.medium.com/v2/resize:fit:640/format:webp/0*BDMxygc5PFxiAZK_.png)

- Precision: This is the ratio of correctly predicted positive observations to the total predicted positives. The question it answers is: Of all the samples predicted as positive, how many were actually positive?
- Recall (sensitivity): The ratio of correctly predicted positive observations to all the actual positives. It answers the question: Of all the actual positive samples, how many did we predict correctly?
- F1-score: The harmonic mean of the precision and recall metrics. It gives a better measure of the incorrectly classified cases than the accuracy metric, especially when there's an imbalance between classes.
- Support: This metric is the number of actual occurrences of the class in the specified dataset.
- Accuracy: The ratio of correctly predicted observations to the total observations.
- Macro Avg: This averages the unweighted mean per label.
- Weighted Avg: This metric averages the support-weighted mean per label.

## Step 6: Testing the Model
Let's try  to test a model

    message = vectorizer.transform(["Today's Offer! Claim ur £150 worth of discount vouchers! Text YES to 85023 now! SavaMob, member offers mobile! T Cs 08717898035. £3.00 Sub. 16 . Unsub reply X "])
    prediction = classifier.predict(message)
    print("The email is :", prediction[0])

![model_testing](https://miro.medium.com/v2/resize:fit:720/format:webp/0*srIwrObadVFqkuuV.png)

## Testing against a new set of data
I have another file that to test the model. Repeat the same thing to see the result.

    test_data=pd.read_csv("test_emails.csv")
    numerical_data_new = vectorizer.transform(test_data['Messages'])
    new_predictions = classifier.predict(numerical_data_new)
    results_df = pd.DataFrame({'Messages': test_data['Messages'],'Prediction':new_predictions})
    print(results_df)

 ![new_data](https://miro.medium.com/v2/resize:fit:720/format:webp/0*JPFCppWRgLz2cv_E.png)

The full script and supported files can be found [here](https://github.com/hhphu/InfoSec/blob/main/Projects/AI%20Spams%20Detection/detecting_spam.py)
