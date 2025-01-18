SFTP 클라이언트에서 사용하는 메서드는 파일 업로드, 다운로드, 디렉토리 탐색 등 파일 작업과 관련된 기능을 제공합니다. 이 메서드는 일반적으로 Python의 **`paramiko`** 또는 Java의 **JSch** 라이브러리 등에서 사용됩니다. 주요 메서드와 그 역할을 설명하겠습니다.

---

## 주요 SFTP 클라이언트 메서드

### 1. **연결 및 인증 관련**
- `connect(hostname, port, username, password)`  
  - SFTP 서버에 연결합니다.  
  - **매개변수**:
    - `hostname`: 서버 주소 (IP 또는 도메인).
    - `port`: SFTP 포트 (기본값: 22).
    - `username`: 사용자 이름.
    - `password`: 사용자 비밀번호 또는 키 인증.

- `close()`  
  - SFTP 세션을 종료하고 연결을 닫습니다.

---

### 2. **파일 전송**
- `put(local_path, remote_path)`  
  - 로컬 파일을 SFTP 서버의 지정된 경로로 업로드합니다.  
  - **매개변수**:
    - `local_path`: 업로드할 로컬 파일 경로.
    - `remote_path`: 업로드될 서버 파일 경로.

- `get(remote_path, local_path)`  
  - SFTP 서버의 파일을 로컬로 다운로드합니다.  
  - **매개변수**:
    - `remote_path`: 다운로드할 서버 파일 경로.
    - `local_path`: 저장될 로컬 파일 경로.

---

### 3. **디렉토리 및 파일 탐색**
- `listdir(remote_path)`  
  - 서버의 특정 디렉토리에 있는 **파일 및 디렉토리 목록**을 가져옵니다.  
  - **매개변수**:  
    - `remote_path`: 탐색할 디렉토리 경로.

- `listdir_attr(remote_path)`  
  - 디렉토리의 파일 목록과 메타데이터(파일 크기, 권한 등)를 가져옵니다.

- `cwd(remote_path)`  
  - 현재 작업 디렉토리를 변경합니다.  
  - **매개변수**:  
    - `remote_path`: 이동할 디렉토리 경로.

- `stat(remote_path)`  
  - 특정 파일 또는 디렉토리의 속성 정보를 반환합니다.  
  - **반환값**: 파일 크기, 권한, 생성 시간 등.

---

### 4. **파일 및 디렉토리 조작**
- `remove(remote_path)`  
  - 서버에서 지정된 파일을 삭제합니다.  
  - **매개변수**:  
    - `remote_path`: 삭제할 파일 경로.

- `rename(old_path, new_path)`  
  - 서버의 파일 또는 디렉토리 이름을 변경합니다.  
  - **매개변수**:  
    - `old_path`: 기존 경로.
    - `new_path`: 새 경로.

- `mkdir(remote_path)`  
  - 서버에 새 디렉토리를 생성합니다.  
  - **매개변수**:  
    - `remote_path`: 생성할 디렉토리 경로.

- `rmdir(remote_path)`  
  - 빈 디렉토리를 삭제합니다.  
  - **매개변수**:  
    - `remote_path`: 삭제할 디렉토리 경로.

---

### 5. **권한 및 속성**
- `chmod(remote_path, mode)`  
  - 파일 또는 디렉토리의 권한을 변경합니다.  
  - **매개변수**:  
    - `remote_path`: 경로.
    - `mode`: Unix 권한 값(예: `0o755`).

- `chown(remote_path, uid, gid)`  
  - 파일 소유자 및 그룹을 변경합니다.  
  - **매개변수**:  
    - `remote_path`: 경로.
    - `uid`: 사용자 ID.
    - `gid`: 그룹 ID.

---

### 6. **파일 검증**
- `exists(remote_path)`  
  - 파일 또는 디렉토리의 존재 여부를 확인합니다.  
  - **반환값**: 파일이 존재하면 `True`, 그렇지 않으면 `False`.

---

## Python `paramiko` 라이브러리 예제

```python
import paramiko

# SFTP 연결 설정
hostname = 'example.com'
port = 22
username = 'user'
password = 'password'

# SSH 클라이언트 생성
ssh = paramiko.SSHClient()
ssh.set_missing_host_key_policy(paramiko.AutoAddPolicy())
ssh.connect(hostname, port, username, password)

# SFTP 세션 시작
sftp = ssh.open_sftp()

# 파일 업로드
sftp.put('local_file.txt', '/remote/path/remote_file.txt')

# 파일 다운로드
sftp.get('/remote/path/remote_file.txt', 'local_file.txt')

# 디렉토리 탐색
files = sftp.listdir('/remote/path')
print("Files:", files)

# 연결 종료
sftp.close()
ssh.close()
```

---

이 메서드를 활용하면 SFTP 서버와의 파일 작업을 효율적으로 처리할 수 있습니다. 필요에 따라 타 언어(Java 등)의 구현 방식도 제공할 수 있으니 요청하세요! 😊
