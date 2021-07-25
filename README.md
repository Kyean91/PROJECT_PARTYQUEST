# PROJECT_PARTYQUEST
- 데이터 출처 : https://www.kaggle.com/skihikingkevin/pubg-match-deaths
- 분석 목표 : 유저들의 게임 데이터로 각 티어간의 특징의 차이를 도출하여 다음 티어로 나아가기 위한 방향성을 조언
---
- 2021/07/23
1. aggregate 데이터와 death 데이터에서 공통된 match_id만 필터링
2. aggregate 데이터의 match_mode가 'tpp' 한 종류 이므로 분석의 영향을 미치지 않을 것으로 판단하여 제거
3. aggregate 데이터의 survive_time 컬럼을 kill_deaths 데이터의 time과 매칭시키기 위해 round 함수를 사용하여 반올림 수행 -> 소수점에서의 차이가 유의미한 데이터를 발견하여 rollback
4. kill_deaths 데이터의 각 컬럼별 결측치 비율을 확인 
5. map 컬럼의 결측값은 다른 데이터와 연결되지 않아 결측치 대체가 어렵다 판단하여 제거

- 2021/07/25
1. aggregate 데이터의 party_size가 1인 Solo 게임의 경우 Player_name의 결측값을 채울 수 있으므로 해당 작업 수햄
2. aggregate 데이터의 match_id와 team_placement 컬럼과 kill_deaths 데이터의 match_id와 killer_placement 변수를 각각 연결하여 kill_deaths 데이터의 killer_name의 결측값을 채움
-> 다른 값들이 중복되어 결측값을 채우기 어려운 데이터가 발견되어 결측치를 채우기 어렵다 판단하여 차선책인 결측치 제거로 선회
3. aggregate 데이터와 kill_deaths 데이터를 match_id와 player_name으로 merge하여 데이터를 하나로 연결
4. kill이 없는 유저는 map 컬럼의 값이 NaN으로 입력되어 map 컬럼의 NaN 값을 채움
5. party_size와 map별로 데이터를 나눔

질문 사항 
1. 같은 게임에서 등수가 같은데 생존 시간이 다른 경우??
2. 같은 시간대에 같은 아이디에 다른 매치 id가 있는 경우??   
<추가 컨설팅 시간에 질문 사항이 있으시면 적어주세요!!>
