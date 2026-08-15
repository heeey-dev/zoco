# Performance Marketer --- Codex Implementation Handoff

> **목적:** 이 문서는 아직 구현되지 않은 `Performance Marketer`
> 프로젝트를 Codex가 기획 의도를 잃지 않고 구현할 수 있도록 전달하는
> 인수인계 문서다.\
> **현재 단계:** Architecture / Workflow Design → Implementation 준비\
> **핵심 원칙:** "카피를 생성하는 프롬프트"가 아니라, **좋은 마케팅
> 판단과 카피에 도달하는 사고 환경**을 구현한다.

------------------------------------------------------------------------

## 0. Codex에게 가장 먼저 전달할 것

이 프로젝트를 단순한 **AI 카피 생성기**로 해석하지 않는다.

사용자가 원하는 것은 다음과 같다.

> 고객과 상품을 이해하고, 문제를 정의하고, 현실에서 작동할 가능성을
> 검토하고, 고객별 설득 전략을 세운 뒤, 그 전략을 카피와 마케팅
> 실행안으로 변환하는 시스템.

따라서 구현의 중심은 "문장을 잘 쓰는 능력" 자체가 아니다.

``` text
이해
→ 판단
→ 구체화
→ 검증
→ 설득 전략
→ 카피
→ 실행 전략
```

이 흐름이 재사용 가능하고 유지보수 가능한 형태로 작동해야 한다.

------------------------------------------------------------------------

# 1. Project Concept

## Performance Marketer

### Concept

**이쁘기만 한 카피?**

아니다.

> **고객을 설득하고, 행동하게 만들고, 실제로 팔기 위한 퍼포먼스 마케터**

를 만든다.

카피라이팅은 독립된 문장 기술이 아니라 실제 마케팅 업무 흐름 안에서
사용한다.

### Slogan

> **기본 세일즈 카피라이팅부터, 브랜드를 위한 카피라이팅까지.**

> **고객을 설득하고, 팔기 위한 진짜 카피라이팅.**

------------------------------------------------------------------------

# 2. 사용자의 핵심 기획 의도

이 프로젝트의 가장 중요한 특징은 **정답을 출력하는 프롬프트보다 정답에
도달하는 사고 환경을 먼저 설계한다는 것**이다.

즉,

``` text
Input
→ Prompt
→ Copy
```

와 같은 단순 구조를 목표로 하지 않는다.

대신:

``` text
Base
→ Knowledge
→ Reference
→ Research
→ Analysis
→ Persona
→ Strategy
→ Copy
→ Review
→ Output
```

처럼 여러 종류의 정보와 판단을 구분하고 관계를 만든다.

### 중요한 해석 원칙

파일 구조는 단순한 저장 위치가 아니다.

> **파일 구조는 시스템 내부의 발상 흐름 및 참조 흐름과 유사해야 한다.**

따라서 Codex는 폴더와 MD 파일을 만들 때 단순 분류 체계만 설계하지 말고,\
**어떤 기능이 어떤 지식을 언제 읽어야 하는지**까지 함께 고려해야 한다.

------------------------------------------------------------------------

# 3. 무엇을 만들려는가

최종적으로 다음 기능을 가진 시스템을 지향한다.

1.  문제 분석 및 Benefit / Merit 수립
2.  Target Persona A / B / C 정의
3.  Persona별 Copywriting 작성
4.  Copy Formula 기반 Persona별 Copywriting 작성
5.  브랜드를 위한 Copywriting
6.  Copy Review & Revision
7.  Persona별 광고 / 마케팅 전략 수립
8.  업무용 산출물 생성
9.  Reference / Research / Output 관리
10. 향후 축적되는 Copywriting Knowledge Asset 활용

단, **처음부터 모든 기능을 완성하지 않는다.**

------------------------------------------------------------------------

# 4. 가장 중요한 시스템 개념

## 4.1 Knowledge

**"무엇을 알고 있는가 / 어떻게 판단해야 하는가"**

카피라이팅과 마케팅에 대한 일반화 가능한 지식이다.

예:

-   고객 페르소나 이론
-   구매 동기
-   Benefit 설계
-   설득 원리
-   Copywriting Formula
-   좋은 카피의 평가 기준
-   브랜드 카피 원칙

