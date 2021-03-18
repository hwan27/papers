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

