# gemma-3-4b-vqa

This repository provides sample code for performing Japanese Vision-Language (VQA: Visual Question Answering) inference using the [SakanaAI/JA-VG-VQA-500](https://huggingface.co/datasets/SakanaAI/JA-VG-VQA-500) dataset and Google's Gemma3 model ([google/gemma-3-4b-it](https://huggingface.co/google/gemma-3-4b-it)).

---

## File Structure

- **main.py**  
  A sample script that retrieves images and corresponding multiple Q&A from the JA-VG-VQA-500 dataset, passes them to Gemma3, and saves the model outputs in JSONL format.

---

## Environment Setup Instructions

### 1. Install Python
Python version 3.8 or higher is recommended (versions 3.7 and below may encounter issues with some libraries).

### 2. Install Required Packages
```bash  
git clone <repository URL>  
cd gemma-3-4b-vqa  
```

The script sequentially reads (image, question) pairs from the test split of JA-VG-VQA-500.

To avoid EXIF processing errors from Pillow (PIL), images are loaded as raw bytes by specifying `decode=False`.

The binary image is temporarily saved as a file (`temp_***.jpg`) and passed to Gemma3 for inference using its file path.

---

### Output File (gemma3_output.jsonl)
Inference results are saved to `gemma3_output.jsonl`. Each entry is JSON-formatted with the following fields:

- `image_id`: ID of the input image
- `qa_id`: Unique ID for each question
- `url`: External URL of the image
- `question`: Question text
- `pred_answer`: Gemma3's inferred answer
- `gt_answer`: Ground truth answer from the dataset (for comparison)

Output Example (one QA per JSON line):
```json
{
  "image_id": 100,
  "qa_id": 10674688,
  "url": "https://example.com/image.jpg",
  "question": "What is the weather like?",
  "pred_answer": "Sunny",
  "gt_answer": "Clear weather"
}
```

The Gemma3 4B model requires substantial GPU memory. If you encounter memory issues, consider using `device_map="auto"` to automatically split processing between CPU and GPU, or use an instance with more GPU memory.

Temporary File Cleanup
The script attempts to delete the temporary `temp_***.jpg` files after inference, but files might remain depending on your environment. Manually clean these up if necessary.

### License
The JA-VG-VQA-500 dataset follows the SakanaAI/JA-VG-VQA-500 dataset license.

The Gemma3 model usage is governed by Google's provided license.