Knowledge는 특정 회사나 상품의 사실을 저장하는 공간이 아니다.

------------------------------------------------------------------------

## 4.2 Reference

**"이번에 다루는 브랜드 / 회사 / 상품 / 서비스는 무엇인가"**

프로젝트 내부 자료다.

예:

-   회사소개서
-   브랜드북
-   상품기획서
-   제품 설명
-   상세페이지
-   기존 광고
-   기존 카피
-   내부 문서

### 중요

Reference의 내용은 무조건 객관적 사실로 취급하지 않는다.

예를 들어 회사소개서에

> "우리 제품은 품질이 뛰어나다."

라고 적혀 있다면,

시스템은 이를 곧바로

> "품질이 뛰어난 제품"

이라는 확정 사실로 사용해서는 안 된다.

우선 다음처럼 해석할 수 있어야 한다.

> "회사가 품질을 주요 강점으로 주장하고 있다."

즉, **Reference에는 사실뿐 아니라 회사의 주장, 관점, 포지셔닝도 포함될
수 있다.**

------------------------------------------------------------------------

## 4.3 Research

**"현실 세계에서는 무엇이 실제로 작동하고 있는가"**

Research는 단순 자료 수집 폴더가 아니다.

Knowledge와 Reference에서 만들어진 초기 가설을 현실과 충돌시키는 **검증
레이어**다.

예:

``` text
Reference
회사는 '품질'을 강점으로 주장한다.
        ↓
Knowledge
추상적 우수성보다 구체적인 Proof가 설득에 유리하다.
        ↓
Research
경쟁사는 품질을 어떻게 증명하는가?
실제 고객 리뷰에서 어떤 불만이 반복되는가?
시장에서 고객은 무엇을 비교하는가?
        ↓
Decision
'품질이 좋다'는 추상 문장을 버리고
고객이 이해할 수 있는 Benefit / Proof로 변환한다.
```

------------------------------------------------------------------------

# 5. Knowledge × Reference × Research

이 세 영역은 서로 대체하지 않는다.

``` text
Knowledge
"판단법"

Reference
"이번 대상에 대한 정보"

Research
"현실 검증"
```

현재의 핵심 가설은 다음과 같다.

``` text
Knowledge
   ↓
초기 판단 기준
   ↓
Reference
   ↓
대상에 맞는 구체화
   ↓
가설 형성
   ↓
Research
   ↓
현실성 검증 / 반례 탐색 / 비교
   ↓
Strategy
   ↓
Copy
```

### 목표

이 구조를 통해 시스템이 단순히 "좋아 보이는 카피"를 만드는 것을 넘어,

> **팔리는 세계와 안 팔리는 세계를 분별하는 판단력**

을 조금씩 갖도록 한다.

### 주의

이 구조는 현재 **핵심 가설**이지 완전히 확정된 구현 명세가 아니다.

Codex가 임의로 복잡한 RAG, Vector DB, Agent Network 등으로 확대 구현하지
않는다.

먼저 MD 기반의 단순한 구조에서 실제로 유효한지 테스트한다.

------------------------------------------------------------------------

# 6. Base의 역할

`Base`는 일반적인 Knowledge와 다르다.

Base에는 **지식을 받아들이고 사용하는 태도**가 들어간다.

예:

-   공식에 상황을 억지로 끼워 맞추지 않는다.
-   Reference의 주장을 객관적 사실로 오인하지 않는다.
-   좋은 표현보다 고객의 이해와 행동을 우선한다.
-   고객 Persona를 상상만으로 과도하게 만들어내지 않는다.
-   정보가 부족하면 추정과 사실을 구분한다.
-   Formula는 답이 아니라 사고 도구로 사용한다.
-   Research는 장식용 출처 수집이 아니라 가설 검증에 사용한다.
-   브랜드 카피와 퍼포먼스 카피의 목적 차이를 이해한다.
-   여러 지식을 단순 합산하지 않고 현재 문제와의 관계를 판단한다.

Base는 모든 Workflow에 영향을 미치는 **상위 판단 원칙**으로 취급한다.

------------------------------------------------------------------------

# 7. Copywriting Knowledge Asset

## 7.1 기본 Knowledge

단순하지만 근본적인 이론을 우선한다.

예:

