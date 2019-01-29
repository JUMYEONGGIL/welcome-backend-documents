# ☀️ Solr Setting Documentation
각 코어의 data-config.xml, schema.xml를 관리한다


## 🚩 Project
---
[Backend Search Vertical Finder](http://devlab.celebtide.com:8081/dev/backend_search_vertical_finder)

**Project Readme**
```text
통합 파인더 솔라 코어 설정 관리
각 코어의 data-config.xml, schema.xml를 관리한다.

Master
ec2 id : i-077024a77bec85851 (tokyo / 52.197.138.74)

Slave
ec2 id : i-08b8f189e4c185a2b (tokyo / 13.114.150.33)
ec2 id : i-02c45c769e98b4131 (tokyo / 52.199.203.230)
```


## 🙅‍♀️ Attention
---
- Slave Shutdown 은 **절대 동시에 진행하지 않는다**
    - 하나는 동작하는 상태를 유지해야 함
- Master 를 참조하는 서비스는 없다
- `.xml` 파일의 설정 오류인 경우 보통은 해당 Core 만 동작하지 않는다
    - 전체가 안된다면 다른 문제를 확인해 봐야 함
- Solr Core 는 backend database > celeb > view_{vertical}_finder 에 값이 있어야 data import 가 가능하다
    - View 를 만들기 위해서는 `vertical_ranking_summary` 테이블 값 세팅이 필요하다
    - 개체별 속성 점수, 필터 등이 있어야 한다는 의미



## 💻 Useful Commands
---

🦐  **Shell Command**
```bash
# show tail log of solr activation
$ tail -f /home/solr_was/solr_m1/logs/catalina.out

# shutting down
$ sh /home/solr_was/solr_m1/shutdown-solrwas_m1.sh

# start up
$ sh /home/solr_was/solr_m1/startup-solrwas_m1.sh

# [combination] shut down & restart
$ sh /home/solr_was/solr_m1/shutdown-solrwas_m1.sh && sh /home/solr_was/solr_m1/startup-solrwas_m1.sh
```


📝  **Vim Command**
```
# 찾아 바꾸기
:%s/찾을 단어/바꿀 단어/g
```

## 🥑 신규 코어 추가
---

**요약**
1. 프로젝트 복사
2. 저장소 반영
3. 아래 3개 파일 작업
    1. `core.properties`
    2. `data-config.xml`
    3. `schema.xml`
4. view_{vertical}_finder 테이블 생성
5. Master Restart
6. Slave Restart


**작업 순서**
1. 기존 rest_{vertical}_finder 프로젝트 복제
    - 경로: `/home/solr_home/`
2. 원격 저장소에 Push
3. 로컬 저장소에서 Pull
4. `data-config.xml`
    1. 기존의 내용 참고하여 작업
    2. {vertical_pk} 컬럼부터 regist_date 까지를 보통 변경해야 함
    3. 만약 위치정보 검색이 필요한 경우 `yelp`, `matzip` 프로젝트 참고
        1. 새로 추가할 Core 가 위치정보 활용이 없을 경우 그러한 기존 버티컬에서 복제하는 것이 좋음
    4. multiValue 처리 필수
5. `schema.xml`
    1. `<field>` 항목 수정
    2. `<entity>` 항목 수정
    3. `<script>` 태그 안의 자바스크립트 코드에서
        1. multiValue 컬럼 입력해야 함
        2. 기존 프로젝트 참고
6. 원격 저장소 Push
7. 원격 서버에서 Pull
8. `/data` directory 삭제
    - data 가 있으면 그 전의 결과를 caching 하는 듯 하다
    - data directory 를 삭제할 때는
        1. **먼저 solr 프로세스 종료**
        2. `rm -rf data`
        3. solr 프로세스 재시작
        - Useful Command 참고
9. `core.properties` 에서 {vertical} 이름 변경
    - cartoon 프로젝트를 복제했다면, cartoon → {신규버티컬} 명으로 찾아 바꾸기
    - Useful Command 참고
10. [**Slave Only**] `conf/solrconfig.xml` 파일에서 {vertical} 이름 변경
    - {vertical} 이름 전체 변경
    - Useful Command 참고
11. Master, Slave 에서 모두 동일하게 작업
    - **단, Slave 에서는 schema.xml, data-config.xml 작업은 필요없음!**
        - Master 에서 Replication 하기 때문
    - Slave 는 반드시 하나씩 재 시작 할 것
