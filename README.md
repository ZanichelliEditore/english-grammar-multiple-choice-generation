# Generation and Evaluation of English Grammar Multiple-Choice Cloze Exercises

This repository provides an implementation for the generation and evaluation of English grammar multiple-choice cloze (MCC) exercises. MCC exercises are crucial in language learning, as they help improve grammatical proficiency and comprehension by requiring learners to select the correct word or phrase to complete a sentence. This project automates MCC exercise creation using Large Language Models (LLMs), reducing the need for extensive manual work from language experts.

## Overview

The goal of this project is to explore the use of LLMs for automatically generating MCC exercises that are both contextually appropriate and challenging. The generated exercises include:
- **Body**: A sentence with a blank indicating the missing word or phrase.
- **Key**: The correct answer to fill in the blank.
- **Distractors**: Plausible but incorrect options that enhance the difficulty and maintain learner engagement.

This repository includes a dataset of MCC exercises, metrics for evaluating exercise quality, and a fine-tuning process for an LLM-based generator.

## Key Features

- **Automated MCC Generation**: Uses LLMs to create grammar exercises on 19 topics without predefined limitations, enhancing diversity and creativity in generated items.
- **Dataset Curation**: A comprehensive dataset curated with 19 distinct grammar topics, tailored to ensure diverse and accurate representations.
- **Evaluation Metrics**: Includes automatic metrics, validated against human judgment, to assess structural compliance and linguistic diversity in generated exercises.

## Usage

1. Load the model and tokenizer.
   ```
   import unsloth
   from unsloth import FastLanguageModel
   from transformers import TextStreamer
   
   model, tokenizer = FastLanguageModel.from_pretrained(
       model_name = "/english-grammar-multiple-choice-generation/model/", #insert model directory here
       max_seq_length = 2048,
       dtype = None,
       load_in_4bit = True,
   )
   FastLanguageModel.for_inference(model)                                           
   ```
2. Generate MCC Exercises.
   ```
   prompt = "Write a fill the gap exercise on {topic} with {n_distractors}."

   topics = [
       'relatives', 'future_simple', 'persobal_pronouns',
       'past_simple', 'passive', 'non_finites', 'present_perfect',
       'past_perfect', 'modals', 'possessives',
       'present_continuous', 'articles', 'present_simple', 'quantifiers',
       'comparisons', 'conditionals', 'prepositions',
       'past_continuous', 'wh_questions'
   ]
   
   inputs = tokenizer(
       [
           prompt.format(
               topic = "past_simple",  
               n_distractors = "3"
           )
       ],
       return_tensors="pt",
   ).to("cuda")
   text_streamer = TextStreamer(tokenizer)
   res = model.generate(**inputs, streamer=text_streamer, max_new_tokens=250, temperature=0.7)
   ```

## Evaluation

Two main evaluation criteria are used:
- **Structural Compliance**: Ensures the exercise structure is correct and distractors are homogeneous, plausible, and unambiguous.
- **Linguistic Diversity**: Uses SELF-BLEU to minimize repetitive text and maintain originality across exercises.

Results have shown that the model achieves approximately 85% structural compliance and limited text repetition, with a strong correlation between automatic and human evaluations.

## License

This project is licensed under the Creative Commons Attribution 4.0 International License. See [LICENSE](LICENSE) for details.

## Acknowledgements

We wish to thank Zanichelli editore for their support which enabled data up-sampling, human evaluation, and experimentation with their infrastructure. 
We also thank Eleonora Cupin for her valuable contribution to the human evaluation of the dataset.
This work was partially supported by project FAIR: Future Artificial Intelligence Research (European Commission NextGeneration EU programme, PNRR-M4C2-Investimento 1.3, PE00000013-"FAIR" - Spoke 8).
