# 데이터분석 2주차 정규과제

📌데이터분석 정규과제는 매주 정해진 분량의 『*혼자 공부하는 데이터 분석 with 파이썬*』 을 읽고 학습하는 것입니다. 이번 주는 아래의 **DataAnalysis_2nd_TIL**에 나열된 분량을 읽고 공부하시면 됩니다.

아래의 문제를 풀어보며 학습 내용을 점검하세요. 문제를 해결하는 과정에서 개념을 스스로 정리하고, 필요한 경우 제시된 강의를 참고하여 보완하는 것이 좋습니다.

<!-- 강의 링크는 아래와 같습니다.
https://www.youtube.com/watch?v=s_-VvTLb3gs&list=PLVsNizTWUw7FGzSRCkQrPEEe-ljVXgS7k&index=4
https://www.youtube.com/watch?v=Il6L8OtNFpc&list=PLVsNizTWUw7FGzSRCkQrPEEe-ljVXgS7k&index=5
-->


## DataAnalysis_2nd_TIL

### 2장 데이터 수집하기
#### 01. API 사용하기
#### 02. 웹 스크래핑 사용하기


## Study Schedule

| 주차  | 공부 범위     | 완료 여부 |
| ----- | ------------- | --------- |
| 1주차 | p.24~81    | ✅         |
| 2주차 | p.84~151   | ✅         |
| 3주차 | p.154~219  | 🍽️         |
| 4주차 | p.222~279 | 🍽️         |
| 5주차 | p.282~325 | 🍽️         |
| 6주차 | p.328~379 | 🍽️         |
| 7주차 | p.382~430 | 🍽️         |

<br>

<!-- 여기까진 그대로 둬 주세요-->


# 1️⃣ 개념 정리 

## 01. API 사용하기

### API란?
**API(Application Programming Interface)**란 두 프로그램이 서로 대화하기 위한 방법을 정의한 것이다. 데이터베이스에 직접 접근하기 어려울 때, 인증된 URL만 있으면 언제든지 필요한 데이터에 편리하게 접근할 수 있는 방식이다.

### 웹 기반 API와 HTTP
- **HTTP(Hyper Text Transfer Protocol)**: 인터넷에서 웹 페이지를 전송하는 기본 통신 방법
- 웹 기반 API는 HTTP 프로토콜을 사용하며, HTML 대신 주로 **CSV, JSON, XML** 형태로 데이터를 주고받는다
- HTML은 구조가 복잡해 프로그램 간 데이터 교환에 적합하지 않기 때문

### JSON 다루기
**JSON(JavaScript Object Notation)**: 자바스크립트에서 시작되었지만, 현재는 범용 데이터 포맷으로 널리 사용된다. 파이썬의 딕셔너리·리스트와 구조가 유사하다.

| 함수 | 설명 |
|------|------|
| `json.dumps(obj, ensure_ascii=False)` | 파이썬 객체 → JSON 문자열로 변환 |
| `json.loads(json_str)` | JSON 문자열 → 파이썬 객체로 변환 |
| `pd.read_json(json_str)` | JSON 문자열 → 판다스 데이터프레임으로 변환 |

```python
import json

# 파이썬 딕셔너리 → JSON 문자열
d = {"name": "혼자 공부하는 데이터 분석"}
d_str = json.dumps(d, ensure_ascii=False)

# JSON 문자열 → 파이썬 딕셔너리
d2 = json.loads(d_str)

# JSON 배열 → 판다스 데이터프레임
import pandas as pd
pd.read_json(d4_str)
```

> 💡 **직렬화(serialization)**: 프로그램 객체를 저장·전송 가능한 형태(텍스트)로 변환하는 것  
> 💡 **역직렬화(deserialization)**: 직렬화된 데이터를 다시 객체로 복원하는 것

### XML 다루기
**XML(eXtensible Markup Language)**: 컴퓨터와 사람 모두 읽기 쉬운 구조적 문서 포맷. 엘리먼트(element)들이 계층 구조를 이루며 정보를 표현한다.

```xml
<book>
    <name>혼자 공부하는 데이터 분석</name>
    <author>박해선</author>
    <year>2022</year>
</book>
```

