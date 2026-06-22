# 🎬 r/TrueFilm Discourse Classification Project

## 📌 Project Overview

I chose **r/TrueFilm** because it is a community dedicated to serious film discussion, where users regularly engage in analysis, criticism, interpretation, and debate about movies.

Unlike general movie subreddits, r/TrueFilm exhibits high variation in discourse style:
- structured analytical film interpretation
- emotional or subjective reactions

This makes it well-suited for a classification task that distinguishes:
- analysis (structured reasoning)
- reaction (subjective response)

The goal is to classify how users construct meaning in film discussion.

---

# 🧠 Community Choice & Reasoning

I chose r/TrueFilm because it is a community dedicated to serious film discussion, where users regularly engage in analysis, criticism, interpretation, and debate about movies. Unlike general movie subreddits, the quality and style of reasoning varies significantly across posts. Some users support their claims with detailed evidence from cinematography, narrative structure, editing, or film history, while others primarily express personal opinions or emotional reactions.

This variation makes r/TrueFilm a strong fit for a classification task because posts often discuss similar films but differ in how arguments are constructed and supported. The goal of the classifier is to learn meaningful distinctions between evidence-based analysis, unsupported opinion, and emotional reaction.

---

# Label Taxonomy

**analysis**: The post makes a structured argument supported by specific evidence from the film, such as scenes, cinematography, editing, dialogue, historical context, or comparisons.
**reaction**: The post primarily expresses an opinion, critique, judgment, or emotional response without substantial supporting evidence.
These labels are mutually exclusive: each post is assigned to exactly one category based on its dominant mode of discourse.

## 😶 analysis

### Example 1 
> “Upon returning to Barry Lyndon for the first time in years (a truly phenomenal film), I was struck by the complexity of its focus on masculinity, especially the manner in which these traits manifest in an honour culture – honour culture being a type of culture where violent reactions are appropriate in the defense of social slights and one’s image... From memory, I thought the film was mainly a straight satire... Although, I found upon this rewatching, that its treatment... is paradoxical and provides an extremely nuanced view...”

### Example 2 
> “I'm not sure I've seen a more perfectly constructed film than Rosemary's Baby. Every scene, every shot, every performance tone and note seem to work in a completely tireless movie that spends the right amount of time and emphasis required, beat by beat. The film is like a clockwork.”

---

## 😶 reaction

### Example 1 
> “It's baaaad. Soulless and flat, lacking anything resembling a vision. Characters without personality who exist only to spew trailer bait lines. We are bombarded endlessly with music and lens flares. Everything is over explained and under thought.”

### Example 2 
> “I found it to be pretty terrible mostly due to the script. The bad guys are perhaps the dumbest group of people to ever exist in film. How they managed to keep aliens a secret for so long is beyond belief.”

---

# 📦 Dataset Collection & Annotation

## Data Source
- r/TrueFilm Reddit posts

## Dataset Size
- Total: 200 posts

## Annotation Process

1. Posts collected manually into spreadsheet  
2. Initial labeling performed using Gemini (LLM-assisted labeling)  
3. All labels manually reviewed and corrected  

LLM labeling was used only for efficiency, not as final ground truth.

---

# 📊 Dataset Distribution

| Class     | Count |
|-----------|------|
| analysis  | 120  |
| reaction  | 80   |

The dataset is moderately imbalanced toward analysis, reflecting r/TrueFilm’s naturally analytical culture.

---

# ⚠️ Difficult-to-Label Cases

## 1. Rosemary’s Baby discussion

> "Rosemary's Baby was great, though I am not sure about the perfectly constructed part or how to really analyze that... I can understand why Rosemary is the most popular, because it has the most tangible plot, but all three are genius horrors."

**Why it is hard:** This post compares multiple films and discusses elements like plot, atmosphere, and psychological impact, which gives it an analytical tone. However, the discussion remains largely subjective and does not reference specific scenes, cinematography, or other concrete film evidence.

**My decision:** reaction

**Why:** The dominant mode of discourse is personal opinion and preference ("I prefered them slightly more," "it's maybe even my favorite") rather than a structured argument supported by evidence from the films.

---

## 2. 1980s comedy film era critique

> "The 1980s was really the golden age for comedy films like Airplane, Naked Gun, Christmas Vacation, etc... there really are no tentpole comedy films anymore. I don't even remember the last one, save for Naked Gun last year, which I really liked."

**Why it is hard:** The post presents a coherent argument about the evolution of comedy films and compares different eras of filmmaking. This makes it resemble analysis, but the claims are broad cultural observations rather than evidence-based examination of specific films.

**My decision:** reaction

**Why:** The argument is driven by personal judgments and general impressions about comedy as a genre, without supporting claims through detailed discussion of scenes, techniques, or narrative structure.

---

## 3. Jaws theatre viewing breakdown

> "The biggest revelation was the many town scenes in the first half... Spielberg exploits the full width of the frame to include fore, mid, and background, building layers of action... The ferry scene is a lesson in story telling... As the dialogue gets more serious, more coercive, more ominous, the blocking becomes closer, more intimate, more intense."

