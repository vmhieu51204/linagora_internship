# VinaSmol Evaluation Report
Date: 3/5/2016
## 1. Overview
Evaluate the VinaSmol model 

## 2. Detail

The model analyzed was the `VinaSmol_vi_alpaca` checkpoint, extracted from the provided VinaSmol_vi_alpaca.tar.gz archive. 

### 2.1 Model Characteristics:
* **Base Architecture**: SmolLM 360M (`LlamaModel` architecture).
* **Parameters**: ~368.33 Million.
* **Context Window**: Trained with a maximum sequence length of 1024 tokens.
* **Prompt Format**: Uses the **ChatML** template (`<|im_start|>user\n...<|im_end|>\n<|im_start|>assistant`).
* **VRAM**: ~800 MB
---
### 2.2 Evaluation Dataset
#### VLMU: https://vmlu.ai/#Download
* **Dataset Used**: huggingface `tridm/VMLU`
* **Evaluation Technique (Logit Polling)**: Due to the model's small parameter size (360M), traditional text generation is prone to hallucinated formatting. Instead, a custom PyTorch loop was written to extract the exact probability (logits) the model assigned to the specific tokens `"A"`, `"B"`, `"C"`, and `"D"` given the question context.
* **Result**: Final Accuracy: 170/744 (22.85%)
**Sample answer:**
PROMPT SENT TO MODEL:
Trả lời câu hỏi trắc nghiệm sau bằng cách chọn một trong các đáp án A, B, C hoặc D.

Câu hỏi: Hoạt động nào sau đây của ngân hàng Trung Ương sẽ làm tăng cơ sở tiền tệ
Các lựa chọn:
A. Bán ngoại tệ trên thị trường ngoại hối
B. Cho các ngân hàng thương mại vay
C. Hạ tỷ lệ dự trữ bắt buộc đối với các ngân hàng thương mại
D. Tăng lãi suất chiết khấu

Đáp án đúng là:

Generating full creative response...

🧠 MODEL'S ACTUAL GENERATED TEXT:
"Trong bài viết này, chúng ta sẽ tìm hiểu về các loại thuốc và cách sử dụng chúng.
Thuốc là một loại thuốc được sử dụng để điều trị bệnh hoặc điều trị một vấn đề."

 LOGIT PROBABILITIES FOR THIS QUESTION:
Probability of generating 'A': 7.06
Probability of generating 'B': 7.75
Probability of generating 'C': 7.72
Probability of generating 'D': 7.22
Ground Truth Correct Answer: C

#### Squad Vietnamese: https://www.kaggle.com/datasets/nkhachao/vietnamese-squad
* **Dataset Used**: 200 first question in https://www.kaggle.com/datasets/nkhachao/vietnamese-squad.
* **Evaluation Technique (Generative)**: Unlike VMLU, SQuAD requires generating raw text. We constrained the `max_new_tokens=30` and utilized a low temperature (`0.1`) to encourage factual extraction over creative writing.
* **Result**:
Exact Match (EM):     0.00%
Token Overlap (F1):   0.38%
ROUGE-1:              9.53%
ROUGE-2:              0.71%
ROUGE-L (Longest Seq):7.03%
**Sample answer:**
QUESTION: Normandy nằm ở quốc gia nào?
TRUTH:    Pháp
PREDICT:  Trong bài viết này, chúng ta sẽ tìm hiểu về các loại thuốc chống viêm và cách sử dụng chúng.
1. Thuốc chống viêm
Thuốc

