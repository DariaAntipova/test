# Multi-Agent Translation Workflow
This project implements a multi-agent translation workflow using open-source technologies to deliver high-quality machine translation with terminology management.

# Workflow Steps
## Step 0. Configure Anthropic API integration
Set up variables and functions for API calls to Anthropic.

Translation direction: Italian to English (Romance language to Germanic language).

LLM selection: this notebook uses Anthropic's claude-3-5-sonnet-20240620 for glossary extraction, translation, and post-editing, chosen for its demonstrated accuracy in English translation tasks and precise instruction following.

## Step 1. Create test dataset
Select 500 segments from open-source legal corpora ([ELRC-845](https://opus.nlpl.eu/ELRC-845-Corpora_legal_text/it&en/v1/ELRC-845-Corpora_legal_text)), filtering for segments with > 3 words to ensure meaningful content.

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
  
The increased number of deviations in post-editing (13 vs 22) primarily stems from style adaptations required for informal tone.