**Why it is hard:** The post begins as a personal reflection on seeing the film in theaters, which resembles a reaction post. However, it develops into a detailed discussion of framing, blocking, camera movement, performance, and storytelling techniques.

**My decision:** analysis

**Why:** The dominant content is a structured examination of how the film creates meaning through specific cinematic techniques and scene-level evidence, which matches the definition of analysis.


# 🤖 Models

## 🧪 Baseline Model (Zero-shot LLM)

### Prompt Used

```python
SYSTEM_PROMPT = """
You are classifying posts from r/TrueFilm.
Assign each post to exactly one of the following categories.

analysis: The post makes a structured argument supported by specific evidence from the film, such as scenes, cinematography, editing, dialogue, historical context, or comparisons.

Example:
"No Country For Old Men - The Characterization of Sheriff Bell
I just rewatched this film for the first time in years..."

reaction: The post primarily expresses an opinion, critique, judgment, or emotional response without substantial supporting evidence.

Example:
"He's my favorite contemporary Japanese director. Check out I Wish when you get a chance..."

Respond with ONLY the label name.
Do not explain your reasoning.

Valid labels:
analysis
reaction
"""
```

## Evaluation Setup

The baseline model used Groq's llama-3.3-70b-versatile in a zero-shot classification setting. The model received the label definitions for analysis and reaction along with classification instructions, but it was not trained on any r/TrueFilm examples. Performance was evaluated on the same held-out test set used for the fine-tuned DistilBERT model to enable a direct comparison between zero-shot prompting and supervised fine-tuning.

---

## Baseline Performance

| Metric      | Score |
|------------|------|
| Accuracy   | 0.70 |
| Macro F1   | 0.68 |
| Weighted F1| 0.70 |

| Label    | Precision | Recall | F1   |
|----------|----------|--------|------|
| analysis | 0.74     | 0.78   | 0.76 |
| reaction | 0.64     | 0.58   | 0.61 |

---

# 🧠 Fine-tuned Model

## Base Model
- distilbert-base-uncased

## Training Setup
- task: binary classification  
- split: 70/15/15  
- loss: cross entropy  
- epochs: 3  
- batch size: 16  
- learning rate: 2e-5  

---

## Hyperparameter Choice
A learning rate of 2e-5 was used as a standard stable value for small datasets (~200 samples), balancing convergence and overfitting risk.

---

# 📊 Evaluation Report

## Overall Performance

| Model                    | Accuracy |
|-------------------------|----------|
| Baseline (LLM)         | 0.70     |
| Fine-tuned DistilBERT  | 0.60     |

---

## Confusion Matrix

| True \ Predicted | analysis | reaction |
|------------------|----------|----------|
| analysis         | 18       | 0        |
| reaction         | 12       | 0        |

---

## Per-Class Metrics

| Label    | Precision | Recall | F1   | Support |
|----------|----------|--------|------|---------|
| analysis | 0.60     | 1.00   | 0.75 | 18      |
| reaction | 0.00     | 0.00   | 0.00 | 12      |

---

# 🔍 Error Analysis

The primary failure mode of the fine-tuned model was its tendency to predict the analysis label for nearly every test example. As shown in the confusion matrix, all reaction posts were misclassified as analysis, while no analysis posts were predicted as reaction.

Several factors likely contributed to this behavior. First, the dataset contained more analysis examples than reaction examples, creating a bias toward the majority class during training. Second, many reaction posts still contain film-related vocabulary such as references to scripts, directing, cinematography, or characters. Because these terms frequently appeared in analysis posts, the model may have learned to associate film-specific language with analysis regardless of whether the post contained actual supporting evidence.

The error pattern suggests that the model learned surface-level linguistic cues rather than the intended distinction between structured reasoning and subjective evaluation. As a result, criticism and opinionated commentary were often interpreted as analytical discussion.


---

### Example 1

> "I found it to be pretty terrible mostly due to the script. The bad guys are perhaps the dumbest group of people to ever exist in film. How they managed to keep aliens a secret for so long is beyond belief."

- **True Label:** reaction
- **Predicted Label:** analysis
- **Confidence:** 0.52

**Analysis:** The model likely associated discussion of the script and plot logic with analytical reasoning. However, the post primarily expresses a personal opinion and does not support its claims with detailed evidence from specific scenes or filmmaking techniques. According to the labeling guidelines, this is a reaction rather than analysis.

### Example 2

> "I just watched a short video essay last night which also argued that both were human. I’m not 100% convinced there is an answer..."

- **True Label:** reaction
- **Predicted Label:** analysis
- **Confidence:** 0.54

**Analysis:** The model appears to treat discussion of fan theories and interpretations as analysis. However, the author is mainly expressing uncertainty and speculation rather than constructing an evidence-based argument grounded in the film itself.

### Example 3

> "But yeah, his direction is great in Frailty. My one complaint is the last 10 minutes has like 10 twists and it gets a little hard to follow..."

