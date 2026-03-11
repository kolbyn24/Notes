---
creation date: March 10th 2026
last modified date: March 10th 2026
aliases: []
tags:
---

Primary Categories: { Add link(s) [[]] back to related PRIMARY categories }
Secondary Categories:  { Add link(s) [[]] back to related SECONDARY categories }
Links: {Add link(s) [[]] to related terms}
Search Tag: #📕  

# [[LLM Tokenizer]]  
___

## Description:  
Used to pre process data to be used for pre-training. Tokenization is process to break down sentences into parts and assigned a token.

How do you prepare input text for training LLMs? You tokenize the documents and feed it the tokens to train a model. 

Step 1: Split the text into individual work and subword tokens

Step 2: Convert tokens into token IDs

Step 3: Encode token IDs into vector representations

### Example: 

This is an example = input text

1       2    3      4
(this) (is) (an) (example) = Tokenized text (step 1)

(4013) (201) (302) (1134) = Token IDs (step 2)

step 3 is to convert these into Token/vector embeddings. Token embeddings are given as input data to LLM/GPT.

### Full notes link for clarification
Entire Code file link that explains each step: [https://drive.google.com/file/d/1_lgi...](https://www.youtube.com/redirect?event=video_description&redir_token=QUFFLUhqbGZ3V0tHdHJLV0hfeXZQdV95RTVfcndvX1NrZ3xBQ3Jtc0ttNTVzaDk4a3A1ZTVNV1pDb0NYb0tlWmhqdEdRd3JuODRWVkRSajFNUmE0NXJfeTA2SlNyUTU3X2tsNXl6YVU1RVVsaTg0QlVsZTBwSDhDVE1vNHhmNEZMMjRsajhnZlg2bEd5ZmhvMUVjWHY3WUFqTQ&q=https%3A%2F%2Fdrive.google.com%2Ffile%2Fd%2F1_lgiXKLwsQEg0t4ziei9cWf9Zl91HXcH%2Fview%3Fusp%3Dsharing&v=rsy5Ragmso8)

Need to setup VS Code + Python + Jupyter extension or use google colab and upload the file.
### Tokenizing the text - Step 1
(There are prebuilt ones available)

Basic Tokenizer
Replace .txt file with what you want to Tokenize.
```
import re

with open("the-verdict.txt", "r", encoding="utf-8") as f:

    raw_text = f.read()
    
preprocessed = re.split(r'([,.:;?_!"()\']|--|\s)', raw_text)

preprocessed = [item.strip() for item in preprocessed if item.strip()]

print(preprocessed[:30])

print(len(preprocessed))
    
```
### Convert Tokens into Token IDs - Step 2

We now have: `['I', 'HAD', 'always', 'thought', 'Jack', 'Gisburn', 'rather', 'a', 'cheap', 'genius', '--', 'though', 'a', 'good', 'fellow', 'enough', '--', 'so', 'it', 'was', 'no', 'great', 'surprise', 'to', 'me', 'to', 'hear', 'that', ',', 'in']`

Need to go from words to numbers (token IDs): 1, 2, 3, 4, 5, .........

each unique word is mapped to a token ID. 

```
vocab = {token:integer for integer,token in enumerate(all_words)}

for i, item in enumerate(vocab.items()):

    print(item)

    if i >= 50:

        break
```
('!', 0)
('"', 1)
("'", 2)
('(', 3)
(')', 4)
(',', 5)
('--', 6)
('.', 7)
(':', 8)
(';', 9)
('?', 10)
('A', 11)
('Ah', 12)
('Among', 13)
('And', 14)
('Are', 15)
('Arrt', 16)
('As', 17)
('At', 18)
('Be', 19)
('Begin', 20)
.....
('Hermia', 50)

etc.




___

## Resources:

| Hyperlink | Info |
| --------- | ---- |


Created Date: March 10th 2026 (11:53 am)
Last Modified Date: March 10th 2026 (11:53 am)
