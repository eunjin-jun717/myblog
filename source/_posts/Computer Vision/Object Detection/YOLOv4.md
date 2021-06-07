---
title: "[Paper Review] YOLOv4"
date: 2021-06-02
tags: DL
---

*references*
논문 리뷰에 도움을 준 논문과 자료들입니다. 감사합니다🙏
- [https://arxiv.org/abs/2004.10934](https://arxiv.org/abs/2004.10934)
- [https://keyog.tistory.com/31](https://keyog.tistory.com/31)

# [Paper Review] *YOLOv4 - Optimal Speed and Accuracy of Object Detection*
논문 리뷰할 알고리즘 소개 전에 먼저 사이트 하나 소개해드리겠습니다.
딥러닝의 각 분야들 중에서 SOTA 알고리즘들의 순위를 알려주는 사이트가 있어 아래에 링크 걸어놓겠습니다👇
- [https://paperswithcode.com/sota](https://paperswithcode.com/sota)
![](/hueman_images/dl/sota.png)

제가 리뷰해 볼 논문은 Object Detection의 알고리즘 중 "YOLOv4" 입니다. 객체 검출에 있어서 높은 정확도와 높은 FPS(Frame Per Second)를 가진 알고리즘 입니다. 최근에는 YOLOv5까지 나와서 kaggle 대회의 객체 검출하는 곳에서 많이 쓰인다고 합니다. 추후에 kaggle 대회에서 YOLOv5를 이용해 object detection하는 내용을 블로그에 올리도록 하겠습니다.

본격적으로 YOLOv4의 논문리뷰를 보시기 전에 YOLO의 이전 버전들이 궁금하시다면 아래의 링크에서 YOLO, YOLOv2 그리고 YOLOv3 알고리즘들에 대해 간단히 살펴보실 수 있습니다. 
- [https://eunjin-jun717.github.io/2021/06/01/Computer%20Vision/Object%20Detection/yolo/](https://eunjin-jun717.github.io/2021/06/01/Computer%20Vision/Object%20Detection/yolo/)









# 1. Introduction
- 최신 신경망은 실시간으로 작동하지 않고, 큰 mini-batch-size로 훈련하기 위해서 많은 수의 GPU를 요구합니다. 
- 따라서 이러한 문제를 해결하기 위해 기존 GPU에서 실시간으로 작동하는 CNN을 생성하는 것이 이 논문의 목적입니다. 훈련에는 단일 GPU만 필요로 합니다. 
![](/hueman_images/dl/figure1.png)
- x축: FPS(Frame Per Second) // 1초 당 몇개의 frame이 우리에게 보여지는지 
- y축: AP(Average Precision) // 정밀도(정확도)의 평균
- 두개의 지표는 높을 수록 좋다.


## 1-1. 이 논문의 주요 목표
- BFLOP(연산량)의 숫자를 줄인다기 보다는 제품 시스템에서 빠른 작동속도를 가지면서 병렬적으로 최적화된 계산이 가능하게 하는 것이다. 
- 따라서 저자는 YOLOv4를 사용하면 누구나 기존의 GPU를 가지고 위와 같은 결과를 실시간 환경에서 얻을 수 있다고 말합니다. 


## 1-2. YOLOv4의 contributions
1. 효율적이고 강력한 object detection을 1080Ti, 2080Ti 같은 환경에서도 빠르고 정확하게 훈련시킬 수 있다.
2. Detector의 훈련과정에서 bag-of-freebies 와 bag-of-specials methods의 영향을 검증한다.
3. 최신식의 method들을 수정하고 만들면서 single GPU에서의 훈련도 더 효율적이고 적합하게한다. 이때, CBN, PAN, SAM과 같은 기술들이 포함된다. 


# 2. Related Work
![](/hueman_images/dl/figure2.png)



## 2-1. Backbone
- Upper layers에 해당하는 pre-trained network이다. 
> GPU flatform의 backbone: VGG, ResNet, ResNeXt, DenseNet 등\
> CPU flatform의 backbone: SqueezeNet, MobilNet, ShuffleNet 등 

## 2-2. Head
- 2-stage와 1-stage로 나뉨
> Ex) 2-stage: R-CNN, R-FCN, Libar R-CNN 등 // anchor free 2-stage : RepPoints\
> Ex) 1-stage: YOLO,SSD,RetinaNet // anchor-free: CentorNet, CornerNet, FCOS 등


## 2-3. Neck
- Neck은 backbone과 head 사이에 삽입되는 layer이며, 여러 단계에서 feature map을 수집한다. 
> Neck: FPN, PAN, BiFPN, NAS-FPN 등이 있다. 

## 2-4. Bag-of-Freebies
- 오프라인 환경에서 훈련되는 모델들은 훈련방법만 변경하거나 훈련 비용만 증가시켜 정확도를 높이는 방법이다. 
- data augmentation도 있다. (데이터 증강 기법)
- 목적
> input 이미지의 가변성을 증가시키고, 설계된 모델이 다양한 이미지에 대해 높은 견고성을 갖게 하는 것
- 예로는 광도 왜곡(밝기, 색조, 채도, 노이즈추가 등), 기하 왜곡(크기 변화,crop, flip, rotate)
- 객체가 가려지는 이슈에 중점을 두며, classify나 detection에서 우수한 결과를 얻었다고 한다.
- Random 삭제나 cutout 기법은 사각형 영역을 임의로 선택하고 무작위로 0 또는 상보값을 채울 수 있다.
- 이러한 방법들 외에도 stype transfer GAN을 통한 데이터 증강으로 texture bias를 훈련과정에서 줄여 줄 수 있다. 
- 마지막 BoF는 Bbox(bounding box)를 regression할 때 사용되는 함수들인데, 논문에는 IOU, GIOU,DIOU,CIOU 에 대해 얘기한다. 

## 2-5. Bag-of-Specials
- 플러그인 모듈과 post-processing을 통해 inference cost를 약간 증가시키지만 정확도를 크게 향상시킬 수 있는 방법을 bag of specials라고 부른다.
- 이 플러그인은 특정 속성을 강화하기 위한 것이며, 그것들은 receptive field 확대, attention mechanism 도입 또는 feature integreation capability 강화 등이다. 
- Receptive field 강화에 사용되는 모듈로는 SPP(Spacial Pyramid pooling),ASPP, RFP(receptive field block)가 있다. 
- Attention mechanism에는 기존에 사용되던 SAM, FPN 이 있으며 SFAM, ASFF, BiFPN이 있따.
- 활성화 함수의 변경 또한 이 분야의 연구에서 활발하게 진행되며, 기존 가장 많이 사용되는 ReLU 부터 LReLU, PReLU, ReLU6, SELU, Swish, hard-Swish, Mish 등이 있다. 
- 마지막으로, post-processing 방법으로는 NMS가 있다.

# 3. Methodology(방법론)
- YOLOv4의 기본적인 목표는 이론적 지표인 BFLOPS를 줄이는 것이 아니라 시스템 내에서 병렬계산을 위한 최적화와 신경망의 빠른 작동속도를 가지게 하는 것이다. 

## 3-1. Selection of Architecture
- Receptive field를 강화시키는 모듈을 선택하는 것이 우리가 할 일이다.\
1) 네트워크의 input 사이즈를 높게 가져갈 수 있는 것(작은 객체를 감지하기 위해)\
2) layer증가 가능 여부(receptive field에서 네트워크의 input 사이즈가 증가되기 위해)\
3) 파라미터 증가 가능 여부(단일 이미지에서 크기가 다른 여러 객체를 감지할 수 있기 위해)
- YOLOv4는 FPN대신 PANet을 사용하여 backbone level에서 다양한 detector level로 집계 시킬 수 있게 한다. 
> 결과적으로, YOLOv4는 CSPdarknet53을 backbone으로 SPP block을 추가했으며, detection neck으로 PANet를 사용하고, detection head에서는 YOLOv3의 아키텍쳐를 따른다. 
- YOLOv4는 cross-GPU batch normalization 과 같은 비싼 특수장치를 사용하지 않고 1080Ti와 2080Ti에서 연구되었다고 한다. 

