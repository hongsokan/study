# 자바

## 자바 예외 처리
[참고자료](https://www.nextree.co.kr/p3239/)

### Error vs Exception
- Error : 시스템 레벨에서 비정상적인 상황이 발생하는 심각한 수준의 오류.
- Exception : 개발자가 구현한 로직에서 발생.

### Throwable
- 모든 예외클래스는 Throwable 클래스를 상속 받고, Throwable은 Object의 자식 클래스이다.
- Throwable을 상속받는 클래스는 Error와 Exception이 있다. 일반적으로 Error는 시스템을 종료하거나 변화를 주어 처리해야 하고, Exception은 개발자가 로직을 추가하여 처리.

### RuntimeException
- Exception은 Checked Exception과 RuntimeException(Unchecked Exception)으로 구분됨.
- Checked Exception은 컴파일 단계에서 명확하게 Exception 체크가 가능함. 반드시 try/catch로 감싸거나 throw로 던져서 처리.
- Unchecked Exception은 Runtime 중 특정 논리에 의해 발견됨. 명시적인 예외처리를 하지 않아도 됨.

### Checked Exception vs Unchecked Exception
- Checked는 컴파일 단계에서 발생하는 에러이므로 반드시 잡아야 되는 에러. 대표적으로 IOException, SQLException.
- Unchecked는 런타임 단계(RuntimeException)에서 발생하는 에러. 대표적으로 NullPointerException.

### Rollback 여부
- Checked Exception은 예외 발생 시 트랜잭션을 rollback하지 않고 예외 처리
- Unchecked Exception은 트랜잭션을 rollback 함.