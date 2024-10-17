---
title: Textual Criticism with text-fabric
---

This chapter explores how to perform textual criticism using `text-fabric`. Textual criticism involves comparing different versions of a text to identify variations and assess their significance.  This requires a dataset that includes multiple versions of the same text, often represented as different "witnesses" or "variants".

We assume your dataset is structured to allow comparison between different versions.  This might involve separate feature values for each version or a structure that explicitly links corresponding nodes across versions.  Adapt the code examples to your specific dataset structure and feature names.  Replace `"path/to/your/dataset"` with your dataset's path.


## Recipe 1: Identifying Variant Readings

This recipe demonstrates how to identify variant readings between two versions of a text.  We'll assume your dataset has features indicating the text of each version (e.g., `version1_text`, `version2_text`).

```python
from text_fabric import TF

tf = TF('path/to/your/dataset')

def compare_versions(node):
    """Compares the text of two versions at a given node."""
    text1 = tf.features.words.version1_text.v(node) #replace with your feature names
    text2 = tf.features.words.version2_text.v(node) #replace with your feature names
    if text1 != text2:
        print(f"Node {node}: Version 1: {text1}, Version 2: {text2}")


# Iterate through all nodes and compare versions
for node in tf.features.words.otype.s:
    compare_versions(node)
This code iterates through all word nodes and uses the compare_versions function to highlight discrepancies between the two versions. You'll need to adapt the feature names (version1_text, version2_text) to match your dataset.

Recipe 2: Variant Frequency Analysis
This recipe focuses on identifying the frequency of specific variants. Assume a feature called variant that indicates a variant reading (and potentially a feature indicating the 'base' reading).


from text_fabric import TF
from collections import Counter

tf = TF('path/to/your/dataset')

# Extract all variants
variants = [tf.features.words.variant.v(i) for i in tf.features.words.otype.s if tf.features.words.variant.v(i) is not None]

# Count variant frequencies
variant_counts = Counter(variants)

print("Variant Frequencies:")
for variant, count in variant_counts.most_common():
    print(f"- {variant}: {count}")
This code extracts all variant readings, counts their frequencies using Counter, and presents the results. Adapt the code to handle how variants are represented in your data. Consider adding error handling to deal with missing values for the variant feature.

Recipe 3: Comparing Specific Passages
This recipe demonstrates comparing specific passages across versions. Assume you have a way to identify passage boundaries in your dataset (e.g., a passage feature).


from text_fabric import TF

tf = TF('path/to/your/dataset')

passage_id = 123 # Replace with the ID of the passage you want to compare

# Get nodes belonging to the passage (adapt to your dataset's structure)
passage_nodes = tf.search(tf.L.passage == passage_id)

# Compare versions for each node in the passage using the compare_versions function from Recipe 1
for node in passage_nodes:
    compare_versions(node) # Assuming compare_versions is defined as in Recipe 1.
This focuses comparison on a specific passage identified by passage_id. You need to replace this with a relevant ID and adapt the code to correctly retrieve nodes within that passage based on your dataset structure.

Important Considerations:
Dataset Structure: The effectiveness of these recipes depends heavily on how your dataset represents multiple versions of the text and variant readings. Clearly understand your dataset's structure before applying these techniques.
Feature Names: Adapt all feature names (version1_text, version2_text, variant, passage) to match your dataset's actual feature names.
Data Cleaning: Preprocessing your data to handle missing values or inconsistencies is crucial for accurate analysis.
Advanced Techniques: More advanced textual criticism might involve using phylogenetic methods or statistical approaches to analyze variant relationships and reconstruct ancestral texts. These techniques often require specialized packages and algorithms beyond the scope of this basic introduction.
This chapter offers a foundation for textual criticism using text-fabric. Adapt these recipes to your specific dataset and explore the text-fabric API for more advanced techniques.
```
