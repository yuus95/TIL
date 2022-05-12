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