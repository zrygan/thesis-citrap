---
tags:
  - working
dg-publish: true
---
**Problems**: 
- How can we compute the similarity of two drugs $X$ and $Y$. 
- If $X$ is a generic name and $Y$ is a brand name, how can we calculate similarity?
	- Especially when $Y$ is very different from $X$ (as with "Advil" and "Ibuprofen").

**Why bother?**
- Drug prescribing errors due to look-alike and sound-alike (LASA) persists [LGL+19](https://qualitysafety.bmj.com/content/qhc/28/11/908.full.pdf).
- This becomes a pavement to explore string similarity algorithms with modern transformers (BERT) as assistants.
## Methods: what was done already?
**Title**: A hybrid approach for drug similarity computation

### Similarity Algorithms
### String-Matching Algorithms
Since the problem is with both look-alike (*orthographic*) and sound-alike (*phonetic*) we can use similarity algorithms for both orthographic and phonetic strings.
#### Orthographic Similarity

> To work on, maybe we need to do a separate literature on this.
#### Phonetic Matching
- Phonetic Alignment due to [Kon03](https://webdocs.cs.ualberta.ca/~kondrak/papers/chum.pdf).

> To add, maybe we need to do a separate literature review on this.
### Embedding-Based Similarity
Let $B:\mathcal D\mapsto \mathbb{R}^n$ be a mapping from a set of pharmaceutical identifiers $\mathcal D$ to an $n$-dimensional continuous vector space, where $B(d)$ for some $d\in\mathcal D$ is a dense, real-valued embedding representation as produced by some language model $B$.

Then the cosine similarity $S_c$ of two word embeddings $B(d_i)$ and $B(d_{j})$ of two drugs $d_i$ and $d_{j}$ is given by
$$
S_{c}(B(d_{i}),B(d_{j})):= \frac{B(d_{i})\cdot B(d_{j})}{||B(d_{i})||\,||B(d_{j})||}
$$
Furthermore, 
$$S_{c}(B(d_{i}),B(d_{j}))\in[-1,1]
$$
> The language model we talk about here is [[Drug Similarity#Issue what about brand names?|BERT]].
## Issue: what about brand names?
Looking at Advil and Ibuprofen, how can we determine that these two **are the same**?
[[[SC16] Characteristics That May Help in the Identification of Potentially Confusing Proprietary Drug Names.pdf|SC16]].
## Who the f_ck is BERT?
We need a domain-specific language model that specifically looks at drug named and other pharmaceutical words.

Thankfully [VSR+23](https://academic.oup.com/bib/article/24/4/bbad226/7197744) with their [PharmBERT](https://huggingface.co/Lianglab) BERT model provides a recent solution.

> Since this BERT model is only recent, this adds to the motivation for pursuing this research.
## Proposed Idea
We identify the gap:
1. Existing LASA-based methods ([[[KD05] Automatic identification of confusable drug names.pdf|]]) do not leverage recent advances in transformer-based language models.
2. PharmBERT by VSR+23 is not systematically evaluated for LASA detection or brand–generic matching.
3. Brand and generic names may be less represented, limiting the effectiveness of pure embedding-based similarity.
4. Many countries or institutions lack **large, curated biomedical corpora** or **rich drug databases** (e.g., RxNorm, DrugBank) needed to train or fine-tune domain-specific language models like PharmBERT.

Putting it all together we can create a hybrid method for drug similarity?
1. If two drugs $d_i$ and $d_j$ are very similar to one another (i.e., acetaminophen and ibuprofen) it's not necessary to use BERT.
2. On the other hand, if one is a [[Drug Similarity#Issue what about brand names?|brand name]] or the two are very different from each other. Then, a cosine similarity may be necessary. 