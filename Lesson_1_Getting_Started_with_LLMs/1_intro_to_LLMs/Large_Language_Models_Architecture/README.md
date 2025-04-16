# 2. Large Language Models Architecture

## How Do LLMs Actually Work? Understanding Their Architecture

To truly understand how large language models (LLMs) generate text, we have to look under the hood and explore their architecture. This includes not just the final model you interact with, but the deep design decisions that allow them to process language so efficiently. In this section, we'll walk through the evolution that brought us here—from classic neural networks to the revolutionary transformer architecture—and finally explore the three major types of transformer-based LLMs you'll encounter in practice.

## The Evolution Behind LLMs

### The Foundations: Neural Networks in NLP

Neural networks are computational models that mimic the architecture of the human brain. These networks are composed of layers of artificial neurons that process incoming input via a series of mathematical operations. A neural network can learn to recognize patterns in data by changing the weights of neurons connections during training, allowing the model to make predictions or decisions based on new data.

In the early days of artificial intelligence, neural networks were rather simplistic and incapable of effectively handling the complexity of sequential data such as language. While they were capable of doing image recognition and categorization tasks, they struggled with activities requiring a more in-depth comprehension of context, such as language translation, question responding, or text generation.

### Recurrent Neural Networks (RNNs) and LSTMs

Recurrent neural networks (RNNs) were created to address the issue of sequential dependencies in language. Unlike traditional Feedforward Neural Networks (FNNs), where information flows in one direction, RNNs have feedback loops that allow information to persist across time steps, enabling them to "remember" previous inputs and consider them when processing subsequent ones.

Despite being popular and effective for handling sequential data, RNNs still faced two major challenges:

- **Vanishing and Exploding Gradients:** When training an RNN, the gradients—used to adjust the model's weights—either became too small (vanishing gradients) or too large (exploding gradients), especially when sequences are long. This occurs because gradients are multiplied by values of the weight matrices multiple times, leading to exponential decay or growth.
  
- **Short-Term Memory:** Even though RNNs have feedback loops designed to remember information, they still struggled with long-term dependencies; they had a hard time holding on to information over long sequences, like an entire paragraph or a full document, which limited their effectiveness in more complex tasks.

These particular issues prompted the development of Long Short-Term Memory (LSTM) networks. Which are a particular type of RNN that uses memory cells and gates to selectively recall or forget information, which allows them to handle long-range dependencies better than traditional RNNs and reducing the risk of vanishing gradients.

## The Transformer Breakthrough

The transformer architecture redefined everything. It abandoned recurrence entirely and introduced a new mechanism: self-attention. Instead of reading words one by one, transformers look at entire sequences in parallel—and decide how much focus (or "attention") each word should get in context.

### Self-Attention, Explained Simply

Here's the trick of transformers:

1. **Words as Vectors:** Each word gets turned into a numerical vector.
2. **Query, Key, Value (Q/K/V):** For every word, the model creates three vectors:
   - **Query:** What am I looking for?
   - **Key:** What do I offer?
   - **Value:** What do I mean?
3. **Calculating Attention:** Words compare their Queries with others' Keys to compute attention scores—basically: "How relevant is this word to me?"
4. **Weighting the Value:** Each word uses those scores to pull meaning from others, blending context together.

This is done not once, but multiple times in parallel by multi-head attention, allowing different parts of the model to focus on different linguistic patterns—syntax, semantics, or long-distance dependencies.

### Positional Encoding

Because transformers look at all words simultaneously, they don't naturally understand order. That's where positional encoding comes in. It injects information about word position (often using sinusoidal patterns), allowing the model to distinguish "The dog chased the cat" from "The cat chased the dog."

### Feedforward Layers & Normalization

After attention, each word's vector goes through a mini feedforward neural network and layer normalization. This adds non-linearity and stabilizes training, ensuring smoother and faster convergence.

## The Three Main Model Types

Transformers can be configured in different ways depending on the task. LLMs generally fall into three buckets:

### Encoder-Only Models

An Encoder is the first part of the transformer architecture. Its role is to convert the input text into a numerical representation (Vectors) that the model can interpret. It's basically a translator that transforms words into numbers and captures the meaning of each word in relation to others.

For an encoder-only model, the encoder processes the input text all at once to understand each word by examining its context within the sentence. Once this is complete, the encoder generates a set of vectors representing the meaning of the text. These vectors are then used for tasks like classification, question answering, or sentiment analysis. Here, the model doesn't generate text; instead, it focuses on understanding the input and pulling out the important information.

### Decoder-Only Models

A Decoder is the Second part of a transformer responsible for generating text based on a given input. Unlike the encoder, which focuses on understanding the input, the decoder is trained to predict the next word in a sequence, using the words before it.

Decoder-Only Models work step-by-step, generating one word at a time while considering the context of previous words. The use of self-attention captures the relationships between words in the sequence, This helps the model predict coherent and contextually relevant words. This process continues until the model generates a full response. Decoder-only models excel at tasks like text generation, conversational agents, and creative writing.

### Encoder-Decoder Models

The Encoder-Decoder model combines the strengths of both the encoder and decoder. The encoder processes the input text, converting it into vectors that represent the meaning of the input, as described earlier. The decoder then takes these vectors and uses them to generate an output, such as translated text, summaries, or answers to questions.

The main difference between encoder-decoder models and decoder-only models lies in the way they handle inputs and outputs. While decoder-only models generate text by predicting the next word based on previous ones, encoder-decoder models process all of the input first and then pass this information to the decoder.

## Why Use Pretrained Models

Pretrained models save both time and resources. Training a language model from scratch requires massive datasets and significant computational resources. Pretrained models allow us to utilize a model that has already mastered the basics of language. Rather than starting from zero, we fine-tune these models for specific tasks, needing less data and time. This is known as transfer learning.
