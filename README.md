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

0. Clone the repo and install requirements.
   ```
   git clone https://github.com/ZanichelliEditore/english-grammar-multiple-choice-generation.git
   # move into the repo
   cd english-grammar-multiple-choice-generation
   git lfs pull
   pip install -r requirements.txt
   ```
2. Load the model and tokenizer.
   ```
   import unsloth
   from unsloth import FastLanguageModel
   from transformers import TextStreamer
   
   model, tokenizer = FastLanguageModel.from_pretrained(
       model_name = "/english-grammar-multiple-choice-generation/model/", # insert model directory here
       max_seq_length = 2048,
       dtype = None,
       load_in_4bit = True,
   )
   FastLanguageModel.for_inference(model)                                           
   ```
3. Generate MCC Exercises.
   ```
   prompt_format ='<|begin_of_text|><|start_header_id|>user<|end_header_id|>\n\nWrite a fill the gap exercise on {grammar_topic} with {n_distractors} distractors.<|eot_id|><|start_header_id|>assistant<|end_header_id|>\n\n'
   topics = [
       'relatives', 'future simple', 'personal pronouns',
       'past simple', 'passive', 'non finites', 'present perfect',
       'past perfect', 'modals', 'possessives',
       'present continuous', 'articles', 'present simple', 'quantifiers',
       'comparisons', 'conditionals', 'prepositions',
       'past continuous', 'wh questions'
   ]
   inputs = tokenizer(
       [
           prompt_format.format(
               grammar_topic='present simple',
               n_distractors=3
           )
       ],
       return_tensors="pt",
   ).to("cuda")
   text_streamer = TextStreamer(tokenizer)
   res = model.generate(**inputs, streamer=text_streamer, max_new_tokens=250, temperature=0.7)
   exercise = tokenizer.batch_decode(res)[0].split("<|start_header_id|>assistant<|end_header_id|>")[1]
   print(exercise)
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

## Citation
```
   @inproceedings{donati-etal-2024_MCC_Generation,
       title = "Generation and Evaluation of {E}nglish Grammar Multiple-Choice Cloze Exercises",
       author = "Donati, Nicol{\`o}  and
         Periani, Matteo  and
         Di Natale, Paolo  and
         Savino, Giuseppe  and
         Torroni, Paolo",
       editor = "Dell'Orletta, Felice  and
         Lenci, Alessandro  and
         Montemagni, Simonetta  and
         Sprugnoli, Rachele",
       booktitle = "Proceedings of the 10th Italian Conference on Computational Linguistics (CLiC-it 2024)",
       month = dec,
       year = "2024",
       address = "Pisa, Italy",
       publisher = "CEUR Workshop Proceedings",
       url = "https://aclanthology.org/2024.clicit-1.39/",
       pages = "325--334",
       ISBN = "979-12-210-7060-6",
       abstract = "English grammar Multiple-Choice Cloze (MCC) exercises are crucial for improving learners' grammatical proficiency andcomprehension skills. However, creating these exercises is labour-intensive and requires expert knowledge. Effective MCCexercises must be contextually relevant and engaging, incorporating distractors{---}plausible but incorrect alternatives{---}tobalance difficulty and maintain learner motivation. Despite the increasing interest in utilizing large language models (LLMs)in education, their application in generating English grammar MCC exercises is still limited. Previous methods typicallyimpose constraints on LLMs, producing grammatically correct yet uncreative results. This paper explores the potentialof LLMs to independently generate diverse and contextually relevant MCC exercises without predefined limitations. Wehypothesize that LLMs can craft self-contained sentences that foster learner`s communicative competence. Our analysisof existing MCC exercise datasets revealed issues of diversity, completeness, and correctness. Furthermore, we addressthe lack of a standardized automatic metric for evaluating the quality of generated exercises. Our contributions includedeveloping an LLM-based solution for generating MCC exercises, curating a comprehensive dataset spanning 19 grammartopics, and proposing an automatic metric validated against human expert evaluations. This work aims to advance theautomatic generation of English grammar MCC exercises, enhancing both their quality and creativity."
   }
```
