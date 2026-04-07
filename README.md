# Lidar-based Multi-Object Tracking

基于 nuScenes 风格预处理 LiDAR 检测的多目标跟踪（C++）。使用 **卡尔曼滤波** 做状态估计，**最近邻（Nearest Neighbor）** 做检测–轨迹关联（距离门限约 3 m），用 **MOTA** 与真值对比评估，并在 **BEV（鸟瞰）** 下可视化跟踪结果。评估阶段用匈牙利算法匹配真值与发布轨迹以计算 MOTA。

**LiDAR-based multi-object tracking** on preprocessed nuScenes-style detections: Kalman filtering, NN gating for association, MOTA evaluation, and BEV visualization.

## 跟踪演示 / Demo

![BEV tracking demo](https://raw.githubusercontent.com/allrivertosea/Lidar-based-Multi-Object-Tracking/main/visualization/output_video.gif)


## 功能概览 / Features

- 逐帧读取 `data/` 下的检测与真值，在自车坐标系下跟踪 3D 框（位置、尺寸、朝向等）。
- 跟踪结果写入 `results/<scene>_results_<timestamp>.dat`，程序结束打印 MOTA。

## 依赖 / Dependencies

- **CMake** ≥ 3.16（建议；`CMakeLists.txt` 中 `cmake_minimum_required` 为 2.8，可按环境提高）
- **Eigen** 3.x（开发包，如 `libeigen3-dev`）
- **OpenCV** 4.x（需 `core`、`imgproc`、`imgcodecs`、`videoio`）

测试参考版本：Eigen 3.4.0、OpenCV 4.5.5、CMake 3.16.3。

## 编译 / Build（Linux）

在仓库根目录执行。程序通过相对路径读取 `../data/` 与写入 `../results/`，**请从 `build` 目录运行可执行文件**（与 `main.cc` 中路径一致）。

```bash
mkdir -p build && cd build
cmake ..
make -j$(nproc)
```

若 `results/` 不存在，请先创建，否则写结果会失败。

## 运行 / Usage

```bash
# 在 build 目录下
./lidar_mot [scene_name]
```

- **scene_name**（可选）：与 `data/<scene_name>_lidar_detections.dat`、`<scene_name>_gt_tracks.dat` 前缀一致。未传参时默认为 `scene-0061`。
- 本仓库自带示例数据前缀为 **`scene-0103`**，可运行：`./lidar_mot scene-0103`。

## 数据布局 / Data layout

```
data/
  <scene>_lidar_detections.dat   # 逐帧 LiDAR 检测与自车位姿
  <scene>_gt_tracks.dat          # 逐帧真值轨迹（用于 MOTA）
results/
  <scene>_results_<timestamp>.dat
```
