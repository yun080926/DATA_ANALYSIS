# 데이터분석 5주차 정규과제

📌데이터분석 정규과제는 매주 정해진 분량의 『*혼자 공부하는 데이터 분석 with 파이썬*』 을 읽고 학습하는 것입니다. 이번 주는 아래의 **DataAnalysis_5th_TIL**에 나열된 분량을 읽고 공부하시면 됩니다.

아래의 문제를 풀어보며 학습 내용을 점검하세요. 문제를 해결하는 과정에서 개념을 스스로 정리하고, 필요한 경우 제시된 강의를 참고하여 보완하는 것이 좋습니다.

<!-- 강의 링크는 아래와 같습니다.
https://www.youtube.com/watch?v=ho0LZ6GWhtc&list=PLVsNizTWUw7FGzSRCkQrPEEe-ljVXgS7k&index=10
https://www.youtube.com/watch?v=deYY4xHsI0o&list=PLVsNizTWUw7FGzSRCkQrPEEe-ljVXgS7k&index=11
-->


## DataAnalysis_5th_TIL

### 5장 데이터 시각화하기
#### 01. 맷플롯립 기본 요소 알아보기
#### 02. 선 그래프와 막대 그래프 그리기


## Study Schedule

| 주차  | 공부 범위     | 완료 여부 |
| ----- | ------------- | --------- |
| 1주차 | p.24~81    | ✅         |
| 2주차 | p.84~151   | ✅         |
| 3주차 | p.154~219  | ✅         |
| 4주차 | p.222~279 | ✅         |
| 5주차 | p.282~325 | ✅         |
| 6주차 | p.328~379 | 🍽️         |
| 7주차 | p.382~430 | 🍽️         |

<br>

<!-- 여기까진 그대로 둬 주세요-->


# 1️⃣ 개념 정리 

## 01. 맷플롯립 기본 요소 알아보기

### Figure 객체
- 맷플롯립의 모든 그래프 구성 요소를 담고 있는 **최상위 객체**
- `scatter()` 같은 함수를 호출하면 자동 생성되고, `plt.show()` 이후 소멸
- `plt.figure()` 로 명시적으로 생성하면 다양한 옵션 제어 가능
```python
plt.figure(figsize=(9, 6))  # 너비 9인치, 높이 6인치
```

- `figsize` 단위는 **인치(inch)**
- 픽셀 크기로 지정하고 싶다면 → `픽셀 값 ÷ DPI`
```python
plt.figure(figsize=(900/72, 600/72))  # 기본 DPI=72 기준
```

### DPI
- **dot per inch**: 1인치를 몇 개의 픽셀로 표현하는지 나타내는 단위
- `figsize`는 캔버스 크기, `dpi`는 해상도(돋보기) 역할
- DPI를 높이면 그래프 자체와 내부 구성 요소(축 숫자, 마커 등)가 모두 커짐
```python
plt.figure(dpi=144)  # 기본값 72의 2배
```

### rcParams 객체
- 맷플롯립 **전역 기본값**을 관리하는 객체
- 값을 바꾸면 이후 그려지는 **모든 그래프에 적용**됨
```python
plt.rcParams['figure.dpi'] = 100          # DPI 기본값 변경
plt.rcParams['scatter.marker'] = '*'      # 마커 기본값을 별 모양으로 변경
```

- 특정 그래프 하나만 바꾸려면 함수의 매개변수를 직접 사용
```python
plt.scatter(..., marker='+')  # rcParams 기본값 무시하고 이 그래프만 적용
```

### 마커(Marker)
- 그래프에서 데이터 포인트를 표시하는 방식
- 기본값: `'o'` (동그라미)
- 변경 방법: `rcParams['scatter.marker']` 또는 `scatter()` 함수의 `marker` 매개변수

### 서브플롯(Subplot)
- 하나의 Figure 안에 여러 개의 그래프 영역(Axes 객체)을 배치하는 것
- `plt.subplots(행, 열)` 로 생성
```python
fig, axs = plt.subplots(2, figsize=(6, 8))   # 세로로 2개
fig, axs = plt.subplots(1, 2, figsize=(10, 4))  # 가로로 2개
```