## 3-2. Selection of BoF and BoS
- 이제 detector의 훈련 성능을 증가 시키기 위해 BoF와 BoS를 선택해야하는데 위에서 언급한 활성함수들 중에서 PReLU와 SeLU는 훈련하기 어렵고 ReLU6는 양자화 네트워크를 위한 활성화 함수이므로 후보에서 제거하였다. 
- Drop Block은 이미 많은 정규화 방법 중 많은 승리를 거두어서 drop block을 사용하는 것을 망설이지 않았다고 한다. 
> (drop block은 drop out과 같은 방식인데, drop out이 일정 feature들을 훈련 시 빼고 훈련하는 반면, drop block은 이름 그대로 feature의 일정 범위를 빼고 훈련한다고 생각하면 됨)

## 3-3. Additional Improvements
![](/hueman_images/dl/figure3.png)
- 여러가지 기능을 추가하여 single GPU에서 적합하게 설계했다고 한다. 
> 1) 데이터 증강: mosaic 기법, SAT(self adversarial training)
> 2) Genetic 알고리즘 적용 및 최적의 하이퍼 파라미터 선정
> 3) 기존의 method를 효율적으로 사용 가능하게 수정: SAM 수정, PAN 수정, CmBN도입
- Cutmix를 이용하여, 2가지의 이미지 input을 외부의 객체를 감지한다. 
- 추가로, mosaic 방식을 이용하여 4가지 다른 이미지 input을 가져가지 때문에 mini-batch 의 크기를 크게 가져갈 필요가 없어진다고 한다. 
- SAT 방식은 2개 단계의 forward pass와 backward pass가 일어나게 되는데 첫번째 단계에서는 가중치를 가져가는 것 대신에 원본 이미지를 변경한다. (이 작업은 원본 이미지에 객체가 없다는 속임수를 만든다) 두번째 단계에서는 1단계에서 수정된 이미지를 detection하는 훈련을 한다. (이 과정은 propagation 과정에서 데이터를 증강 시킬 수 있다.)
![](/hueman_images/dl/figure4.png)
- CmBN은 CBN의 수정된 version이라고 한다. 가중치들의 statistic을 수집하는 과정이 단일 배치내에서만 일어난다는 것이다. 
![](/hueman_images/dl/figure6.png)
- SAM 구조를 수정하여 사용했다. 공간을 attention하는 SAM을 point를 attention할 수 있게 하고 shortcut connection을 사용하는 PAN을 concatenation(연결)로 변경했다고 한다. 



