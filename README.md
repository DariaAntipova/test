# Multi-Agent Translation Workflow
This project implements a multi-agent translation workflow using open-source technologies to deliver high-quality machine translation with terminology management.

# Workflow Steps
## Step 0. Configure Anthropic API integration
Set up variables and functions for API requests to Anthropic.

Translation direction: Italian to English (Romance language to Germanic language).

LLM selection: this implementation uses Anthropic's claude-3-5-sonnet-20240620 for glossary extraction, translation, and post-editing, chosen for its demonstrated accuracy in English translation tasks and precise instruction following.

## Step 1. Create test dataset
Select 500 segments from open-source legal corpora ([ELRC-845](https://opus.nlpl.eu/ELRC-845-Corpora_legal_text/it&en/v1/ELRC-845-Corpora_legal_text)), filtering for segments with > 3 words to ensure meaningful content.

## Step 2. Generate glossary based on test dataset
Extract 50 key terms and their stems from the test set to create a terminology reference for translation, post-editing, and calculation of terminology adherence rate.

## Step 3. Perform glossary-guided translation
Translate source segments in batches of 50, incorporating relevant glossary terms to ensure terminology consistency.

## Step 4. Apply glossary-guided style post-editing
Transform translations to use informal tone and gender-neutral style, processing in 50-segment batches with relevant glossary terms to ensure terminology consistency.

## Step 5. Evaluate terminology adherence
Terminology adherence rate is calculated based on presence of English term stems in stemmed translations and post-edited versions.

## Step 6. Assess translation and post-editing quality
Generate COMET scores for both translations and post-edited versions, visualizing quality changes through a comparative graph.

# Quality Analysis
## Terminology Adherence Rate
## Translation Adherence:
  Total (segments): 249
  
  Adherent: 236
  
  Non-Adherent: 13
  
  Adherence %: 94.78


## Post-Editing Adherence:
  Total (segments): 249
  
  Adherent: 227
  
  Non-Adherent: 22
  
  Adherence %: 91.16
  
## Analysis of Non-Adherence Cases:
1.	Translation (13 segments):
   
-	Stem matching issues: Different words sharing the same stem in Italian map to different English terms.
  
  Example: 'testimon' stem in Italian:

  o	testimonianza → testimony

  o	testimone → witness

  The algorithm expects 'testimoni — witnesses', causing false non-adherence flags

-	Minor reformulations:
  
  Example: 'reati di corruzione' translated as 'offense of corruption' instead of glossary's 'corruption offenses'

2.	Post-editing (22 segments):
   
-	Same issues as in translation
  
-	Additional reformulations due to formal-to-informal tone conversion requirement
  
The increased number of deviations in post-editing (13 vs 22) primarily results from style adaptations required for informal tone.

## Impact of Post-Editing on Translation Quality
The COMET score comparison reveals a decline in translation quality following post-editing, primarily attributable to two factors:

1.	Formality level mismatch: legal texts traditionally require formal language, whereas the post-edited versions adopted a more informal tone, diverging from conventional legal writing standards.
   
2.	Gender-neutral language: the implementation of gender-neutral terminology, while promoting inclusivity, was likely flagged as semantic deviation from the source text.
   
## Examples of Formality Changes

Single word/phrase modifications:

- subsequent → later
- in conjunction → together
- obtain → get
- requested → asked
- children → kids
- it will → it'll
- are unable → can't
- verify → check

Complex sentence modifications:

Translation (formal): "to verify the effective implementation of the plan and its suitability, as well as to propose modifications to it when significant violations of the requirements are ascertained or when changes occur in the organization or activities of the administration;"

Post-editing (informal): "to check if the plan is working well and fits, and to suggest changes if there are big rule breaks or if the organization or what it does changes;"

## Gender-neutral language adaptations:

- his or her → their
- men → partners
- she	→ they

# Deliverables
1. Working prototype: "antipova_test_task_intento.ipynb"

2. Excel spreadsheets:
   
•	"legal_it-en.xlsx"

   Contains open-source legal corpora ([ELRC-845](https://opus.nlpl.eu/ELRC-845-Corpora_legal_text/it&en/v1/ELRC-845-Corpora_legal_text)) in Italian and English.
   
•	"term_adherence_df.xlsx"

   Contains sources, references, glossary terms, English term stems, translations and post-edits with their stemmed versions as well as term adherence flags for each segment with terms (True/False).
   
•	"translation_post_edit_df.xlsx"

   Contains sources, references, translations, post-edits, and COMET scores for both steps.

3. Documentation on workflow architecture and quality analysis: "README.md"

4. Graph: "comet_scores.png"
   
   Showcases COMET score improvements/degradations between translation and post-editing.

