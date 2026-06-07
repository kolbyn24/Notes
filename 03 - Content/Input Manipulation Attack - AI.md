---
creation date: June 7th 2026
last modified date: June 7th 2026
aliases: []
tags:
---

Primary Categories: { Add link(s) [[]] back to related PRIMARY categories }
Secondary Categories:  { Add link(s) [[]] back to related SECONDARY categories }
Links: {Add link(s) [[]] to related terms}
Search Tag: #📕  

# [[Input Manipulation Attack - AI]]  
___

## Description:  

The examples below are for a spam classification model that will take an inputted message a classify it as "ham" (good) or spam. (the code can be found from the Manipulating the model section in introduction to red team AI course: https://academy.hackthebox.com/app/module/294/section/3342 Download files: https://cdn.services-k8s.prod.aws.htb.systems/content/questions/file/f8362a2f-e7ff-4f8b-bd6e-8787b80dd499.zip).

#### Rephrasing

Often, we are only interested in getting our victim to click the provided link. To avoid getting flagged by spam classifiers, we should thus carefully consider the words we choose to convince the victim to click the link. In our case, the model is trained on spam messages, which often utilize prizes to trick the victim into clicking a link. Therefore, the classifier easily detects the above message as spam.

First, we should determine how the model reacts to certain parts of our input message. For instance, if we remove everything from our input message except for the word `Congratulations!`, we can see how this particular word influences the model. Interestingly, this single word is already classified as spam:

        shellsession
`kolbyn24@htb[/htb]$ python3 main.py Predicted class: Spam Probabilities:      Ham: 35.03%    Spam: 64.97%`

We should continue this process with different parts of our input message to get a feel for the model's reaction to certain words or combinations of words. From there, we know which words to avoid to get our input past the classifier:

|Input Message|Spam Probability|Ham Probability|
|---|---|---|
|`Congratulations!`|64.97%|35.03%|
|`Congratulations! You won a prize.`|99.73%|0.27%|
|`Click here to claim: https://bit.ly/3YCN7PF`|99.34%|0.66%|
|`https://bit.ly/3YCN7PF`|87.29%|12.71%|

With this knowledge, we can try different words and phrases with a low probability of being flagged as spam. In our particular case, we are successful with a different scenario for the reasons outlined earlier. If we change the input message to `Your account has been blocked. You can unlock your account in the next 24h: https://bit.ly/3YCN7PF`, the input will (barely) be classified as ham:

        shellsession
`kolbyn24@htb[/htb]$ python3 main.py  Predicted class: Ham Probabilities:      Ham: 57.39%    Spam: 42.61%`


#### Overpowering

Another technique is overpowering the spam message with benign words to push the classifier toward a particular class. We can achieve this by simply appending words to the original spam message until the ham content overpowers the message's spam content. When the classifier processes many ham indicators, it finds it overwhelmingly more probable that the message is ham, even though the original spam content is still present. Remember that Naive Bayes makes the assumption that each word contributes independently to the final probability. For instance, after appending the first sentence of an English translation of Lorem Ipsum, we end up with the following message:

        txt
`Congratulations! You won a prize. Click here to claim: https://bit.ly/3YCN7PF. But I must explain to you how all this mistaken idea of denouncing pleasure and praising pain was born and I will give you a complete account of the system, and expound the actual teachings of the great explorer of the truth, the master-builder of human happiness.`

After running the classifier, we can see that it is convinced that the message is benign, even though our original spam message is still present:

        shellsession
`kolbyn24@htb[/htb]$ python3 main.py Predicted class: Ham Probabilities:      Ham: 100.0%    Spam: 0.0%`

This technique is particularly effective in cases where we can conceal the appended message from the victim. Think of websites or e-mails that support HTML, where we can hide words from the user in HTML comments, while the spam classifier may not be HTML context-aware and thus still base the spam verdict on words contained in HTML comments.

## Manipulating the Training Data

After exploring how manipulating the input data affects the model's output, let us proceed to the training data. To achieve this, let us create a separate training data set to experiment on. We will shorten the training data set significantly so our manipulations will have a more significant effect on the model. Let us extract the first 100 data items from the training data set and save them to a separate CSV file:

        shellsession
`kolbyn24@htb[/htb]$ head -n 101 train.csv  > poison.csv`

Afterward, we can change the training data set in `main.py` to `poison.csv` and run the Python script:

        shellsession
`kolbyn24@htb[/htb]$ python3 main.py Model accuracy: 94.4%`

As we can see, the model's accuracy drops slightly to `94.4%`, which is impressive for the tiny size of the training data set. The drop in accuracy can be attributed to the significant reduction in training data, which makes the classifier less representative and more sensitive to changes. However, this sensitivity to changes is precisely what we aim to demonstrate by injecting fake spam entries into the data set (`poisoning`). To observe the effect of manipulations on the training data set, let us adjust the code as we did before to print the output probabilities for a single input message:

        python
`model = train("./poison.csv") message = "Hello World! How are you doing?" predicted_class = classify_messages(model, message)[0] predicted_class_str = "Ham" if predicted_class == 0 else "Spam" probabilities = classify_messages(model, message, return_probabilities=True)[0] print(f"Predicted class: {predicted_class_str}") print("Probabilities:") print(f"\t Ham: {round(probabilities[0]*100, 2)}%") print(f"\tSpam: {round(probabilities[1]*100, 2)}%")`

If we run the script, the classifier classifies the input message as ham with a confidence of `98.7%`. Now, let us manipulate the training data so that the input message will be classified as spam instead.

To achieve this, we inject additional data items into the training data set that facilitate our goal. For instance, we could add fake `spam` labeled data items with the two phrases of our input message to the CSV file:

        csv
`spam,Hello World spam,How are you doing?`

After rerunning the script, the model now produces the following result:

        shellsession
`kolbyn24@htb[/htb]$ python3 main.py Predicted class: Spam Probabilities:      Ham: 20.34%    Spam: 79.66%`

As we can see, this minor adjustment to the training data set was already sufficient to alter the classifier's prediction. We can further increase the confidence by appending two additional fake data items to the training data set. This time, we will use a combination of both phrases:

        csv
`spam,Hello World! How are you spam,World! How are you doing?`

Keep in mind that duplicates are removed from the data set before training. Therefore, adding the same data item multiple times will have no effect. After appending these two data items to the training data set, the confidence is at `99.6%`:

        shellsession
`kolbyn24@htb[/htb]$ python3 main.py Predicted class: Spam Probabilities:      Ham: 0.4%    Spam: 99.6%`

As a final experiment, let us add the evaluation code back in to see how our training data set manipulation affected the overall model accuracy:

        python
`model = train("./poison.csv") acc = evaluate(model, "./test.csv") print(f"Model accuracy: {round(acc*100, 2)}%") message = "Hello World! How are you doing?" predicted_class = classify_messages(model, message)[0] predicted_class_str = "Ham" if predicted_class == 0 else "Spam" probabilities = classify_messages(model, message, return_probabilities=True)[0] print(f"Predicted class: {predicted_class_str}") print("Probabilities:") print(f"\t Ham: {round(probabilities[0]*100, 2)}%") print(f"\tSpam: {round(probabilities[1]*100, 2)}%")`

Running the script a final time reveals that the accuracy took only a minor hit of `0.4%`:

        shellsession
`kolbyn24@htb[/htb]$ python3 main.py Model accuracy: 94.0% Predicted class: Spam Probabilities:      Ham: 0.4%    Spam: 99.6%`

We forced the classifier to misclassify a particular input message by manipulating the training data set. We achieved this without a substantial adverse effect on model accuracy, which is why data poisoning attacks are both powerful and hard to detect. Remember that we deliberately shrunk the training data set significantly so that our manipulated data items had a greater impact on the model. In larger training data sets, many more manipulated data items are required to influence the model in the desired way.
















___

## Resources:

| Hyperlink | Info |
| --------- | ---- |


Created Date: June 7th 2026 (03:09 pm)
Last Modified Date: June 7th 2026 (03:09 pm)