-   고객은 왜 구매하는가
-   구매하지 않는 이유는 무엇인가
-   Feature와 Benefit의 차이
-   고객 문제와 욕구
-   Persona
-   Objection
-   Proof
-   Offer
-   CTA
-   브랜드 언어와 세일즈 언어의 차이

------------------------------------------------------------------------

## 7.2 대표적인 Copy Formula

부록형 Knowledge Asset으로 관리한다.

### AIDA

Attention → Interest → Desire → Action

### PAS

Problem → Agitation → Solution

### FAB

Feature → Advantage → Benefit

### BAB

Before → After → Bridge

### 4Ps

Promise → Picture → Proof → Push

### 4Cs

Clear → Concise → Compelling → Credible

### 중요

공식을 사용하기 위해 카피를 만드는 것이 아니다.

현재 Persona와 문제에 적합한 경우에만 공식을 선택한다.

------------------------------------------------------------------------

# 8. 문장 구조로 보는 카피라이팅 공식

이 프로젝트의 장기적인 핵심 지식 자산 중 하나다.

좋은 카피 문장을 발견했을 때 문장 자체만 저장하지 않는다.

``` text
실제 Copy
   ↓
문장 구조 분석
   ↓
무엇이 설득을 만드는지 분석
   ↓
추상적인 원리 추출
   ↓
재사용 가능한 Formula
   ↓
다른 Persona / 상품에 적용
```

예를 들어 향후 각 Formula MD는 다음과 같은 구조를 가질 수 있다.

``` text
# Formula Name

## Original Example
실제 예시

## Sentence Structure
문장 구조

## Persuasion Mechanism
왜 작동하는가

## Best Use Case
언제 사용하는가

## Bad Use Case
언제 사용하면 안 되는가

## Variables
교체 가능한 요소

## Variations
응용 구조

## Notes
주의사항
```

이 구조는 구현 과정에서 테스트 후 수정 가능하다.

------------------------------------------------------------------------

# 9. Persona A / B / C

한 상품에도 서로 다른 구매 이유를 가진 고객이 존재한다고 전제한다.

따라서 하나의 거대한 Persona보다 주요 고객을 A / B / C로 구분할 수 있다.

Persona는 최소한 다음을 고려한다.

``` text
상황
문제
욕구
구매 동기
구매를 망설이는 이유
중요하게 생각하는 가치
반응하는 메시지
필요한 Proof
적합한 Benefit
적합한 Copy Direction
```

### 중요한 원칙

Persona는 소설 속 캐릭터처럼 과도하게 창작하지 않는다.

마케팅 판단에 필요한 수준으로만 구체화한다.

가능하면 Reference와 Research에 근거한다.

------------------------------------------------------------------------

# 10. Benefit / Merit Workflow

제품 Feature를 그대로 광고 문장으로 사용하지 않는다.

기본 변환 사고는 다음과 같다.

``` text
Feature
  ↓
Advantage
  ↓
Benefit
  ↓
Customer Value
  ↓
Possible Proof
```

예:

``` text
Feature
배송 시스템이 자동화되어 있음

↓ Advantage

처리 속도가 빠름

↓ Benefit

고객이 오래 기다리지 않아도 됨

↓ Customer Value

급한 업무에서도 일정 예측이 쉬움

↓ Proof

평균 처리 시간 / 실제 납기 데이터 등
```

실제 구현에서는 반드시 모든 항목을 억지로 채울 필요는 없다.

------------------------------------------------------------------------

# 11. Core Workflow

전체 심화 Workflow의 개념은 다음과 같다.

``` text
브랜드 / 회사 / 상품 / 서비스 이해
                ↓
             문제 정의
                ↓
        Benefit / Merit 정의
                ↓
       Persona A / B / C
                ↓
      Persona별 Copy Draft
                ↓
         Copy Formula 적용
                ↓
          Copy 비교 / 검토
                ↓
            Copy 결정
                ↓
          Copy Strategy
                ↓
   Advertising / Marketing Strategy
```

그러나 이 전체 흐름을 항상 강제로 실행하지 않는다.

기능별 Workflow가 독립적으로 호출될 수 있어야 한다.

------------------------------------------------------------------------

# 12. User Flow와 Workflow

두 개념을 구분한다.

## Workflow

