# 🏙️ HackerRank | Weather Observation Station 6

- 📅 Date: 2026-02-06
- 🔗 Link: https://www.hackerrank.com/challenges/weather-observation-station-6/problem?isFullScreen=true
- 🏷️ Topic: 문자열 패턴 필터링 / 중복제

---

## 🧩문제/상황
도시 이름(CITY) 중에서  
- ✅ **모음(i.e., a, e, i, o, or u)으로 시작하는 CITY**
를 중복없이 각각 출력해야 한다.

⚙️ 출력 규칙:
- 출력 컬럼: `CITY`
- 결과가 같다면 중복없이 출력

---

## ✅핵심 쿼리 (MySQL)

### 1) 간단 버전
```sql
SELECT DISTINCT CITY
  FROM STATION
WHERE LOWER(SUBSTR(CITY, 1, 1)) IN ('a','e','i','o','u')
;
```

### 2) 정규식 버전
```sql
SELECT DISTINCT CITY
  FROM STATION
 WHERE CITY REGEXP '^[aeiouAEIOU]'
 ;

 ### 3) 나의 시도 수정 버전
 ```sql
 WITH VOWELS AS (
  SELECT CITY,
         LOWER(SUBSTR(CITY, 1, 1)) AS CITY_VOW
    FROM STATION
)
SELECT DISTINCT CITY
  FROM VOWELS
 WHERE CITY_VOW IN ('a','e','i','o','u');
;
```

---

## ❌처음 시도했던 쿼리 
```sql
WITH VOWELS AS ( 
    SELECT * 
      FROM ( SELECT SUBSTR(CITY,1,1) AS CITY_VOW FROM STATION )AS T 
     WHERE CITY IN ('a', 'e', 'i', 'o', 'u') 
) 
SELECT DISTINCT(CITY) 
FROM VOWELS 
;
```
## 👩🏻‍🏫 시도했던 쿼리에서 개선이 필요한 점
- 1) WHERE CITY IN ('a','e','i','o','u') ← **비교 대상 컬럼이 틀림**  
  - CTE VOWELS에서 만든 건 `CITY_VOW`(도시명의 첫 글자)인데, 필터는 `CITY`로 하고 있음
  - 의도: 첫 글자(CITY_VOW)가 모음인지 확인
  - 현재: 도시 전체 이름(CITY)이 'a' 같은 한 글자와 정확히 일치하는지 확인
  → 거의 항상 매칭이 안 되어 결과가 0건
- 또한 CTE 내부에 CITY 컬럼이 존재하지도 않음. (CTE가 `CITY_VOW`만 반환하니까)

- 2) CTE에서 CITY 자체를 잃어버림
  - CTE에서 `SELECT SUBSTR(CITY,1,1) AS CITY_VOW`만 뽑아서, 바깥 `SELECT DISTINCT(CITY) FROM VOWELS`에서 `CITY`를 출력하려 해도 컬럼이 없음.
  - 즉, 지금 구조는 
  - CTE: CITY_VOW만 있음 , 바깥: CITY를 SELECT

- 3) 불필요하게 복잡함
  - 이 문제는 굳이 CTE/서브쿼리 없이 한 줄로 끝나는 유형 (가독성 + 성능 측면 모두)

---

## 🔍포인트(왜 이렇게)
- 🧠 “최소/최대 + 동률이면 알파벳순 1개” 패턴은
  → `ORDER BY` (길이), `CITY`로 정렬 규칙을 그대로 코드에 반영하고 `LIMIT 1`로 1개만 확정하는 게 가장 직관적이고 안전함.
- 🧩 `MIN(CHAR_LENGTH(CITY))`로 길이만 뽑고 다시 매칭하면  
  → 동률 처리(알파벳순 1개) 때문에 쿼리가 복잡해지기 쉬움.
- 🔤 MySQL에서 문자열 길이 계산은 `CHAR_LENGTH()`가 일반적으로 더 안전함  
  → 멀티바이트 환경에서 “문자 수” 기준.
- `SUBSTR(CITY,1,1)`도 있지만 `LEFT(CITY,1)`도 가능함

---

## ⚠️실수/주의
- ⚠️ `LOWER()`를 씌우면 데이터에 대문자가 섞여도 안전
- ⚠️ 정규식 시작 앵커 `^`로 “첫 글자”만 체크
- ⚠️ NULL/빈 문자열: 실무 데이터면 CITY IS NOT NULL AND CITY <> '' 같은 방어가 필요할 수 있음
- 🚨 CTE 남발 금지
---

## ➡️ 다음 액션
- ✅ “파생컬럼 만들었으면, 필터도 그 컬럼으로” 패턴 고정하기
  - CITY_VOW 만들었으면 WHERE CITY_VOW ...로 끝까지 일관성 유지
- ✅ CTE 쓰기 전 체크리스트 2개만 습관화
  - 바깥에서 뽑을 컬럼이 CTE에 실제로 포함돼 있나?
  - WHERE가 의도한 컬럼을 보고 있나?
- ✅ 문자열 조건 문제에서 **대소문자 normalize(LOWER/UPPER)**를 기본으로 넣기
