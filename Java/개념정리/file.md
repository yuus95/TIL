## file 

- File exits함수를 사용할 경우 디렉토리일 때도 참값이 나오기 때문에 isDirectory값을 확인하여 사용하도록 해야한다.


```java
File.exists() // 파일또는 디렉토리가 존재하는지 확인할 수 있다.
File file1 = new File("디렉토리명 또는 파일명")
if(file1.exits()){
	if(file1.isDirectory()){
		// 디렉토리일 경우 트루.	
	)
    else{
        // 파일일 경우 
    }
}

```

### Paths
```
This class consists exclusively of static methods that return a Path by converting a path string or URI.

path String 또는 URI을 이용해 Path를 반환한다. 
이 Path는 File 클래스와 상호작용할 수 있다.
```

- **readAllLines** vs **lines**

- `readAllLines` 는 모든라인을 읽고 다음 함수를 처리해야 해서 대용량 데이터를 처리할 때 불리하다.
- `lines` 스트림을 제공하여 최소한의 데이터 파이프링을 이용해 데이터를 처리해 대용량 데이터를 처리할 때 유리하다.

```java

    // 함수형 프로그래밍 활용
    // 스트림을 이용하여 내부반복을 이용해 데이터를 처리한다. 
        List<String> dataTxtResult 
        = Files.lines(currentPath).collect(Collectors.toList());

```
