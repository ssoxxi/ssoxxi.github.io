---
layout: default
title: " sql | HackerRank | Weather Observation Station 5"
date: 2026-02-04
category: sql
tags: [sql, practice]
excerpt: "문자열 길이 / 정렬 / LIMIT"
---
<div class="prose">


# 🏙️ HackerRank | Weather Observation Station 5

- 📅 Date: 2026-02-04
- 🔗 Link: https://www.hackerrank.com/challenges/weather-observation-station-5/problem
- 🏷️ Topic: 문자열 길이 / 정렬 / LIMIT

---

## 🧩문제/상황
도시 이름(CITY) 중에서  
- ✅ **가장 짧은 CITY**
- ✅ **가장 긴 CITY**
를 각각 출력해야 한다.

⚙️ 출력 규칙:
- 출력 컬럼: `CITY`, `CITY 길이`
- 길이가 같다면 **CITY를 알파벳 오름차순**으로 가장 앞선 것 선택
- 결과는 **짧은 것 1개 + 긴 것 1개** 총 2줄

---

## ✅핵심 쿼리 (MySQL)

### 1) 가장 짧은 CITY 1개
```sql
SELECT CITY, CHAR_LENGTH(CITY) AS LEN
FROM STATION
ORDER BY LEN ASC, CITY ASC
LIMIT 1;
```

### 2) 가장 긴 CITY 1개
```sql
SELECT CITY, CHAR_LENGTH(CITY) AS LEN
FROM STATION
ORDER BY LEN DESC, CITY ASC
LIMIT 1;
```

### 3) 합쳐서 제출 ver.
```sql
(
  SELECT CITY, CHAR_LENGTH(CITY) AS LEN
  FROM STATION
  ORDER BY LEN ASC, CITY ASC
  LIMIT 1
)
UNION ALL
(
  SELECT CITY, CHAR_LENGTH(CITY) AS LEN
  FROM STATION
  ORDER BY LEN DESC, CITY ASC
  LIMIT 1
);
```

---

## 🔍포인트(왜 이렇게)
- 🧠 **“최소/최대 + 동률이면 알파벳순 1개”**는  
  → `ORDER BY 길이, CITY` 후 `LIMIT 1`이 가장 직관적이고 안전함.
- 🧩 `MIN(CHAR_LENGTH(CITY))`로 길이만 뽑고 다시 매칭하면  
  → 동률 처리(알파벳순 1개) 때문에 쿼리가 복잡해지기 쉬움.
- 🔤 MySQL에서 문자열 길이 계산은 `CHAR_LENGTH()`가 일반적으로 더 안전함  
  → 멀티바이트 환경에서 “문자 수” 기준.

---

## ⚠️실수/주의
- ⚠️ `IN (SELECT MIN(CHAR_LENGTH(CITY)), CITY ...)` 같은 형태는  
  → `IN`이 단일 컬럼 비교인데, 서브쿼리가 2컬럼을 내보내 구조가 맞지 않음.
- ⚠️ `LENGTH()` vs `CHAR_LENGTH()`  
  - `LENGTH()` = 바이트 수 (한글/이모지에서 값 커질 수 있음)  
  - `CHAR_LENGTH()` = 문자 수 (문제 의도에 더 부합)

---

## ➡️ 다음 액션
- ✅ “최솟값/최댓값 + tie-break” 패턴 문제 3개 더 풀기
- ✅ `GROUP BY + ORDER BY + LIMIT` 기반 Top-N 패턴 정리하기
- ✅ `CHAR_LENGTH vs LENGTH`를 `cheatsheet.md`에 누적하기
</div>
