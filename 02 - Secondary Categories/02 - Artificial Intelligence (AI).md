Primary Categories: { Add link(s) [[]] back to related PRIMARY categories }
Search Tag: #🗺  

# [[Artificial Intelligence (AI)]]  
{ Links to content pages }
Artificial Intelligence -> Machine Learning -> Neural Nets -> Deep learning

`Artificial Intelligence` (`AI`) is a broad field focused on developing intelligent systems capable of performing tasks that typically require human intelligence. These tasks include understanding natural language, recognizing objects, making decisions, solving problems, and learning from experience. `AI` systems exhibit cognitive abilities like reasoning, perception, and problem-solving across various domains.
`Machine Learning` (`ML`) is a subfield of AI that focuses on enabling systems to learn from data and improve their performance on specific tasks without explicit programming. ML algorithms use statistical techniques to identify patterns, trends, and anomalies within datasets, allowing the system to make predictions, decisions, or classifications based on new input data.
`Deep Learning` (`DL`) is a subfield of ML that uses neural networks with multiple layers to learn and extract features from complex data. These deep neural networks can automatically identify intricate patterns and representations within large datasets, making them particularly powerful for tasks involving unstructured or high-dimensional data, such as images, audio, and text.

### Artificial Intelligence (AI)
Section for anything AI related.

## Core Concepts  
- [[Fundamentals Of AI For Red Teaming]]
- [[Applications of AI in InfoSec]]
	- Model examples - [[AI Models Examples]]
- [[Introduction to Red Teaming AI]]
	- [OWASP Machine Learning Security Top Ten (any system using machine learning like spam detection)](https://owasp.org/www-project-machine-learning-security-top-10/)
	- [[Input Manipulation Attack - AI]]
	- [OWASP Top 10 for Large Language Model Applications (ChatGPT-style bots, customer-support, doc summarizer)](https://owasp.org/www-project-top-10-for-large-language-model-applications/)
	- Components of Generative AI Systems - [[AI Components Attacks]]
		- **Model** - Model-based security vulnerabilities comprise any vulnerabilities within the model itself. For instance, for text generation models, this includes vulnerabilities like prompt injection and insecure output handling.
		- **Data** - Everything related to the data the ML model operates on belongs to the umbrella of the data component, including training data as well as data used for inference.
		- **Application** - This component refers to the application in which the generative AI is integrated. For instance, assume a web application uses an AI chatbot for customer support. Security vulnerabilities in the application component include traditional web vulnerabilities within the web application related to the ML-based system.
		- **System** - The system component includes system hardware, the operating system, and the system configuration. Furthermore, it also includes details about the model deployment. A simple example of a security vulnerability in the system component is a denial-of-service attack through resource exhaustion due to a lack of rate limiting or insufficient hardware to run the ML model.
	- [[AI Prompt Injection Attacks]]
	- [[AI Jailbreaking]]
	- Tools for accessing model security
		- [Adversarial-Robustness-Toolbox](https://github.com/Trusted-AI/adversarial-robustness-toolbox)
		- [PyRIT](https://github.com/microsoft/PyRIT)
		- [Garak](https://github.com/NVIDIA/garak)
	- Next
  
---  
  
# Courses  
  
## Building LLMs from Scratch  
- [ ] Watch playlist (https://www.youtube.com/playlist?list=PLPTV0NXA_ZSgsLAr8YCgCwhPIJNNtexWu)  
  
Course Notes:  
- [[LLM Tokenizer]]  
- [[LLM Embeddings]]  
- [[Attention Mechanism]]  
- [[Transformer Architecture]]  
- [[Training LLMs]]  
- [[LLM Inference]]



