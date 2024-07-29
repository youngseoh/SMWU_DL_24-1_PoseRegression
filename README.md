## Project 소개

Photogrammetric Reality Capture Method, ‘Reality Capture’ 을 활용하여,

‘숙명여자대학교 르네상스관 4층’ 에서의 장소에 대한 데이터셋을 생성하고,

 해당 장소 이미지에 대한 camera pose regression 을 수행

Link :

[24-1 딥러닝 기말 프로젝트 Team1 link](https://www.notion.so/24-1-Team1-675184762b8148079afe75fe4f64b623?pvs=21)

Slide : 

[24-1 딥러닝 기말 프로젝트 Team1 slide](https://drive.google.com/file/d/1WEBWY7Wfyl6kmHr4x7sCz4m2rTl4sbMZ/view?usp=drive_link)

## Contributors

숙명여자대학교 김희경(통계학과 19), 고나경(통계학과 20), 황영서(인공지능공학부 20)

## Reference Paper

Paper : 

[arxiv.org](https://arxiv.org/pdf/1505.07427)

Paper Review :  

[[Paper Review] PoseNet: A Convolutional Network for Real-Time 6-DOF Camera Relocalization](https://velog.io/@youngseoh6/Paper-Review-PoseNet-A-Convolutional-Network-for-Real-Time-6-DOF-Camera-Relocalization)

## Image Data 수집

1. 숙명여자대학교 르네상스관 4층 복도 전체를 11분 영상으로 찍어서, 'reality capture'를 활용하여 약 1200장의 유의미한 이미지를 얻음
    
   ![posenet1](https://github.com/youngseoh/SMWU_DL_24-1_PoseRegression/assets/100707876/580b33ad-c357-4272-88e3-435e3bc8a23d)
    
2. 교수님 이미지 데이터 약 30장을 우리 map에 추가 registration 해줌
3. 교수님이 만드신 dataset 과 우리가 만든 dataset 의 axis 와 scale 가 다르기 때문에, 이를 맞추기 위해서 transformation을 해줌

![posenet2](https://github.com/youngseoh/SMWU_DL_24-1_PoseRegression/assets/100707876/d755b0c0-7968-4728-8945-6a3499863074)

## Experiments

기존의 PoseNet 은 CSAILVision의 Places365 로 pretrain 된 GoogleNet 을 backbone으로 활용하여 camera pose (x,y,z) quaternion(p,q,rs) 을 출력함

-> 약간의 불안정한 학습으로 인해서 여러가지 실험을 해보도록 함

1. PoseNet backbone 바꾸기
GoogleNet 보다 성능이 좋다고 알려진 ResNet18 을 활용함
- GoogleNet 의 inceptionblock 을 사용하여 regression head 를 구성하는 posenet 구조에서, 해당 부분을 제거하고 ResNet18의 residual block으로 대체하여 모델을 구성함
- CSAILVision의 Places365 로 pretrain 된 ResNet50의 가중치를 사용하여 ResNet 18 가중치 초기화
    
    -> GoogleNet 대신 ResNet18을 사용햇을떄, error 가 비교적 안정적이고 지속적으로 낮은값 유지하지만, 여전히 불안정한 학습
    
2. lerarning rate 변경
- stepLR 도입 ( 현재 epoch의 1/3 일 때, learning rate 0.5 배씩 줄임 )
- stepLR 도입 ( 현재 epoch의 1/3 일 때, learning rate 0.5 배씩 줄임 )
    
    -> StepLR 을 도입하여 안정적인 학습을 하려고 했지만, 오히려 불안정한 학습을 보여 learning rate 를 일정하게 유지하기로함
    
3. batch size 변경
    
    → 안정적인 학습을 위해 batchsize 를 64->128 로 키웠지만 성능이 비슷하여 train 속도를 고려하여 128로 조정하기로함
    

## Results

![posemet3](https://github.com/youngseoh/SMWU_DL_24-1_PoseRegression/assets/100707876/fd3fe89e-a144-4109-8f9a-a9b2f54276dd)
