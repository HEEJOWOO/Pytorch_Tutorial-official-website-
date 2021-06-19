# Pytorch_Tutorial-official-website-

## Quick Start
* quick_start.py
  - 데이터 작업을 위한 기본적인 두 가지 요소 : torch.utils.data.DataLoader(Dataset을 반복 가능한 객체로 만듦), torch.utils.data.Dataset(샘플과 정답을 저장)
  - Torch,Text, TorchVision, TorchAudio와 같이 도메인 특화 라이브러리를 데이터셋과 함께 제공
  - TorchVision Dataset은 샘플과 정답을 각각 변경하기 위한 transform과 target_transform의 두 이자를 포함
  - nn.Module(신경망 모듈, 매개변수를 캡슐화 하는 간편한 방법으로, GPU로 이동, 내보내기, 불러오기 등의 작업을 위한 헬퍼를 제공)을 상속받은 클래스를 생성하여 정의, __init__ 함수에서 신경망의 계층을 정의하고 forward에서 신경망에 데이터를 어떻게 전달할지 지정, 가능한 GPU로 신경망을 이동시켜 연산을 가속
  - 각 학습 단계에서 모델은 학습 데이터셋에 대한 예측을 수행하고, 예측 오류를 역전파하여 모델의 매개변수를 조정
  - 모델을 저장하는 일반적인 방법은 내부 상태 사전을 직렬화 하는 것
  
