# 데이터 모델링의 이해
## (1) 모델링이란?
데이터베이스의 모델링은 **'현실 세계를 단순화하여 표현하는 기법**으로 현실 세계에서 필요한 데이터를 저장하는 데이터베이스를 구축하기 위한 **분석/설계의 과정**이라고 할 수 있다.
## (2) 모델링의 특징
1. **추상화(Abstraction)** - 아이디어나 개념을 간략하게 표현하는 과정
2. **단순화(Simplification)** - 복잡한 현실 세계를 정해진 표기법으로 단순하고 쉽게 표현하는 것
3. **명확화(Clarity)** - 불분명함을 제거하고 명확하게 해석할 수 있도록 기술하는 것
> 데이터베이스의 모델링은 현실 세계를 **추상화**,**단순화**,**명확화**하기 위해 일정한 표기법에 의해 표현하는 기법이다.
## (3) 모델링의 3가지 관점
1. **데이터 관점** - 데이터 위주의 모델링.어떤 데이터들이 얽혀있는지,그리고 그 **데이터 간에는 어떤 관계가 있는지**에 대해서 모델링하는 방법
2. **프로세스 관점** - 프로세스 위주의 모델링.이 업무가 **실제로 처리하고 있는 일은 무엇**인지 또는 앞으로 **처리하여야 하는 일은 무엇**인지 모델링하는 방법
3. **데이터와 프로세스의 상관 관점** - 데이터와 프로세스의 관계를 위주로 한 모델링.**프로세스의 흐름에 따라 데이터가 어떤 영향을 받는지**를 모델링하는 방법
## (4) 데이터의 품질 보장을 위한 유의점
1. **중복(Duplication)** - 같은 데이터가 여러 엔티티에 중복으로 저장되는 것을 피해야 함
2. **비유연성(Inflexibility)** - 데이터 모델의 설계에 따라 애플리케이션의 사소한 변경점에도 데이터 모델이 수시로 변경되는 상황을 막기 위하여 데이터 모델과 프로세스를 분리해 유연성을 높여야 함
3. **비일관성(Inconsistency)** - 데이터 모델링을 할 때 데이터 간의 연관 관계에 대해 명확하게 정의하여야 함
## (5) 모델링의 단계
1. **개념적 데이터 모델링(Conceptual Data Modeling)** - **회사 전체 차원의 데이터 모델링 수행 시** 행해지며 **추상화 레벨이 가장 높은** 모델링임.**업무 중심적**이고 **포괄적인** 수준의 모델링이 진행
2. **논리적 데이터 모델링(Logical Data Modeling)** - **재사용성이 가장 높은** 모델링으로 데이터베이스 모델에 대한 **Key**,**속성**,**관계** 등을 모두 표현하는 단계
3. **물리적 데이터 모델링(Physical Data Modeling)** - 실제 데이터베이스로 구현할 수 있도록 **성능**이나 **가용성** 등의 물리적 성격을 고려하여 모델을 **표현**하는 단계
## (6) 3단계 스키마 구조
1. **외부 스키마(External Schema)** - 각 사용자가 보는 데이터베이스의 스키마를 정의
2. **개념 스키마(Conceptual Schema)** - 사용자가 보는 데이터베이스의 스키마를 통합하여 전체 데이터베이스를 나타내는 것으로 데이터베이스에 저장되는 데이터들을 표현하고 데이터들 간의 관계는 나타냄
3. **내부 스키마(Internal Schema)** - 물리적인 저장 구조를 나타내며 실질적인 데이터의 저장 구조나 컬럼 정의,인덱스 등이 포함
> **ANSI-SPARC 아키택쳐**에서 3단계 스키마 구조를 보장하는 이유는 다음과 같다
> - 논리적 독립성: 개념 스키마가 변경되어도 외부 스키마는 영향받지 않음
> - 물리적 독립성: 내부 스키마가 변경되어도 외부/개념 스키마는 영향받지 않음
## (7) ERD
시스템에 어떤 엔티티들이 존재하며 그들 간에 어떤 관계가 있는지를 나타내는 다이어그램.ERD의 작성 순서는 다음과 같다.
1. 엔티티를 도출하고 그린다
2. 엔티티를 적절하게 배치한다
3. 엔티티 간의 관계를 설정한다
4. 관계명을 기입한다
5. 관계의 참여도를 기입한다
6. 관계의 필수/선택 여부를 기입한다
## (8) 엔티티
엔티티의 사전적인 의미는 <b>'독립체'</b>로 데이터베이스에서는 **식별이 가능한 객체**를 의미,업무에 쓰이는 데이터를 용도별로 분류한 그룹.
엔티티는 반드시 <b>속성(Attribute)</b>를 갖게 되며 속성의 개수는 엔티티마다 상이함.엔티티는 다음과 같은 특징을 가지고 있음.
1. **업무에서 쓰이는 정보여야 함** - 업무에서 쓰이지 않는 불필요한 엔티티라면 삭제하는게 바람직함
2. **유일함을 보장할 수 있는 식별자가 있어야 함** - 엔티티에 속한 각각의 인스턴스가 중복되거나 식별이 모호하면 이 엔티티는 설계가 잘못된 것
3. **2개 이상의 인스턴스를 가지고 있어야 함** - 현재 인스턴스가 1개만 존재하고 앞으로도 1개로 존재할 예정이라면 이것은 엔티티라 볼 수 없음
4. **반드시 속성을 가지고 있어야 함** - 속성이 없는 엔티티는 존재할 수 없으며 자신을 상세히 나타낼 수 있는 속성을 가지고 있어야 함
5. **다른 엔티티와 1개 이상의 관계를 가지고 있어야 함** - 각각의 엔티티는 다른 엔티티와의 연관성을 가지고 있어야 함
## (9) 엔티티의 분류
1. **유형 엔티티** - 물리적인 형태 존재, 안정적, 지속적
2. **개념 엔티티** - 물리적인 형태 없음, 개념적
3. **사건 엔티티** - 행위를 함으로써 발생, 빈번하고 통계 자료로 이용 가능
4. **기본 엔티티** - 업무에 본래 존재하는 정보를 의미.독립적으로 생성되며 자식 엔티티를 가질 수 있음(ex: 상품,회원,사원,부서 등)
5. **중심 엔티티** - 기본 엔티티로부터 파생되며 행위 엔티티 생성,중심적인 역할을 하며 데이터의 양이 많이 발생(ex: 주문,매출,계약 등)
6. **행위 엔티티** - 2개 이상의 엔티티로부터 파생되고 데이터가 자주 변경되거나 증가할 수 있음(ex: 주문 내용,이벤트 응모 이력 등)
> 엔티티의 이름을 정할 때 유의점은 다음과 같다
> - 업무에서 실제로 쓰이는 용어 사용
> - 한글은 약어를 사용하지 않고 영문은 대문자로 표기
> - 단수 명사로 표기하며 띄어쓰기는 금지
> - 다른 엔티티와 의미상 중복 불가능
> - 해당 엔티티가 가진 데이터가 무엇인지 명확히 표현
## (10) 속성
사물이나 개념의 **특징을 설명해줄 수 있는 항목들**을 속성이라 부르며 의미상 더 쪼개지지 않아야 하고 프로세스에 필요한 항목이여야 함.**각각의 속성은 속성값**을 가지며 속성값은 하나의 인스턴스를 구체적으로 나타내주는 데이터라고 볼 수 있음.
## (11) 분류
1. **기본속성(Basic Attribute)** - 업무 프로세스 분석를 통해 바로 정의가 가능한 속성
2. **설계속성(Designed Attribute)** - 업무에 존재하지는 않지만 설계 과정에서 필요하다고 판단된 속성
3. **파생속성(Derived Attribute)** - 다른 속성의 속성값을 계싼하거나 특정한 규칙으로 변형하여 생성한 속성
4. **PK속성** - 엔티티의 인스턴스들을 식별할 수 있는 속성
5. **FK속성** - 다른 엔티티의 속성에서 가져온 속성
6. **일반속성** - PK,FK를 제외한 나머지 속성
> 속성이 가질 수 있는 **속성값의 범위**를 도메인이라고 하며 우편번호는 5자리 숫자의 범위를 가지고 있고 이것은 엔티티르 정의할 때 데이터 타입과 크기로 나타냄
## (12) 관계
엔티티와 엔티티의 관계를 의미하며 어떠한 연관성이 있는지 타입을 분류하여 2가지로 분류 가능
1. **존재 관계** - 어머니와 아기처럼 **존재 자체로 연관성이 있는 관계**를 의미
2. **행위 관계** - **특정한 행위를 함으로써 연관성**이 생기는 관계를 의미(ex: 회원과 주문)
> 관계의 표기법은 정해진 규칙이 있는데 아래 3가지가 중심 규칙임
> - 관계명: 관계의 이름
> - 관계차수: 관계에 참여하는 수
> - 관계선택사양: 필수인지 선택인지의 여부
## (13) 식별자
속성 중에 **각각의 인스턴스를 구분 가능하게 만들어주는 대표격인 속성**을 의미하며 다양한 기준에 따라 분류 가능함.
- ### 대표성 여부
    - 주식별자: 유일성,최소성,불변성,존재성을 가진 대표 식별자
    - 보조식별자: 인스턴스를 식별 가능하지만 대표 식별자는 아님
