# AIFFEL Campus Online Code Peer Review Templete
- 코더 : 코더의 이름을 작성하세요.
- 리뷰어 : 리뷰어의 이름을 작성하세요.


# PRT(Peer Review Template)
- [ ]  **1. 주어진 문제를 해결하는 완성된 코드가 제출되었나요?**
    - 문제에서 요구하는 최종 결과물이 첨부되었는지 확인
        - 중요! 해당 조건을 만족하는 부분을 캡쳐해 근거로 첨부
    
- [ ]  **2. 전체 코드에서 가장 핵심적이거나 가장 복잡하고 이해하기 어려운 부분에 작성된 
주석 또는 doc string을 보고 해당 코드가 잘 이해되었나요?**
    - 해당 코드 블럭을 왜 핵심적이라고 생각하는지 확인
    - 해당 코드 블럭에 doc string/annotation이 달려 있는지 확인
    - 해당 코드의 기능, 존재 이유, 작동 원리 등을 기술했는지 확인
    - 주석을 보고 코드 이해가 잘 되었는지 확인
        - 중요! 잘 작성되었다고 생각되는 부분을 캡쳐해 근거로 첨부
        
- [ ]  **3. 에러가 난 부분을 디버깅하여 문제를 해결한 기록을 남겼거나
새로운 시도 또는 추가 실험을 수행해봤나요?**
    - 문제 원인 및 해결 과정을 잘 기록하였는지 확인
    - 프로젝트 평가 기준에 더해 추가적으로 수행한 나만의 시도, 
    실험이 기록되어 있는지 확인
        - 중요! 잘 작성되었다고 생각되는 부분을 캡쳐해 근거로 첨부
        
- [ ]  **4. 회고를 잘 작성했나요?**
    - 주어진 문제를 해결하는 완성된 코드 내지 프로젝트 결과물에 대해
    배운점과 아쉬운점, 느낀점 등이 기록되어 있는지 확인
    - 전체 코드 실행 플로우를 그래프로 그려서 이해를 돕고 있는지 확인
        - 중요! 잘 작성되었다고 생각되는 부분을 캡쳐해 근거로 첨부
        
- [ ]  **5. 코드가 간결하고 효율적인가요?**
    - 파이썬 스타일 가이드 (PEP8) 를 준수하였는지 확인
    - 코드 중복을 최소화하고 범용적으로 사용할 수 있도록 함수화/모듈화했는지 확인
        - 중요! 잘 작성되었다고 생각되는 부분을 캡쳐해 근거로 첨부


# 회고(참고 링크 및 코드 개선)

* 학습 전 결측치 및 이상치 확인

train.isnull().sum() = 0 #결측치 없음

import numpy as np
from scipy.stats import zscore

import pandas as pd
import numpy as np
from scipy.stats import zscore

* 이상치 확인 시 910개 조회 : 약 1만개 데이터 중 9% 이상의 데이터에서 이상치 확인

num_cols = train.select_dtypes(include=np.number).columns.tolist()

z_scores = train[num_cols].apply(zscore)

outlier_mask = (z_scores.abs() > threshold).any(axis=1)

outlier_indices = train.index[outlier_mask]
print(f"총 이상치 개수: {len(outlier_indices)}")

이상치 제거 또는 대체 필요

* 이상치 제거

df_clean = df.loc[~outlier_mask].reset_index(drop=True)

print("Cleaned DataFrame shape:", df_clean.shape)

- 이상치 제거 후 학습 시 기존 RMSE 147 -> 변경 122 로 LOSS 줄어듬

이상치 대체

# 수치형 컬럼 리스트
num_cols = df.select_dtypes(include=np.number).columns.tolist()

threshold = 3.0
for col in num_cols:
    # 각 행의 Z-score
    zs = zscore(df[col].fillna(df[col].median()))  # 결측 없도록
    # 정상 범위에 해당하는 원본값들의 하한/상한
    valid = df[col][np.abs(zs) <= threshold]
    lower, upper = valid.min(), valid.max()
    # 클리핑 적용
    df[col] = df[col].clip(lower=lower, upper=upper)
