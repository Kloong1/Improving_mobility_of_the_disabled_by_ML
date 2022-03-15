# 장애인 이동성 증진을 위한 AI 기술 연구
2021.02 - 2021.10에 한양대학교 컴퓨터소프트웨어학과 졸업프로젝트로 진행한 연구 프로젝트. ML을 활용하여 장애인의 이동성 개선 방안에 대해 연구함.

팀원: 김요환, 진승현  
담당교수: 김광욱

## 프로젝트 목표
DL 모델에 스마트폰 센서 데이터를 학습하여 스마트폰 사용자의 이동 수단을 classification한다. 특히 전동 휠체어 데이터를 학습 데이터에 포함시킴으로써, 비장애인 데이터와 장애인 데이터를 통합하여 classification한다.

## 프로젝트 진행 과정
### 1. SHL Dataset Classification
먼저 영국에서 수집한 잘 정제된 스마트폰 센서 데이터인 SHL Dataset을 classification 하는 것을 시도했다.

해당 dataset은 영국에서 성인 남성이 Torso, Backpack, Hand, Pocket에 위치한 4개의 스마트폰으로 측정한 dataset으로, 8개의 이동수단(Car, Bus, Train, Subway, Walk, Run, Bike, Still)으로 라벨링 되어있다.

본 연구에서는 SHL Dataset 중 SHL Challange 2018 (SHL Dataset을 주어진 조건 하에서 classification 하는 대회)에서 주어진 dataset을 가지고 classification을 시도했다. 해당 dataset은 Pocket에 위치한 스마트폰으로 수집한 데이터만 사용한다.

DL만 사용하여 classification을 시도했다. 스마트폰 센서 데이터 중 gyroscope와 acceleration 데이터만 사용했고, 해당 데이터에 FFT(Fast Fourier Transform) 연산을 해서 spectrogram으로 변환한 뒤 CNN 모델에 학습시켰다. 8개의 이동수단 class를 전부 사용하지 않고, Car, Bus, Subway 3개의 class만 학습시켰다.

![](images/스크린샷%202022-03-15%20오후%206.16.06.png)

Gyroscope 센서 데이터로 만든 spectrogram과 acceleration 센서 데이터로 만든 spectrogram을 그대로 사용하지 않고, 위 아래로 concatenate 하여 synth spectrogram을 만들어서 CNN 모델에 학습시켰다.

![](images/스크린샷%202022-03-15%20오후%206.19.37.png)

3개의 Conv2D layer와 2개의 Dense layer로 구성된 모델을 사용했다.

학습 후 테스트를 한 결과 93.75%의 정확도를 보였고, 이를 한국형 데이터에 적용하는 것을 시도했다.

### 2. 한국형 데이터 수집
2명의 성인 남성이 6개의 이동수단(Car, Bus, Metro, Still, Walking, Power wheelchair)에 대해, 주머니에 위치한 스마트폰으로 데이터를 수집했다.

![](images/스크린샷%202022-03-15%20오후%206.22.31.png)

### 3. 한국형 데이터 classification
SHL Dataset과 동일한 방식으로 preprocessing을 하고, 동일한 모델 구조를 사용했다.

![](images/스크린샷%202022-03-15%20오후%206.23.14.png)

## 프로젝트 결과 정리 및 의의
영국에서 수집한 SHL Dataset을 기반으로 진행된 이동수단 classification 연구를 한국에서 수집한 한국형 데이터에 적용했다.

또 SHL Dataset에는 없는 이동수단인 전동 휠체어를 추가함으로써, 장애인과 비장애인의 스마트폰 센서 데이터를 통합하여 학습 및 classification 하는 데 성공했다.

이를 통해 추후에 이루어질 장애인 이동성 증진 관련 연구에 기여할 수 있다. 예를 들어 임의의 스마트폰 사용자의 센서 데이터가 주어졌을 때, 해당 연구에서 학습된 모델에 주입하여 classification 하면 사용자의 휠체어 탑승 여부를 알 수 있게 된다. 이 정보와 사용자의 GPS 데이터를 연동시키면 비장애인의 이동성에 제약을 주는 지역(과 그 지역의 특성)에 대해 알 수 있을 것으로 보인다.

## 프로젝트의 한계점과 향후 연구 방향
본 연구에서는 Bus, Car, Metro에서 틀리는 경향이 지속적으로 발생했다. 이는 관련 연구에서도 동일하게 나타나는 경향이므로 추후 연구가 더 필요할 것으로 보인다.

또 일반적으로 이동 시 스마트폰의 위치는 사람마다 각기 다르고, 이동 중에 바뀌기도 한다. 하지만 본 연구에서는 해당 변수를 고려하지 않았으므로 이와 관련된 후속 연구가 필요할 것으로 여겨진다.

## 참고자료
Ito, C., Cao, X., Shuzo, M., Maeda, E.: Application of CNN for human activity recognition with FFT spectrogram of acceleration and gyro sensors. In: UbiComp 2018, pp. 1503–1510. ACM Press (2018)
