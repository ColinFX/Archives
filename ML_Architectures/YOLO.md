# YOLO 实时目标检测

## 目标检测

计算机视觉的三大任务，由 1 到 3 依次从易到难：

1. 图像分类：将整张图片作为一个整体，为图片贴上标签。是一个最基础的分类器。

2. 目标检测：在一张图像中检测物体。

3. 语义分割：将一张图像在像素级别上分割成多个代表不同物体的区域。

<img src="https://user-images.githubusercontent.com/45534476/170124075-973581ae-87e1-4205-93e4-ab057ed543fe.png" with="500">

目标检测算法的两大分类：

1. 1-stage：单个 CNN 网络直接预测物体的类别和位置，如 Yolo，SSD 等。运算速度更快，准确性更低。

2. 2-stage：R-CNN 系，先用 selective search 或者 CNN 网络产生 region proposal，然后对每一个 region 进行分类。

<img src="https://user-images.githubusercontent.com/45534476/170123841-f73d63df-0868-40ae-b63c-90f8e611949f.png" width="500">

## 滑窗和 CNN

基本原理：采用不同大小和比例的“滑动窗口”在整张图片上以一定的步长进行滑动，然后对每一次局部图片做图像分类。

CNN 中的卷积层能够实现所谓“滑动窗口”的效果。

## YOLO V1

只需要浏览一次就可以识别出图中的物体的类别和位置。

图片中的一个物体需要包含五个参数信息：物体中心位置 $(x,y)$，物体框的长宽 $(h,w)$，物体的类别。

### STEP 1. 分割单元框

假设输入图片为正方形，YOLO 先将一张图片平均分割为 $s \times s$ 个单元框。

YOLO 不要求一个物体完全出于一个单元框内，而只要求该物体的中心在这个单元框中。这与最简单的滑框法有所不同。

### STEP 2. 预测 Bounding box

YOLO 让每一个单元框都预测出 $B$ 个 bounding box，每个 bounding box 包含五个参数：$(x,y),(h,w)$，和预测的置信度。

每个 bounding box 用于锁定物体的位置。









