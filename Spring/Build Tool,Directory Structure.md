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
