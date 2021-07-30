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
5. killed_by 컬럼을 재분류화함
6. party_size와 map별로 데이터를 나눔

<컨설팅 시간에 질문할 사항들>
1. 같은 게임에서 등수가 같은데 생존 시간이 다른 경우??
2. 같은 시간대에 같은 아이디에 다른 매치 id가 있는 경우??   

- 2021/07/26
1. party_size와 map별로 나뉘어진 데이터의 Column들에 Outlier가 있는지, 있다면 어떤 값들을 Outlier로 지정해야 하는지 회의
2. 컨설팅 받기   

<컨설팅에서 강사님의 답변 내용>
1. 같은 게임에서 등수가 같은데 생존 시간이 다른 경우?? -> 개수가 많지 않고 특별한 의미를 갖지 않는다고 생각되면 drop
2. 같은 시간대에 같은 아이디에 다른 매치 id가 있는 경우?? -> 개수가 많지 않고 특별한 의미를 갖지 않는다고 생각되면 drop

- 2021/07/27
- Outlier 기준 선정
1. game_size : solo - 80, duo - 40, squad - 20 미만의 데이터는 Outlier로 판단(최대 game_size의 80%)
2. player_dise_walk, player_dist_ride : ride - 30km, walk - 10km 초과의 데이터는 Outlier로 판단 (맵의 크기가 8km x 8km임을 고려)
3. player_dmg : 3000 초과의 데이터는 Outlier로 판단(프로 게이머들도 30킬 이상은 드문 경우 임을 고려)
4. player_kills : 30 초과의 데이터는 Outlier로 판단(프로 게이머들도 30킬 이상은 드문 경우 임을 고려)
5. player_survive_time : 1900 초과의 데이터는 Outlier로 판단(게임 내에서 살아남을 수 있는 최대 시간의 +1~2분으로 설정)
6. player_dbno : 11 초과의 데이터는 Outlier로 판단(12번째 기절부터는 2초 내에 팀원의 도움이 있어야 살 수 있음을 고려)
7. killer와 victim의 거리 : 400m 초과 데이터는 Outlier로 판단(게임 내에서의 탄도학이 적용되고, 고배율 조준경이 있지 않는 이상 사람이 볼 수 없음을 고려)   
-> Outlier들은 제거하지 않고 데이터셋을 만들어 따로 보관(추후 Anomaly Detection에 활용할 )

- 2021/07/28
1. Outlier를 분리하여 정상적인 Dataset과 Outlier Dataset으로 나눔
2. Eragel 데이터와 Miramar 데이터를 다시 하나의 데이터로 합침
3. 각 플레이어의 Tear를 정하기 위해 tear_score계산((28 - team_placement) * 0.5 + kills * 2 + assists * 2)
4. Tear_score를 기반으로 플레이어들의 Tear를 나눔
5. 플레이어별 KDA를 구함
6. player_dmg - player_kills의 상관관계를 확인
7. player_survive_time - team_placement의 상관관계를 확인
8. player_dist_walk - team_placement, player_dist_ride - team_placement의 상관관계를 확인 후 player_dist_walk + player_dist_ride와 team_placement의 상관관계 확인

- 2021/07/29
1. tear_score가 0에 많이 몰려있는 것을 발견 -> 해결 방안 고민해보기
2. player_dmg ~ player_kills의 상관계수는 solo/duo/squad 모두 0.9이상 - > kills 변수 제거 가능
3. player_survive_time ~ team_placement의 상관계수 solo/duo/squad 모두 -0.8이하 -> 
4. player_dist_walk ~ team_placement
5. player_dist_ride ~ team_placement
6. 플레이어 별 match 수가 5 이상인 사람들만 뽑아내서 데이터의 크기를 줄이기로 결정

- 2021/07/30
1. tear_score의 계산식을 log(team_placement + player_kills * 2 + player_assists * 2)로 변경
2. 변경된 tear_score를 기준으로 6, 6.75, 7.5, 8.25, 9를 기준으로 티어를 나눔

- 2021/07/30
1. 할 일 1
2. 할 일 2

<분석을 수햄함에 있어서 어려운 점>
1. 데이터의 크기가 너무 커서 Data Load와 전처리시 SQL을 사용하기가 어려웠음
2. Python으로 Data를 Load하여도 Kernel이 자주 죽는 문제가 발생