- **True Label:** reaction
- **Predicted Label:** analysis
- **Confidence:** 0.54

**Analysis:** References to directing and plot structure likely triggered the analysis label. However, the comment is primarily an evaluation of the viewing experience and lacks the detailed supporting evidence required for analysis.

---
After reviewing the 12 misclassified examples and using an AI tool to identify patterns, I found that every error involved a reaction post being incorrectly classified as analysis. The AI suggested that the model was likely relying on surface-level features such as plot discussion, character references, film theories, and argumentative language, and manual review confirmed this pattern. Many of the posts were relatively detailed and discussed interpretations of films, which made them appear analytical, but they ultimately expressed opinions, critiques, or speculation without supporting claims through specific evidence such as scenes, cinematography, editing, dialogue, or historical context. I initially considered whether post length was the primary cause, but several long posts were still clearly reactions, so I discarded that explanation. Overall, the baseline model struggled to distinguish between evidence-based reasoning and opinion-driven discussion, causing it to overpredict the analysis label whenever a post sounded thoughtful or interpretive.

# 🧠 Model Interpretation

## Intended Behavior

The goal of the classifier was to distinguish between evidence-based film analysis and subjective reactions. Analysis posts were intended to be identified by the presence of specific references to scenes, cinematography, narrative structure, dialogue, or other forms of concrete support. Reaction posts were intended to capture personal opinions, emotional responses, and evaluations that lacked substantial supporting evidence.

## Learned Behavior

The fine-tuned model appears to have learned a much simpler pattern than intended. Rather than distinguishing between evidence and opinion, it often treated longer, more structured posts as analysis regardless of whether they contained actual supporting evidence. As a result, many reaction posts that referenced film-related concepts or contained detailed criticism were incorrectly classified as analysis. The confusion matrix shows that the model predicted every test example as analysis, suggesting a strong bias toward the majority class. This indicates that the model relied heavily on surface-level linguistic features instead of learning the deeper distinction between structured argumentation and subjective reaction that the label taxonomy was designed to capture.
---

# 🚨 Key Failure Mode

- representation collapse  
- majority class bias  
- weak boundary between emotional critique vs analysis  

---

# 🚀 Improvements

- increase reaction dataset size  
- expand dataset beyond 200 samples
- include more borderline examples  
- improve annotation rules  

---

# 📚 Reflection

## What Worked

The project successfully produced a usable classification pipeline and demonstrated that the label taxonomy could be applied consistently across a real-world dataset. The annotation process became more reliable after establishing clear rules for distinguishing evidence-based analysis from subjective reaction. The strong performance of the zero-shot baseline also suggests that the label definitions were understandable and meaningful.

## What Didn't Work

The fine-tuned DistilBERT model underperformed the zero-shot baseline and failed to learn the reaction class effectively. Rather than identifying whether a post contained supporting evidence, the model appeared to rely on superficial features such as post length, structure, and the presence of film-related terminology. This led to a strong bias toward predicting analysis.

This outcome highlights an important limitation of small datasets for subjective classification tasks. Although the labels were conceptually clear, 200 examples were not sufficient for the model to learn the subtle distinction between analytical reasoning and opinion-based commentary.


---

# 🧾 Spec Reflection

## What helped

- Having taxonomy made annotation consistent and reduced ambiguity between `analysis` and `reaction`
- Structuring the project before modeling helped clarify decision boundaries and evaluation criteria

## What diverged

- LLM-assisted labeling introduced initial bias toward the `analysis` class, requiring manual correction during review
- The dataset was slightly imbalanced, with more `analysis` examples than initially intended
- Some borderline cases made the labeling subjective

---



# 🤖 AI Usage Disclosure

I used Gemini to assist with initial label annotation for the r/TrueFilm dataset by generating preliminary classifications between `analysis` and `reaction` based on the provided taxonomy. I reviewed all outputs and corrected a subset of cases where the model over-labeled opinion-based or critique-heavy posts as `analysis`, particularly when they contained references to plot details or film elements without supporting evidence. However, many of the generated labels were already accurate and did not require modification, so the final dataset reflects a combination of AI-assisted labeling and human verification.For error analysis, I used ChatGPT to examine misclassified predictions and identify recurring patterns in the model’s mistakes. I directed it to analyze why `reaction` posts were frequently predicted as `analysis`, and it helped surface trends such as over-reliance on plot/character mentions and interpretive language. I also used ChatGPT to support the structuring and formatting of the final report, including organizing markdown sections and refining evaluation tables for clarity. While I incorporated most of the structural suggestions, I revised wording for accuracy and removed any content that did not align with the actual evaluation results. The final write-up and conclusions were manually checked to ensure consistency with the dataset and model outputs.
---

# ✅ Final Conclusion

This project shows that:

- LLMs can outperform small fine-tuned models on limited datasets  
- discourse classification depends on subtle linguistic boundaries  
- emotional vs analytical distinction remains a challenging NLP task even within a constrained domain  
