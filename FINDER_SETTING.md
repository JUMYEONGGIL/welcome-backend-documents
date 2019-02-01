# ☀️ FINDER Setting Documentation
각 버티컬의 finder를 세팅한다.


## 🚩 Project
---
- config
[mycelebsPythonConfig](http://devlab.celebtide.com:8081/dev/mycelebsPythonConfig)

- 도쿄리전
[serverlessRestCeleb](http://devlab.celebtide.com:8081/dev/serverlessRestCeleb)
- 서울리전
[serverlessRestCeleb_seoulregion](http://devlab.celebtide.com:8081/dev/serverlessRestCeleb_seoulregion)

## Workflow
---
1. [선행조건] SOLR_SETTING.md flow를 모두 진행 하여 solr 생성
2. mycelebsPythonConfig
    - config
        - `fields_{vertical}.json` 파일생성
            - 해당 버티컬 input field 추가(q)
        - `fl_fields_{vertical}.json` 파일생성
            - 해당 버티컬 기본 output 컬럼 추가
        - `solr_core.json`
            - "rest_{vertical}_finder": "solr_finder_new" 추가

3. serverlessRestCeleb
    - mycelebsPythonConfig에 추가된 내용 적용
        - $ pipenv shell
        - $ pip install -t vendor -r requirements.txt --upgrade
    - `finder.py`
        - RESULT_SUB_TITLE_FIELD에 추가 title, sub_title 조합하여 추가
        - profile_url 추가 (front에서 요청하는 url 형식 추가)

4. serverlessRestCeleb_seoulregion
    - serverlessRestCeleb과 동일하게 맞춤

## 🙅‍♀️ Attention
---
- finder 배포하기 전에 충분한 테스트를 한다.(실시간 서비스이기 때문에)
- 배포하기전에 postgre에 미리 api주소를 작성한 후 배포하자마자 호출해본다.
- !! 반드시 serverlessRestCeleb 와 serverlessRestCeleb_seoulregion는 동일화 시킨다 !!

