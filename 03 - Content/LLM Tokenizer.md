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

## Overview  
  
Large Language Models cannot process raw text directly.    
Instead, text must first be converted into **tokens**, which are numeric representations of pieces of text.  
  
Example:  
  
Text:  
Hello world  
  
Tokens:  
[15496, 995]  
  
These token IDs are what the neural network actually processes.  
  
---  
  
## Why Tokenization Exists  
  
Neural networks operate on **numbers**, not words.  
  
The pipeline looks like:  
  
Text → Tokenizer → Token IDs → Embeddings → Transformer  
  
Without tokenization, the model would have no way to interpret language.  
  
---  
  
## What Is a Token?  
  
A token is **a chunk of text mapped to a numeric ID**.  
  
Tokens are not always words.  
  
Examples of tokens:  
  
| Text | Possible Tokens |  
|-----|-----|  
| hello | hello |  
| unbelievable | un + believ + able |  
| GPT-4 | GPT + - + 4 |  
  
This allows models to efficiently represent language.  
  
---  
  
## Types of Tokenization  
  
### Word Tokenization  
  
Splits text by spaces.  
  
Example:  
  
Hello world → ["Hello", "world"]  
  
Problem:  
Vocabulary becomes extremely large.  
  
---  
  
### Character Tokenization  
  
Each character becomes a token.  
  
Example:  
  
Hello → ["H", "e", "l", "l", "o"]  
  
Problem:  
Sequences become very long.  
  
---  
  
### Subword Tokenization (Used by LLMs)  
  
Modern LLMs use **subword tokenization**.  
  
Example:  
  
unbelievable → ["un", "believ", "able"]  
  
Advantages:  
  
• smaller vocabulary    
• handles rare words    
• shorter sequences than character tokenization  
  
---  
  
## Byte Pair Encoding (BPE)  
  
Most LLMs use **Byte Pair Encoding**.  
  
The algorithm works by repeatedly merging common token pairs.  
  
Example process:  
  
Start with characters:  

t h e

  
Frequent merges:  

t + h → th  
th + e → the

  
Eventually common words become single tokens.  
  
---  
  
## Token Vocabulary  
  
Each token corresponds to an **integer ID** stored in the tokenizer vocabulary.  
  
Example:  
  
| Token | ID |  
|-----|-----|  
| hello | 15496 |  
| world | 995 |  
  
Typical vocabulary sizes:  
  
- GPT-2: ~50k  
- GPT-3/4: ~50k-100k  
  
---  
  
## Context Window  
  
LLMs process a limited number of tokens at once.  
  
This limit is called the **context window**.  
  
Example:  
  
| Model | Context |  
|------|------|  
GPT-3 | 4k tokens |  
GPT-4 | up to 128k tokens |  
  
---  
  
## Full Tokenization Pipeline  

Text Input  
↓  
Tokenizer  
↓  
Token IDs  
↓  
Embeddings  
↓  
Transformer Model

  
---  
  
## Related Notes  
  
[[LLM Embeddings]]    
[[Transformer Architecture]]    
[[Attention Mechanism]]




___

## Resources:

| Hyperlink | Info |
| --------- | ---- |


Created Date: March 10th 2026 (11:53 am)
Last Modified Date: March 10th 2026 (11:53 am)
