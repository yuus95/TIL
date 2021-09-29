# JPA 활용2


## 지연 로딩과 조회 성능 최적화 (주문 + 배송정보 _ 회원을 조회하는 API)


- V1 엔티티를 직접 노출

  - ```java

        @GetMapping("/api/v1/simple-orders")
        public List<Order> orderV1(){
            List<Order> all = orderRepository.findAllByString(new OrderSearh());

        for (Order order:all){
            order.getMember().getName(); // Lazy강제 초기화(프록시객체)

            order.getDelivery().getAddress();

            return all;

            }
         }

    ```
    
  - 엔티티를 직접 노출하는 것은 좋지 않다. 
  - jackson 라이브러리는 기본적으로 이 프록시 객체를 json으로 어떻게 생성해야 하는지 모름 -> 예외 발생 
  
  - 엔티티를 직접 노출할 때는 양방향 연관관계가 걸린 곳은 꼭 한 곳을 @JsonIgonore 처리 해야 한다.



- v2 엔티티를 DTO로 변환 
  - ```java
    
    @GetMapping("/api/v2/simple-orders)
    public List<SimpleOrderDto> ordersV2(){

        List<Order> orders = orderRepository.findAllByString(new OrderSearch());
        
        List<SimpleOrderDto> result = orders.stream()
        .map( o -> new SimpleOrderDto(o))
        .collect(Collectors.toList());
    return result;
    }

    @Data
    static class SimpleOrderDto{
        private Long orderId;
        private String name;
        private LocalDateTime orderDate;
        private OrderStatus orderStatus;
        private Address address;

        public SimpleOrderDto(Order order){
            orderId=order.getId();
            name = order.getName();
            orderDate=order.getOrderDate();
            orderStatus=order.getStatus();
        }
    }


    ```
  - 엔티티를 DTO로 변환하는 일반적인 방법
  - 쿼리가 총 1+ N+N번 실행 된다.(V1과 쿼리수 결과는 같다)



- 간단한 주문 조회 V3 엔티티를 DTO로 변환 페치 조인 최적화 (지연로딩을 즉시로딩으로 변환시켜줌)


  - ```java
    
    @GetMapping("/apli/v3/simple-orders")
    public List<simpleOrderDto>ordersV3(){
        List<Order> orders = orderRepository.findAllWithMemberDelivery();

        List<SimpleOrderDto> result = orders.stream()
        .map( o-> new SimpleOrderDto(o))
        .collect(Collectors.toList());
    return result;
    }

    // Repository 
    
    public List<Order> findAllWithMemberDelivery(){
        return em.createQuery(
            "select o from Order o " +
            " join fetch o.member m " +
            " join fetch o.delivery d",Order.class
        ).getResultList();
    }

    ```
  - 엔티티를 페치 조인을 사용해서 쿼리 1번에 조회  1,2번과 다름 


- V4 JPA에서 DTO로 바로 조회 


  - ```java
    
    @GetMapping("/api/v4/simple-orders")
    public List<OrderSimpleQueryDto>ordersV4(){
        return orderSimpleQueryRepository.findOrderDtos();

    }


    //OrderSimpleQueryRepository

    private fnal EntitiyManager em;

    public List<OrderSimpleQueryDto> findOrderDtos(){
     return   em.createQuery(
                "select new com.example.jpa_shop.repository.order.simplerquery.OrderSimpleQueryDto(o.id,m.name,o.orderDate,o.status,d.address) from Order o " +
                        " join o.member m" +
                        " join o.delivery d", OrderSimpleQueryDto.class)
                .getResultList();
    }
    ```

  - 일반적인 SQL을 사용할 떄 처럼 원하는 값을 선택해서 조회
  - new 명령어를 사용해서 JQPL의 결괄르 DTO로 즉시 전환
  - 리포지토리 재사용성 떨어짐, API 스펙에 맞춘 코드가 리포지토리에 들어가는 단점



## 쿼리 방식 선택 권장 순서

- 쿼리용 레퍼지토리 생성하기 ( 기본 레포는 온전한 상태에 대한 기능만 남겨둬야 한다)

- 우선 엔티티를 DTO로 변환하는 방법을 선택한다 (V2)

- 필요하면 페치 조인으로 성능을 최적화 한다 -> 대부분의 성능 이슈가 해결된다.

- DTO로 직접 조회하는 방법을 사용한다 (V4)


---

## 컬렉션 조회 최적화

