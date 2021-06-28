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

-필요하면 페치 조인으로 성능을 최적화 한다 -> 대부분의 성능 이슈가 해결된다.

- DTO로 직접 조회하는 방법을 사용한다 (V4)

