1. eval-vilawt5-qachatbot.ipynb là code đánh giá các mô hình (ViT5, VilawT5, VilawT5-RL)
2. pretrained-vit5-qachatbot-vilaw.ipynb là code huấn luyện model ViLawT5-base (huấn luyện các bộ luật)
3. rewardmodel-qachatbot-vilaw.ipynb là code huấn luyện model phần thưởng (reward model) hỗ trợ cho RLHF
4. rlhf-vit5-qachatbot.ipynb là code huấn luyện model VilawT5-RL bằng reward model và VilawT5 model
5. vilawt5-qachatbot-finetuned.ipynb là code finetuned dữ liệu QA cho model VilawT5-base
6. vit5-qachatbot-finetuned.ipynb là code finetuned dữ liệu QA cho model ViT5


Quy trình thực hiện (kaggle.com):
1. Huấn luyện mô hình ban đầu:
   ├── pretrained-vit5-qachatbot-vilaw.ipynb → Huấn luyện VilawT5-base (các bộ luật)
   ├── vit5-qachatbot-finetuned.ipynb → Fine-tune QA cho ViT5
   ├── vilawt5-qachatbot-finetuned.ipynb → Fine-tune QA cho VilawT5-base

2. Huấn luyện mô hình phần thưởng:
   ├── rewardmodel-qachatbot-vilaw.ipynb → Huấn luyện Reward Model

3. Tinh chỉnh bằng RLHF:
   ├── rlhf-vit5-qachatbot.ipynb → Huấn luyện VilawT5-RL bằng Reward Model và VilawT5

4. Đánh gain mô hình:
   ├── eval-vilawt5-qachatbot.ipynb → Đánh giá ViT5, VilawT5, VilawT5-RL