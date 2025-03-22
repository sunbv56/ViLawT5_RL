# ViLawT5_RL

## Cấu Trúc Thư Mục
```
source/
├── data/                     # Chứa dữ liệu huấn luyện
│   ├── boluat/               # Bộ luật Việt Nam (Huấn luyện không giám sát)
│   ├── xml_data/             # Dữ liệu QA từ Bộ Luật Dân Sự 2015 (Fine-tuning)
│   ├── reward_data/          # Dữ liệu phản hồi người dùng (Huấn luyện Reward Model)
│
└── Source/                     # Chứa các file code huấn luyện và đánh giá
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
