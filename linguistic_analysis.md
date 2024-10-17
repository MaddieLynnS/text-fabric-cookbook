---
title: Linguistic Analysis
---

This chapter demonstrates how to perform linguistic analysis using `text-fabric`. We'll explore techniques for extracting grammatical information, analyzing sentence structures, and identifying relationships between words.  Remember to replace `"path/to/your/dataset"` with your dataset's path and adapt feature names as needed.


## Recipe 1: Extracting Grammatical Features

This recipe shows how to extract grammatical features associated with words.  We assume your dataset contains features like `pos` (part-of-speech), `lemma` (lemma), and `morph` (morphological features).

```python
from text_fabric import TF

tf = TF('path/to/your/dataset')


def print_grammatical_info(node):
    """Prints grammatical information for a given node."""
    print(f"  Node: {node}")
    print(f"  Word form: {tf.features.words.word.v(node)}")
    print(f"  Part-of-speech: {tf.features.words.pos.v(node)}")
    print(f"  Lemma: {tf.features.words.lemma.v(node)}")
    # Add other features as needed:  print(f"  Morph: {tf.features.words.morph.v(node)}")


# Example: Print info for the first 5 words
for i in range(1, 6): # Assuming words are numbered from 1
    print_grammatical_info(i)


# Example: Find all verbs
verbs = tf.search(tf.L.pos == 'VERB')
print("\nVerbs:")
for verb_node in verbs:
    print_grammatical_info(verb_node)
This code defines a helper function print_grammatical_info to neatly display information. It then demonstrates how to access different grammatical features for specific nodes (the first five words are shown as an example). Finally, it shows how to find all verbs using tf.search(). Modify the 'VERB' value to match your dataset's POS tag for verbs.

Recipe 2: Analyzing Sentence Structures
Analyzing sentence structures depends heavily on how sentence boundaries are represented in your dataset. Assume your dataset has a feature called sentence that assigns each word to a sentence. This recipe shows how to analyze sentences:


from text_fabric import TF

tf = TF('path/to/your/dataset')

# Get all sentences
sentences = tf.get_sentence_objects() # adapt this to your dataset structure

for sentence in sentences:
    words_in_sentence = sentence.words
    print(f"Sentence: {tf.text(words_in_sentence)}")
    #Further analysis of the sentence, for example:
    #print(f"Number of words: {len(words_in_sentence)}")
    #print(f"Words: {[tf.features.words.word.v(w) for w in words_in_sentence]}")
This code assumes a method tf.get_sentence_objects() exists (you will need to adapt this to your specific dataset and how sentences are defined). This method retrieves sentence objects, and then we iterate through them, printing each sentence and potentially performing further analysis (such as counting words or analyzing word types within each sentence).

Recipe 3: Identifying Relationships between Words (Dependency Parsing)
Dependency parsing requires a dataset with dependency relations. This example is illustrative and assumes a feature called dephead (dependency head) and deprel (dependency relation). Adapt to your dataset's structure.


from text_fabric import TF

tf = TF('path/to/your/dataset')

# Iterate through words and print dependency information (adapt features)
for i in tf.features.words.otype.s:
    head = tf.features.words.dephead.v(i)
    relation = tf.features.words.deprel.v(i)
    word = tf.features.words.word.v(i)
    print(f"Word: {word}, Head: {head if head else 'None'}, Relation: {relation}")
This code iterates through words and prints their dependency head and relation. The if head else 'None' handles cases where a word is the root of the sentence.

Important Considerations:

Dataset Structure: The success of linguistic analysis heavily depends on the structure and features of your dataset. Make sure you understand how grammatical information is represented in your data.
Feature Names: Adapt feature names (pos, lemma, morph, sentence, dephead, deprel) according to your dataset's specifications.
Error Handling: Implement error handling (e.g., try-except blocks) to gracefully handle missing data or unexpected values.
This chapter provides a starting point for linguistic analysis. Explore the text-fabric API to discover more advanced techniques and adapt these recipes to your specific research questions and dataset.
```