| 함수/메서드 | 설명 |
|-------------|------|
| `et.fromstring(xml_str)` | XML 문자열 → Element 객체로 변환 |
| `element.findtext('태그명')` | 자식 엘리먼트의 텍스트 반환 |
| `element.findall('태그명')` | 동일 이름의 모든 자식 엘리먼트 반환 |
| `pd.read_xml(xml_str)` | XML → 판다스 데이터프레임으로 변환 (판다스 1.3.0+) |

```python
import xml.etree.ElementTree as et

book = et.fromstring(x_str)
name = book.findtext('name')   # 태그 이름으로 안전하게 검색

# 여러 자식 엘리먼트 순회
for book in books.findall('book'):
    print(book.findtext('name'))
```

### 파이썬으로 API 호출하기 (requests 패키지)
`requests` 패키지를 사용하면 URL을 HTTP GET 방식으로 호출해 데이터를 받을 수 있다.

```python
import requests

url = "http://data4library.kr/api/loanItemSrch?format=json&startDt=2021-04-01&age=20&authKey=인증키"
r = requests.get(url)

# JSON 응답을 파이썬 객체로 변환
data = r.json()

# 데이터프레임으로 변환
books = [d['doc'] for d in data['response']['docs']]
books_df = pd.DataFrame(books)
```

> 💡 **HTTP GET 방식**: URL 뒤에 `?파라미터=값&파라미터=값` 형태(쿼리 스트링)로 데이터를 전달하는 방식

---

## 02. 웹 스크래핑 사용하기

### 웹 스크래핑이란?
**웹 스크래핑(Web Scraping)** 또는 **웹 크롤링(Web Crawling)**: 프로그램으로 웹사이트의 페이지를 옮겨가며 HTML에서 필요한 데이터를 추출하는 작업. 공개 API가 없을 때 사용하는 최후의 수단이다.

### 데이터프레임 행·열 선택: loc 메서드
판다스의 `loc` 메서드로 원하는 행과 열을 선택할 수 있다.

```python
# 특정 행·열 선택 (리스트)
df.loc[[0, 1], ['bookname', 'authors']]

# 슬라이싱으로 범위 선택 (마지막 항목 포함)
df.loc[0:1, 'bookname':'authors']

# 전체 행, 특정 열 범위
books = books_df.loc[:, 'no':'isbn13']

# 스텝 지정
books_df.loc[::2, 'no':'isbn13']
```

> 💡 `loc`: 레이블(이름) 기준 선택  
> 💡 `iloc`: 정수 위치(인덱스 번호) 기준 선택

### BeautifulSoup (뷰티플수프)으로 HTML 파싱
HTML 문서에서 원하는 태그와 텍스트를 찾을 때 사용하는 대표적인 파이썬 패키지.

```python
from bs4 import BeautifulSoup

# HTML 파싱 객체 생성
soup = BeautifulSoup(r.text, 'html.parser')

# 태그 찾기 - 첫 번째 일치 태그
prd_link = soup.find('a', attrs={'class': 'gd_name'})

# 태그 속성 접근 (딕셔너리처럼)
print(prd_link['href'])  # /Product/Goods/74261416

# 모든 일치 태그 리스트로 반환
prd_tr_list = prd_detail.find_all('tr')

# 태그 안의 텍스트 추출
text = tag.get_text()
```

### 스크래핑 전체 흐름 예시

```python
import requests
from bs4 import BeautifulSoup

def get_page_cnt(isbn):
    # 1. 검색 결과 페이지 HTML 가져오기
    url = 'http://www.yes24.com/Product/Search?domain=BOOK&query={}'
    r = requests.get(url.format(isbn))
    soup = BeautifulSoup(r.text, 'html.parser')

    # 2. 상세 페이지 링크 추출
    prd_info = soup.find('a', attrs={'class': 'gd_name'})
    if prd_info is None:
        return ''

    # 3. 상세 페이지 HTML 가져오기
    r = requests.get('http://www.yes24.com' + prd_info['href'])
    soup = BeautifulSoup(r.text, 'html.parser')

    # 4. 품목정보에서 쪽수 추출
    prd_detail = soup.find('div', attrs={'id': 'infoset_specific'})
    for tr in prd_detail.find_all('tr'):
        if tr.find('th').get_text() == '쪽수, 무게, 크기':
            return tr.find('td').get_text().split()[0]
    return ''
```

