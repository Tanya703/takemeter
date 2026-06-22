# r/TrueFilm Discourse Classification — Planning Document

## Community

I chose r/TrueFilm because it is a community dedicated to serious film discussion, where users regularly engage in analysis, criticism, interpretation, and debate about movies. Unlike general movie subreddits, the quality and style of reasoning varies significantly across posts. Some users support their claims with detailed evidence from cinematography, narrative structure, editing, or film history, while others primarily express personal opinions or emotional reactions.

This variation makes r/TrueFilm a strong fit for a classification task because posts often discuss similar films but differ in how arguments are constructed and supported. The goal of the classifier is to learn meaningful distinctions between evidence-based analysis, unsupported opinion, and emotional reaction.

---

## Label Taxonomy

### Labels

- **analysis**: The post makes a structured argument supported by specific evidence from the film, such as scenes, cinematography, editing, dialogue, historical context, or comparisons.
- **reaction**: The post primarily expresses an opinion, critique, judgment, or emotional response without substantial supporting evidence.

These labels are mutually exclusive: each post is assigned to exactly one category based on its dominant mode of discourse.

---

## Examples

### analysis

- *No Country For Old Men – The Characterization of Sheriff Bell*: compares film and novel characterization and discusses themes, narrative structure, and meaning of the ending.  
- *The loss of hapticness in modern cinema*: argues that digital filmmaking reduces tactile cinematic quality, supported with comparisons across directors, technologies, and films.

### reaction

- *Nymphomaniac Vol I & II – profound, extreme filmmaking...and not what I was expecting*: focuses on personal viewing experience and emotional response.  
- *Disclosure Day — almost great, and that almost really hurts*: centers on disappointment and emotional reaction to the film.

---

## Hard Edge Cases

Some posts fall between analysis and reaction.

**Example:**

> “Aftersun completely devastated me because its portrayal of depression felt painfully realistic.”

This case is ambiguous because it contains both emotional reaction and interpretive language.

### Decision Rule

- If the post is primarily emotional or opinion-driven → **reaction**
- If the post is supported by structured film evidence → **analysis**

In ambiguous cases, the dominant intent determines the label. In the example above, the emotional response is dominant, so it is labeled **reaction**.

---

## Data Collection Plan

Data will be collected from r/TrueFilm using naturalistic sampling methods:

- browsing discussion threads directly in r/TrueFilm  
- searching specific film titles (e.g., *Aftersun*, *No Country for Old Men*)  
- sampling across different thread types (discussion posts, reviews, interpretation threads)  

This approach ensures variation in writing style, topic, and depth of analysis.

---

## Dataset Size

I will collect at least 200 labeled examples:

- ~100 **analysis**
- ~100 **reaction**

The dataset will be kept approximately balanced to avoid class bias during training.

---

## Handling Class Imbalance

If one label is underrepresented after initial collection, I will adjust sampling:

- If **analysis** is underrepresented: focus on essay-style and interpretation-heavy posts  
- If **reaction** is underrepresented: focus on casual discussion and review-style posts  

If imbalance persists after 200 examples, I will continue collecting until both classes are within ~10–15% of each other.

---

## Evaluation Metrics

Model performance will be evaluated using multiple metrics:

- **Accuracy**: overall correctness on the test set  
- **Precision and recall (per class)**: class-level performance measurement  
- **Macro F1 score**: balanced performance across both labels  
- **Confusion matrix**: identification of systematic misclassification patterns  

Accuracy alone is insufficient because it does not capture class imbalance or error distribution across labels.

---

## Definition of Success

The classifier is considered successful if it meets the following thresholds:

- Overall accuracy ≥ 85%  
- Macro F1 ≥ 0.85  
- Precision and recall for each class ≥ 0.80  

Additionally, the model should not show systematic bias toward one label (e.g., over-predicting “reaction”), as this would reduce practical usefulness even if accuracy is high.

Success is defined not only by numeric performance but also by whether the model consistently distinguishes structured analysis from emotional or opinion-based reaction.

---

## Evaluation Plan Review

The evaluation plan is fully objective. All metrics are computed on a fixed test set, and success thresholds are explicitly defined. This allows for clear determination of whether the model meets deployment requirements without subjective interpretation.

---

## AI Tool Plan

### Label Stress Testing

An LLM will be used to generate 5–10 borderline examples between analysis and reaction based on the label definitions. These examples will be used to test whether the taxonomy is clearly separable. If ambiguity is high, label definitions will be refined before dataset collection.

### Annotation Assistance

An LLM may be used to pre-label a portion of the dataset. All labels will be manually reviewed and corrected if necessary. AI-generated labels will be tracked separately and treated as suggestions only.

### Failure Analysis

After model evaluation, misclassified examples will be analyzed using an LLM to identify patterns such as:

- over-reliance on emotional language cues  
- long posts being incorrectly classified as analysis  
- implicit evidence-based reasoning being missed  

All findings will be manually validated before being included in the final evaluation discussion.
