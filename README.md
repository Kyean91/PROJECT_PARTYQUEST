# PROJECT_PARTYQUEST
- 데이터 출처 : https://www.kaggle.com/skihikingkevin/pubg-match-deaths
- 분석 목표 : 유저들의 게임 데이터로 각 티어간의 특징의 차이를 도출하여 다음 티어로 나아가기 위한 방향성을 조언
---
- 2021/07/23
1. aggregate 데이터와 death 데이터에서 공통된 match_id만 필터링
2. aggregate 데이터의 match_mode가 'tpp' 한 종류 이므로 분석의 영향을 미치지 않을 것으로 판단하여 제거
3. aggregate 데이터의 survive_time 컬럼을 kill_deaths 데이터의 time과 매칭시키기 위해 round 함수를 사용하여 반올림 수행
4. kill_deaths 데이터의 각 컬럼별 결측치 비율을 확인 
5. map 컬럼의 결측값은 다른 데이터와 연결되지 않아 결측치 대체가 어렵다 판단하여 제거

- 2021/07/24
1. aggregate 데이터의 party_size가 1인 Solo 게임의 경우 Player_name의 결측값을 채울 수 있으므로 해당 작업 수햄
1.1. aggregate 데이터의 match_id와 team_placement 컬럼과 kill_deaths 데이터의 match_id와 killer_placement 변수를 각각 연결하여 aggregate 데이터의 player_name의 결측값을 채움
1.2. 
3. 수행한 내용 2
