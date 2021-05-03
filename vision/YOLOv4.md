## 1. Introduction
### 목적
- 빠른 처리 속도의 object detector
- 병렬 계산의 최적화

### 1.1 요약
- 빠르고 강력한 object detection model
  - EfficientDet 보다 2배 빠르고
  - YOLOv3'의 AP 및 FPS에서 10~12% 개선
- state-of-the art methods(Bag-of-Freebies, Bag-of-Specials)의 적용 및 개선

### 1.2 설계
- one-stage-detector: input => backbone => neck => dense prediction
  - input: image, patches, image pyramid
  - backbone: VGG16, ResNet-50, ResNeXt-101, Darknet53
  - neck: FPN, PANet, Bi-FPN
  - dense prediction: RPN, YOLO, SSD, RetinaNet, FCOS
- two-stage-detector: + sparse prediction
  - sparse prediction: Faster R-CNN, R-FCN

## 2. Related work
### 2.1 Object detection model
- modern detector
  - Input: Image, Patches, Image Pyramid
  - Backbones: pre-trained on ImageNet
    - VGG16, ResNet-50, SPineNet, EfficientNet-B0/B7, CSPResNeXt50, CSPDarknet53
  - Neck: backbone과 head 사이에서 feature map을 모으는 레이어
    - Additional blocks: SPP, ASPP, RFB, SAM
    - Path-aggregation blocks: FPN, PAN, NAS-FPN, Fully-connected FPN, BiFPN, ASFF, SFAM 
  - Heads: 클래스 및 bounding boxes 예측
    - Dense Prediction(one-stage)
      - anchor-based: RPN, SSD, YOLO, RetinaNet
      - anchor-free: CornerNet, CenterNet, MatrixNet, FCOS
    - Sparse Prediction(two-stage)
      - anchor-based: Faster R-CNN, R-FCN, Mask R-CNN
      - anchor-free: RepPoints
      
### 2.2 Bag of freebies
- 보통 conventional object detector들은 오프라인에서 학습된다.
- 이 때, 학습 전략을 개선하거나 학습 비용만 증가시켜서 정확도를 개선하는 방식을 bag of freebies라고 함
- 주로 사용하는 것이 data augmentation
  - image classification, object detection에서 좋은 결과를 냄
  - photometric distortion: 밝기, 대조, 색조, 명도, 노이즈 등의 조정
  - geometric distortion: 랜덤 스케일링, 크롭핑, 플립핑, 회전 등
  - 둘 다 pixel-wise으로 원본의 픽셀 정보는 보존된다.
  - 만약 augmentation이 feature map에 적용된다면 DropOut, DropConnect, DropBlock 등의 방법이 있다.
  - MixUp: 두 개의 이미지를 특정 비율로 합치고 그 비율에 따라 label을 조정
  - CutMix: 두 개의 cropped 이미지를 겹치고 겹쳐진 면적에 따라 label을 조정
  - style transfer GAN
- semantic distortion: 데이터셋이 bias를 가지고 있을 때 발생: 클래스마다 가지고 있는 데이터의 불균형
  - hard negative example mining
  - online hard example mining 
  - 하지만 example mining은 one-stage object detector에서는 쓰기 힘들다 (dense prediction architecture)
  - 따라서 focal loss을 제안
- one-hot representation으로는 서로 다른 카테고리 간의 관계를 표현하기 힘들다
  - label smoothing을 위해 label을 정제하는 네트워크를 위한 knowledge distilation
- Objective function of Bounding Box regression
  - 기존에는 MSE나 왼쪽위, 오른쪽아래의 좌표를 이용하는 등의 방식
  - object 자체를 고려하지 못함
  - IoU loss: predicted BBox와 ground-truth BBox가 차지하는 면적을 모두 고려
    - GIoU, DIoU, CIoU

### 2.3 Bag of specials
