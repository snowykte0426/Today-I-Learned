# **Build Tool**,**Directory Structure**
이 문서(TIL)에서는 **빌드 도구**, **Spring 디렉터리 구조**을 알아본다.
## 빌드 도구(Build Tool)
+ 소스 코드에서 애플리케이션 생성을 **자동화** 하는 도구(프로그램)을 **빌드 도구**라 칭한다.
+ 기존에 프로그래머가 직접 하던 빌드 작업을 프로그램이 대신 자동화 해준다고 생각하면 된다.
+ Java 뿐 아니라 C,C# 등 타 언어에도 빌드 도구가 존재한다.
+ **Make**,**CMake**,**Ninja**,**MSBuild**,**Ant**,**Maven**,**Gradle** 등 수많은 빌드 도구가 사용되고 개발 중이다.
    ### **Ant**

    <img src="https://upload.wikimedia.org/wikipedia/commons/2/2f/Apache-Ant-logo.svg" width=300/>

    + make와 비슷하나 Java로 만들어져 **Java 실행환경**이 필요하며 빌드를 위한 환경구성을 **XML**을 사용한다.
    + 이클립스(eclipse) IDE에 탑재되던 빌드 도구로 현재는 많이 사용되지 않는다.
    ### **Maven**

    <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/5/52/Apache_Maven_logo.svg/180px-Apache_Maven_logo.svg.png" width=250/>

    + Java 프로젝트들을 위한 **빌드 자동화 도구**로 Java 뿐 아니라 **C#**,**Ruby**,**Scala** 등의 언어로 개발된 프로젝트 역시 빌드하고 관리할 수 있다.
    + 프로젝트들은 **오브젝트 모델(Project Object Model; POM)**을 사용하여 구성되며 **pom.xml** 파일에 저장된다.
    + **생명주기(Life Cycle)** 개념이 도입되어 빌드 순서등을 정의할 수 있다.
    ### **Gradle**

    <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/6/6b/Gradle_logo.svg/180px-Gradle_logo.svg.png" width=240/>

    + **Java**,**Kotlin**,**Groovy**로 개발된 빌드 도구로 **안드로이드 스튜디오(Android Studio)**의 공식 빌드 시스템이자 **Java**,**C/C++**,**Python**등 다양한 언어를 지원한다.
    + **Build.gradle**에 스크립트를 작성하며 프로젝트의 규모가 커질수록 복잡해지는 XML 기반 스크립트에 비해 **간편하다는 장점**이 있다.

    ### **Maven vs Gradle**
    | **항목**                  | **Gradle**                                                                 | **Maven**                                                         |
    |---------------------------|-----------------------------------------------------------------------------|-------------------------------------------------------------------|
    | **모델**                  | 작업 의존성 그래프 기반                                                     | 고정적이고 선형적인 단계 기반                                     |
    | **다중 모듈 빌드**        | 병렬 실행 가능, 태스크 업데이트 여부를 체크하여 증분 빌드 허용             | 병렬 실행 가능, 고정적인 빌드 단계                               |
    | **설정 공유**             | 설정 주입 방식 제공                                                        | 상속을 통해 설정 공유                                            |
    | **캐시**                  | Concurrent에 안전한 캐시, checksum 기반의 캐시 사용, repository와 동기화 가능 | 기본적인 캐시 지원                                               |
    | **커스터마이징**          | 간편한 커스터마이징                                                         | 제한된 커스터마이징                                               |
    | **증분 빌드**             | 이미 업데이트된 태스크는 실행하지 않으므로 빌드 시간 단축                   | 증분 빌드 미지원                                                 |
    | **유연성**                | 매우 유연하며 다양한 플러그인 및 확장 가능                                   | 상대적으로 덜 유연하고 제한적인 플러그인 지원                    |
    | **성능**                  | 빠른 빌드 시간, 특히 큰 프로젝트에서 유리                                    | 상대적으로 느림                                                  |

## 디렉터리 구조(Directory Structure)
+ 파일 시스템(File System)에서 **파일**을 조직하고 **관리**하는 방법을 설명하는 개념이다.
+ 디렉터리 구조는 종류에 따라 다를 수 있지만 파일 시스템에서 다양한 기능을 제공한다.
    + **파일 및 디렉터리 관리**: 파일,디렉터리를 **생성**하거나 **삭제**하고 **이름을 변경**하여 식별과 관리를 개선하는 것
    + **파일 및 디렉터리 조직화**: **계층적 구조**를 만들거나 관련된 파일을 **그룹화**하여 관리할 수 있는 것
    + **파일 접근 및 탐색**: 파일과 디렉터리의 위치를 **경로(path)** 로 지정하여 접근하고 CLI,GUI 등을 이용하여 원하는 파일,디렉터리를 **탐색**할 수 있는 것
    + **파일 시스템 유지보수**: **백업 및 복원** 기능을 갖추거나 **주기억장치(RAM),보조기억장치(ROM)의 공간 관리**등을 수행할 수 있는 것
