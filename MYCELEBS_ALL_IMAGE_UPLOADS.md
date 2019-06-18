# MYCELEBS_ALL 개체 이미지 업로드 (Shell Script)
> Created: 2019.06.04
>
> Author: xxel(hosung)

## 📸 신규 MYCELEBS_ALL 개체의 이미지 업로드

- `AWS S3 - mycelebs-images`
    - 이 버킷에 이미지가 저장됩니다.
    - 이미지는 9개의 사이즈를 준비하여 업로드 합니다.
    - CF를 통해 `https://all.image.mycelebs.com/{series_id}/{series_id}_{size@size}.png` 주소로 클라이언트에 제공됩니다

#### aws-cli 설치

```bash
$ python3 --version
Python 3.6.5

$ pip3 --version # 3.6과 pip가 연결 되어 있는지 확인합니다. 아니라면 WELCOME.md 를 참고해 파이썬을 설치하세요
pip 18.0 from /usr/local/lib/python3.6/site-packages/pip (python 3.6)

🌵 master [!?]
in welcome-backend-documents 🐶 kyoong λ pip3 install awscli
Requirement already satisfied: awscli in /usr/local/lib/python3.6/site-packages (1.16.14)
Requirement already satisfied: botocore==1.12.4 in /usr/local/lib/python3.6/site-packages (from awscli) (1.12.4)
Requirement already satisfied: rsa<=3.5.0,>=3.1.2 in /usr/local/lib/python3.6/site-packages (from awscli) (3.4.2)
Requirement already satisfied: colorama<=0.3.9,>=0.2.5 in /usr/local/lib/python3.6/site-packages (from awscli) (0.3.9)
Requirement already satisfied: s3transfer<0.2.0,>=0.1.12 in /usr/local/lib/python3.6/site-packages (from awscli) (0.1.13)
Requirement already satisfied: docutils>=0.10 in /usr/local/lib/python3.6/site-packages (from awscli) (0.14)
Requirement already satisfied: PyYAML<=3.13,>=3.10 in /usr/local/lib/python3.6/site-packages (from awscli) (3.13)
Requirement already satisfied: urllib3<1.24,>=1.20 in /usr/local/lib/python3.6/site-packages (from botocore==1.12.4->awscli) (1.23)
Requirement already satisfied: jmespath<1.0.0,>=0.7.1 in /usr/local/lib/python3.6/site-packages (from botocore==1.12.4->awscli) (0.9.3)
Requirement already satisfied: python-dateutil<3.0.0,>=2.1; python_version >= "2.7" in /usr/local/lib/python3.6/site-packages (from botocore==1.12.4->awscli) (2.7.3)
Requirement already satisfied: pyasn1>=0.1.3 in /usr/local/lib/python3.6/site-packages (from rsa<=3.5.0,>=3.1.2->awscli) (0.4.4)
Requirement already satisfied: six>=1.5 in /usr/local/lib/python3.6/site-packages (from python-dateutil<3.0.0,>=2.1; python_version >= "2.7"->botocore==1.12.4->awscli) (1.11.0)
```

#### 업로드 실행


### 🌊 Flow
---
1. Shell Script를 작성하여 실행합니다. 

2. Shell Script 내용은 아래와 같이 작성 합니다. ( file name : mycelebs_all_img.sh)

￼

```#!/bin/bash
target=$1
cd $target
echo 'target path :'$1
here=$(pwd)
s3_path='s3://mycelebs-images'
cmd='aws s3 cp .'
for folder in *

do
cd $here
cd ./$folder
        series_id=$folder
        $cmd $s3_path/$series_id --recursive --exclude "*" --include "*_*@*.png"
done
```

3. 해당 스크립트에 실행 권한을 부여합니다.
```user [~] → chmod +x mycelebs_all_img.sh```


4. 스크립트 실행 전 업로드 할 이미지가 들어있는 폴더의 절대경로를 확인합니다.
- pwd 명령어를 사용합니다.
```user [~] → pwd```

- 해당 경로 내에는 series_id/이미지.png 가 존재해야 합니다. 

해당 폴더 구조는 예시. series_id = 351518 / 상위 폴더 날짜로 생성 (20190604)
```/Users/hosunglee/mycelebs_imgs/20190604/351518/351518_108@108.png
/Users/hosunglee/mycelebs_imgs/20190604/351518/351518_144@144.png
/Users/hosunglee/mycelebs_imgs/20190604/351518/351518_240@240.png
/Users/hosunglee/mycelebs_imgs/20190604/351518/351518_249@249.png
/Users/hosunglee/mycelebs_imgs/20190604/351518/351518_288@288.png
/Users/hosunglee/mycelebs_imgs/20190604/351518/351518_573@372.png
/Users/hosunglee/mycelebs_imgs/20190604/351518/351518_1035@420.png
/Users/hosunglee/mycelebs_imgs/20190604/351518/351518_1035@738.png
/Users/hosunglee/mycelebs_imgs/20190604/351518/351518_1125@1464.png
```

4. 스크립트 실행 시 업로드 할 이미지가 모여 있는 상위 폴더 경로를 입력 해줍니다.
- star_img.sh /Users/hosunglee/mycelebs_imgs/20190604/ -> 업로드 된 파일의 구분을 위해 업로드 상위폴더(20190604)를 생성 관리합니다.

```user [~] → star_img.sh /Users/hosunglee/mycelebs_imgs/20190604/```


#### Common Rules
1. **이미지는 반드시 png 파일 형식이어야 합니다** 
    - 담당자에게 필히 확인 받으세요
2. **파일 이름은 형식은 아래와 같습니다**
- `{series_id}_{size@size}.png`
- 해당 이미지들은 꼭 series_id 로 된 상위 폴더내에 존재해야 합니다. ( ~/{series_id}/{series_id}_{size@size}.png)

### 👀 이미지 정상 등록 URL로 확인
https://all.image.mycelebs.com/{series_id}/{series_id}_{size@size}.png

#### S3에서 확인
Amazon S3 내 mycelebs-images 내에 아래와 같은 형태로 저장되어 있는지 확인합니다. 
`{series_id}/{series_id}_{size@size}.png` 형태로 저장되었는지 확인합니다


