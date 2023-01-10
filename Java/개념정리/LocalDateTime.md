## LocalDateTime이 null일 경우 `-`을 표시해야 되는 경우가 생김

- 기존 응답값을 LocalDateTime으로 보낼 경우 `-`처리를 할 수 없다.
- 응답값을 String 으로 변경하고 LocalDateTime을 String으로 변환하는 함수 로직 생성한다.
    - 이 때 LocalDateTime 이 `null`값일 경우 `-`을 반환한다.

```

    @Test
    public void testTime() {
        //given
        LocalDateTime now = LocalDateTime.now();
        LocalDateTime test = null;
        System.out.println(transferTime(now));
        System.out.println(transferTime(test));
    }

    public String transferTime(LocalDateTime time) {
        if (time == null) {
            return "-";
        }
        return time.format(DateTimeFormatter.ofPattern("yyyy.MM.dd HH:mm"));
    }

```