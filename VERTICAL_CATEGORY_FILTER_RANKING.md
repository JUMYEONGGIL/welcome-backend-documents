# 필터 랭킹 등록
> Created: Nov 20, 2018 10:30 PM
>
> Author: Kyoong

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

# 실행중인 프로그램의 실시간 로그 확인 및 프로세스 종료 확인
$ tail -f /service/backend/ba-word2vec/logs/export_emotion_filter_ranking.log
```




## 🙈 실행 및 결과 확인
---

```bash
[root@ip-172-31-27-152 /]# python /service/backend/ba-word2vec/export_emotion_filter_ranking.py -r start -v 25
start daemon

[root@ip-172-31-27-152 /]# tail -f /service/backend/ba-word2vec/logs/export_emotion_filter_ranking.log
2018-11-20 06:15:02,120 dt.export_emotion_filter_ranking [INFO] START TCC_IDX : 22
2018-11-20 06:15:14,799 dt.export_emotion_filter_ranking [INFO] END PROCESS_CATEGORY_FILTER_RANKING TCC_IDX : 22
2018-11-20 06:15:14,812 dt.export_emotion_filter_ranking [INFO] filter_list cnt : 10
2018-11-20 06:15:14,824 dt.export_emotion_filter_ranking [INFO] -----------------tcc_idx: 22, cnt : 10
2018-11-20 06:15:19,604 dt.export_emotion_filter_ranking [INFO] END PROCESS_FILTER_RANKING TCC_IDX : 22
2018-11-20 06:15:19,645 dt.export_emotion_filter_ranking [INFO] 종료 시간 2018-11-20 06:15:19.645527
2018-11-20 06:15:19,645 dt.export_emotion_filter_ranking [INFO] 경과 시간 0:00:17.659540
2018-11-20 10:53:18,067 dt.export_emotion_filter_ranking [INFO] Program Started
2018-11-20 10:53:18,067 dt.export_emotion_filter_ranking [INFO] 시작 시간2018-11-20 10:53:18.067827
2018-11-20 10:53:18,195 dt.export_emotion_filter_ranking [INFO] START TCC_IDX : 25
2018-11-20 10:53:36,128 dt.export_emotion_filter_ranking [INFO] END PROCESS_CATEGORY_FILTER_RANKING TCC_IDX : 25
2018-11-20 10:53:36,137 dt.export_emotion_filter_ranking [INFO] filter_list cnt : 10
2018-11-20 10:53:36,148 dt.export_emotion_filter_ranking [INFO] -----------------tcc_idx: 25, cnt : 10
2018-11-20 10:53:41,730 dt.export_emotion_filter_ranking [INFO] END PROCESS_FILTER_RANKING TCC_IDX : 25
2018-11-20 10:53:41,757 dt.export_emotion_filter_ranking [INFO] 종료 시간 2018-11-20 10:53:41.757942
2018-11-20 10:53:41,758 dt.export_emotion_filter_ranking [INFO] 경과 시간 0:00:23.689962
```

```sql
SELECT *
FROM celeb.vertical_category_filter_ranking
WHERE cd_idx = {cd_idx};
```s