- 각 서브플롯에서 사용 가능한 주요 메서드:

| 메서드 | 기능 |
|---|---|
| `axs[i].set_title()` | 서브플롯 제목 설정 |
| `axs[i].set_xlabel()` | x축 이름 설정 |
| `axs[i].set_ylabel()` | y축 이름 설정 |
| `axs[i].set_yscale('log')` | y축 로그 스케일 설정 |

### 맷플롯립 그래프 구성 요소 정리
- **Figure**: 전체 그래프를 담는 최상위 컨테이너
- **Axes(서브플롯)**: 실제 그래프가 그려지는 영역
- **Axis**: x축 / y축 (눈금, 레이블 포함)
- **마커**: 데이터 포인트 표시 방식
- **레이블**: 축 이름

---

## 02. 선 그래프와 막대 그래프 그리기

### 선 그래프 (Line Graph)
- 데이터 포인트 사이를 **선으로 이은** 그래프
- 한 축을 따라 데이터의 **변화 추이**를 보는 데 적합 (예: 연도별 발행 도서 수)
- `plt.plot()` 함수로 그림
```python
plt.plot(count_by_year.index, count_by_year.values)
# 또는 시리즈 객체를 바로 넘겨도 됨 → 자동으로 인덱스를 x축으로 사용
plt.plot(count_by_year)

plt.title('Books by year')   # 제목
plt.xlabel('year')           # x축 이름
plt.ylabel('number of books')  # y축 이름
plt.show()
```

#### 선 스타일 / 마커 / 색상 옵션

| 매개변수 | 옵션 | 설명 |
|---|---|---|
| `linestyle` | `'-'` | 실선 (기본값) |
| | `':'` | 점선 |
| | `'-.'` | 쇄선 |
| | `'--'` | 파선 |
| `color` | `'red'`, `'#ff0000'` 등 | 선 색상 |
| `marker` | `'.'`, `'*'`, `'+'` 등 | 데이터 포인트 마커 |
```python
plt.plot(count_by_year, marker='.', linestyle=':', color='red')
# 단축 표현도 가능
plt.plot(count_by_year, '*-g')  # 별 마커, 실선, 초록색
```

#### 눈금(tick) 설정
- x축 눈금은 `xticks()`, y축 눈금은 `yticks()`
```python
plt.xticks(range(1947, 2030, 10))  # 10년 단위로 눈금 표시
```

#### 그래프에 텍스트 표시: `annotate()`
```python
plt.annotate(val, (idx, val))  # 기본: 마커와 같은 좌표 기준

# 텍스트 위치 조절
plt.annotate(val, (idx, val), xytext=(2, 2), textcoords='offset points')
# offset points: 포인트 단위 상대 위치 (1pt = 1/72인치)
# offset pixels: 픽셀 단위 상대 위치
```

> y축 스케일이 x축보다 훨씬 클 경우, 절대 좌표로 거리를 지정하면 효과가 거의 없음 → `offset points` 사용 권장

---

### 막대 그래프 (Bar Graph)
- 데이터 포인트의 크기를 **막대 높이**로 나타내는 그래프
- x축: 연속적이지 않은 **범주형** 데이터 (예: 주제분류번호)
- `plt.bar()` 함수로 그림
```python
plt.bar(count_by_subject.index, count_by_subject.values)
plt.title('Books by subject')
plt.xlabel('subject')
plt.ylabel('number of books')
plt.show()
```

#### 막대 스타일 옵션

| 매개변수 | 설명 |
|---|---|
| `width` | 막대 두께 (기본값 0.8, 1이면 간격 없음) |
| `color` | 막대 색상 |

#### annotate() 텍스트 정렬 옵션 (막대 그래프)
```python
plt.annotate(val, (idx, val),
             xytext=(0, 2), textcoords='offset points',
             fontsize=8, ha='center', color='green')
```

