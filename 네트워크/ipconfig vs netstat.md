
### ✅ `ipconfig` (Windows 전용)

- **풀네임**: Internet Protocol Configuration
- **주용도**: 컴퓨터의 **IP 주소, 서브넷 마스크, 기본 게이트웨이** 등 네트워크 어댑터의 **구성 정보**를 확인할 때 사용
- **자주 쓰는 옵션**:
  - `ipconfig /all`: 모든 네트워크 어댑터에 대한 상세 정보 출력
  - `ipconfig /release`: 현재 IP 주소 반환
  - `ipconfig /renew`: IP 주소 재할당
  - `ipconfig /flushdns`: DNS 캐시 초기화

📌 **예시 출력**:
```
Ethernet adapter Ethernet:
   IPv4 Address. . . . . . . . . . . : 192.168.0.10
   Subnet Mask . . . . . . . . . . . : 255.255.255.0
   Default Gateway . . . . . . . . . : 192.168.0.1
```

---

### ✅ `netstat` (네트워크 상태)

- **풀네임**: Network Statistics
- **주용도**: 컴퓨터의 **현재 네트워크 연결 상태**를 확인할 때 사용  
- **주로 확인할 수 있는 것들**:
  - 현재 열려 있는 포트
  - 연결된 IP 주소 및 포트
  - 사용 중인 프로토콜 (TCP, UDP)
  - 상태 (LISTENING, ESTABLISHED 등)
- **자주 쓰는 옵션**:
  - `netstat -a`: 모든 연결 및 수신 포트 표시
  - `netstat -n`: 주소와 포트를 숫자로 표시
  - `netstat -o`: 연결된 프로세스 ID(PID) 표시

📌 **예시 출력**:
```
Proto  Local Address          Foreign Address        State
TCP    192.168.0.10:5000      172.217.24.206:443     ESTABLISHED
TCP    0.0.0.0:80             0.0.0.0:0              LISTENING
```

---

### 🔍 정리하자면

| 항목       | ipconfig                        | netstat                              |
|------------|----------------------------------|---------------------------------------|
| 목적       | IP 정보 확인                    | 네트워크 연결 상태 확인              |
| 플랫폼     | Windows 전용                    | Windows, Linux 등 다양한 OS          |
| 주 정보    | IP 주소, 게이트웨이, DNS 등     | 연결된 IP, 포트, 상태, PID 등        |
| 실사용 예 | 인터넷이 안 될 때 IP 확인       | 포트 점유 상태나 트래픽 분석할 때   |

---