시스템 내부에서 문제를 해결하는 사고 / 작업 흐름.

## User Flow

사용자와 시스템이 실제로 주고받는 대화와 선택의 흐름.

### 핵심 기획 방향

User Flow를 Workflow와 완전히 별개의 UI 시나리오로 만들기보다,

> **Workflow 안의 Checkpoint / Decision / Approval Point로 녹여낸다.**

예:

``` text
[Internal Workflow]

문제 정의
   ↓
Benefit 후보 도출
   ↓
──────────────
User Checkpoint
"이 방향이 맞는가?"
──────────────
   ↓
Persona 설계
   ↓
Copy Draft
   ↓
──────────────
User Checkpoint
"어느 방향을 발전시킬 것인가?"
──────────────
   ↓
Final Copy
```

### 중요한 UX 원칙

매 단계마다 사용자를 귀찮게 확인시키지 않는다.

확인이 필요한 것은:

-   방향이 크게 갈리는 지점
-   잘못 판단하면 이후 작업 전체가 달라지는 지점
-   사용자 취향 / 사업 판단이 필요한 지점
-   Reference만으로 확정할 수 없는 지점

정도로 제한한다.

------------------------------------------------------------------------

# 13. 작업 깊이 판단 --- User Request Routing

모든 사용자 요청에 동일한 Workflow와 동일한 수준의 분석을 적용하지
않는다.

먼저 요청의 복잡도, 필요한 판단 수준, 기존 정보의 충분성, 결과물의
목적을 판단하여 **QUICK / STANDARD / DEEP** 중 적절한 작업 깊이를
선택한다.

``` text
                  User Request
                       │
                작업 복잡도 판단
                       │
          ┌────────────┼────────────┐
          ↓            ↓            ↓
        QUICK        STANDARD       DEEP
          │            │            │
      한방 생성     핵심 Knowledge   Reference
                       +           + Knowledge
                    Reference      + Research
                       │           + Persona
                       │           + Strategy
          └────────────┼────────────┘
                       ↓
                     Output
```

## QUICK

목적이 명확하고 추가적인 전략 판단이 거의 필요하지 않은 요청이다.
최소한의 맥락만 사용해 빠르게 처리한다.

예: 짧은 카피 변형, 이미 결정된 방향의 문장 작성, 기존 카피의 간단한
대안 생성.

**한방 Prompt가 더 좋은 결과를 낼 수 있다면 굳이 복잡한 Workflow를
호출하지 않는다.**

## STANDARD

일반적인 마케팅 판단이 필요한 요청이다. 핵심 Knowledge와 현재 프로젝트의
Reference를 사용해 문제와 고객 맥락을 이해한 뒤 결과를 만든다.

예: 상품별 세일즈 카피, 기본 Persona 기반 카피, Benefit을 반영한 광고
문구.

## DEEP

문제 정의 자체가 필요하거나 여러 가설의 비교, 전략적 판단, 현실 검증이
필요한 요청이다.

필요에 따라 다음 자산을 **선택적으로** 사용한다.

``` text
Reference
+ Knowledge
+ Research
+ Persona
+ Benefit
+ Strategy
+ User Checkpoint
```

예: 새로운 브랜드의 Copy Strategy, 고객군이 불명확한 상품의 Persona
설계, 시장·경쟁 검토가 필요한 Copywriting, 광고·마케팅 전략까지 연결되는
작업.

## Routing Principle

이 분기는 단순한 **난이도 분류**가 아니다. 핵심은 **이 요청에 어느
정도의 사고 환경이 실제로 필요한가**를 판단하는 것이다.

> **복잡한 구조를 사용하는 것 자체가 목적이 아니라, 좋은 결과에 필요한
> 만큼만 사고한다.**

따라서 시스템은 항상 DEEP Workflow를 선호해서는 안 된다.

``` text
더 많은 분석
≠
더 좋은 결과
```

구조의 복잡성은 실제 Output 품질, 재현성, 근거성 또는 전략적 가치의
향상으로 정당화되어야 한다.

## Baseline Principle

가능하면 단순한 **Reference + 고품질 단일 Prompt** 방식도 Baseline으로
유지한다.

복잡한 Workflow가 Baseline보다 실제로 나은 결과를 만들지 못한다면
불필요한 단계 제거, 호출 조건 축소, Knowledge 선택 방식 개선, Research
호출 조건 개선, Workflow 단순화를 검토한다.

