# gemma-3-4b-vlm-in-the-wild

## Overview
This Jupyter notebook demonstrates how to evaluate Google's Gemma-3-4b-it model on Japanese vision-language tasks using the "SakanaAI/JA-VLM-Bench-In-the-Wild" dataset. The notebook implements a pipeline to process images with associated Japanese questions and generate answers using the vision-language model.

## Features
- Loads Google's Gemma-3-4b-it multimodal model
- Processes the JA-VLM-Bench-In-the-Wild dataset containing real-world Japanese images and questions
- Runs inference on the test dataset
- Saves evaluation results to a JSONL file for further analysis

## Requirements
- transformers
- accelerate
- datasets
- torch
- PIL

## Dataset
The SakanaAI/JA-VLM-Bench-In-the-Wild dataset contains real-world Japanese image-question-answer triplets. Each example includes:
- page_url: Source webpage URL
- image_url: URL of the image
- image: The actual image data
- question: A question in Japanese about the image
- answer: The ground truth answer in Japanese

## Usage
1. Install the required libraries
2. Load the model and processor
3. Load the dataset
4. Run the inference pipeline on the test set
5. Analyze the results in the generated JSONL file

## Output File(gemma3_inference_vlm_wild.jsonl)
The notebook generates a JSONL file containing:
- Original page and image URLs
- Questions about the images
- Ground truth answers
- Model-predicted answers

## Inference Details
The model uses a system prompt indicating it's a Japanese vision-language assistant and provides the image along with the question for generating appropriate answers.
"""

# Print the content
print(readme_content)
