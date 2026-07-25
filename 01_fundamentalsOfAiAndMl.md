#  Machine Learning Fundamentals

Building a machine learning model means collecting and preparing data, picking a good algorithm, training the model, and checking how well it works.

## Sorting Out the Data

The process begins with gathering data. Bad data gives bad results, often called garbage in, garbage out. An ML model only works as well as the data used to train it. Preparing data takes time, but it is the most vital step. If you rush it, the model will fail.

Here is a look at the different types of data you will use.

| Data Type | What it means |
|---|---|
| Labeled | Data tagged with clear answers. |
| Unlabeled | Raw data without any tags. |
| Structured | Organised data, like numbers in a SQL database. |
| Unstructured | Messy data, like text documents or photos. |

## How the Machine Actually Learns

Next, we feed the prepared data into algorithms. The learning process splits into a few main types.

| Learning Type | How it works | Goal |
|---|---|---|
| Supervised | Learns from labeled data. | Predict outputs for new data. |
| Unsupervised | Learns from unlabeled data. | Find hidden patterns or structures. |
| Reinforcement | Uses rewards and penalties. | Improve decision making over time based on feedback. |
| Semi supervised | Uses some labeled data. | Guide the learning when you lack fully labeled data. |

## Putting It to Work

Once trained, the model starts making predictions. We call this inferencing. You have two main ways to do this.

| Inferencing Type | Process | Best For |
|---|---|---|
| Batch | Analyses massive chunks of data all at once. | Tasks where speed matters less than accuracy. |
| Real time | Makes quick decisions as new data arrives. | Instant answers, like chatbots or self driving cars. |

Your specific needs will decide which method you use. [Inference] Self driving cars rely heavily on real time processing to stay safe.

## Questions You Might Have Missed

**What happens if the training data is biased?**
[Inference] The model will learn and repeat that bias. You must check your data carefully to avoid unfair results.

**Can a model keep learning after we deploy it?**
[Inference] Yes, some models use continuous learning. They take in new data and update themselves while running.

**How much data do I actually need?**
[Inference] It depends on the task. Simple tasks need a few thousand examples, while big models need billions of data points.
# Deep Learning Fundamentals

Deep learning mimics brain function using artificial neural networks. These models use tiny units called nodes arranged in layers.

## Neural Network Structure

```mermaid
graph LR
    I1[Input 1] --> H1[Hidden 1]
    I1 --> H2[Hidden 2]
    I1 --> H3[Hidden 3]
    I1 --> H4[Hidden 4]
    I1 --> H5[Hidden 5]

    I2[Input 2] --> H1
    I2 --> H2
    I2 --> H3
    I2 --> H4
    I2 --> H5

    I3[Input 3] --> H1
    I3 --> H2
    I3 --> H3
    I3 --> H4
    I3 --> H5

    I4[Input 4] --> H1
    I4 --> H2
    I4 --> H3
    I4 --> H4
    I4 --> H5

    H1 --> O1[Output]
    H2 --> O1
    H3 --> O1
    H4 --> O1
    H5 --> O1
```

Networks learn by adjusting node links when shown data examples. Once trained, they can predict outcomes for new data.

## AI Branches

- **Computer Vision:** Helps computers understand images and videos for tasks like object detection.
    
- **Natural Language Processing:** Deals with human language for tasks like translation and sentiment analysis.
    

### Questions You Might Have Missed

- **What are the layers in a neural network?** An input layer, hidden layers, and an output layer.
    
- **What does computer vision do?** It helps computers make sense of digital images and videos.


# Generative AI Fundamentals

Generative AI emerged due to massive investments in computing resources, hiring large teams, and developing big ideas.

## Foundation Models (FMs)

Foundation models are pretrained on internet-scale data. Rather than gathering labeled data for individual models, a single FM adapts to perform multiple tasks, serving as a starting point for specialised systems.


``` mermaid
graph LR
    U[Unlabeled Data] -->|Pretrain| F[Foundation Model]
    F -->|Adapt| T1[Text generation]
    F -->|Adapt| T2[Text summarisation]
    F -->|Adapt| T3[Information extraction]
    F -->|Adapt| T4[Image generation]
    F -->|Adapt| T5[Chatbot interactions]
    F -->|Adapt| T6[Question answering]
```

### FM Lifecycle Stages

- **Data Selection:** Uses massive unstructured and unlabeled datasets from diverse sources.
    
- **Pre-training:** Employs self-supervised learning to autogenerate labels from data structure, teaching the model word meanings and contexts. Continuous pre-training expands knowledge across domains.
    
- **Optimisation:** Enhances pre-trained models using prompt engineering, retrieval-augmented generation, or fine-tuning.
    
- **Evaluation:** Measures performance using appropriate metrics and benchmarks against business requirements.
    
- **Deployment:** Integrates the model into production software, APIs, or applications.
    
- **Feedback and Continuous Improvement:** Monitors performance and collects user input to guide future iterations, retraining, or fine-tuning.
    

## Large Language Models (LLMs)

Most state-of-the-art LLMs use the transformer architecture to understand and generate human-like text by learning relationships between words and phrases.

```mermaid
graph TD
    A[Text Input] --> B[Tokens]
    B --> C[Embeddings and Vectors]
    C --> D[Transformer Architecture]
    D --> E[Coherent Text Generation]
```

### Tokens, Embeddings, and Vectors

- **Tokens:** Basic processing units like words, phrases, or characters that standardise input data.
    
- **Embeddings and Vectors:** Numerical representations assigning a list of numbers to each token to capture semantic relationships in space. For instance, the vector for "cat" sits close to "feline" and "kitten".
    

## Diffusion Models

Diffusion models start with pure noise and iteratively add meaningful information to create clear outputs like images or text.

Code snippet

```mermaid
graph LR
    A[Original Image] -->|Forward Diffusion| B[Pure Noise]
    B -->|Reverse Denoising| C[Generated Output]
```

- **Forward Diffusion:** Gradually adds noise to an input until only random noise remains.
    
- **Reverse Diffusion:** Gradually denoises random data until a new, coherent output is generated.
    

## Multimodal Models

Multimodal models process and generate multiple data types simultaneously, such as combining text and images. They understand how different modalities influence each other to handle tasks like video captioning or visual question answering.

## Other Generative Models

```mermaid
graph TD
    A[Generative Models] --> B[Generative Adversarial Networks]
    A --> C[Variational Autoencoders]
    B --> B1[Generator vs Discriminator zero-sum game]
    C --> C1[Encoder maps data to lower-dimensional latent space, Decoder reconstructs input]
```

- **GANs:** Feature a generator creating synthetic data and a discriminator trying to distinguish real data from generated data until outputs become indistinguishable.
    
- **VAEs:** Combine autoencoders and variational inference using an encoder to map input to a latent space and a decoder to reconstruct original data from sampled probability distributions.
    

## Optimising Model Outputs

```mermaid
graph TD
    A[Optimisation Techniques] --> B[Prompt Engineering]
    A --> C[Fine-tuning]
    A --> D[Retrieval-Augmented Generation]
    B --> B1[Fastest and lowest cost method using instructions, context, input data, and output indicators]
    C --> C1[Supervised learning process modifying model weights using narrow datasets]
    D --> D1[Supplies domain-relevant documents as context without altering weights]
```

### Questions You Might Have Missed

- **What is continuous pre-training?** It is an additional pre-training phase on extra data meant to expand a model's knowledge base and improve generalization across new domains.
    
- **How does RAG differ from fine-tuning regarding model weights?** RAG supplies external document context without changing underlying foundation model weights, whereas fine-tuning modifies those weights using task-specific datasets.
