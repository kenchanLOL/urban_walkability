# urban_walkability
An AI tool to analyze walkability of street-view images. 

## Part 1: Preprocessing 
Images are splited into training, validation and testing datasets. Corresponding files are also generated

## Part 2: Ranking Algorithms
1. Computes 2 types of ranking algorithms (Elo & Trueskill) based on crowdsourcing comparison results for 100 sample runs
2. visulize the change of elo during sample runs
![trueskill_change_record](https://user-images.githubusercontent.com/55791584/205874429-cc5d422f-fabd-4c61-a776-c33de9d6cc96.png)
3. visualize the distribution of overall and individual images
![overall_dist](https://user-images.githubusercontent.com/55791584/205874664-a9990eca-e896-4eb4-8178-1e1e2d47da1a.png)
![individual_dist](https://user-images.githubusercontent.com/55791584/205874685-b50f4c0a-c369-40e2-9acc-ade74322328e.png)

## Part 3: Model experiments
Experiments on different learning objectives:
1. Linear Regression
2. Classification 
3. Image Quality Assestment (IQA) -> estimate the elo distribution of images [^1]
4. Self-supervised Learning (MAE)
5. New proposed self-supervised Learning -> walkabilityAI_pretrain notebook
   - Randomly select 2 images  
   - compute their visual embeddings
   - predict the difference in elo by cosine similarity 
   - add MLP layer in finetuning stage
6. (In progress) Contrstive learning modified on CoNT: Contrastive Neural Text Generation[^2]

Experiments on model
1. ResNet
2. ViT
3. Swin Transformer
4. Customized model 

## Part 4: Results and Visualization with CAM
| Model | Learning Objective|Loss |
| ----------- | ----------- | ----------- |
| ResNet | Regression | 0.35 (MSE) |
| ResNet | Classification | 0.22 (Cross Entropy) |
| ResNet | **Regression(w. self-proposed task)** | 0.69(self-proposed task), **0.32** (MSE) |
| ResNet | IQA | 0.47 (MSE) |
| SwinTransformer | Regression(w. MAE) | 0.3(MAE) 1.0 (MSE)|
| SwinTransformer | **Regression(w. self-proposed task)** | 0.69(self-proposed task), **0.42** (MSE)|
| SwinTransformer | Regression(w/o. MAE) | 43.3 (MSE)|
| ViT | Regression(w. MAE) | 0.3(MAE) 0.8 (MSE)|
| ViT | Regression(w. self-proposed task) |NA|
| ViT | Regression(w/o. MAE) |NA|

For model selection, we found that Transformer models require much larger dataset size to reach high performance.Even we tried high dropout rate and stronger data augementation, the model easily overfitted and stopped learning. So, only ResNet will be used in furthre experiments. According to CAM visualization, ResNet also seems to be more reasonable and related to human perception of walkability.
### Resnet CAM
![resnet101_true_inverse_CAM_test](https://user-images.githubusercontent.com/55791584/205890213-f75d14c4-3da9-445d-a5d9-9e6c6c6ce7cb.jpg)

### Swin Transformer CAM
![swin_pretrain_elo_CAM_test](https://user-images.githubusercontent.com/55791584/205890318-75d47802-313a-4e49-b66e-76f35d22fbf8.jpg)

For learning objective, we achieved a small succuess in using a new self-supervised and contrastive learning based method. The future work will mainly focus on developing more contrastive learning tasks such as modifying the example selection, loss function and ranking algorithms proposed by CoNT, which archieved success in applying both contrastive learning and ranking algorthms in NLG aspect.


[^1]: https://arxiv.org/abs/1709.05424
[^2]: https://arxiv.org/abs/2205.14690 
