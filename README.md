# FER Challenge - Facial Expression Recognition

Kaggle Competition: Challenges in Representation Learning: Facial Expression Recognition

## Dataset
- 28,709 training images (48x48 grayscale)
- 7 emotion classes: Angry, Disgust, Fear, Happy, Sad, Surprise, Neutral
- Class imbalance: Happy (7215) >> Disgust (436)

## Experiments

### Model 1: SmallCNN (Underfitting)
- 2 Conv layers, 149K parameters
- Train Acc: 50% | Val Acc: 48%
- **Analysis**: მოდელი ძალიან პატარაა. Train და Val accuracy ახლოს არიან,
  მაგრამ ორივე დაბალია — underfitting. მოდელს არ შეუძლია საკმარისი
  pattern-ების სწავლა 48x48 სახის სურათებიდან.

### Model 2: MediumCNN (Better, but slow convergence)
- 3 Conv blocks + BatchNorm + Dropout(0.25), 1.3M parameters
- Train Acc: 46% | Val Acc: 55%
- **Analysis**: Val > Train რადგან Dropout training-ზე აქტიურია.
  BatchNorm-მა სტაბილიზაცია მოახდინა, მაგრამ კონვერგენცია ნელია.

### Model 3: OverfitCNN (Overfitting)
- 4 Conv layers, Dropout/BatchNorm გარეშე, 2.6M parameters
- Train Acc: 72% | Val Acc: 60%
- **Analysis**: კლასიკური overfitting. Train/Val გაპი 12%.
  Epoch 11-დან Val Loss იზრდება სანამ Train Loss მცირდება.
  Regularization-ის არარსებობა იწვევს train data-ს დაზეპირებას.

### Model 4: BestCNN (Best Balance) ✅
- 3 Conv blocks + BatchNorm + Dropout + LR Scheduler, 2.7M parameters
- Train Acc: 66% | Val Acc: **65%**
- **Analysis**: Train/Val გაპი მხოლოდ 1% — კარგი გენერალიზაცია.
  ReduceLROnPlateau scheduler-მა epoch 29-ზე lr შეამცირა 0.001→0.0005
  და მაშინვე გაუმჯობესდა შედეგი.

## Results Summary

| Model | Params | Train Acc | Val Acc | Issue |
|-------|--------|-----------|---------|-------|
| SmallCNN | 149K | 50% | 48% | Underfitting |
| MediumCNN | 1.3M | 46% | 55% | Slow convergence |
| OverfitCNN | 2.6M | 72% | 60% | Overfitting |
| BestCNN | 2.7M | 66% | 65% | ✅ Best balance |

## Key Decisions
- **BatchNorm**: სტაბილიზაციისთვის და სწრაფი კონვერგენციისთვის
- **Dropout**: Overfitting-ის თავიდან ასაცილებლად
- **ReduceLROnPlateau**: LR-ის ავტომატური შემცირება plateau-ზე
- **Data Augmentation**: RandomHorizontalFlip + RandomRotation

## WandB
[Project Link](https://wandb.ai/mgior23-free-university-of-tbilisi-/fer-challenge)

## Requirements
See `requirements.txt`
