# clova-gan-kaist-readme

각 연구실 당 기본적으로 8대 씩 (4개 x 2) 할당 된 상황이나, 물리적으로 나누어 놓지 않아서 96개 지피유가 전부 보이는 상황입니다. 비어 있으면 좀 더 쓰셔도 되나, 급한 경우 저희가 임의로 끌 수도 있습니다. (나중에 다른 연구실/연구자가 갑자기 쓸 수도 있기 때문에 20개 이상 지나치게 점유는 지양 부탁드립니다.)

**연구실 별 돌리는 세션이 구분되지 않기 때문에, 실행 후 웹상에서 연필 버튼을 눌러서 확인을 위해 어느 연구실에서 돌린 것인지 메모 작성을 부탁드립니다. (예: [cviplab] 비디오생성 실험)**

### 웹 로그인
  https://ai.nsml.navercorp.com/ 
  여기서 알려드린 아이디/비번으로 로그인이 가능합니다.
  
### nsml 다운로드 & 로그인
https://ai.nsml.navercorp.com/download

여기서 다운 받으시면 됩니다. 아래 내용은 linux 기준으로 설명하고 있습니다.

nsml login을 터미널에 치시고 웹 로그인 할 때와 같은 아이디/비번을 사용하시면 됩니다.

### session 돌리기
https://n-clair.github.io/ai-docs/_build/html/en_US/contents/session/run_a_session.html

이 문서에 작성되어 있으나, 현재는 사내용 docs만 업데이트가 되고 있는 것 같습니다. 제가 쓰고 있는 명령어를 참고해서 아래에 붙이면
```
nsml run -d gy_ffhq256_1k_zip -g 4 -c 8 -v -e train.py --memory 64G --shm-size 64G --gpu-driver-version 418.39 -a "--outdir=./output/ --data=train/ffhq256x256_1k.zip --cfg paper256 --gpus=4 --aug=noaug"
```
- -d gy_ffhq256_1k_zip
- -g 4 세션에 할당할 지피유 개수
- -c 8 세션에 할당할 cpu 개수
- --memory 64G 메모리
- --shm-size 64G shared memory가 작으면 torch에서 잘 터져서 추가했습니다.
- --gpu-driver-version 418.39 특정 드라이버 이후의 머신을 배정받으려면 사용합니다.
- -a "--outdir..." 메인 실행파일에 들어가는 config 인자들입니다.
  


### 데이터 업로드
```
nsml dataset push --private 데이터이름 데이터경로
```

공용 계정이다보니 데이터 이름은 관리자를 인식 가능하도록 부탁드립니다. (예: kaist-cviplab-ffhq)


### 코드 변경 (데이터 읽기)
사실 코드상에서는 dataset을 읽어오기 위한 path를 변경하는 것만 해주면 대체로 잘 돌아갑니다.

``` 
# 예시
from nsml import DATASET_PATH
data = os.path.join(DATASET_PATH, 'train/' + datasetname) 
```
  
  보통 nsml dataset push --private mydataset ./mnist 이런 식으로 올리면 DATASET_PATH/train/mnist 이런 path로 올라가지게 됩니다.

### output 다운로드

코드 상에 저장된 애들은 nsml download로 받아오시면 됩니다.
```
# 예시
nsml download SESSION_NAME PATH -s '/app/my_output_path_from_mainpy'
```
경로가 이상하면 nsml sh SESSION_NAME 'ls /app' 으로 생성된 결과가 어디에 있는지 찾아보시면 됩니다.
