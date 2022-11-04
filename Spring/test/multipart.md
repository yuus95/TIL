

### multipart테스트 코

- 의도 : 엑셀 파일을 읽기 위한 테스트 코드를 작성하기전에 multipart 관련 테스트 코드를 작성했다. 

```

    @Test
    public void readMultipart() throws IOException {
        String fileName = "voter_sample";
        String contentType = "xls";
        String filePath = "./src/main/resources/excel/voter_sample.xls";
        MockMultipartFile mockMultipartFile = getMockMultipartFile(fileName, contentType, filePath);
        String getFileName = mockMultipartFile.getOriginalFilename().toLowerCase();
        assertThat(getFileName, is(fileName.toLowerCase() + "." + contentType));
    }
    private MockMultipartFile getMockMultipartFile(String fileName, String contentType, String path) throws IOException, FileNotFoundException {
        FileInputStream fileInputStream = new FileInputStream(path);
        return new MockMultipartFile(fileName, fileName + "." + contentType, contentType, fileInputStream);
    }
```