# Student-ID-Recognition-Project

## Project Experience


## 1. 이미지 불러오기 – 이미지가 잘 불러와지나 테스트
#### 출력 결과 : 원본 컬러 이미지 출력

![image](https://github.com/Lee-ChangHo/Student-ID-Recognition-Project/assets/110157972/5bf46a5a-bf64-4371-b1b5-5a331bb33ce9)

## 2. 이미지 흑백으로 변경 – 이미지 인식을 더 잘 되게 하기 위해 채널을 하나로 줄이는 과정

#### 출력 결과 : Grayscale 된 흑백 이미지 출력, 채널 0에 대해 명암 값에 따른 픽셀 수의 분포 그래프 궁금해서 출력해봄 


## 3. 노이즈 제거 유무에 따라 어떤 결과 값이 나오는지 비교하기 위해 가우시안 블러 적용 (커널사이즈 = (7,7) 사용) - 이미지의 색수차를 줄여 색상간의 경계선을 흐릿하게 만들어 이미지를 부드럽게 만들어 노이즈 제거하기 위함

#### 출력 결과 : 이미지 스무딩이 성공적으로 되어 전체적으로 흐릿하게 보이는 흑백 이미지 출력


## 4. 배경과 객체를 구분하고 관심 영역과 비관심 영역을 구분하기 위해 노이즈 제거를 한 이미지, 하지 않은 이미지 둘 다 이진화.
이 때 전체 이미지를 기준으로 적정 임계치를 찾는 Otsu Thresh를 이용해 이미지 이진화함.

#### 출력 결과 : 왼쪽이 흑백이미지를 바로 이진화 처리한 결과
오른쪽은 가우시안 블러를 적용하여 노이즈를 제거한 후 이진화 처리를 한 결과

가우시안 블러 처리한 사진이 노이즈제거가 잘 되었지만 글씨가 뭉개진 걸 관찰 할 수 있음.
결국 최종적으로 학번을 인식하는 게 목표인데 글씨가 뭉개져 버린 걸 사용하면 학번 인식이 원활하게 되지 않을 거 같아 가우시안 블러 처리 하지 않은 이미지를 사용할 것임.


















## 5. 대략적으로 학생증만 화면에 따기 위해 (여백 제거를 위해) 이진화 된 이미지의 x축, y축에 각각에 대해 프로젝션을 실행해 y축, x축에 대한 픽셀값들의 히스토그램 시각화. 

#### 출력 결과 : 이미지를 x축, y축 각각에 대해 프로젝션 한 결과










## 6. 대략적인 학생증의 위치를 추출하기 위해 x축, y축에 대해 프로젝션한 결과를 더해 공통된 부분만 추출

#### 출력 결과 : x축, y축 프로젝션들의 공통 부분이 출력 된 것을 관찰
















## 7. 대략적인 학생증의 위치를 파악하기 위해 일정 픽셀값을 초과하는 영역의 범위를 찾고 그 범위에 맞게 이미지를 자른 후 역투영
노이즈 값들을 대략적으로 n(2000)개라 설정 후 n(2000)개 이상인 영역을 잘라내기.

### 첫 번째. y축 프로젝션 값을 이용해 x축 범위 잘라내기
### 두 번째. x축 프로젝션 값을 이용해 y축 범위 잘라내기
### 세 번째. x축 프로젝션 + y축 프로젝션 두 값을 더한 걸로 학생증만 대강 잘라내기
출력 결과 : 학생증의 대략적인 위치를 기준으로 여백이 잘 날라간 걸 확인할 수 있음.

### 첫 번째
### 두 번째
### 세 번째.

## 8. 직사각형 모양의 학생증을 정방향으로 회전시키기 전에 테두리를 먼저 따기

#### 출력 결과 : cv2.RETR_EXTERNAL 함수를 사용해서 최외곽 엣지를 찾고 엣지를 근사화 한 후 일정 필셀값보다 낮은 엣지는 제거하고 네 개의 선이 이어져있는 테두리만 남기는 형식으로 찾음










## 9. 여백을 제거하고 테두리를 딴 이미지를 학생증을 기준으로 정방향으로 회전시키기
 
### 9-1. 시계방향, 반시계방향으로 각각 1도씩 15도, 15도 돌리며 그때 마다 이미지를 x축 프로젝션을 함.
### 9-2. x축 프로젝션을 할때마다 y축의 픽셀값들의 합이 가장 긴 부분들을 모두 저장
### 9-3. -15도,  +15도해서 총 30도를 회전 할 동안 출력 된 2번 값들을 비교해 가장 긴 부분이 정방향이라고 가정
### 9-4. 3번에서 나온 값을 토대로 이미지 역투영

#### 시계 방향 회전 예시  


#### 반시계 방향 회전 예시  


#### 정방향이라 가정한 부분		


#### 정방향 이미지의 역투영

## 10. 올바르게 회전 된 학생증의 네 꼭지점을 바탕으로 이미지를 정확히 펴기 

#### 출력 결과 : 좌측 하단, 좌측 상단, 우측 상단, 우측 하단 순서로 네 꼭지점을 입력 후 원근 변환(Perspective Transform) 함수를 이용해 이미지를 평평하게 펴기단, 이때 src 초기 입력값이 총 세가지 경우로 나뉘는데 각 경우에 따라 좌표값을 다르게 잡아야함. 따라서 세가지 경우에따라 나눴고 좌표값도 각각 다르게 설정함.

### 1번 경우 : src 초기값이 반시계방향으로 회전된 학생증 일 경우.
### 2번 경우 : src 초기값이 시계방향으로 회전된 학생증 일 경우.
### 3번 경우 : src 초기값이 정방향인 학생증 일 경우.

### 1번. (왼쪽 : src, 오른쪽 : 원근 변환으로 평평하게 편 이미지)


### 2번. (왼쪽 : src, 오른쪽 : 원근 변환으로 평평하게 편 이미지)


### 3번. (왼쪽 : src, 오른쪽 : 원근 변환으로 평평하게 편 이미지)

## 11. 확대된 학생증에서 학번을 뽑아내기위해 흑백으로 변환 및 이진화

#### 출력 결과 : 별 다른 문제 없이 이진화 성공


## 12. 올바른 방법으로 평평하게 펴진 학생증에서 학번의 대략적인 위치는 모두 비슷할테니 학번이 있는 지점에 관심영역(ROI)을 네모칸으로 만들어서 그 네모칸의 각각의 꼭지점을 기준으로 학번을 제외한 나머지 정보를 모두 날린다.

#### 출력 결과 : 학번만 남아있는 모습 관찰 가능


## 13. 다른 학생증으로 동일한 방법을 사용해 진행 시켜 본 결과

#### 출력 결과 : 학번까지 잘 출력된 것을 확인 함.

### 첫 번째 : 원본
### 두 번째 : 학생증만 평평하게 출력
### 세 번째 : 학번만 평평하게 출력

### 첫 번째


### 두 번째


### 세 번째


## 14. tesseract-OCR을 이용해서 뽑아낸 학번을 인식
#### 출력결과 : 인식 성공


## 15. 다른 학생증으로도 학번 인식해보기
#### 출력결과 : 인식 성공