> **Performance Marketer의 목적은 가장 복잡한 시스템을 만드는 것이
> 아니라, 문제에 필요한 최소한의 사고 깊이로 반복해서 좋은 결과에
> 도달하는 것이다.**

------------------------------------------------------------------------

# 14. Resource-oriented Architecture --- 흐름은 깊게, 기능은 독립적으로

이 프로젝트는 본질적으로 **흐름을 중요하게 생각하는 시스템**이다.

그러나 구현 단계에서는 모든 지식과 기능을 하나의 고정된 Pipeline으로
연결하지 않는다.

> **Knowledge / Reference / Research / Persona / Formula / Review는
> 반드시 지나야 하는 단계가 아니라, 필요할 때 꺼내 쓰는
> 자원(Resource)으로 취급한다.**

즉, 설계 단계에서는 전체 흐름을 깊게 이해하되 구현 단계에서는 각 기능이
독립적으로 호출될 수 있어야 한다.

``` text
잘못된 구현

Knowledge
   ↓
Reference
   ↓
Research
   ↓
Benefit
   ↓
Persona
   ↓
Formula
   ↓
Copy
   ↓
Review
   ↓
Strategy
```

위 구조를 모든 요청에 강제로 적용하면 시스템은 쉽게 과설계되고, 단순한
작업에도 불필요한 비용과 판단 단계를 요구하게 된다.

대신 다음 구조를 지향한다.

``` text
                  User Request
                       │
                필요한 작업 판단
                       │
          ┌────────────┼────────────┐
          ↓            ↓            ↓
      Knowledge     Reference     Research
          │            │            │
          ├──── Persona / Benefit ──┤
          │            │            │
          └──── Formula / Review ───┘
                       │
                 필요한 기능만 호출
                       │
                     Output
```

## Resource Principle

각 자산과 기능은 가능한 한 **단일 책임(Single Responsibility)** 을
가진다.

예를 들어:

-   Knowledge는 판단 기준을 제공한다.
-   Reference는 현재 대상에 대한 정보를 제공한다.
-   Research는 외부 현실을 검증한다.
-   Persona는 고객 관점을 정의한다.
-   Benefit은 제품 가치를 고객 언어로 변환한다.
-   Formula는 설득 구조를 제공한다.
-   Review는 결과를 평가하고 수정한다.

이들은 하나의 거대한 Workflow 안에 종속된 단계가 아니라, 요청과 상황에
따라 선택적으로 조합되는 **재사용 가능한 기능 단위**다.

## Flow ≠ Mandatory Pipeline

전체 흐름을 이해하는 것은 중요하다.

하지만 흐름을 이해한다는 것이 모든 요청에 그 흐름을 강제로 통과시킨다는
뜻은 아니다.

> **흐름은 사고의 지도이고, 기능은 필요할 때 꺼내 쓰는 도구다.**

전체 Architecture는 관계를 이해하기 위해 존재하고, 실제 실행은 가능한 한
짧고 적절한 경로를 선택한다.

## Deep Design, Shallow Implementation

이 프로젝트의 구현 원칙은 다음 문장으로 요약한다.

> **생각은 깊게 해두되, 구현은 얕게 시작한다.**

설계 단계에서는 다음을 충분히 고민한다.

-   각 지식의 역할
-   서로의 관계
-   참조 우선순위
-   판단 분기
-   실패 가능성
-   향후 확장성

그러나 V1에서는 그 모든 가능성을 미리 구현하지 않는다.

``` text
Deep Design
전체 지도와 관계를 충분히 이해한다.
        ↓
Shallow Implementation
가장 작은 기능 단위로 구현한다.
        ↓
Real Test
실제 작업에 사용한다.
        ↓
Evidence
품질 향상이 확인되는 구조만 남긴다.
        ↓
Expansion
필요성이 증명된 기능만 확장한다.
```

## Implementation Rule

새로운 기능이나 Layer를 추가할 때는 다음을 확인한다.

1.  이 기능이 독립적으로 호출 가능한가?
2.  다른 Workflow에서도 재사용할 수 있는가?
3.  이 기능이 없어도 되는 요청은 건너뛸 수 있는가?
4.  실제 Output 품질 또는 판단 품질을 개선하는가?
5.  Baseline보다 유의미한 차이를 만드는가?

