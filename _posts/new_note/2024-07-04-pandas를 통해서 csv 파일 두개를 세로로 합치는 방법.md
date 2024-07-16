---
layout: posts
title: pandas를 통해서 csv 파일 두개를 세로로 합치는 방법
categories:
  - new_note
tags:
  - new_note
author_profile: false
sidebar:
  nav: counts
search: true
---

<div class="notice--info" markdown="1" style='font-size: 20px'>
**motivation:**  pandas를 통해서 csv 파일 두개를 세로로 합치는 방법 을 정리해보겠다.
</div>



``` python

import pandas as pd

  

# 파일을 불러올 때 첫 행을 열 이름으로 사용

df1 = pd.read_csv('./augment_first_step.csv',index_col=False)
df2 = pd.read_csv('./augment_second_step.csv', skiprows=1,index_col=False) # 가장 첫열(index) 안읽고, 가장 첫 row 안읽는다.

# 첫 번째 열 삭제
df1.drop(df1.columns[0], axis=1, inplace=True)
df2.drop(df2.columns[0], axis=1, inplace=True)


# 열 이름이 동일한지 확인하고, 필요한 경우 조정
df2.columns = df1.columns

# 두 DataFrame을 세로로 합치기, 새 인덱스 부여
concatenated_df = pd.concat([df1, df2], ignore_index=True)

# 결과 출력
print(concatenated_df)


# 결과 DataFrame을 CSV 파일로 저장, 인덱스 저장하지 않음
concatenated_df.to_csv('augument_first_and_second_concat.csv', index=True)
```