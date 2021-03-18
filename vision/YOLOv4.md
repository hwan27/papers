## Introduction
### 목적
- 빠른 처리 속도의 object detector
- 병렬 계산의 최적화
### 요약
- 빠르고 강력한 object detection model
  - 2x faster than EfficientDet
  - 10~12% improved from YOLOv3's AP and FPS
- State-of-the art methods(Bag-of-Freebies, Bag-of-Specials)의 적용 및 개선
### 설계
- one-stage-detector: input => backbone => neck => dense prediction
  - input: image, patches, image pyramid
  - backbone: VGG16, ResNet-50, ResNeXt-101, Darknet53
  - neck: FPN, PANet, Bi-FPN
  - dense prediction: RPN, YOLO, SSD, RetinaNet, FCOS
- two-stage-detector: + sparse prediction
  - sparse prediction: Faster R-CNN, R-FCN
