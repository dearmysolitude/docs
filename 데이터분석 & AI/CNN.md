![](https://onedrive.live.com/embed?resid=C4F97B3B64AE3E7A%218045&authkey=%21AFoqXtvB4artRPA&width=1036&height=380)
Convolutional Neural Network, 합성곱 신경망. 인간의 시신경 구조를 모방한 기술. 이미지 인식에 많이 사용된다.
- Convolutional Neural Network, 합성곱 신경망
- 인간의 시신경 구조를 모방. 이미지 인식에 많이 사용한다.
- {특징을 추출하는 feature extraction} + {feature extraction을 통과한 이후에 결과값을 도추랗는 classification} 으로 구성
- Feature extraction: Convolution layer와 Pooling layer가 섞여있다.
- Classification: fully-connected layer
![](https://onedrive.live.com/embed?resid=C4F97B3B64AE3E7A%218043&authkey=%21ABvslpmT21Pr310&width=774&height=515)
## Feature Extraction
### Convolution 
![](https://onedrive.live.com/embed?resid=C4F97B3B64AE3E7A%218044&authkey=%21AEd9MfjgrYxRb8w&width=1141&height=380)
- Image에서 물체를 판별 할 때, 다른 위치에 있는 같은 물체를 다른 물체라고 판단. 이러한 문제를 해결해주는 것이 Convolution layer이다.
- Image 크기보다 작은 filter를 사용하여, filter 로 특징을 판별한다는 점을 인지. (filter를 kernel이라고도 함)
- Stride : 얼마 씩 이동하여 합성곱 메트릭스를 만들 것인가?  (그림4) 
- Zero Padding : 빈 테두리에 0을 채워서 처음 size를 유지하게 만듦 (그림5)
- Activate : ReLU사용 : 음수는 0으로 처리 (그림6)
![](https://onedrive.live.com/embed?resid=C4F97B3B64AE3E7A%218046&authkey=%21AB2ZHMuVd-As5Bk&width=996&height=466)
### Pooling Layer

## Classification
