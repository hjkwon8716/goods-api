# goods-api
 study goods-api

## project structure
(core, client_rest, message_kafka, service_spring, store_jpa는 각각의 모듈형태로 구성하여 추후에는 구성할 수 있다.)

    core : pojo 형태의 java 파일만 존재 각 레이아웃 interface 및 service 구현체만 존재.
        client : 외부 api 접근 시 필요한 인터페이스 집합.
        domain : 서비스 domain 객체 집합.
        message : message 시 필요한 인터페이스 집합.
        service : 서비스 비즈니스 인터페이스 집합.
            impl : 서비스 비즈니스 구현체.
        store : DB 접근 시 필요한 인터페이스 집합.

    client_rest : restTemplate 기반의 외부 api 접근 구현체.
    
    message_kafka : kafka 기반의 message 송/수신 구현체.

    service_spring : spring 프레임워크 적용.

    store_jpa : JPA 기반의 DB 접근 구현체.

## 위 project structure에서 설계 관점

#### 첫번째 service 계층에서 특정 기술 메소드를 직접 호출 하지 않는다.
    외부 api 접근 시 RestTemplate, message 송/수신 시 kafka, DB 접근 시 jpa repository를 service 단에서 직접적으로 호출 하지 않는다.
    service 계층에서 외부 기술을 직접 호출 한다면 그 기술의 변경 및 다른 기술로 대체 시 service 계층에서 변경 부분이 발생합니다.
    그러하기에 중간에 interface를 통해 한번 감싼 다음 service 계층에서는 한번 감싼 interface를 호출 하도록 하였습니다.
    그렇게 되면 특정 기술에 대한 의존성이 강한 의존에서 약한 의존이 되고, 기술의 변경 및 대체 시 중간에 감싼 interface를 잘 설계 하였다면 service 계층에서는 변경 부분 없이 수정 및 확장가능한 구조를 가질 수 있습니다.
 
#### 두번째 멀티 모듈 형태가 가능한 구조
    core 모듈은 java의 pojo 형태로 구성 할 수 있고, lombok, 단위테스트에 필요한 기술정도만 의존을 가지는 형태로 구성할 수 있습니다.
    client_rest는 외부 api 호출에 필요한 기술만 의존성을 가지는 형태,
    message_kafka 모듈은 kafka에 대한 의존성만을 가지는 형태,
    store_jpa 모듈은 jpa대한 의존성만을 가지는 형태,
    service_spring 모듈은 spring에 대한 의존성을 가지는 형태
    이렇게 각 모듈에서는 필요한 기술들만 의존성을 가지게 되어 모듈형태가 간단하면서 복잡하지 않은 설계가 가능합니다.

#### 세번째 익숙한 명칭으로 package/모듈 이름 구성
    많은 사람들이 사용한 명칭으로 package명을 구성하였고, 그로 인해 처음 접하는 사람들도 package 명칭 또는 모듈을 보고도 어떤 일을 하는지 알 수 있게 하였습니다.
 
#### 하지만 가장 중요하게 생각한 부분은 어떤 기술에 대해서 직접 접근이 아닌 interface 기반의 약한 의존이 되는 것의 목표로 project structure를 설계 하였습니다.


 