```java

 // v2과 차이점은 레포에서 fetch join을 통해 객체그래프를 탐색할 수 있게 만든것만 차이난다.
    @GetMapping("/api/v3/orders")
    public List<OrderDto> ordersV3(){
        List<Order> orders = orderRepository.findAllWithItem();

        List<OrderDto> result = orders.stream()
                .map(o -> new OrderDto(o))
                .collect(Collectors.toList());
        return result;
    }

    //DTO를 사용하여 데이터를 보낼떄는 엔티티에 대한 의존도를 완전히 뗴어내야된다.
            @Data
            static class OrderDto{

                private Long orderId;
                private String name;
                private LocalDateTime orderDate;
                private OrderStatus orderStatus;
                private Address address;

                // DTO 안에 엔티티가 있으면 안된다. 매핑하는것도 안됨
        private List<OrderItemDto> orderItems; // DTO로 바꿔서 사용해야한다.


// OrderDto
        public OrderDto(Order o){
            orderId= o.getId();
            name = o.getMember().getName();
            orderDate = o.getOrderDate();
            orderStatus = o.getStatus();
            address=o.getDelivery().getAddress();

//            o.getOrderItems().stream().forEach(o-> o.getItem().getName());
//            orderItems=o.getOrderItems();
            orderItems = o.getOrderItems().stream()
                    .map( orderItem-> new OrderItemDto(orderItem))
                    .collect(Collectors.toList());


        }
    }

    // OrderItemDTo
    @Getter
    static class OrderItemDto{

        private String itemName; // 상품명
        private int orderPrice; // 주문가격
        private int count; // 주문 수량


        public OrderItemDto(OrderItem orderItem){
            itemName = orderItem.getItem().getName();
            orderPrice = orderItem.getItem().getPrice();
            count = orderItem.getCount();

        }
    }


// repo 


    public List<Order> findAllWithItem() {
        return em.createQuery(
                // mysql dinstinct 는 아예 모든 컬럼이 다같은값일 경우만 삭제
                // jpa - distinct는 애플리케이션 계층에 들어오는 데이터에 한해서 값은 id값을 가진 데이터들의 중복을 없애준다
                "select distinct o from Order o " +
                " join fetch o.member m " +
                " join fetch o.delivery d" +
                " join fetch o.orderItems oi" +
                " join fetch oi.item i ", Order.class)
                .getResultList();

    }
```


- 페치 조인과 distinct을 사용하여 컬렉션 조회했을 떄 단점
  - 페이징 불가능


- 컬렉션 페치 조인을 사용하면 페이징이 불가능하다. 하이버네이트는 경고 로그를 남기면서 모든 데이터를 DB에서 읽어오고, 메모리에서 페이징 해버린다.

- 컬렉션 페치 조인은 1개만 사용할 수 있다. 컬렉션 둘 이상에 페치 조인을 사용하면 안된다. 데이터가 부정합하게 조회될 수 있다.
 
 ---

## 컬렉션 페이징과 한계 돌파

- ToOne(OneToOne,ManyToOne)관계를 모두 페치조인한다. - ToOne 관계는 row수를 증가시키지 않으므로 페이징 쿼리에 영향을 주지 않는다.

- 컬렉션은 지연 로딩으로 조회한다.( 일대다)

- 지연 로딩 성능 최적화를 위해 yml 파일에 속성하나를 추가한다 (default_batch_fetch_size: 100)

```java
spring:
  jpa:
    hibernate:
      ddl-auto: create
    properties:
      hibernate:
#        show_sql: true
        format_sql: true
        default_batch_fetch_size: 100

//  개별 설정 방법
// 컬렉션은 컬랙션 필드에, 엔티티는 엔티티 클래스에 적용

```

- 장점
  - 쿼리 호출 수가 1+ N - > 1 + 1 로 최적화 된다
  - 조인보다 DB데이터 전송량이 최적화 된다.(Order와 OrderItem을 조인하면 Order가 OrderItem 만큼 중복해서 조회 된다. 이 방법은 각각 조회하므로 전송해야할 중복 데이터가 없다.) -- > DB는 일대다이면 다쪽에다 데이터를 맞춘다(뻥튀기)


  - 페치 조인 방식과 비교해서 쿼리 호출 수가 약간 증가하지만, DB 데이터 전송량이 감소한다.

  - 컬렉션 페치 조인은 페이징이 불가능 하지만 이방법은 페이징이 가능하다.

- 결론
  - ToOne관계는 페치 조인해도 페이징에 영향을 주지 않는다. 따라서 ToOne관계는 페치조인으로 쿼리 수를 줄이고 해결한 뒤, 나머지는 batch_fatch_size을 이용하자


### Spring Data Jpa Pageable
- PageRequest 객체를 이용하여 paging 및 sort기능을 사용할 수 있다.

```java
//BookRepository

public interface BookRepository extends JpaRepository<BookEntity,id>{
  Page<BookEntity> findAllByBookType(String bookType, Pageable pageable)
}

//BookService
@Service
@Requiredargsconstructor
public BookService{
  private BookRepository bookRepository;
  public List<bookDto> getBookByType(String type){
    PageRequest pageRequest = PageRequest.of(0,5,Sort.by("createAt").Descending;
    List<bookListResponse> bookList = bookRepository.findByBookType(type,           pageRequest)
            .stream()
            .map(o-> bookListResponse.from(o))
            .collect(Collectors.toList));

  }
}


```