- `ha`: 수평 정렬 (`'center'`, `'left'`, `'right'`(기본))

---

### 가로 막대 그래프
- `plt.barh()` 함수 사용
- `width` → `height` 매개변수로 두께 조절
- x축과 y축 이름을 **바꿔서** 지정
- `annotate()` 좌표도 `(idx, val)` → `(val, idx)` 로 바꿔야 함
- 수직 정렬: `va` 매개변수 사용 (`'center'`, `'top'`, `'bottom'`, `'baseline'`(기본))
```python
plt.barh(count_by_subject.index, count_by_subject.values, height=0.7, color='blue')
plt.xlabel('number of books')
plt.ylabel('subject')
for idx, val in count_by_subject.items():
    plt.annotate(val, (val, idx), xytext=(2, 0), textcoords='offset points',
                 fontsize=8, va='center', color='green')
plt.show()
```

---

### 이미지 출력 및 저장
```python
# 이미지 읽기 → 넘파이 배열 반환 (높이, 너비, 채널)
img = plt.imread('jupiter.png')

# 이미지 화면에 출력
plt.imshow(img)
plt.axis('off')   # 축과 눈금 숨기기
plt.show()

# 넘파이 배열을 이미지 파일로 저장
plt.imsave('jupiter.jpg', arr_img)  # 확장자로 포맷 자동 변환

# 그래프를 이미지로 저장 (show() 이전에 호출해야 함!)
plt.savefig('books_by_subject.png')
plt.show()
```

> `savefig()`의 DPI를 높게 설정하면 인쇄용 고해상도 이미지 생성 가능

---

### 데이터 전처리 포인트 (이번 절 실습 기준)
- `value_counts()`: 고유값의 등장 횟수 계산
- `sort_index()`: 인덱스 기준 오름차순 정렬 (선 그래프 x축이 날짜일 때 필수)
- 인덱스 필터링: `series[series.index <= 2030]` 처럼 부등호로 이상값 제거
- 명목형 데이터(순서 무의미) vs 순서형 데이터(순서 의미 있음) — 막대 그래프는 정렬 불필요할 수도 있음


# 2️⃣ 수행 인증

<!-- 교재에서 안내된 과정을 직접 실행해본 뒤, 진행 결과가 보이도록 4~6장의 스크린샷을 캡처하여 아래에 첨부해주세요.-->
<img width="1310" height="736" alt="image" src="https://github.com/user-attachments/assets/65a632a9-e257-4b32-b8de-1c115262bfd6" />
<img width="1181" height="842" alt="image" src="https://github.com/user-attachments/assets/dc567ed5-f051-4a1c-b812-6a8fe7e303ea" />
<img width="1202" height="785" alt="image" src="https://github.com/user-attachments/assets/49353748-6b05-4f72-8aca-08ca7bea4d22" />
<img width="1200" height="830" alt="image" src="https://github.com/user-attachments/assets/9a7bf520-4aed-4548-838c-df562664ec2b" />



<br>
<br>

# 3️⃣ 확인 문제

## 문제 1.

> **🧚Q. 다음 데이터를 이용하여 matplotlib으로 선그래프를 그리는 코드를 작성해주세요.**
- x = [1, 2, 3, 4, 5]
- y = [2, 4, 6, 8, 10]
> 조건은 아래와 같습니다.
```
1️⃣ 제목은 "Linear Trend"로 설정해주세요.
2️⃣ x축 이름은 "X values"로 설정해주세요.
3️⃣ y축 이름은 "Y values"로 설정해주세요.
4️⃣ 마커(marker)를 포함하여 선그래프를 그려주세요.
```

```
import matplotlib.pyplot as plt

x = [1, 2, 3, 4, 5]
y = [2, 4, 6, 8, 10]

plt.plot(x, y, marker='o')
plt.title('Linear Trend')
plt.xlabel('X values')
plt.ylabel('Y values')
plt.show()
```



### 🎉 수고하셨습니다.
