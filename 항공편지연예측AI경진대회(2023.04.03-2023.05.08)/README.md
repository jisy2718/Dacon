대회에 대한 설명문서 입니다.



### 제출결과

+ tval LogLoss 기준 값(리더보드값)

|                             | 베이스   | 전처리2-1                                                  | 전처리2-2 | 전처리2-3                      | 전처리2-4                      | 전처리2-5 | 전처리3  | 전처리9 | 전처리10 |
| --------------------------- | -------- | ---------------------------------------------------------- | --------- | ------------------------------ | ------------------------------ | --------- | -------- | ---- | ---- |
| 베이스모델                  | (1.1568) | (1.1494)                                                   |           |                                |                                |           |          |      |      |
| lgbm<br />+ grid            |          | 0.4395(1.1430)<br />0.4391(1.1403)<br />**0.4379(1.1400)** |           | 0.4381()                       |                                |           | (1.9952) |      |      |
| lgbm <br />+ 베이지안최적화 |          |                                                            |           | 0.4394<br />()<br />sb_lb5.csv | <br />(1.1530)<br />sb_lb3.csv |           |          |      |      |
| xgboost<br />+ grid         |          |                                                            |           | 0.4384(1.1424)<br />sb_xb2.csv |                                |           |          |      |      |
| catboost                   |          |                                                            |           |                                |                                |           |          | 0.650(public)<br />0.638(private) | 0.649(public)<br />0.637(private) |
| vae + semi-super-vised | | | | | | | | | 1.064(public)<br />0.786(private) |



#### 전처리

2 : Airport & AIRLINE 결측치 처리

2-1 : 2 + 나머지 결측치를 -1, null로 사용

2-2 : 2-1 + 2개 컬럼 제거

2-3 : 2 + 나머지 결측치를 최빈값

2-4 : 2-3 + 2개 컬럼 제거

2-5 : 2-3 + 변수 표준화

3 : 2-3 + 결측 y변수 예측 후 적합 1회

+ 전체 결측 y변수에 대해서 예측값을 채워넣고 적합하니, 스코어가 매우 안좋음

+ 할 일

  1. 결측이 있는 data에서 validation set 떼어두고, 나머지로 y 변수 결측치 예측

  2. y변수 결측치 예측시, 전체 값 예측하지 말고, probability가 특정값 이상인 것들만 0,1로 예측하기

  3. 2를 반복.



5 : 2-3 + lgbm으로 결측치 예측 + validation set 만들어서 평가

+ 결측치 예측 안한 것이 더 좋게 나옴

6: 2 + 결측 column있는 행 제거 + lgbm으로 결측치 예측 + validation set 만들어서 평가

+ 결측치 예측 안한 것이 더 좋게 나옴

7 : 2 + vae로 semi-supervised learning + validation set 만들어서 평가 + `DataLoader` 이용

+ train set을 한 번에 onehotencoding 하면, 메모리가 터져서`DataLoader` 이용해서 진행하려고 했지만, batch를 가져오는데 시간이 너무 오래걸려서 `DataLoader` 없이 배치를 만들기로 하고 8번으로 넘어감

8 : 2  + vae로 semi-supervised learning + validation set 만들어서 평가 + batch를 직접 생성해서 신경망 훈련

9 : 데이콘 1등 코드 참고해서 전처리


~~3 : 2 + 나머지 결측치를 이동 거리 + Airline 이용해서 추측해서 채우기~~



### semi-supervised learning

+ https://sanghyu.tistory.com/177 참고



### Loss

+ Log Loss 이용
    $$
    Log Loss = - (1/n) * Σ_{i=1}^n (y_i * log(p_i) + (1 - y_i) * log(1 - p_i))
    $$

+ 이진 분류에서 사용되는 Loss로, 실제 $y$ 값과 $y$ 에 대한 예측 확률인 $p$ 값이 일치할수록 Loss가 작아진다.

    + $y \in \{0,1\}$

    + $p_i$ 는 $y=1$ 일 확률






### 파일

1. 전처리 파일들

   + [2](./전처리방법2.ipynb), [2-1](./전처리방법2-1.ipynb), [2-2](./전처리방법2-2.ipynb), [2-3](./전처리방법2-3.ipynb), [7](./전처리방법7.ipynb), [8](./전처리방법8.ipynb), [9](./전처리방법9.ipynb)

     

2. [베이스코드따라하기.ipynb](./베이스코드따라하기.ipynb)

   + 베이스코드 따라서 RandomForestClassifier 이용

     

3. [여러ML모델비교하기.ipynb](./여러ML모델비교하기.ipynb)
   + 여러 ML 모델 비교

4. [lbgm적합](./LGBM적합후제출.ipynb)
   + grid search와 bayesian opt 이용해서 최적화

5. [xgboost적합](./model_xgboost.ipynb)
   + grid search와 계층적cv 이용

6. semi-supervised-learning

   + [내 전처리 + vae코드](./semi-supervised-learning.ipynb)
   + [1등전처리 + vae코드](./semi-supervised-learning2.ipynb)

