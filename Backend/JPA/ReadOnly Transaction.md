# Read Only Transaction
```java
@Transactional(readOnly = true)
public Member getMember(Long memberId) {
    Optional<Member> member = memberRepository.findById(memberId);
    ...
    ..
    ..
    return member;
}
```
Spring에서 트랜젝션을 사용할 때 조회 관련 요청일 경우 트랜젝션을 적용하되 ``readOnly`` 파라미터를 넘기는 경우가 있다.어떤 효과가 있길래 이러한 것을 사용하는 것일까?
## 1. 문서화의 기능
사실 클래스명,메서드명 등에 ``Get``,``Find``,``Query`` 등의 명칭을 붙여 조회 기능을 수행하고 있음을 바로바로 알릴수도 있지만 트랜젝션 어노테이션의 인자값으로 ``readOnly`` 값을 ``true``로 넘긴다면 곧바로 해당 클래스/메서드가 조회 기능을 수행함을 알 수 있다.즉, **해당 인자를 사용하는 것**만으로도 **강력한 문서화**의 기능을 할 수 있는 것이다.
## 2. Replication 부하 분산
DB의 장애를 빠르게 복구하고 트래픽을 분산시키기 위하여 복제본 DB를 두는 **레플리케이션(Replication) 방식**을 사용할 수 있다.레플리케이션은 Master-Slave 구조로 복제본 DB를 함께 운용함으로써, Master DB의 장애 발생 시 Slave DB를 Master DB로 승격시켜 장애를 빠르게 복구할 수 있으며, 조회 작업은 Slave DB에서 수행하고 수정 작업은 Master DB에서 수행함으로써 트래픽을 분산할 수 있다는 장점이 있다.

![참고 이미지](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fbx66cl%2FbtsdZz2V4dX%2Fi9VUWmK5Hss2qUwI2vTd70%2Fimg.png)
 

이러한 데이터베이스 구조를 가져갈 때, ``readOnly = true``가 설정되어있는 메서드의 경우 Slave DB에서 데이터를 가져오도록 동작한다. 이를 통해 레플리케이션의 목적에 맞게 **트래픽 분산을 온전하게 적용**할 수 있다는 추가적인 이점이 존재한다.
## 3. JPA의 Dirty Checking
<b>영속성 컨텍스트(Persistence Context)</b>는 엔티티(Entity) 조회 시 초기 상태에 대한 스냅샷(Snapshot)을 저장한다.트랜잭션이 커밋(commot) 될 때, 초기 상태의 정보를 가지는 스냅샷과 엔티티의 상태를 비교하여 변경된 내용에 대해 ``UPDATE`` 쿼리를 생성해 쓰기 지연 저장소에 저장한다.

그 후, 일괄적으로 쓰기 지연 저장소에 저장되어 있는 SQL 쿼리를 플래쉬(flush) 하고 데이터베이스의 트랜잭션을 커밋 함으로써 우리가 ``update``와 같은 메서드를 사용하지 않고도 엔티티의 수정이 이루어진다. 이를 **변경 감지(Dirty Checking)** 라고 한다.

 

이 때, ``readOnly = true``를 설정하게 되면 Spring 프레임워크는 JPA의 세션 플래시 모드를 ``MANUAL``로 설정한다.

* ``MANUAL`` 모드는 트랜잭션 내에서 사용자가 수동으로 플래쉬를 호출하지 않으면 플래쉬가 자동으로 수행되지 않는 모드이다.

즉, 트랜잭션 내에서 강제로 ``flush()``를 호출하지 않는 한, 수정 내역에 대해 DB에 적용되지 않는다.

이로 인해 트랜잭션 커밋 시 영속성 컨텍스트가 자동으로 플래쉬 되지 않으므로 조회용으로 가져온 엔티티의 예상치 못한 수정을 방지할 수 있다.

또한, ``readOnly = true``를 설정하게 되면 JPA는 해당 트랜잭션 내에서 조회하는 엔티티는 조회용임을 인식하고 변경 감지를 위한 스냅샷을 따로 보관하지 않으므로 메모리가 절약되는 성능상 이점 역시 존재한다.