하나의 기능이 단지 "전체 Workflow에 있으니까" 존재해서는 안 된다.

## Core Principle

> **전체 사고는 Flow로 설계하되, 실제 시스템은 Resource와 단일 기능의
> 조합으로 구현한다.**

이를 통해 Performance Marketer는 거대한 고정 Pipeline이 아니라,

> **요청에 맞춰 필요한 판단 도구를 꺼내 조합하는 유연한 업무 시스템**

으로 발전해야 한다.

------------------------------------------------------------------------

# 15. 초기 구현 범위 --- MVP

처음부터 거대한 자율형 Marketing Agent를 만들지 않는다.

**V0 / MVP는 다음 4개 Workflow를 우선 구현한다.**

``` text
01. Problem & Benefit Analysis
02. Persona A / B / C Definition
03. Persona-based Copywriting
04. Copy Review & Revision
```

이 네 기능이 실제 대화에서 자연스럽게 연결되는지 먼저 검증한다.

그 후 다음 기능을 추가한다.

``` text
05. Formula-based Copywriting
06. Research Validation
07. Brand Copywriting
08. Copy Strategy
09. Advertising / Marketing Strategy
```

------------------------------------------------------------------------

# 16. 제안 Repository Structure

초기 구조 예시다.

Codex는 실제 구현 전 이 구조를 검토하고 필요한 최소 수정안을 제안할 수
있다.

``` text
performance-marketer/
│
├─ README.md
├─ AGENTS.md
│
├─ base/
│   └─ BASE.md
│
├─ knowledge/
│   ├─ COPYWRITING.md
│   ├─ PERSONA.md
│   ├─ BENEFIT.md
│   │
│   └─ formulas/
│       ├─ README.md
│       ├─ AIDA.md
│       ├─ PAS.md
│       ├─ FAB.md
│       ├─ BAB.md
│       ├─ 4PS.md
│       └─ 4CS.md
│
├─ workflows/
│   ├─ 01_problem-benefit.md
│   ├─ 02_persona-definition.md
│   ├─ 03_persona-copywriting.md
│   ├─ 04_copy-review.md
│   └─ README.md
│
├─ reference/
│   └─ README.md
│
├─ research/
│   └─ README.md
│
├─ outputs/
│   └─ README.md
│
└─ templates/
    ├─ benefit-analysis.md
    ├─ persona.md
    ├─ copy-draft.md
    └─ copy-review.md
```

### 구조 설계 원칙

폴더를 많이 만드는 것이 목적이 아니다.

각 파일에는 가능한 한 명확한 책임이 있어야 한다.

------------------------------------------------------------------------

# 17. 예상 참조 규칙

예를 들어 `Persona-based Copywriting` 기능이 호출되면 다음처럼 동작할 수
있다.

``` text
1. BASE.md
   ↓
2. PERSONA.md / COPYWRITING.md
   ↓
3. 현재 프로젝트 Reference
   ↓
4. 기존 Persona 결과가 있다면 해당 Output
   ↓
5. 필요할 경우 적합한 Formula
   ↓
6. 필요할 경우 Research
   ↓
7. Copy Draft 생성
   ↓
8. Review / User Checkpoint
```

### 중요

모든 파일을 매번 읽는 방식은 피한다.

> **현재 작업에 필요한 지식만 선택적으로 참조하는 구조**

를 지향한다.

------------------------------------------------------------------------

# 18. Output Philosophy

결과물은 채팅 답변만을 의미하지 않는다.

업무에서 재사용 가능한 파일을 만들 수 있어야 한다.

모든 주요 Output은 가능하면 다음을 고려한다.

-   재사용
-   유지보수
-   협업
-   버전 관리
-   인수인계
-   추후 확장

예:

``` text
outputs/
└─ project-name/
   ├─ 01_problem-benefit.md
   ├─ 02_persona.md
   ├─ 03_copy-draft.md
   ├─ 04_copy-review.md
   └─ 05_copy-strategy.md
```

------------------------------------------------------------------------

# 19. Brand Copywriting --- 심화 기능

일반적인 Performance Copy와 Brand Copy를 완전히 동일하게 취급하지
않는다.

