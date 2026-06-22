Community: What community did you choose and why? Why is this community a good fit for a classification task — what makes the discourse varied enough to be interesting?
I chose r/TrueFilm because it is a community dedicated to serious film discussion, where users regularly engage in analysis, criticism, interpretation, and debate about movies. Unlike general movie subreddits, the quality and style of reasoning vary significantly across posts: some users support their claims with detailed evidence from cinematography, narrative structure, or film history, while others primarily express personal opinions or emotional reactions. This variation makes r/TrueFilm an excellent community for a classification task because posts often discuss similar films but differ in how arguments are constructed and supported. As a result, the classifier can learn meaningful distinctions between evidence-based analysis, unsupported hot takes, and personal reactions, which are discourse patterns that community members themselves recognize and value differently.

Labels: What are your 2–4 labels? Define each in a complete sentence. Include 2 example posts per label.

| Label        | Definition                                                                                                                                                                |
| ------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **analysis** | The post makes a structured argument supported by specific evidence from the film, such as scenes, cinematography, editing, dialogue, historical context, or comparisons. |
| **reaction** | The post primarily expresses an opinion, critique, judgment, or emotional response without substantial supporting evidence.                                               |

Hard edge cases: What type of post will be genuinely ambiguous between two labels? How will you handle it when you encounter it during annotation?
Data collection plan: Where will you collect examples? How many per label? What will you do if a label is underrepresented after 200 examples?
Evaluation metrics: Which metrics will you use to evaluate your model and why are those the right ones for this specific task? (Accuracy alone is not enough — explain what else you need and why.)
Definition of success: What performance would make this classifier genuinely useful? What would you accept as "good enough" for deployment in a real community tool?

