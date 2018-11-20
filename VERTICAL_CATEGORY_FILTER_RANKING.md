# 필터 랭킹 등록

## 📝 들어가기 전에
---

개체가 등록되고, 필터가 있는 버티컬이라면
모듈 분석 과정에서 필터 랭킹이라는 항목을 사용할 수 있습니다

이 때, **개체만 등록하고 이 점수를 만들지 않으면**
**모듈 분석 과정에서 오류가 발생합니다**

보통의 오류 메시지는 아래와 같습니다

```python
Traceback (most recent call last):
  File "/var/task/cook/similar_cook_by_taste.py", line 170, in main
    for item in filter_rank:
UnboundLocalError: local variable 'filter_rank' referenced before assignment
```

이런 문제가 있을 경우에는 `celeb.vertical_category_filter_ranking` 테이블에
해당 개체의 점수 값이 없는 경우입니다

이럴때는 application 서버에서 **점수를 생성하는 프로그램을 실행** 해야 합니다

## 💯 점수 생성
---

```bash
$ ssh root@13.113.223.47

# vertical_category_filter_ranking 테이블에 업데이트
# tcc_idx 의 경우는 분석하려는 버티컬의 type_celeb_code 를 입력하시면 됩니다
$ python /service/backend/ba-word2vec/export_emotion_filter_ranking.py -r start -v {tcc_idx}
```

## 🙈 결과 확인
---

```sql
SELECT *
FROM celeb.vertical_category_filter_ranking
WHERE cd_idx = {cd_idx};
```