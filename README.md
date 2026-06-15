# ViLawT5_RL

🚀 Demo App: [V-LegalQA-Chatbot](https://huggingface.co/spaces/sunbv56/V-LegalQA-Chatbot)

Ứng dụng chatbot hỏi đáp pháp luật tiếng Việt chuyên ngành, tập trung vào **Bộ luật Dân sự 2015**. Hệ thống demo được phát triển với 4 phiên bản mô hình khác nhau đại diện cho các giai đoạn cải tiến liên tục.

## Các Phiên Bản Mô Hình

Tất cả các phiên bản mô hình dưới đây đều được phát triển dựa trên kiến trúc **ViT5-base** với **220 triệu tham số (220M parameters)** tối ưu cho tiếng Việt. Hệ thống bao gồm 4 phiên bản chính đại diện cho các giai đoạn phát triển và cải tiến khác nhau:

1. **`ViT5_QAChatbot` (Baseline QA - 220M params):** 
   - Tinh chỉnh trực tiếp (**Fine-tuning**) mô hình ngôn ngữ tiếng Việt tổng quát `ViT5-base` (`VietAI/vit5-base`) trên tập dữ liệu QA pháp lý tự xây dựng (`data/xml_data`).
   - Không áp dụng tiền huấn luyện thích ứng tên miền (domain adaptation).
2. **`ViLawT5_QAChatbot` (Domain-Adapted QA - 220M params):**
   - Tiền huấn luyện không giám sát (**Domain Adaptation**) trên dữ liệu bộ luật Việt Nam (`data/boluat`), tạo ra mô hình nền tảng chuyên ngành `ViLawT5-base`, sau đó fine-tune trên dữ liệu QA.
   - Nâng cao khả năng hiểu sâu sắc thuật ngữ pháp lý và ngữ nghĩa văn bản luật chuyên ngành.
3. **`ViLawT5_RL` (RLHF Baseline - 220M params):**
   - Tinh chỉnh mô hình `ViLawT5_QAChatbot` bằng phương pháp Học tăng cường từ phản hồi của con người (**RLHF**) thông qua thuật toán **PPO** sử dụng Reward Model huấn luyện trên phản hồi người dùng (`data/reward_data`).
   - Tối ưu hóa độ mạch lạc của câu trả lời và giảm thiểu hiện tượng ảo tưởng thông tin (hallucination).
4. **`V-LegalQA` (ViLawT5_RL_Plus - Phiên bản cuối cùng - 220M params):**
   - Phiên bản RLHF cải tiến bằng cách tích hợp thêm câu hỏi làm ngữ cảnh (Context) vào Reward Model khi huấn luyện PPO.
   - Đạt hiệu suất cao nhất và câu trả lời hoàn thiện nhất trên toàn bộ các thang đo chất lượng.

## Bảng Điểm So Sánh (Evaluation Scores)

Các mô hình được đánh giá trên tập kiểm thử gồm **200 câu hỏi** đồng bộ (chi tiết trong thư mục `eval/`):

| Phiên bản Mô hình | BLEU | ROUGE-1 | ROUGE-2 | ROUGE-L | BERTScore |
| :--- | :---: | :---: | :---: | :---: | :---: |
| **`ViT5_QAChatbot`** | 0.2440 | 0.6427 | 0.4613 | 0.5315 | 0.8119 |
| **`ViLawT5_QAChatbot`** | 0.2652 | 0.6511 | 0.4801 | 0.5486 | 0.8152 |
| **`ViLawT5_RL`** | 0.2512 | 0.6355 | 0.4631 | 0.5296 | 0.8114 |
| **`V-LegalQA`** *(RL Plus)* | **0.3841** | **0.7142** | **0.5960** | **0.6475** | **0.8448** |

## Cấu Trúc Thư Mục
```
source/
├── data/                     # Chứa dữ liệu huấn luyện
│   ├── boluat/               # Bộ luật Việt Nam (Huấn luyện không giám sát)
│   ├── xml_data/             # Dữ liệu QA từ Bộ Luật Dân Sự 2015 (Fine-tuning)
│   ├── reward_data/          # Dữ liệu phản hồi người dùng (Huấn luyện Reward Model)
│
└── Source/                   # Chứa các file code huấn luyện và đánh giá
    ├── pretrained-vit5-qachatbot-vilaw.ipynb      # Huấn luyện ViLawT5-base trên boluat
    ├── vit5-qachatbot-finetuned.ipynb             # Fine-tune QA cho ViT5 trên xml_data
    ├── vilawt5-qachatbot-finetuned.ipynb          # Fine-tune QA cho ViLawT5-base trên xml_data
    ├── rewardmodel-qachatbot-vilaw.ipynb          # Huấn luyện Reward Model trên reward_data
    ├── rlhf-vit5-qachatbot.ipynb                  # Huấn luyện ViLawT5-RL với RLHF
    ├── eval-vilawt5-qachatbot.ipynb               # Đánh giá mô hình ViT5, ViLawT5, ViLawT5-RL
```

## Cài Đặt Môi Trường

Code được chạy trên Kaggle với GPU P100. Trước khi chạy cần cài đặt các thư viện sau:
```bash
pip install kaggle-environments==1.14.14 
pip install evaluate 
pip install sentence-transformers==2.2.2 
pip install faiss-cpu 
pip install rouge_score 
pip install bert-score 
pip install trl==0.7.4 
pip install transformers==4.31.0
```

## Quy Trình Hoạt Động

### 1. Chuẩn Bị Dữ Liệu
- `data/boluat/` → Dữ liệu bộ luật Việt Nam, dùng để huấn luyện ViLawT5-base.
- `data/xml_data/` → Dữ liệu hỏi đáp từ Bộ Luật Dân Sự 2015, dùng để fine-tune mô hình QA.
- `data/reward_data/` → Phản hồi người dùng, dùng để huấn luyện Reward Model.

### 2. Huấn Luyện Mô Hình Nền Tảng
- Chạy `Source/pretrained-vit5-qachatbot-vilaw.ipynb` để huấn luyện ViLawT5-base trên `data/boluat/`.

### 3. Fine-tuning Mô Hình QA
- Chạy `Source/vit5-qachatbot-finetuned.ipynb` để fine-tune QA cho ViT5 trên `data/xml_data/`.
- Chạy `Source/vilawt5-qachatbot-finetuned.ipynb` để fine-tune QA cho ViLawT5-base trên `data/xml_data/`.

### 4. Huấn Luyện Mô Hình Phần Thưởng
- Chạy `Source/rewardmodel-qachatbot-vilaw.ipynb` để huấn luyện Reward Model trên `data/reward_data/`.

### 5. Huấn Luyện Với RLHF
- Chạy `Source/rlhf-vit5-qachatbot.ipynb` để tinh chỉnh ViLawT5-base bằng RLHF.
