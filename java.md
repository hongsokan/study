# 자바

## 자바 예외 처리
[참고자료](https://www.nextree.co.kr/p3239/)

### Error vs Exception
- Error : 시스템 레벨에서 비정상적인 상황이 발생하는 심각한 수준의 오류.
- Exception : 개발자가 구현한 로직에서 발생.

### Throwable
- 모든 예외클래스(Error와 Exception)는 Throwable 클래스를 상속 받고, Throwable은 Object의 자식 클래스이다.

### Checked Exception vs Unchecked Exception
- Exception은 Checked Exception과 RuntimeException(Unchecked Exception)으로 구분됨.
- Checked Exception은 컴파일 단계에서 발생하는 에러. 명확하게 Exception 체크가 가능함. 반드시 try/catch로 감싸거나 throw로 던져서 처리. 대표적으로 IOException, SQLException.
- Unchecked Exception은 Runtime 단계에서 발생하는 에러. 명시적인 예외처리를 하지 않아도 됨. 대표적으로 NullPointerException

