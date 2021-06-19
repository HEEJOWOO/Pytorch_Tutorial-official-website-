# Pytorch_Tutorial-official-website-

## Quick Start
* quick_start.py
  - 데이터 작업을 위한 기본적인 두 가지 요소 : torch.utils.data.DataLoader(Dataset을 반복 가능한 객체로 만듦), torch.utils.data.Dataset(샘플과 정답을 저장)
  - Torch,Text, TorchVision, TorchAudio와 같이 도메인 특화 라이브러리를 데이터셋과 함께 제공
  - TorchVision Dataset은 샘플과 정답을 각각 변경하기 위한 transform과 target_transform의 두 이자를 포함
  - nn.Module(신경망 모듈, 매개변수를 캡슐화 하는 간편한 방법으로, GPU로 이동, 내보내기, 불러오기 등의 작업을 위한 헬퍼를 제공)을 상속받은 클래스를 생성하여 정의, __init__ 함수에서 신경망의 계층을 정의하고 forward에서 신경망에 데이터를 어떻게 전달할지 지정, 가능한 GPU로 신경망을 이동시켜 연산을 가속
  - 각 학습 단계에서 모델은 학습 데이터셋에 대한 예측을 수행하고, 예측 오류를 역전파하여 모델의 매개변수를 조정
  - 모델을 저장하는 일반적인 방법은 내부 상태 사전을 직렬화 하는 것

## Tensor
* tensor.py
  - 텐서는 배열이나 행렬과 매우 유사하며 특수한 자료구조, Pytorch에서는 텐서를 사용해 모델의 입력과 출력, 모델의 매개변수들을 부호화(encode)함
  - 텐서는 GPU나 다른 하드웨어 가속기에서 실행할 수 있다는 점만 제외하면 Numpyu의 ndarray와 유사
  - 텐서의 초기화는 데이터로 부터 직접 생성, numpy 배열로부터 생성, 다른 텐서로부터 생성
  - numpy배열과는 다르게 텐서는 cpu, gpu에서 사용 가능하며 gpu가 사용 가능하면 gpu로 옮겨 고속 연산을 하는게 유리
  - numpy의 배열 변경 사항이 tensor에도 변경 되면, tensor의 변경 사항이 numpy의 배열에도 변경됨
  
## dataset & dataloader
* data_tutorial.py
  - torch.utils.data.DataLoader : Dataset을 샘플에 쉽게 접근할 수 있도록 반복 가능한 객체로 만듦
  - torch.utils.data.Dataset : 샘플과 정답을 저장
  - Pytorch에서는 미리 준비해준 데이터셋을 제공해줌
  - datasets에서 root는 "학습/데이터가 저장되는 경로"
  - datasets에서 train는 "학습용 또는 테스트용 데이터셋 여부를 지정"
  - datasets에서 download=True는 "root에 데이터가 없는 경우 인터넷에서 다운"
  - datasets에서 transform과 target_transform는 "특징과 정답 변형을 지정"

## data transform
* transform.py
  - pytorch에서 제공해주는 데이터는 항상 머신러닝 알고리즘 학습에 필요한 최적화된 형태로 제공되지 않음 따라서 변형을 해서 데이터를 조작하여 학습에 적합하게 만들어야함
  - TorchVision의 데이터셋들은 특징을 변경하기 위한 transform과 정답을 변경하기 위한 target_transform을 갖음 
  - 사용한 FashionMNIST 특징을 PIL Image 형식이며 정답은 정수 임, 학습을 하려면 정규화된 텐서 형태의 특징과 원핫으로 부호화(encode)된 텐서 형태의 정답이 필요 따라서 변형을 하기 위해 ToTensor와 Lambda를 사용 
  - ToTensor() : PIL image나 Numpy를 FloatTensor로 변환하고, 이미지의 픽셀의 크기 값을 [0.,1.] 범위로 비례하여 조정
  - Lambda : 사용자 정의 람자 함수를 적용, 정수를 우너핫으로 부호화된 테서로 바꾸는 함수를 정의, 먼저 크기 10짜리인 zero tensor를 만들고, scatter_를 호출하여  주어진 정답 y에 해당하는 인덱스에 value=1을 할당 

## Model
* model.py
  - 신경망은 데이터에 대한 연산을 수행하는 계층, 모듈로 구성
  - Pytorch의 모든 모듈은 nn.Module의 하위 클래스
  - 신경망은 다른 모듈로 구성된 모듈, 이러한 중첩된 구조는 복잡한 아키텍처를 쉽게 구축하고 관리할 수 있음
  - 활성화 함수는 입력과 출력 사이에 복잡한 관계를 만듦, 선형 변환 후에 적용되어 비선형성을 도입하고, 신경망이 다양한 현상을 학습 할 수 있도록 도움

## Autograd
* Autograd.py
  - 모델 가중치는 주어진 매개변수에 대한 손실 함수의 변화도(gradient)에 따라 조정됨
  - 이러한 변화도를 계산하기 위해 Pytorch에서는 torch.autograd 라고 불리는 자동 미분 엔진을 지원
  - 가중치와 바이어스는 최적화를 해야하는 매개변수 

## Optimization
- 각 반복 단게에서 모델은 출력을 추측하고, 추측과 정답 사이의 손신을 계산하고 매개변수에 대한 오류의 도함수를 수집한 뒤 경사하강법을 사용하여 이 파라매터들을 최적화 함
  - 하이퍼파라미터 : 모델 최적화 과정을 제어할 수 있는 조절 가능한 매개변수로 에폭: 데이터셋을 반복하는 횟수 / 배치크기 : 각 에폭에서 한 번에 모델에 전달할 데이터 샘플의 수 / 학습율 : 각 배치/에폭에서 모델의 매개변수를 조절하는 비율, 값이 작을수록 학습 속도가 느려지고, 값이 크면 학습 중 예측할 수 없는 동작이 발생할 수 있음
  - 손실 함수는 획득한 결과와 실제 값 사이의 틀린 정도를 측정하여, 학습 중에 이 값을 최소화 하려고 함, 주어진 데이터 샘플을 입력으로 계산한 예측과 정답을 비교하여 손실을 계산
  - Optimizer, 최적화는 각 학습 단계에서 모델의 오류를 줄이기 위해 모델 매개변수를 조정하는 과정, 최적화 알고리즘이란 이 과정이 수행되는 방식을 의미 
  - 학습 단게에서 최적화는 세단계로 구성
    1) optimizer.zero_grad() : 모델 매개변수의 변화도를 재설정, 변화도는 더해지기 때문에 중복 계산을 막기 위해 반복할 때마다 명시적으로 0으로 설정
    2) loss.backward() : 예측손실을 역전파
    3) optimizer.step() : 역전파 단계에서 수집된 변화도로 매개변수를 조정 
