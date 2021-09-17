# PROJECT_PARTYQUEST
- 데이터 출처 : 넥슨 개발자센터(https://developers.nexon.com/fifaonline4) 'Data based on NEXON DEVELOPERS'
- 발표자료(ppt) : 
- 분석 목표 : 
---
- 2021/09/16 : 넥슨 개발자센터(https://developers.nexon.com) 에서 Open API를 사용하여 약 500만 건의 최근 match_id 수집, 전체 수집은 1,000만 건을 시도하였지만 결과적으로는 중복되지 않는 match는 약 500만건 수집 \
=> 수집한 match_id를 기반으로 해당 match의 상세 정보를 수집할 예정
- 2021/09/17 : 수집된 match_id에 참여한 유저의 ID(AccessId)를 수집, 한 경기에 2명의 유저가 플레이 하므로 약 1,000만건의 유저 ID가 수집될 것으로 예측하였지만, 한 경기에 1명의 유저만 존재하는 경우도 있었기 때문에 계획보다 적은 수의 AccessId를 수집, 수집한 AccessId를 기반으로 해당 유저의 match_id를 수집하여 match_id별 아래의 형태의 JSON 파일을 수집\
![json](https://user-images.githubusercontent.com/85550229/133777418-6f2d9050-a3e7-4b63-99db-c2119975c541.PNG)
이후 JSON 파일을 변환하여 데이터를 ID별 최대 100경기의 'MatchDetail', 'Shoot', 'ShootDetail', 'Pass', 'Defense', 'Player' 데이터를 아래의 Schema형식으로 변환\
![schema](https://user-images.githubusercontent.com/85550229/133790733-df4baf33-3ed3-4e3a-bcf5-4122a506ed68.png)
=> SQL을 활용하여 데이터 전처리를 수행하고, 전처리가 끝난 데이터를 Python으로 불러와서 분석을 수행할 예정
-  

