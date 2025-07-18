# 전송계층 
## TCP의 오류제어
### TCP의 오류제어
- 신뢰성을 보장하기 위해서 오류를 제어할 수 있어야 한다. 이를 위해서 TCP는 잘못된 세그먼트를 재전송하는 방법을 사용한다. 


### 오류 검출과 재전송 
- TCP 세그먼트에 오류 검출을 위한 체크섬 필드가 있다고 하지만 이것만으로 신뢰성을 보장하기 어렵다. 
- 체크섬은 단순히 세그먼트의 훼손여부만 나타내고 체크섬이 잘못되었다면 호스트는 패킷을 읽지 않고 폐기하기 때문에 송신 호스트는 세그먼트 전송 과정에서 문제가 있다는 것을 인식 불가능하다. 
- TCP가 신뢰성을 보장하려면 송신 호스트가 송신한 세그먼트에 문제가 발생했음을 인지가능 해야하고, 오류를 감지하면 세그먼트를 재전송 가능해야 한다. 
  
### 중복된 ACK 세그먼트를 수신했을 때 
- TCP는 중복된 ACK 세그먼트를 수신했을 때 오류가 생겼음을 감지한다. 
- 송수신이 올바르게 이루어진 경우 아래와 같다.  
- A-> B 첫번째 순서 번호 세그먼트를 전송한다.  
- B -> A 그에 대한 ACK 세그먼트를 받은 후 다음 순서 번호를 담은 세그먼트를 전송한다. 
- 수신 호스트 측이 받은 세그먼트의 순서 번호 중에서 일부가 누락되면 중복된 ACK 세그먼트를 전송하게 된다. 

### RTT(Round Trip Time) 
- 메시지를 전송한 뒤에 그에 대한 답변을 받는 데 까지 걸리는 시간을 RTT라고 한다. 

### 타임아웃이 발생 했을 때 
- TCP는 타임아웃이 발생하면 문제가 생겼음을 인지한다. 
- TCP 세그먼트는 송신하는 모든 호스트는 재전송 타이머(Retransmission Timer)라는 값을 유지한다. 
- 호스트는 세그먼트가 전송할 때마다 재전송 타이머를 시작하게 되는데 이 타이머의  카운트 다운이 끝난 상황을 타임아웃이라고 한다. 
- 타임아웃이 발생할 때 까지 ACK 세그먼트를 받지 못하면 세그먼트가 상대 호스트에게 정상적으로 도착하지 않았다고 간주하여 세그먼트를 재전송한다. 
  
### ARQ(Automic Repeat Request) 기법 
- 수신 호스트의 답변과 타임아웃 발생을 토대로 문제를 진단하고 문제가 생긴 메시지를 재전송함으로써 신뢰성을 확보하는 방식을 ARQ 자동 재전송 요구 라고 한다. 
- ARQ 의 종류는 다양한데 가장 대표적인 세가지 방식인 Stop And Wait ARQ 방식과 Go-Back-N ARQ, Selective Repeat ARQ 이 있다. 

### Stop-And-Wait ARQ 
- Stop And Wait ARQ는 제대로 전달했음을 확인하기 전까지는 새로운 메시지를 보내지 않는 방식이다. 
- 메시지를 송신하고 이에 대한 응답을 받고 다시 메시지를 송신하고 응답을 받는 과정을 반복한다. 
- 단순하지만 높은 신뢰성을 보장하는 방식이다. 
- 전송되었음을 확인해야만 다음 전송을 시작하는 Stop-And-Wait 방식은 송신 호스트 입장에서는 확인 응답을 받기 전까지 다음 전송을 할 수 있어도 하지 못 하기 때문에 네트워크 이용 효율이 낮고 성능 저하로 이어질 수 있다. 

### Go Back N ARQ 
- Stop-And-Wait ARQ를 해결할려면 각 세그먼트에 대한 ACK 세그먼트가 도착하기 전이라도 여러 세그먼트를 보낼 수 있어야 한다. 
- Go-Back-N, Selective Repeat는 이러한 방식으로 동작한다. 
- Go-Back-N ARQ는 파이프라이닝 방식을 통해 여러 세그먼트를 전송하고 도중에 잘못 전송된 세그먼트가 발생할 경우 해당 세그먼트 부터 다시 전송하는 방식이다. 
- Go-Back-N ARQ에서 순서번호 n 에 대한 ACK는 n 번만의 확인 응답이 아니라 n 번까지의 확인 응답이라고 볼 수 있는데 이러한 점에서 Go-Back-N ARQ의 세그먼트를 누적확인 응답(Cumulative ACK) 이라고도 한다. 

### Selective Repeat ARQ 
- Selective Repeat ARQ 는 이름 그대로 선택적으로 재전송하는 방법을 의미한다. 
- 수신 호스트 측에서 제대로 전송 받은 각각의 패킷들에 대해서 ACK 세그먼트를 보내는 방식이다. 
- Selective Repeat ARQ의 ACK 세그먼트는 개별 확인 응답으로 볼 수 있다. 
- 오늘날의 대부분의 호스트는 TCP 통신에서 Selective Repeat ARQ를 지원한다. 두 호스트가 연결을 수립할 때 서로의 Selective Repeat ARQ 지원 여부를 확인하는데 만약 이를 사용하지 않을 경우에는 Go-Back-N ARQ 방식으로 동작한다. 

--- 
> 강민철, 『혼자 공부하는 네트워크』, 한빛미디어 (2018)    
