---
icon: lucide/whole-word
---

# Product Features
Product features characterize the linguistic properties of the translation product, the source and target texts, as a static artifact. In the TPR-DB 3.0 they encompass various PoS features, morpho-syntactic features, but also information-theoretic and entropy-based estimates of lexical variability and translational uncertainty.

## Linguistic features
The TPR-DB 3.0 processes all source texts and translations using NLTK (Natuaral Language Toolkit) and Stanza (Stanford NLP's neural annotation pipeline). Each text (other than Chinese and Japanese) is first segmented, tokenized and PoS tagged using NLTK. Subsequently, Stanza's annotation chain is applied comprising part-of-speech (PoS) tagging, lemmatization, multi-word token (MWT) expansion, named entity recognition (NER), and dependency parsing if those processes are defined for the languag.

Tokenization segmentes the raw text into individual tokens, while MWT expansion resolved language-specific contractions and fused forms into their constituent units — a step particularly relevant for morphologically rich languages. PoS tagging and lemmatization assign grammatical categories and canonical base forms to each token, enabling normalized frequency and complexity measures. NER identifies and classifies named entities such as persons, locations, and organizations, providing a measure of referential density. Finally, dependency parsing established syntactic relationships between tokens, yielding tree-based measures of structural complexity such as mean dependency distance and branching factor. Together, these annotations form the basis for a rich set of linguistic product features capturing both surface form and underlying syntactic structure.



## Information and Entropy Features

To capture the informational complexity of translation decisions at the word level, we computed word translation entropy (HTra) for each source token. Word translation entropy quantifies the uncertainty inherent in mapping a source language word to its target language equivalent — a high entropy value indicates that a given source word has many plausible translations of roughly equal probability, reflecting genuine translational ambiguity, while a low entropy value indicates that the translation choice is largely deterministic. 

HTra is derived from probabilistic word alignment models trained on large parallel corpora, which yield a probability distribution over all candidate translations for each source token. This measure has been shown to correlate with cognitive effort in human translation: words with high translational entropy tend to elicit longer fixation durations, more regressive eye movements, and longer production pauses, suggesting that translators invest more processing resources precisely where the translation space is most uncertain. By incorporating HTra as a product feature, we capture not only the surface linguistic properties of the output but also the underlying decision complexity that shaped it.

*[MWT]: multi-word token
*[NLTK]: Natuaral Language Toolkit
*[PoS]: part-of-speech 
*[NER]: named entity recognition
*[HTra]: word translation entropy 
