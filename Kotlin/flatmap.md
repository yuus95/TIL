## flatmap

리스트내부에 리스트가 있을 경우 flatmap조건을 사용하면 반환값은 하나의 리스트를 반환한다.


```
flatMap Test

@Test
    fun flatMap() {
        //given
        val flatMap = listInList.flatMap { it }
        val flatMapFilter = listInList.flatMap {
            it.filter { it % 2 == 0 }
        }

        println(flatMap) //[1,2,3,1,5,6,1,5,7] 내부로 꺼내준다.
        println(flatMapFilter) // [2]
        //when

        //then

    }
```