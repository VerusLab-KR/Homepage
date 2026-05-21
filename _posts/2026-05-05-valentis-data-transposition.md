---
layout: post
title: "Valentis 데이터 변환 — SWX Sim, Flow, Generic CSV/XLSX 지원"
display_title: "Valentis 데이터 변환<br>— SWX Sim, Flow, Generic CSV/XLSX 지원"
date: 2026-05-05 10:00:00
category: Valentis
description: "Valentis 데이터 변환이 지원하는 세 가지 파일 형식(SWX Sim CSV, SWX Flow XLSX, Generic CSV/XLSX)과 입력-출력 구조 구성, Run All 파이프라인 실행까지의 흐름."
---

앞선 글에서는 CAE 해석이 끝난 뒤 결과 데이터를 입력-출력 구조로 정리하는 것이 왜 중요한지 살펴봤다.

이번 글에서는 Valentis의 데이터 변환(Data Transposition) 기능이 어떤 파일 형식을 지원하고, 불러온 데이터를 어떻게 입력-출력 구조로 구성하는지 구체적으로 살펴본다.

<figure>
  <img src="/assets/blog/2026-05-05-valentis-data-transposition/valentis_data%20transposition.jpeg" alt="Valentis 데이터 변환 메인 화면">
  <figcaption>Valentis 데이터 변환 메인 화면 — 좌측 Load File에서 SWX Sim / SWX Flow / Generic 포맷을 선택하면, 우측 테이블에 케이스별 값과 INPUT·OUTPUT 배지가 정리된다.</figcaption>
</figure>

---

## 지원하는 파일 형식 3가지

Valentis 데이터 변환은 세 가지 유형의 파일 형식을 지원한다.

### SWX Sim (CSV)

SWX Sim은 SOLIDWORKS Simulation Design Study에서 내보낸 CSV 파일을 대상으로 한다.

Valentis는 이 파일에서 케이스별 파라미터와 결과값을 읽어 입력-출력 데이터로 재구성한다.

파일 구조는 다음과 같이 해석된다.

1. 1행: 제목
2. 2행: 케이스 수
3. 3~4행: 헤더 및 계산 항목
4. 5행 이후: 파라미터 데이터

각 파라미터 행에는 이름, 제약 조건, 단위, 초기값, 케이스별 값이 순서대로 저장된다.

Valentis는 이 정보를 기준으로 Input과 Output을 자동 구분한다.
제약 조건이 비어 있는 항목은 Input으로, 제약 조건 값이 있는 항목은 Output으로 분류된다.

### SWX Flow (XLSX)

SWX Flow는 SOLIDWORKS Flow Simulation에서 내보낸 Goals 결과 XLSX 파일을 대상으로 한다.

Valentis는 파일 안의 Goals 시트를 자동으로 찾아 읽는다.
각 행은 하나의 파라미터에 해당하고, 열 방향으로 케이스별 값이 나열된 구조를 사용한다.

SWX Flow는 Goals 시트의 모든 항목을 Output으로 불러온다.
파라미터 스터디 입력값이 포함된 경우, 해당 열 상단의 배지를 클릭하면 바로 Input으로 전환할 수 있다.

### Generic (CSV / XLSX)

Generic 형식은 SOLIDWORKS 외의 CAE 도구 결과물을 위한 범용 포맷이다.

구조는 3행으로 구성된다.

1. 1행: input 또는 output 마커
2. 2행: 파라미터 이름
3. 3행 이후: 케이스별 데이터

마커는 각 열 그룹의 첫 번째 열에만 입력하면 된다.

예를 들어 첫 4개 열이 Input이고, 이후 열이 Output이라면
A1에 input, E1에 output만 입력하면 된다.
나머지 열은 같은 그룹으로 자동 인식된다.

어떤 CAE 도구를 사용하든 이 구조에 맞게 결과 파일을 정리하면 Valentis에서 바로 불러올 수 있다.

---

## 불러온 뒤 확인

파일을 불러오면 상단 정보 카드에 다음 정보가 표시된다.

- 파일명
- 케이스 수
- Input 파라미터 수
- Output 파라미터 수

우측에는 전체 데이터 테이블이 표시된다.
각 열에는 파라미터 이름과 단위가 표시되고, 상단에는 INPUT 또는 OUTPUT 배지가 붙는다.
또한 각 열의 Min/Max 값도 함께 확인할 수 있다.

Input/Output 구분을 수정할 때도 간단하다.
열 상단의 INPUT / OUTPUT 배지를 클릭하면 즉시 전환된다.

---

## 내보내기

데이터 확인이 끝나면 Export를 실행한다.

Input 파라미터는 `{원본파일명}_Input.csv`로,
Output 파라미터는 `{원본파일명}_Output.csv`로 각각 분리되어 저장된다.

이 두 파일은 이후 민감도 분석, 메타모델 구축, 신뢰성 해석, 파레토 최적화에서 공통 입력 데이터로 사용된다.

---

## 분석 흐름과의 연결

파일을 불러오면 자동 분석 실행 준비가 완료된다.

조건과 목표를 정의하고 Run All을 실행하면, 민감도 분석부터 파레토 최적화까지 모든 분석이 순서대로 자동 수행된다.

이 과정은 다음 글에서 자세히 살펴본다.

---

## 마무리

CAE 도구마다 결과 파일 형식은 다르다.

하지만 데이터 변환을 거치면 서로 다른 형식의 결과도 동일한 분석 흐름으로 연결할 수 있다.

SWX Sim CSV, Flow Simulation XLSX, Generic CSV/XLSX 포맷 중 어떤 형식을 사용하더라도 Valentis Analysis Intelligence에서 사용할 수 있는 입력-출력 데이터 구조로 정리할 수 있다.


---

*키워드: Valentis 데이터 변환, SolidWorks 결과 가져오기, CAE 결과 정리, 해석 데이터 CSV, SWX Sim 결과, Flow Simulation Goals, Generic CAE 포맷*
