# 데이터분석 6주차 정규과제

📌데이터분석 정규과제는 매주 정해진 분량의 『*혼자 공부하는 데이터 분석 with 파이썬*』 을 읽고 학습하는 것입니다. 이번 주는 아래의 **DataAnalysis_6th_TIL**에 나열된 분량을 읽고 공부하시면 됩니다.

아래의 문제를 풀어보며 학습 내용을 점검하세요. 문제를 해결하는 과정에서 개념을 스스로 정리하고, 필요한 경우 제시된 강의를 참고하여 보완하는 것이 좋습니다.

<!-- 강의 링크는 아래와 같습니다.
https://www.youtube.com/watch?v=XD65UhBMOiI&list=PLVsNizTWUw7FGzSRCkQrPEEe-ljVXgS7k&index=12
https://www.youtube.com/watch?v=NTQ5NXelOfw&list=PLVsNizTWUw7FGzSRCkQrPEEe-ljVXgS7k&index=13
-->


## DataAnalysis_6th_TIL

### 6장 복잡한 데이터 표현하기
#### 01. 객체지향 API로 그래프 꾸미기
#### 02. 맷플롯립의 고급 기능 배우기


## Study Schedule

| 주차  | 공부 범위     | 완료 여부 |
| ----- | ------------- | --------- |
| 1주차 | p.24~81    | ✅         |
| 2주차 | p.84~151   | ✅         |
| 3주차 | p.154~219  | ✅         |
| 4주차 | p.222~279 | ✅         |
| 5주차 | p.282~325 | ✅         |
| 6주차 | p.328~379 | ✅         |
| 7주차 | p.382~430 | 🍽️         |

<br>

<!-- 여기까진 그대로 둬 주세요-->


# 1️⃣ 개념 정리 

## 01. 객체지향 API로 그래프 꾸미기

### pyplot 방식 vs 객체지향 API 방식
맷플롯립으로 그래프를 그리는 방법은 두 가지가 있다.

- **pyplot 방식**: `plt.plot()`, `plt.title()` 등 `matplotlib.pyplot`의 함수를 직접 호출하는 방식. 함수들이 하나의 피겨 객체에 대한 상태를 공유한다.
- **객체지향 API 방식**: `fig, ax = plt.subplots()`로 피겨 객체와 Axes 객체를 명시적으로 만들고, `ax.plot()`, `ax.set_title()` 등 객체의 메서드를 사용하는 방식.

간단한 그래프는 pyplot 방식이 편하지만, 복잡한 그래프(여러 서브플롯 등)를 그릴 때는 객체지향 API가 적합하다.

```python
# pyplot 방식
plt.plot([1, 4, 9, 16])
plt.title('simple line graph')
plt.show()

# 객체지향 API 방식
fig, ax = plt.subplots()
ax.plot([1, 4, 9, 16])
ax.set_title('simple line graph')
fig.show()
```

### 그래프에 한글 출력하기
맷플롯립의 기본 폰트는 영문 sans-serif이므로 한글이 깨진다. 한글을 출력하려면 폰트를 설정해야 한다.

- **방법 1) rcParams 직접 설정**: `plt.rcParams['font.family'] = 'NanumGothic'`
- **방법 2) rc() 함수 사용**: `plt.rc('font', family='NanumBarunGothic', size=11)` — 그룹 내 여러 설정을 동시에 지정 가능

코랩에서는 `!sudo apt-get -qq -y install fonts-nanum`으로 나눔 폰트를 설치한 뒤 `fm.fontManager.addfont()`으로 등록해야 한다.

### 산점도에 다양한 정보 담기
`scatter()` 메서드의 매개변수를 활용하면 산점도에 여러 차원의 정보를 표현할 수 있다.

| 매개변수 | 역할 | 비고 |
|---|---|---|
| `s` | 마커 크기 | 배열을 전달하면 데이터마다 크기가 달라짐 |
| `c` | 마커 색상 | 배열을 전달하면 컬러맵에 따라 색이 달라짐 |
| `alpha` | 투명도 (0~1) | 겹치는 마커의 밀도를 시각적으로 표현 |
| `edgecolors` | 마커 테두리 색 | `'k'`(검은색) 지정 시 마커 경계 구분 용이 |
| `linewidths` | 테두리 두께 | 기본값 1.5 |
| `cmap` | 컬러맵 지정 | 기본값 `'viridis'`, `'jet'` 등 사용 가능 |