Performance Copy의 중심 질문:

> "왜 지금 이 고객이 행동해야 하는가?"

Brand Copy의 중심 질문:

> "이 브랜드는 무엇을 어떤 태도와 언어로 말해야 하는가?"

최종적으로는 두 영역을 연결한다.

> **팔리면서도 브랜드의 언어가 남는 카피**

를 목표로 한다.

단, Brand Copy 기능은 MVP 이후 구현한다.

------------------------------------------------------------------------

# 20. Copy → Marketing Strategy 확장

카피는 최종 종착지가 아니다.

향후 다음 흐름으로 확장한다.

``` text
Persona
   ↓
Problem / Desire
   ↓
Benefit
   ↓
Message
   ↓
Copy
   ↓
Creative Direction
   ↓
Channel
   ↓
Campaign
```

이를 통해 Persona별 광고 / 마케팅 전략을 수립할 수 있도록 한다.

------------------------------------------------------------------------

# 21. Codex 구현 시 절대 피할 것

## 19.1 과도한 자동화

처음부터 모든 것을 자동 판단하는 Agent를 만들지 않는다.

사용자 판단이 중요한 부분은 Checkpoint로 남긴다.

## 19.2 과도한 추상화

멋있는 Architecture를 만드는 것이 목적이 아니다.

실제 Copy 품질이 좋아지는지 확인한다.

## 19.3 Formula 남용

AIDA, PAS 등에 모든 문장을 억지로 맞추지 않는다.

Formula는 선택 가능한 사고 도구다.

## 19.4 Reference 맹신

회사 내부 자료의 주장을 객관적 진실처럼 사용하지 않는다.

## 19.5 Research 남용

모든 작업에 무조건 Research를 실행하지 않는다.

현실 검증이 실제로 필요한 경우에 사용한다.

## 19.6 Persona 과창작

근거 없는 나이, 직업, 취미 등의 디테일을 마구 생성하지 않는다.

## 19.7 거대한 단일 MD

모든 지식과 기능을 한 파일에 넣지 않는다.

역할별로 분리하되 지나치게 파편화하지 않는다.

## 19.8 구현을 위한 구현

Vector DB, RAG, 별도 DB, 복잡한 Agent Routing 등은 필요성이 검증되기 전
도입하지 않는다.

------------------------------------------------------------------------

# 22. Codex의 구현 태도

Codex는 이 프로젝트에서 단순 코더가 아니라 **구현 파트너**로 행동한다.

다만 기획 의도를 임의로 바꾸지 않는다.

다음 순서를 따른다.

``` text
1. 현재 문서 전체 이해
2. 핵심 의도 요약
3. 모호한 부분 식별
4. 구현에 필요한 최소 Architecture 제안
5. 사용자 승인
6. 작은 단위 구현
7. 실제 테스트
8. 문제점 관찰
9. 구조 수정
10. 다음 기능 확장
```

------------------------------------------------------------------------

# 23. 구현 전 Codex가 먼저 해야 할 일

이 문서를 읽은 직후 바로 파일을 대량 생성하지 않는다.

먼저 다음을 사용자에게 보고한다.

### A. 이해한 프로젝트의 핵심

3\~7개 항목 정도로 요약.

### B. MVP 구현 범위

어떤 기능부터 만들 것인지 제안.

### C. Repository Structure

현재 제안 구조를 검토하고 최소 수정이 필요한지 설명.

### D. Workflow 호출 구조

MD 파일들이 실제 기능 실행 시 어떻게 참조될지 설명.

### E. 미확정 사항

구현 전에 반드시 결정해야 하는 것과\
구현하면서 실험해도 되는 것을 분리한다.

그 후 사용자의 승인을 받고 구현을 시작한다.

------------------------------------------------------------------------

# 24. 우선 검증해야 할 질문

구현 과정에서 특히 다음을 검증한다.

### Question 1

`Knowledge → Reference → Research` 순서가 실제 카피 품질 향상에 도움이
되는가?

### Question 2

Research는 어느 시점에서 호출해야 가장 효율적인가?

### Question 3

Persona A / B / C 구분이 실제 Copy Direction 차이를 만들어내는가?

### Question 4

Formula Knowledge가 Copy 다양성과 품질을 높이는가, 아니면 오히려
정형화시키는가?

