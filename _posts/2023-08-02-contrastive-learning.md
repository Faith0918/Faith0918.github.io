---
layout: single
title: "[논문 리뷰] Self-supervised Graph Learning for Recommendation (SIGIR'21) & Do we really  need graph augmentation? (SIGIR'22)"
---

### 논문을 읽게 된 계기

Supervised learning을 위한 labeled data는 항상 부족하며 여기에 더해 모델 성능은 크게 발전하여 overfitting의 위험이 존재한다. (모델 capacity가 커질수록 학습 데이터 (labeled data)가 많이 필요하다. 그렇지 않으면 overfitting이 일어난다.) 이를 해결하기 위한 self-supervised learning 기법 중 하나인 contrastive learning을 GCN에 적용한 연구를 리뷰하게 되었다!

## Reviewed Papers

Self-supervised Graph Learning for Recommendation (Wu et al., SIGIR’21)

[Self-supervised Graph Learning for Recommendation](https://arxiv.org/abs/2010.10783)

Are Graph Augmentations Necessary? Simple Graph Contrastive Learning for Recommendation (Yu et al., SIGIR’22)

[Are Graph Augmentations Necessary? Simple Graph Contrastive...](https://arxiv.org/abs/2112.08679)

## Self-supervised Learning on GCN Recsys

### Motivation

- Popularity bias
    - high-degree item이 학습에 영향을 과도하게 많이 줘서 low-degree item에 대한 추천을 방해함
- Noises in interaction
    - user가 선호하지 않는 item에 대한 interaction이 noise로 껴있을 가능성이 있음

### Idea

SSL을 사용해서 위의 bias issue를 해결하겠다

- Contrastive learning
    - positive pair: original image-augmented image → 유사하게 학습!
    - negative pair: original image-negative image → 구별되게 학습!
    
    ![Untitled](/assets/images/contrastive/Untitled.png)
    

→ low degree items에서 성능 향상 & robustness 증가

## Contrastive Learning on GCN Recsys

![Untitled](/assets/images/contrastive/Untitled%201.png)

1. **Augmentation Types**
    - Node Dropout(ND)
    - Edge dropout(ED)
    - Random Walk(RW): ED의 확장, layer마다 서로 다른 mask를 쓴다는 점만이 다름
2. **Contrastive Learning**
    - Node-Node간의 contrast를 위해 node의 서로 다른 views를 만듦
        
        ![Untitled](/assets/images/contrastive/Untitled%202.png)
        
3. **BPR Loss**
    - item간의 상대적인 선호도를 학습하는 것 (i: positive item, j: negative item)
        
        ![Untitled](/assets/images/contrastive/Untitled%203.png)
        
4. **Total Loss**
    1. BPR loss + Contrastive loss + L2 Regularization

![Untitled](/assets/images/contrastive/Untitled%204.png)

### Evaluation

- Contrastive learning을 사용해서 Baseline 모델인 LightGCN보다 높은 성능 달성

![Untitled](/assets/images/contrastive/Untitled%205.png)

- low-degree item에 대해서 개선된 성능

![Untitled](/assets/images/contrastive/Untitled%206.png)

- noise에 대해 robust한 성능

![Untitled](/assets/images/contrastive/Untitled%207.png)

## Do we really  need graph augmentation?

### Motivation

Explaining and harnessing adversarial examples (ICLR’15)

Idea: 원본 벡터에 아주 작은 noise를 더하는것만으로도 robust하게 모델을 만들 수 있다.

![Untitled](/assets/images/contrastive/Untitled%208.png)

2 noise constraints

- very small magnitude
- same sign on each dimension

### Idea

graph augmentation을 제외하고 feature에 noise만을 더해서 contrastive learning을 구현

### Simple Graph Contrastive Learning

- representation-level augmentation (wo. graph augmentation such as ED)

![Untitled](/assets/images/contrastive/Untitled%209.png)

![Untitled](/assets/images/contrastive/Untitled%2010.png)

### Evaluation

- Graph augmentation 없이 simple contrastive learning으로 최고 성능 달성

![Untitled](/assets/images/contrastive/Untitled%2011.png)

- low-rank item에 대해 개선된 성능

![Untitled](/assets/images/contrastive/Untitled%2012.png)