### 컬러맵과 컬러 막대
- **컬러맵(colormap)**: 값에 따른 색상을 사전에 정의한 색상 리스트. 기본값은 `viridis`(진녹색→노란색), `jet`(파란색→노랑→빨강)도 자주 사용된다.
- **컬러 막대(colorbar)**: 색깔이 어떤 값에 대응하는지 참조 정보를 제공하는 막대. `fig.colorbar(sc)`로 추가한다.

```python
sc = ax.scatter(x, y, s=data**1.3, c=data, cmap='jet',
                linewidths=0.5, edgecolors='k', alpha=0.3)
fig.colorbar(sc)
```

### 핵심 함수 정리
| 함수/메서드 | 기능 |
|---|---|
| `plt.rc()` | rcParams 객체의 값을 설정 |
| `fig.colorbar()` | 그래프에 컬러 막대 추가 |

---

## 02. 맷플롯립의 고급 기능 배우기

### 하나의 피겨에 여러 개의 선 그래프 그리기
`plot()` 메서드를 여러 번 호출하면 하나의 피겨에 여러 선 그래프가 그려진다. 맷플롯립은 자동으로 10개의 색을 돌아가며 사용한다.

```python
ax.plot(line1['발행년도'], line1['대출건수'], label='황금가지')
ax.plot(line2['발행년도'], line2['대출건수'], label='비룡소')
ax.legend()  # 범례 추가
```

- **범례(legend)**: `plot()`에 `label` 매개변수를 지정한 뒤 `ax.legend()`를 호출하면 그래프에 범례가 추가된다. `loc` 매개변수로 위치 지정 가능 (기본값 `'best'`).
- **축 범위 지정**: `ax.set_xlim(1985, 2025)` / `ax.set_ylim()`으로 x축·y축 출력 범위를 설정한다.

### 피벗 테이블 (pivot_table)
데이터 구조를 변환할 때 사용한다. `index` 매개변수에 행 인덱스로 쓸 열, `columns` 매개변수에 열 인덱스로 쓸 열을 지정하면, 해당 열의 고유한 값이 새로운 행/열이 된다.

```python
ns_book10 = ns_book9.pivot_table(index='출판사', columns='발행년도')
# values 매개변수를 지정하면 열 이름이 다단으로 구성되지 않음
ns_book11 = ns_book9.pivot_table(index='발행년도', columns='출판사', values='대출건수')
```

- `aggfunc` 매개변수로 집계 방식 지정 (기본값: 평균, `np.sum` 등 사용 가능)
- `fill_value=0`으로 누락값을 미리 채울 수 있음

### 스택 영역 그래프 (Stacked Area Graph)
여러 선 그래프를 y축 방향으로 쌓아 올린 그래프. 그래프 사이의 면적이 각 항목의 y값에 해당한다.

```python
ax.stackplot(year_cols, ns_book10.loc[top10_pubs].fillna(0),
             labels=top10_pubs)
```

- `stackplot()` 메서드의 두 번째 매개변수에 y축 값을 **2차원 배열**로 전달해야 한다.
- 다단 열 이름에서 특정 레벨만 추출: `ns_book10.columns.get_level_values(1)`
- `fillna(0)`으로 누락값을 0으로 채워야 그래프가 정상 출력된다.

### 스택 막대 그래프 (Stacked Bar Graph)
막대 그래프를 y축 방향으로 쌓는 방식. 맷플롯립에는 전용 메서드가 없으므로 두 가지 방법을 사용한다.

**방법 1) bottom 매개변수 사용**
```python
plt.bar(range(5), height1, width=0.5)
plt.bar(range(5), height2, bottom=height1, width=0.5)
```