- ### 스스로 생성되었는지 여부
    - 내부식별자: 엔티티 내부에서 스스로 생성된 식별자
    - 외부식별자: 다른 엔티티에서 온 식별자로 연결고리 역할
- ### 단일 속성의 여부
    - 단일식별자: 하나의 속성으로 구성된 식별자
    - 복합식별자: 두 개 이상의 속성으로 구성된 식별자
- ### 대체 여부
    - 본질식별자: 업무 프로세스에 존재하는 식별자
    - 인조식별자: 주식별자의 속성이 2개 이상인 경우 해당 속성들을 하나로 묶어서 사용하는 식별자로 '대리식별자'라 지칭하기도함
> 식별자 관계와 비식별자 관계 2가지로 분류 가능함.
> - **식별자 관계(Identification Relationship)**: 부모 엔티티의 식별자가 자식 엔티티의 주식별자가 되는 관계로 부모 엔티티가 있어야 생성 가능하며 단일식별자/복합식별자 여부에 따라 **1:1인지 1:M**인지 결정됨
> - **비식별자 관계(Non-Identification Relationship)**: 부모 엔티티의 식별자가 자식 엔티티의 일반 속성이 되는 관계로 부모 엔티티 없는 자식 엔티티 생성이 가능하고 반대로 자식 엔티티가 존재하는 상황에서 부모 엔티티의 삭제가 가능
<br>

> 식별자를 분류하기 위한 특징 4가지가 있음
> - **유일성**: 각 인스턴스에 유일함을 부여하여 식별이 가능케 함
> - **최소성**: 유일성을 보장하는 최소 개수의 속성이여야 함
> - **불변성**: 속성값이 되도록 변하지 않아야 함
> - **존재성**: 속성값이 NULL일 수 없음