---
tags:
  - scrapped
---
>[!warning] This idea is **scrapped**
>Automata Extraction only works on regular languages, since Philippine grammar is non-regular (it is safe to say that majority, if not all, human languages are non-regular). Then we cannot use Automata Extraction for a neural network on a Philippine language.

Work started with [[[kle51]-representation-of-events-in-nerve-nets-and-finite-automata.pdf|Kle51]] and recently worked on by [[[wgy20]-extracting-automata-from-recurrent-neural-networks.pdf|WGY20]].

**DFA Extraction** is a procedure of extracting a DFA $A$ *over* some alphabet $\Sigma$ from a Recurrent Neural Networks (RNNs) $R$ *trained* over $\Sigma$. We can say that $A$ is extracted from $R$ if $$(\forall s_{1}\in \mathcal{L}(A))\iff(s_{1} \text{ can be produced by }R)$$They do this by **Exact Learning** (by Angluin's L* Algorithm [[[Ang87] Learning Regular Sets from Queries and Counterexamples.pdf|Ang87]]), we have an oracle[^1] that can answer two types of questions:
1. Membership Query (MQ): "Does $R$ accept the string $s$?"
2. Equivalence Query (EQ): "Is $A$ correct? If not, provide a *counterexample*."

> Here counterexample is some string $s'\in \mathcal{L}(A)$ that cannot be produced by $R$. Notice that the constraint above uses universal quantification ($\forall$), hence we can disprove the equivalence of $A$ and $R$ by counterexample.

We start with a hypothesis DFA $A_0$ (the subscript 0 denotes the first iteration). We ask one of the questions to the oracle and refine $A_{0}$ depending on its response.
- For MQs and for some string $s$, if the oracle says that $s\in \mathcal{L}(R)$ then update $A_{0}$ (this new DFA is $A_1$, or generally $A_{n+1}$) such that $s\in \mathcal{L}(A_{1})$.
- For EQs and for some "version" $A_n$, if the oracle returns an $s\in A_{n}$, that means $s \not\in \mathcal{L}(R)$. Then, update $A_n$ to $A_{n+1}$ such that $s\not\in A_{n+1}$.

>[!important] Motivation
>Neural networks abstract a lot of things; if we train an RNN over a language $\Sigma$ this does not tell us much about $\Sigma$. On the other hand, creating a DFA $A$ over $\Sigma$ and ensuring that $\forall s$ the RNN knows $s\in A$. We also get a "concrete" model of $\mathcal{L}(A)$ without any form of abstractions. 

More recent research [[[zws24]-automata-extraction-from-transformers.pdf|ZWS24]] builds upon WGY20 and uses the more recent Transformer architecture (think of the T in GPT).

[^1]: We call this oracle a Minimally Adequate Teacher (MAT), one that *correctly* answer MQs and EQs.