**방법 2) cumsum()으로 값을 미리 누적 → 큰 막대부터 그리기**
```python
ns_book12 = ns_book10.loc[top10_pubs].cumsum()
for i in reversed(range(len(ns_book12))):
    ax.bar(year_cols, ns_book12.iloc[i], label=ns_book12.index[i])
```
가장 큰 막대를 먼저 그려야 작은 막대가 덮이지 않는다.

**판다스로 간단하게 그리기**: `df.plot.bar(stacked=True)`만 쓰면 자동으로 쌓아 준다 (cumsum 불필요).

### 원 그래프 (Pie Chart)
전체 데이터에 대한 비율을 부채꼴로 나타낸 그래프.

```python
ax.pie(data, labels=labels, startangle=90,
       autopct='%.1f%%', explode=[0.1]+[0]*9)
```

| 매개변수 | 역할 |
|---|---|
| `labels` | 부채꼴 위에 표시할 이름 |
| `startangle` | 시작 위치 (기본값 3시 방향, 90이면 12시 방향) |
| `autopct` | 비율 포맷 문자열 (예: `'%.1f%%'` → 소수점 1자리 + %) |
| `explode` | 특정 부채꼴을 떼어내어 강조 (반지름 비율로 지정) |

원 그래프는 시각적으로 크기 비교가 어려우므로 반드시 비율을 명시하는 것이 좋다.

### 여러 종류의 그래프가 있는 서브플롯
`plt.subplots(2, 2)`로 2×2 형태의 서브플롯을 만들면 Axes 객체를 2차원 배열처럼 사용한다.

```python
fig, axes = plt.subplots(2, 2, figsize=(20, 16))
axes[0, 0].scatter(...)   # 왼쪽 상단: 산점도
axes[0, 1].stackplot(...) # 오른쪽 상단: 스택 영역 그래프
axes[1, 0].bar(...)       # 왼쪽 하단: 스택 막대 그래프
axes[1, 1].pie(...)       # 오른쪽 하단: 원 그래프
fig.savefig('all_in_one.png')  # 이미지 파일로 저장
```

### 핵심 함수 정리
| 함수/메서드 | 기능 |
|---|---|
| `ax.legend()` | 그래프에 범례 추가 |
| `ax.set_xlim()` | x축 출력 범위 지정 |
| `df.pivot_table()` | 피벗 테이블 기능 |
| `ax.stackplot()` | 스택 영역 그래프 |
| `df.plot.area()` | 판다스로 스택 영역 그래프 |
| `df.plot.bar()` | 판다스로 막대 그래프 |
| `df.cumsum()` | 행/열 방향 누적 합 계산 |
| `ax.pie()` | 원 그래프 |


# 2️⃣ 수행 인증

<!-- 교재에서 안내된 과정을 직접 실행해본 뒤, 진행 결과가 보이도록 4~6장의 스크린샷을 캡처하여 아래에 첨부해주세요.-->
<img width="846" height="782" alt="image" src="https://github.com/user-attachments/assets/ffd2eefc-1612-4a7b-84ad-aacb587cfa64" />
<img width="1047" height="820" alt="image" src="https://github.com/user-attachments/assets/f41a8806-f198-4b4e-b865-1482f5f7caf3" />
<img width="877" height="812" alt="image" src="https://github.com/user-attachments/assets/7fb93b36-c991-4e84-8045-de21c3d7bfc7" />
<img width="862" height="807" alt="image" src="https://github.com/user-attachments/assets/4f9ddfc3-b147-4471-a752-9f8f9a5d3f1c" />
<img width="731" height="785" alt="image" src="https://github.com/user-attachments/assets/a503fd9c-cea6-475a-95ff-8af26a2e6087" />



<br>
<br>

# 3️⃣ 확인 문제

## 문제 1.

> **🧚Q. 이번 주차에는 확인문제 대신 그래프 그리기 실습을 진행합니다.
4주차에서 사용했던 캐글 데이터셋을 활용하여, 다양한 요소를 포함한 복잡한 그래프를 직접 작성해주세요.**

```
https://colab.research.google.com/drive/1jXla1Pfys6YsZ6nlu4Wi3MuBWRrArIn-?usp=sharing
```



### 🎉 수고하셨습니다.