### Question 5

User Checkpoint를 어디에 두어야 작업 흐름을 방해하지 않으면서 방향
오류를 막을 수 있는가?

### Question 6

어떤 Output을 다음 Workflow의 Input으로 재사용할 것인가?

이 질문들의 답은 문서만으로 확정하지 않는다.

실제 사용 테스트를 통해 결정한다.

------------------------------------------------------------------------

# 25. 프로젝트 성공 기준

이 시스템이 성공했다는 것은 파일이 많거나 Architecture가 정교하다는 뜻이
아니다.

다음과 같은 변화가 나타나야 한다.

### Before

``` text
"이 상품 카피 써줘"
        ↓
그럴듯한 문장 여러 개
```

### After

``` text
상품 / 브랜드 이해
        ↓
실제 문제 정의
        ↓
고객별 구매 이유 구분
        ↓
설득 가능한 Benefit 발견
        ↓
필요한 현실 검증
        ↓
Persona별 Message 전략
        ↓
Copy Draft
        ↓
비교 / 검토 / 선택
        ↓
실제 광고 전략으로 연결
```

최종적으로 사용자가 느껴야 하는 차이는 이것이다.

> **"문장을 잘 만들어줬다"가 아니라\
> "왜 이 문장을 써야 하는지까지 생각해줬다."**

------------------------------------------------------------------------

# 26. Long-term Direction

장기적으로 Performance Marketer는 단순한 카피 도구가 아니라 다음 구조로
발전할 수 있다.

``` text
Understand
   ↓
Diagnose
   ↓
Segment
   ↓
Strategize
   ↓
Write
   ↓
Validate
   ↓
Execute
   ↓
Learn
```

그러나 현재는 `Learn`까지 자동화하려 하지 않는다.

우선 **좋은 판단 과정이 재현되는가**를 검증한다.

------------------------------------------------------------------------

# 27. 한 문장으로 정의

> **Performance Marketer는 카피를 대신 써주는 시스템이 아니라, 고객과
> 상품을 이해하고 현실에서 설득 가능한 이유를 찾아 그 판단을 카피와
> 마케팅 전략으로 변환하는 시스템이다.**

------------------------------------------------------------------------

# 28. Codex Start Instruction

아래 지시를 프로젝트 시작 시 사용할 수 있다.

``` text
프로젝트 루트의 PERFORMANCE_MARKETER_CODEX_HANDOFF.md를 처음부터 끝까지 읽어줘.

이 프로젝트를 단순한 AI 카피 생성기나 프롬프트 모음으로 해석하지 마.

핵심 목적은 좋은 카피를 바로 생성하는 것이 아니라,
Knowledge / Reference / Research를 구분하고,
문제 정의 → Benefit → Persona → Copy → Review로 이어지는
재사용 가능한 사고 및 작업 환경을 만드는 것이다.

아직 파일을 대량 생성하거나 구현을 시작하지 마.

먼저 다음 5가지를 보고해줘.

1. 네가 이해한 프로젝트의 핵심 기획 의도
2. MVP에서 먼저 구현해야 할 기능
3. 제안 Repository Structure에 대한 검토
4. 기능 호출 시 MD 파일들의 참조 흐름
5. 구현 전 확정해야 할 사항과 구현 중 실험해도 되는 사항

특히 과도한 Agent화, RAG, Vector DB, 자동화는 피하고,
현재 단계에서 가장 단순하게 검증 가능한 구조를 우선해줘.

내 승인 후 구현을 시작해.
```

------------------------------------------------------------------------

## Final Note

이 프로젝트의 핵심 자산은 개별 Prompt가 아니다.

``` text
Knowledge를 어떻게 사용하고,
Reference를 어떻게 해석하고,
Research로 무엇을 검증하며,
어느 시점에 사용자의 판단을 받고,
그 결과를 다음 Workflow로 어떻게 넘기는가.
```

이 **관계와 흐름 자체가 시스템의 핵심 자산**이다.

따라서 구현 과정에서 기능을 추가할 때마다 다음 질문을 먼저 한다.

> **"이 기능이 더 많은 것을 하게 만드는가?"**

보다,

> **"이 기능이 더 좋은 판단에 도달하게 만드는가?"**

를 우선한다.
