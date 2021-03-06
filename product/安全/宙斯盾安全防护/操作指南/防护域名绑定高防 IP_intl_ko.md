
[Aegis 고도 방어 제품 콘솔](https://console.cloud.tencent.com/gamesec)의 좌측 디렉터리에서 “비즈니스 도메인 이름 리스트”를 선택하고 우측 페이지에서 “비즈니스와 도메인 이름 생성”을 클릭하여 비즈니스를 생성하고 자동으로 방어 도메인 이름이 생성됩니다. 사용자가 비즈니스 도메인 이름을 방어 도메인 이름으로 CNAME을 설정하여 고도 방어에 액세스합니다.

## 프로세스도
![](https://main.qcloudimg.com/raw/d8befef12266bd7cff8544a7109feb15.png)

## 방어 도메인 이름에 고도 방어 IP를 바인딩하는 프로세스
1. **비즈니스 생성**
a. [비즈니스 도메인 이름 리스트]를 클릭하고 “비즈니스 도메인 이름 리스트”에서 [비즈니스와 도메인 이름 생성]을 클릭합니다.
![1](https://main.qcloudimg.com/raw/d4955cf53f8738af373d1db8bce32ccd.png)
b. 관련 정보를 입력하고 [생성]을 클릭합니다. 생성되면 “비즈니스 도메인 이름 리스트”에서 비즈니스와 무료의 방어 도메인 이름이 즉시 생성됩니다.
![2](https://main.qcloudimg.com/raw/f2ea3780e59cace35425b5f0fec6c877.png)
2. **고도 방어 IP 추가**
a. 비즈니스 도메인 이름 리스트 관리 페이지에서 “IP 추가”를 클릭하여 비즈니스 세부 정보 페이지로 리디렉션합니다.
![3](https://main.qcloudimg.com/raw/3b06c191ba6b3c6e6381eec906e30974.png)
b. 비즈니스 세부 정보 페이지의 IP 리소스와 해석 설정에서 “IP 추가”를 한번 클릭합니다.
![4](https://main.qcloudimg.com/raw/59b66ea77aceddb49ca3d0953981c5de.png)
c. 고도 방어 IP 를 선택하고 [확인]을 클릭합니다.
![5](https://main.qcloudimg.com/raw/5b35f2a31ecdf7095fd27d85c3d9d19f.png)
3. **도메인 이름 해석 활성화**
고도 방어 IP를 성공적으로 추가한 후에 “도메인 이름 해석”을 활성화합니다. 방어 도메인 이름은 지능적 해석을 제공합니다. 즉 사용자 오리진 IP를 해당 회선의 IP로 해석합니다. 예를 들면 China Telecom의 사용자가 China Telecom의 고도 방어 IP를 해석하고 China Unicom의 사용자가 China Unicom의 고도 방어 IP를 해석합니다. 어느 회선의 고도 방어 IP가 최고값을 초과한 공격으로 인해 차단되면 기타 사용할 수 있는 고도 방어 IP로 자동으로 해석됩니다.
BGP 회선의 우선 순위 스위치가 활성화된 경우 BGP 회선을 바이딩한 IP가 있는 경우 방어 도메인 이름이 우선적으로 모든 비즈니스 요청을 스케줄링하여 BGP의 IP로 해석합니다. (해석 스위치를 활성화하는 비BGP 고도 방어 IP가 대기 상태에 있습니다). 대규모 트래픽 공격으로 인해 BGP 고도 방어 IP가 차단되면 시스템이 지능적으로 비즈니스 요청을 도메인 이름 해석 스위치를 활성화하는 비BGP 고도 방어 IP로 스케줄링하고 높은 대역폭 방어 능력을 제공합니다. BGP 고도 방어 IP가 차단 해제되면 시스템이 모든 비즈니스 요청을 BGP 고도 방어 IP로 다시 스케줄링합니다.
![6](https://main.qcloudimg.com/raw/1a8e6bc9301e8ab79831b9e2e53e3761.png)
4. **메인 도메인 이름을 방어 도메인 이름에 CNAME 연결**
회선 해석을 활성화한 후에 비즈니스 메인 도메인 이름은 방어 도메인 이름으로 CNAME을 설정하여 지능적으로 고도 방어 IP로 해석합니다.
사용자 검증, 예를 들면 로컬에서 ping 또는 nslookup 방식으로 도메인 이름이 고도 방어 IP로 해석할 수 있는지 확인합니다.

