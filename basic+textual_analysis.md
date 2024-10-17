---
title: Basic Textual Analysis
---

This chapter provides recipes for performing basic textual analysis tasks using `text-fabric`.  We'll cover fundamental techniques such as word counting, searching for specific words or phrases, and identifying word frequencies.  These are building blocks for more advanced analyses.

We assume you've already installed `text-fabric` and have a dataset loaded.  If not, refer to the [Core Concepts](core_concepts.md) chapter.  Replace `"path/to/your/dataset"` with the actual path to your dataset.


## Recipe 1: Counting Word Occurrences

This recipe demonstrates how to count the total number of words in a text.

```python
from text_fabric import TF

tf = TF('path/to/your/dataset')

# Get the number of words
total_words = len(tf.features.words.otype.s)

print(f"Total number of words: {total_words}")
This code leverages the text-fabric API to directly access the number of words in the dataset. The tf.features.words.otype.s attribute provides a sequence of all word nodes. The len() function gives us the count.

Recipe 2: Finding All Occurrences of a Specific Word
This recipe shows how to find all occurrences of a specific word within the text.


from text_fabric import TF

tf = TF('path/to/your/dataset')

target_word = "exampleWord"  # Replace with your target word

# Find occurrences (assuming 'word' feature contains the word forms)
occurrences = tf.search(tf.L.word == target_word)

print(f"Occurrences of '{target_word}':")
for occurrence in occurrences:
    print(f"- Node: {occurrence}, Context: {tf.text(occurrence)}") #tf.text() needs suitable configuration in your dataset
This code uses the tf.search() function, a powerful tool for querying the dataset. We search for nodes where the word feature (replace with the appropriate feature name if needed) matches our target_word. The loop then iterates through the results, printing the node IDs and their context (you may need to adapt how context is obtained based on your dataset structure).

Recipe 3: Identifying Word Frequencies
This recipe shows how to calculate the frequency of each word in the text.


from text_fabric import TF
from collections import Counter

tf = TF('path/to/your/dataset')

# Extract all words (replace 'word' with your word feature if different)
words = [tf.features.words.word.v(i) for i in tf.features.words.otype.s]

# Count word frequencies
word_counts = Counter(words)

print("Word Frequencies:")
for word, count in word_counts.most_common():
    print(f"- {word}: {count}")
This recipe utilizes the collections.Counter object for efficient frequency counting. We first extract all words from the dataset and then use Counter to count the occurrences of each word. The most_common() method displays the words in descending order of frequency.

Recipe 4: Searching for Phrases
Finding specific phrases requires a slightly more sophisticated approach, depending on how your dataset represents phrases. Assuming your dataset has a feature identifying phrases or sentence boundaries:


from text_fabric import TF

tf = TF('path/to/your/dataset')

target_phrase = "example phrase" # Replace with your target phrase

# Adapt this part according to how phrases are represented in your dataset
# This example assumes a 'phrase' feature indicating phrase boundaries
occurrences = tf.search(tf.L.phrase == target_phrase)

print(f"Occurrences of '{target_phrase}': {len(occurrences)}") # or print the occurrences as in Recipe 2
This example shows a basic search; adapting the search logic to your specific data structure is crucial. You might need to search for sequences of nodes that match the words in the target phrase.

Remember to adapt these recipes to your specific dataset and feature names. Consult the text-fabric documentation for details on accessing features and navigating your data.
```