## 3-4. YOLOv4
- Backbone: CSPDarknet53
- Neck: SPP,PAN
- Head: YOLOv3 
- BoF for backbone: Cutmix, Mosaic, DropBlock, Class Label smoothing
- BoS for backbone: Mish 활성화 함수, CSP, Multi-input weighted residual connections
- BoF for detector: CIOU-Loss, CmBN, DropBlock, Mosiac, SAT, Grid 감도 제거, 하나의 ground truth에 multi anchor 사용, cosine annealing scheduler, 하이퍼파라미터 최적화
- BoS for detector: mish활성화 함수, SPP, SAM, PAN, DIOU-NMS
> 이전의 object detector의 알고리즘들 중에서 가장 좋은 기법들을 사용해보고 아닌 것은 빼고 좋은 것은 다 넣은 후 최적화를 시켰다. 



# 4. Experiments
- 간단하게 어떤 실험을 통해서 어떤 결과들이 나왔는지 확인해볼 것이다. 
![](/hueman_images/dl/fig7.png)
> 위의 그림들은 데이터 증강 기법들이다. 


## 4-1. Influence Of different features on **Classifier** training
![](/hueman_images/dl/table2.png)
> CSPReNeXt-50 모델에 적용한 결과 Cutmix, Mosaic/ Class Label smoothing/ Mish activation 을 사용 시 정확도가 향상된 것을 확인할 수 있다. 

![](/hueman_images/dl/table3.png)
> CSPDarknet-53 모델에 적용 결과 역시 위의 기법들의 사용과 동일하게 정확도 향상에 유리했다. 

## 4-2. Influence Of different features on **Detector** training
![](/hueman_images/dl/table4.png)
> loss를 MSE를 기준으로 봤을 때 M, GA, CBN, CA 등을 포함한 것이 정확도 향상에 유리하였다.\
> loss를 GIoU, DIoU, CIoU로 변화시키면서 S, M, IT, GA를 적용하고 CIoU를 사용하는 것이 정확도 향상에 유리하였다. 

![](/hueman_images/dl/table5.png)
> BoS는 PAN, SPP, SAM 을 함께 적용한 경우 가장 우수한 정확도를 가졌다. 

## 4-3. Influence of different backbones and pre-trained weightings on detector training
![](/hueman_images/dl/table6.png)
> BoF와 Mish를 사용한 CSPDarknet53이 우수한 성능을 보였다. 


## 4-4. Influence of different mini-batch size on Detector training
![](/hueman_images/dl/table7.png)
> CSPResNeXt50-PANet-SPP, CSPDarknet53-PANet-SPP 두 모델 모두 mini-batch size의 변경 및 BoF/BoS 추가 유무는 정확도에 거의 영향을 주지 않았다. 

# 5. Results
- M(axwell), P(ascal), V(olta) GPU에서의 여러 detector들의 AP, FPS를 측정한 뒤 비교한 표를 제시하였다. GPU가 나온 시기 순서에서 비교적 가장 최근에 나온 GPU인 Volta에서의 YOLOv4 성능이 가장 좋은 것을 알 수 있으며, 다른 detector들에 비해서도 상당히 뛰어난 성능을 보이는 것을 알 수 있다.
> M: GTX Titan X or Tesla M40\
> P: Titan X or Titan Xp or GTX 1080Ti or Tesla P100\
> V: Titan Volta or Tesla V100

![](/hueman_images/dl/fig8-1.png)
> Maxwell GPU에서의 결과 

![](/hueman_images/dl/fig8-2.png)
> Pascal GPU에서의 결과 

![](/hueman_images/dl/fig8-3.png)
> Volta GPU에서의 결과 

![](/hueman_images/dl/table8.png)
> Maxwell GPU를 사용하며 MS COCO dataset에서 서로 다른 object detector들에 대한 속도와 정확도 비교 

![](/hueman_images/dl/table9.png)
> Pascal GPU를 사용하며 MS COCO dataset에서 서로 다른 object detector들에 대한 속도와 정확도 비교

![](/hueman_images/dl/table10.png)
> Volta GPU를 사용하며 MS COCO dataset에서 서로 다른 object detector들에 대한 속도와 정확도 비교



# 6. Conclusions
1) 사용 가능한 모든 대안 감지기들보다 빠르고(FPS) 정확한(MS COCO AP50...95, AP50) 최신의 감지기를 제안
2) YOLOv4는 8~16 GB의 VRAM으로 기존의 GPU에서 training 및 사용이 가능하므로, 광범위한 적용이 가능
3) Classifier과 Detector 모두의 정확도 개선을 위해 많은 이미지들을 검증하여 선택
4) 이러한 이미지들은 향후 연구 개발을 위한 자료로 사용 가능 
