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

## Installation

To set up the project, clone the repository and install the necessary dependencies.

```bash
git clone https://github.com/yourusername/english-grammar-mcc-exercises.git
cd english-grammar-mcc-exercises
pip install -r requirements.txt
```

## Usage

1. **Data Preparation**: Prepare the input text data and clean it following the project’s guidelines to ensure quality.
2. **Fine-Tuning the Model**: Use the provided dataset to fine-tune an LLM for MCC exercise generation on specific grammar topics. See [Fine-Tuning Instructions](fine_tuning.md) for details.
3. **Generating Exercises**: Run the generation script to produce exercises, specifying grammar topics and the number of distractors.
4. **Evaluation**: Use the provided evaluation metrics to assess the structural and linguistic quality of the generated exercises.

## Evaluation

Two main evaluation criteria are used:
- **Structural Compliance**: Ensures the exercise structure is correct and distractors are homogeneous, plausible, and unambiguous.
- **Linguistic Diversity**: Uses SELF-BLEU to minimize repetitive text and maintain originality across exercises.

Results have shown that the model achieves approximately 85% structural compliance and limited text repetition, with a strong correlation between automatic and human evaluations.

## Contributing

Contributions are welcome! Please refer to the `CONTRIBUTING.md` for guidelines on how to help improve the project.

## License

This project is licensed under the Creative Commons Attribution 4.0 International License. See [LICENSE](LICENSE) for details.