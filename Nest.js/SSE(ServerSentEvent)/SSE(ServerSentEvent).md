## WebSocket과 SSE의 차이점

### WebSocket
- 양방향 통신
- 낮은 지연 시간 (연결 후 추가적인 헤더 교환이 없으므로 효율적)
- 데이터를 자주 주고 받는 상황이 아닌 경우, 오히려 리소스 소모가 큼.
- 에러 핸들링을 위한 WebSocket 프로토콜 에러를 따로 만들어 주어야한다.

### SSE
- 단방향 통신
- 간단한 구현 
- 데이터 전송 제한 (텍스트 데이터만을 전송할 수 있음)
- Http 통신을 사용하여 기존에 사용하던 에러처리 방식을 그대로 사용 할 수 있다.