+ 디렉터리 구조에는 여러가지가 있는데 각 디렉터리 구조의 일장일단이 있고 어떤 파일 시스템에 적용되나에 따라 기능이 추가되거나 없을 수도 있다.
### **1단계 디렉터리(Single-Level Directory)**

<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FdHDJyX%2FbtqCl9nL1XD%2FLpku6PDmU7HCX2iwaMRPYK%2Fimg.png" width=500>

+ **가장 단순한 구조**로 모든 파일이 **하나의 디렉터리**에 존재하게 되는 구조이다.
+ 모든 파일이 하나의 디렉터리에 존재하기 때문에 각각 파일의 이름이 모두 **달라야 하고**,파일이나 사용자의 수가 증가하면 **관리에 어려움을 겪게 된다**.
+ 디렉터리를 통한 그룹화가 **불가**하며 파일이 이름 길이가 시스템에 따라 제한된다.
### **2단계 디렉터리(Two-Level Directory)**

<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Folq8n%2FbtqCsGxtse2%2FVQ8QCONnlFe8vwSy4Hr451%2Fimg.png" width=500/>

+ 사용자 별로 **별도의 디렉터리를 제공**하여 파일의 관리를 손쉽게 하고 각 파일과 디렉터리는 루트 디렉터리(Root Directory)로 부터 자신의 위치까지의 **경로명**를 가진다.
+ 루트(Root; 최상위 디렉터리)는 **마스터 파일 디렉터리(MFD)** 이고 그 아래로 **사용자 파일 디렉터리(UFD)**, 그 아래로는 **파일**이 위치하게 되는 구조다.
+ MFD는 각 **사용자명**,**계정 번호**,**UFD를 가리키는 포인터** 등을 가지고 있으며 UFD는 각 **파일들에 대한 정보**를 가지고 해당 파일들을 관리한다.
+ 파일의 이름이 같더라도 서로 다른 사용자 디렉터리에 속한다면 **동시에 존재할 수 있다.**
+ 사용자가 수가 늘어날수록 관리가 어려워진다.
### **트리 구조 디렉터리(Tree Directory)**

<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fea0rY3%2FbtqCsGK0lGB%2FU0NWirm8NbBfx218VjzAfK%2Fimg.png" width=500>

+ 이름 그대로 **트리의 형태**를 띄는 디렉터리 구조이다.
+ 하나의 **루트 디렉터리**와 여러 개의 **서브 디렉터리**로 구성되어 있다.
+ 각 디렉터리에는 **파일**과 **서브 디렉터리**가 존재한다.
+ 디렉터리 탐색에는 **포인터**를 사용하고 경로명에는 **절대 경로**와 **상대 경로**가 있다.
+ 절대 경로는 **루트에서 부터 해당 파일까지의 모든 경로를 지정**하는 것이고, 상대 경로는 **현재 디렉터리 위치를 기준으로 목적파일까지의 경로를 지정하는 것**이다.
+ 디렉터리의 **생성**과 **제거**가 비교적 용이한 편이다.
### **비순환 그래프 디렉터리(Acyclic Graph Directory)**

<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FcKO3S4%2FbtqCqOpvDaw%2Fe81xagdc76Cvol0st9LKKk%2Fimg.png" width=500>

+ 트리 구조 디렉터리를 **확장**한 것으로, 디렉터리 간의 파일이나 서브 디렉터리의 **공유**가 허용된다.
+ 파일이나 디렉터리가 여러 디렉터리에 속할 수 있도록 하여 한 디렉터리에서 공유한 파일에 수정이 발생되면 다른 디렉터리에서도 해당 파일의 변화를 **확인**할 수 있다.
+ 디렉터리 구조가 **복잡**하고 하나의 파일에 **여러 번 탐색이 발생**할 수 있기 때문에 시스템 성능이 **저하**될 수도 있다.
+ 공유된 파일이 삭제될 경우 **고아 포인터**가 발생하는데 이를 해결할 방안으로 **참조 카운트(Reference Counting; 참조 횟수를 기록하는 기법)**,**가비지 컬렉션(Garbage Collection; 주기적으로 고아 포인터 등 필요 없게 된 영역을 할당해제하는 기법)** 등의 시스템 적인 해결책이 있다.
### **일반 그래프 디렉터리(General Graph Directory)**

<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbNMwwF%2FbtqCtXMsVyq%2FDrtzefNKKCKIdYWRQR0ol1%2Fimg.png" width=500>

+ 트리 구조 디렉터리 방식에서 **순환 참조**를 허용하는 방식이다.
+ 디렉터리와 파일 공유에 대해 융통성이 있고 **탐색 알고리즘**이 **간단**하다.
+ 마지막 참조가 끝났을 때 메모리를 재할당 할지 결정하는 **가비지 컬렉션** 필요하다.