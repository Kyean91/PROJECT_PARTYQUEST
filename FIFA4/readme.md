# PROJECT_PARTYQUEST
- 데이터 출처 : 넥슨 개발자센터(https://developers.nexon.com/fifaonline4) 'Data based on NEXON DEVELOPERS'
- 발표자료(ppt) : 
- 분석 목표 : 
---
- 2021/09/16 : 넥슨 개발자센터(https://developers.nexon.com) 에서 Open API를 사용하여 약 500만 건의 최근 match_id 수집, 전체 수집은 1,000만 건을 시도하였지만 결과적으로는 중복되지 않는 match는 약 500만건 수집 \
=> 수집한 match_id를 기반으로 해당 match의 상세 정보를 수집할 예정
- 2021/09/17 : 수집된 match_id에 참여한 유저의 ID(AccessId)를 수집, 한 경기에 2명의 유저가 플레이 하므로 약 1,000만건의 유저 ID가 수집될 것으로 예측하였지만, 한 경기에 1명의 유저만 존재하는 경우도 있었기 때문에 계획보다 적은 수의 AccessId를 수집, 수집한 AccessId를 기반으로 해당 유저의 match_id를 수집하여 match_id별 아래의 형태의 JSON 파일을 수집\
![json](https://user-images.githubusercontent.com/85550229/133777418-6f2d9050-a3e7-4b63-99db-c2119975c541.PNG)
이후 JSON 파일을 변환하여 데이터를 ID별 최대 100경기의 'match_detail', 'shoot', 'shoot_detail', 'pass_info', 'defence', 'player' 데이터를 아래의 Schema형식으로 변환\
![schema](https://user-images.githubusercontent.com/85550229/133791568-86057dd7-93e2-4c15-b885-b104cc43f6ea.png)
=> SQL을 활용하여 데이터 전처리를 수행하고, 전처리가 끝난 데이터를 Python으로 불러와서 분석을 수행할 예정
- 2021/09/18 ~ 2021/09/28 : API크롤링을 통한 데이터 수집 진행
- 2021/09/29 : 데이터의 크기가 커서 SQL을 활용하는데 어려움이 있다고 판단하여 각각 1,000명의 유저를 sampling하여 데이터를 나눔\
=> SQL을 사용하여 전처리를 진행하고, 모형의 Pipeline을 구축한 후 나머지 데이터를 활용하여 모형의 학습과 테스트를 진행할 예정
- 2021/09/30 : 개별적으로 수집한 데이터를 합치고 1,000명의 
- 

