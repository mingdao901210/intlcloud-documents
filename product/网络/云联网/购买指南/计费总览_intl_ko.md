CCN은 사용량 기반 후불식 요금제를 제공합니다.

- 지역 간 네트워크 인스턴스의 상호 연결에 비용을 청구합니다. 대역폭 제한을 가급적 빨리 설정하시기 바랍니다.

- 같은 지역 네트워크 인스턴스가 1G이하인 상호 연결에 비용을 청구하지 않습니다.


##요금제
CCN 지역 간 상호 연결이 있는 경우, 비용이 발생합니다. 지역별 매월 실제 사용 대역폭은 월별 95번째 백본위수 후불제를 기준으로 요금을 받습니다.

**계정 간 요금제:**

- 계정 간 요금제는 동일 계정의 경우와 같습니다.

- 크로스 계정이 CCN에 가입하는 모든 상호 연결 비용은 CCN 인스턴스의 속한 계정이 지불합니다.


##결제 공식
**CCN 월별 비용** = CCN 사설망 인스턴스가 다른 지역 간 상호 연결 비용의 총합입니다.

**지역 간 상호 연결 월별 비용** = 해당 지역의 상호 연결의 월 95번째 백분위수 피크값 * 유효 일수 비율 * 단계별 가격

- 월 95번째 백분위수 피크값:

5분마다 캡처합니다. 5분마다 지역 간 상호 연결 대역폭 평균값을 통계 포인트를 간주하여 내림차순으로 배열한 다음 가장 높은 5%의 포인트를 빼고 남은 최고값이 바로 당월의 95번째 백분위수 피크값입니다.

**예**: 사용자가 6월부터 CCN을 사용합니다. 지역 A와 지역 B 간에 상호 연결을 가능한 일수는 14일입니다. 5분마다 통계 포인트 하나를 생성하고 하루 통계 포인트의 총수는 288(60분 * 24 / 5분)개입니다. 14일 간의 모든 통계 포인트는 4032(14일 * 288개/일)가 됩니다. 4032개의 통계 포인트의 대역폭 값을 내림차순으로 배열한 다음 가장 높은 5%(4032개 * 0.05 = 201.6개)의 포인트를 빼고, 제202포인트의 대역폭 값은 바로 당월의 월 95번째 백분위수 피크값입니다.

- 유효 일수 비율:

유효 일수는 당일 대역폭이 10Kbps 이상인 통계 포인트를 적어도 하나가 존재하는 것을 의미합니다.

유효 일수 비율 = 당월 유효 일/당월의 일 수

- 단계별 가격:

단계별 가격, 즉,당월 월 95번째 백분위수 피크값이 속한 구간을 말합니다.


##월 95 요금제 가격표
 <table>

 <tr>

 <th>요금제 항목 </th>

 <th>사양</th>

 <th>중국 대륙(홍콩은 포함하지 않음) 지역 간 상호 연결 가격 </th>

 <th>가격 유닛</th>

 </tr>


 <tr>

 <td rowspan=3>영역 간 대역폭</td>

 <td>0 - 100Mbps </td>

 <td>37 </td>

 <td rowspan=3>USD/Mbps/월</td>

 </tr>


 <tr>

 <td>100Mbps - 1000Mbps </td>

 <td>13 </td>

 </tr>


 <tr>

 <td>1000Mbps 이상</td>

 <td>9 </td>

 </tr>


 </table>


>?

>- 같은 도시의 BM과 공유 클라우드 간의 1G 이하인 상호 연결이 무료입니다. 예: 베이징 BM VPC와 베이징 공유 클라우드 VPC 간의 상호 연결이 1G 이하이면 무료입니다.

>- 중국 대륙과 기타 지역 간의 상호 연결 가격은 비즈니스 매니저에게 문의하십시오.


##요금제 예시
사용자가 6월에 CCN을 사용하고 광저우, 베이징, 상하이 지역의 네트워크 인스턴스를 관련하는 경우

예광저우-베이징을 예로,

- 지역 간 월 95번째 백분위수 피크값:

월 95번째 백분위수 피크값 계산 방법에 따라 지역 간의 대역폭 피크값: 광저우 - 베이징 120M.

- 유효 일수 비율:

사용자가 6월에 광저우 - 베이징 지역 간의 상호 연결의 가능한 일은 14일이면 유효 일수 비율은 14 / 30입니다.

- 단계별 가격:

120M이 속한 단계별 가격: 13USD/Mbps/월.


6월에 광저우 - 베이징의 총 비용:

해당 지역 간의 상호 연결의 월 95번째 백분위수 피크값(120M)* 유효 일수 비율(14 / 30) * 단계별 가격(13USD/Mbps/월)= 728USD.

다른 지역의 경우도 마찬가집니다(광저우 - 상하이, 베이징 - 상하이).

따라서 CCN이 6월에 발생한 비용은 **지역 간 상호 연결의 총합**입니다.
[!](https://main.qcloudimg.com/raw/67fc221ad32ae2359e5159e3af219d98.png)
