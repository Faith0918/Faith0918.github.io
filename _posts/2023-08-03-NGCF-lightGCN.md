---
layout: single
title: "[논문 리뷰] NGCF (SIGIR’19) & LightGCN (SIGIR’20)"
---

### 논문을 읽게 된 계기

GCN을 처음 협업 필터링에 사용한 연구 (NGCF)와 이를 단순하게 발전시킨 연구 (LightGCN)를 리뷰하게 되었다!

## Reviewed Papers

[Neural Graph Collaborative Filtering](https://arxiv.org/abs/1905.08108)

[LightGCN: Simplifying and Powering Graph Convolution Network for...](https://arxiv.org/abs/2002.02126)

## Using GCN for Collaborative Filtering

### Collaborative Filtering

- 비슷한 행동을 하는 사용자는 비슷한 취향을 가진다라고 가정함
- user-item 상호작용을 기반으로 새로운 상호작용을 예측한다.

### Matrix Factorization

- user, item ID에 대해 직접적으로 임베딩해서 inner product로 상호작용을 재생하도록 모델링
- 직접적인 중요한 collaborative signal을 반영하기 어렵다.
    - collaborative signal: target user와 비슷한 행동을 한 user에 대한 임베딩 벡터
    
    ![Untitled](/assets/images/ngcf_lightgcn/Untitled.png)
    

### GCN (Kipf et al., ICLR’17)

- aggregate-combine
    - aggregate: 타겟 노드에 대한 이웃 노드 정보를 가져온다.
    - combine: 정보를 합친다 (ex. average, concat ..)
    - $\tilde{D}$는 self-loop를 갖는 인접행렬로부터 얻은 각 노드의 degree를 대각성분으로 갖는 대각행렬
    
    ![Untitled](/assets/images/ngcf_lightgcn/Untitled%201.png)
    

### NGCF

- collaborative signal을 반영하여 high-order connectivity를 모델링할 수 있다.
- learnable parameters: W1, W2, embeddings
- 최종임베딩: 각  GCN layer embedding concat

![Untitled](/assets/images/ngcf_lightgcn/Untitled%202.png)

![Untitled](/assets/images/ngcf_lightgcn/Untitled%203.png)

![Untitled](/assets/images/ngcf_lightgcn/Untitled%204.png)

![Untitled](/assets/images/ngcf_lightgcn/Untitled%205.png)

### Evaluation

- 비교 모델 대비 최고 성능

![Untitled](/assets/images/ngcf_lightgcn/Untitled%206.png)

## Simplifying GCN for Recsys

- NGCF에서 nonlinear activation 없애고 linear transformation 없앤것
- 최종임베딩: 각  GCN layer embedding 평균

![Untitled](/assets/images/ngcf_lightgcn/Untitled%207.png)

### Evaluation

- NGCF보다 성능 개선
- 최근의 논문에서도 사용되는 baseline 모델로, 단순하지만 강력한 모델이다.
- 추천시스템에서 선형변환 없는 단순한 모델 성능이 우수한 경향을 보인다.
    - EASE (WWW’19, Netflix): 은닉층이 없는 오토인코더 기반 선형 모델

![Untitled](/assets/images/ngcf_lightgcn/Untitled%208.png)