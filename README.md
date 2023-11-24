# 2023-oss-hw1
[MASK RCNN] Baloon.py Train / Test


## 개요
- Mask R-CNN에 대해 알아보고 Sample Code인 Balloon.py를 트레이닝 시킨다.
- 트레이닝 후 얻은 가중치를 이용해 풍선 부분을 segmentation 한 결과를 보여준다.


## ⚙️ 환경 설정
- **아나콘다에서 가상환경 생성(python 3.7)**
- **mask-rcnn 다운로드**

    git clone https://github.com/matterport/Mask_RCNN.git
  

- **버전 수정**


기존의 아나콘다 프롬프트에는 tensorflow 2.0이 설치되어 있는데 Mask R-CNN은 tensorflow의 버전이 1.14일 때 만들어졌을 것으로 추정되기 때때문에 아래와 같이 버전을 바꿔서 설치한다.

    pip install tensorflow-gpu==1.14.0 keras==2.1.3


- **balloon dataset 다운로드**


아래의 사이트 사이트에서 balloon dataset을 다운받는다.


<https://github.com/matterport/Mask_RCNN/releases>

  - balloon_dataset.zip : _MACOS에서 가능한 폴더를 제외한 train, val에 대한 balloon dataset을  Mask_RCNN/sample/ballon폴더의 하위 폴더에 추가

  - mask_rcnn_balloon.h5 : 제공되는 balloon dataset을 이용하여 미리 학습시켜둔 가중치 COCO 모델 파일(*.h5)로, coco weights는 training시에 사용되니 같은 root안에 다운 받을 것을 권장


## 🖥️ Train

- **balloon.py의 ROOT_DIR 수정**


balloon.py 38번째의 코드인 기존에 있던 ROOT_DIR 값을 절대 경로로 수정해주어야 한다.

    ROOT_DIR = os.path.abspath("C:/Users/lsjlc/coding/Mask_RCNN")
  
- **모델 학습**


가상환경 경로를 balloon.py가 있는 폴더로 이동한 후 아래의 코드를 통해 훈련을 진행한다.

    python balloon.py train --dataset=/path/to/balloon/dataset --weights=coco  

 

## 📌 test

- **학습완료된 가중치**: Mask_RCNN 폴더 안에 있는 logs 의  "mask_rcnn_balloon_0030.h5"


    - epoch은 전체 데이터에 대한 한번의 학습(forward 와 backward 포함)을 의미
    - 100회씩 30번 반복하며 100회마다 가중치 모델이 갱신되어 경로에 저장된다. 1epoch 실행 시 mask_rcnn_balloon_0001.h5로, 매번 학습이 됨으로써 더 좋은 학습 결과를 얻을 수 있기 때문에 총 30epoch을 실행한 가중치 파일 mask_rcnn_balloon_0030.h5 을 사용하였다.

### 학습이 완료된 가중치 모델을 이용하여 결과 확인
    python Mask_RCNN/samples/balloon/balloon.py splash --weights=coding/Mask_RCNN/logs/balloon20231117T2356/mask_rcnn_balloon_0030.h5 --image=coding/Mask_RCNN/samples/balloon/balloon/val/3800636873_ace2c2795f_b.jpg
