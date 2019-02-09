
# 목차
1. CNN
    1.1. 개발배경
    1.2. CNN 목적
    1.3. CNN 주요내용
    1.4. CNN 구조
    1.5. 예제 코드 리뷰
    
2. R-CNN
    2.1. 개발배경
    2.2. R-CNN 목적
    2.3. R-CNN 주요내용
    2.4. R-CNN 구조
    2.5. 예제 코드 리뷰

3. 결론
 3.1. CNN vs R-CNN

# 1. CNN
## 1.1. 개발배경
 1.1.1. 기존 기법(Multi-Layer Neural Network, MLNN) 문제점
  - 데이터 형상 무시: 3차원 데이터 -> 1차원 데이터로 바꿔 입력
  - 실용성 X: 변수개수, 네트워크 크기, 학습시간
 
 1.1.2 고양이 시각 실험을 통한 통찰
  - 시야 일부 범위 내 시각자극에만 뉴런 반응
  - 일부 뉴런은 수직선 이미지에만 반응, 또 일부 뉴런 다른 각도의 선에 반응, 다른 일부 뉴런은 저수준 패턴 조합된 복잡한 패턴에 반응 ==> 고수준 뉴런이 이웃한 저수준 뉴런 출력에 기반한다는 아이디어 도출

## 1.2. CNN 목표
 - 방대한 양의 입력학습데이터로부터 강인한 특성추출 수행
 - 추출된 특성기반 객체분류(Object classification) 수행

## 1.3. CNN 주요내용
 1.3.1 CNN 성질
  - 학습패턴의 평행이동 불변성: 학습된 패턴이라면 새로운 위치에 나타나더라도 이 패턴 인식 가능
  - 공간적 계층구조학습 가능: 높은 단계 합성곱 층일 수록 고수준 패턴학습을 하도록 CNN은 설계되어 있음. 이 방식을 통해 복잡하고 추상적인 시각적 개념을 효과적 학습 
   - 예) 첫번째 합성곱층은 에지같은 작은 지역패턴 학습, 두번째 합성곱층은 첫번째 합성공층으로부터 학습된 특성들의 조합으로 더 복잡한 패턴학습.

 1.3.2. 합성곱층(Convolution Layer)
  - 합성곱 연산을 하는 단층 신경망
   ![9989933E5BC97E652B.gif](attachment:9989933E5BC97E652B.gif)
  - Input: 이미지
  - Process: 이미지 내 작은 패치 추출 -> 합성곱연산: 추출패치 * 필터(필터의 파라미터=가중치)
  - Output: 특성맵(Feature map)
  ![cnn.gif](attachment:cnn.gif)
  - 패딩: 합성곱 연산 수행 전 입력데이터 주변을 특정값으로 채워 늘림
  - 스트라이드: 필터가 이동할 간격
 1.3.3. 풀링 계층 (Pooling Layer)
  - 목적: 특성맵 크기 강제 다운샘플링을 통해 처리할 특성맵 가중치 개수 감소
  - 풀링하지 않을 경우, 마지막 합성곱 층의 특성이 전체 입력에 대한 정보를 가지고 있지않을 수 있음
  - 풀링하지 않을 경우, 최종 특성맵의 가중치가 과도하게 많아질 수 있어 과대적합 우려 

## 1.4. CNN 구조

![990A613B5BC97E7D1B.png](attachment:990A613B5BC97E7D1B.png)

![%EC%BA%A1%EC%B2%98.PNG](attachment:%EC%BA%A1%EC%B2%98.PNG)

## 1.5. 예제 코드 리뷰

# 2. R-CNN (Regional CNN)
## 2.1. 개발배경
- CNN주요목적인 Object classification에서 Object detection으로 확장
![classifi_detection.jpg](attachment:classifi_detection.jpg)

- Object detection 위해선 Object간 경계분별능력(서로간 관계식별능력) 필요
- "어떻게 하면 CNN 기반으로 복잡한 이미지(다수 객체 등)의 경계분별을 가능케 할까?"
- = "CNN 연구로 어느 정도까지 Object detection을 일반화 시킬 수 있을까?"

## 2.2. R-CNN 목표
- 하나의 이미지에서 주요 객체들을 박스(Bounding box)로 표현하여 정확히 식별하는 것
- Input: 이미지
- Output: 박스표시영역(Bounding box) + 각 객체 레이블링(Class label)

## 2.3. R-CNN 주요내용
2.3.1. 박스위치 탐색
 - 무수히 많은 박스 생성 후 그 중 객체에 해당하는 것을 찾는 방식
 - Selective search: [(http://www.cs.cornell.edu/courses/cs7670/2014sp/slides/VisionSeminar14.pdf) 내용 요약 필요]

2.3.2. Feature 추출
 - 생성된 박스영역을 표준화된 크기로 재구성 후, 학습된 CNN모델(AlexNet)통과시켜 이미지의 Feature 추출

2.3.3. 객체 분류
 - R-CNN 모델 마지막 층에 서포트 벡터 머신(SVM)이용하여 객체 분류
 - 후에 SVM[추가설명필요] -> Softmax[추가설명필요]

2.3.4. 박스위치 탐색 개선
 - 실제 객체에 딱 맞춘 박스영역 탐색을 위해 linear regression이용
 - Input: 객체에 해당하는 이미지 부분(sub-region)
 - Output: sub-region에 맞는 새로운 bounding box 좌표들

2.3.5. 요약
 - 이미지 내 인식객체에 대한 Bounding boxes세트 생성
 - Pre-trained AlexNet에 각각의 box 통과, SVM통해 객체분류
 - 객체 인식 완료된 후 Linear regression을 통해 Box 위치정확성 개선

## 2.4. R-CNN 구조
![R_CNN.PNG](attachment:R_CNN.PNG)

## 2.5. 예제 코드 리뷰

# 3. 결론
## 3.1. CNN vs R-CNN