### 데이터프레임에 함수 적용: apply() 메서드
데이터프레임의 각 행 또는 열에 함수를 일괄 적용할 때 사용한다.

```python
# 각 행에 함수 적용 (axis=1)
page_count = top10_books.apply(lambda row: get_page_cnt(row['isbn13']), axis=1)

# 각 열에 함수 적용 (axis=0, 기본값)
df.apply(lambda col: some_func(col), axis=0)
```

### 데이터프레임 합치기: merge() 함수

```python
import pandas as pd

# 인덱스 기준으로 합치기
result = pd.merge(top10_books, page_count, left_index=True, right_index=True)

# 특정 열 기준으로 합치기
pd.merge(df1, df2, on='col1')

# how 매개변수로 합치는 방식 지정
pd.merge(df1, df2, how='left', on='col1')   # 왼쪽 기준
pd.merge(df1, df2, how='right', on='col1')  # 오른쪽 기준
pd.merge(df1, df2, how='outer', on='col1')  # 모든 행 유지
pd.merge(df1, df2, how='inner', on='col1')  # 교집합만 (기본값)
```

### 스크래핑 시 주의사항
1. **robots.txt 확인**: 웹사이트가 스크래핑을 허용하는지 먼저 확인 (예: `http://www.yes24.com/robots.txt`)
2. **HTML 태그 특정 가능 여부 확인**: 원하는 데이터가 특정 태그로 식별 가능해야 함
3. **페이지 변경 대응**: 웹페이지는 언제든 구조가 바뀔 수 있어 유지보수가 어려움
4. **스크래핑은 최후의 수단**: 공개 API나 데이터베이스가 있다면 그쪽을 먼저 활용

---

### 핵심 함수 정리

| 함수/메서드 | 설명 |
|-------------|------|
| `json.dumps()` | 파이썬 객체 → JSON 문자열 |
| `json.loads()` | JSON 문자열 → 파이썬 객체 |
| `pd.read_json()` | JSON 문자열 → 데이터프레임 |
| `et.fromstring()` | XML 문자열 → Element 객체 |
| `Element.findtext()` | 자식 엘리먼트 텍스트 반환 |
| `Element.findall()` | 모든 자식 엘리먼트 반환 |
| `requests.get()` | HTTP GET 방식으로 URL 호출 |
| `Response.json()` | 응답 JSON → 파이썬 객체 |
| `df.loc[]` | 레이블 기준 행·열 선택 |
| `BeautifulSoup.find()` | 첫 번째 일치 태그 반환 |
| `BeautifulSoup.find_all()` | 모든 일치 태그 리스트 반환 |
| `BeautifulSoup.get_text()` | 태그 안의 텍스트 반환 |
| `df.apply()` | 행 또는 열에 함수 일괄 적용 |
| `pd.merge()` | 데이터프레임/시리즈 합치기 |


# 2️⃣ 수행 인증

<!-- 교재에서 안내된 과정을 직접 실행해본 뒤, 진행 결과가 보이도록 4~6장의 스크린샷을 캡처하여 아래에 첨부해주세요.-->



<br>
<br>

# 3️⃣ 확인 문제

## 문제 1.

> **🧚Q. 다음 중 BeautifulSoup 외에 웹 스크래핑에 사용할 수 있는 파이썬 패키지로 가장 적절한 것은 무엇인가요?**

```
1️⃣ NumPy  
2️⃣ Scrapy  
3️⃣ Matplotlib  
4️⃣ Scikit-learn  
```

```
정답: 2️⃣ Scrapy

이유: Scrapy는 requests와 BeautifulSoup의 기능을 합쳐놓은 것과 유사한 스크래핑 전용
파이썬 패키지로, 웹사이트에서 데이터를 수집하는 데 특화되어 있다.
NumPy는 수치 연산, Matplotlib은 데이터 시각화, Scikit-learn은 머신러닝 라이브러리로
웹 스크래핑과 관련이 없다.
```



### 🎉 수고하셨습니다.
