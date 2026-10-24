# BJ-Crown-Dataset
Individual Tree Crown Delineation Dataset (BJ Crown)

# BJ Crown: A Dual-Altitude UAV RGB Benchmark Dataset for Individual Tree Crown Delineation

**English | 中文说明见后半部分**

**BJ Crown** is a high-resolution unmanned aerial vehicle (UAV) visible-light (RGB) benchmark dataset designed for **individual tree crown (ITC) delineation, connected-crown separation, and cross-altitude generalization** in dense subtropical plantation canopies. It is the official benchmark dataset constructed in the paper:

> **TEGNet: A Topological Energy Guided Network for Individual Tree Crown Delineation in UAV Imagery**

---

## 1. Dataset Overview (English)

### 1.1 Background & Motivation
Delineating individual tree crowns from 2D visible-light UAV imagery in closed-canopy plantations remains challenging due to severe crown interdigitation, intra-crown texture heterogeneity, illumination variations, and scale shifts across flight altitudes. Existing public datasets predominantly focus on temperate coniferous/mixed forests at a single spatial resolution or rely on expensive airborne LiDAR/hyperspectral sensors.

To bridge this gap, we constructed the **BJ Crown** dataset over dense star anise (*Illicium verum*, 八角) plantations in **Yapai Village, Xinhua Town, Funing County, Wenshan Prefecture, Yunnan Province, China**. Star anise is a key subtropical economic tree species characterized by high canopy closure, irregular umbrella-shaped crowns, and frequent inter-crown occlusion, making it an ideal testbed for evaluating boundary refinement and instance separation algorithms.

### 1.2 Key Highlights
- **High-Precision Instance-Disjoint Annotations**: Contains **5,527** image patches ($512 \times 512$ pixels) and **12,813** manually delineated individual tree crowns. Adjacent crowns are strictly annotated as spatially disjoint foreground components in binary masks, enabling direct conversion between binary masks, instance ID maps, and topological distance/energy fields.
- **Dual-Altitude & Cross-Resolution Benchmark**: Captured using a **DJI Matrice 4T** UAV at two distinct flight altitudes:
  - **50 m altitude (`GSD = 2.41 cm/pixel`)**: Spatially partitioned into non-overlapping **Training**, **Validation**, and **Test1 (In-domain)** regions.
  - **80 m altitude (`GSD = 3.98 cm/pixel`)**: Held out as an independent **Test2 (Cross-altitude & Cross-resolution)** generalization benchmark, where the pixel area per crown decreases by approximately 63%.
- **Patch-Level & Full-Orthomosaic Evaluation**: Supports both standard patch-level benchmarking ($512 \times 512$ tiles) and large-scale sliding-window evaluation with crown area estimation ($R^2$ and $\text{rRMSE}$) on full UAV orthomosaics.

---

## 2. Dataset Statistics & Splits

All regional orthomosaics were cropped into $512 \times 512$ pixel patches using a sliding window with a 50% overlap ratio:

| Subset | Source Imagery | Flight Altitude (m) | Spatial Resolution / GSD (cm) | Patch Size (px) | Number of Patches | Annotated Tree Crowns | Task Role |
| :--- | :--- | :---: | :---: | :---: | :---: | :---: | :--- |
| **Train** | 50 m Orthomosaic | 50 | 2.41 | $512 \times 512$ | 3,501 | 6,397 | Model training |
| **Val** | 50 m Orthomosaic | 50 | 2.41 | $512 \times 512$ | 804 | 2,096 | Hyperparameter tuning & early stopping |
| **Test1** | 50 m Orthomosaic | 50 | 2.41 | $512 \times 512$ | 817 | 1,703 | In-domain evaluation (same altitude/GSD) |
| **Test2** | 80 m Orthomosaic | 80 | 3.98 | $512 \times 512$ | 405 | 2,617 | Cross-altitude generalization evaluation |
| **Total** | Dual-altitude UAV RGB | 50 / 80 | 2.41 / 3.98 | $512 \times 512$ | **5,527** | **12,813** | — |

---

## 3. Directory Structure & Data Format

### 3.1 File Organization
After downloading and extracting the dataset archive, the directory structure is organized as follows:

```text
BJ_Crown/
├── train/
│   ├── images/                 # 3,501 RGB image patches (512×512, 8-bit 3-channel)
│   ├── masks/                  # Instance-disjoint binary crown masks (0: background, 255: crown)
│   └── instances/              # Instance label maps (0: background, 1..N: individual crown IDs)
├── val/
│   ├── images/                 # 804 RGB image patches (512×512)
│   ├── masks/
│   └── instances/
├── test1/                      # In-domain test set (50 m altitude, GSD = 2.41 cm)
│   ├── images/                 # 817 RGB image patches (512×512)
│   ├── masks/
│   └── instances/
├── test2/                      # Cross-altitude test set (80 m altitude, GSD = 3.98 cm)
│   ├── images/                 # 405 RGB image patches (512×512)
│   ├── masks/
│   └── instances/
└── orthomosaics/               # Full-area UAV orthomosaics & reference masks
    ├── test1_50m_ortho.tif
    ├── test1_50m_gt_mask.tif
    ├── test2_80m_ortho.tif
    └── test2_80m_gt_mask.tif
```

### 3.2 Annotation Specification
- **RGB Images (`images/`)**: 3-channel visible-light UAV orthomosaic patches ($512 \times 512$ pixels) free of radiometric stretching artifacts.
- **Binary Crown Masks (`masks/`)**: Single-channel binary masks where `0` denotes background (soil, understory vegetation, or inter-crown shadows) and `255` (or `1`) denotes tree crown foreground. During manual delineation, adjacent tree crowns were strictly kept spatially independent (non-touching), so each 8-connected foreground component corresponds one-to-one to a single tree crown instance.

- # BJ Crown：无人机可见光影像单木树冠提取基准数据集（中文说明）

**BJ Crown** 是面向高郁闭度人工林**单木树冠提取（Individual Tree Crown Delineation）、粘连树冠解耦与跨航高泛化评估**的高分辨率无人机可见光（RGB）遥感基准数据集，配套论文为：

> **TEGNet: A Topological Energy Guided Network for Individual Tree Crown Delineation in UAV Imagery**

## 1. 数据集简介与特色

- **研究区与林分特征**：数据采集自**中国云南省文山壮族苗族自治州富宁县新华镇牙牌村**的亚热带高郁闭度八角（*Illicium verum*）人工林。研究区林冠密集、单木交错遮挡普遍，是检验复杂林冠边界刻画与粘连个体分离能力的理想场景。
- **采集平台与双航高设计**：使用搭载可见光传感器的大疆 **DJI Matrice 4T** 无人机分别在 **50 m** 和 **80 m** 航高下获取正射影像：
  - **50 m 航高影像（空间分辨率 `GSD = 2.41 cm`）**：按互不重叠的空间区域划分为**训练集（Train）**、**验证集（Val）**和**同航高测试集（Test1）**；
  - **80 m 航高影像（空间分辨率 `GSD = 3.98 cm`）**：独立作为**跨航高泛化测试集（Test2）**，用于检验模型在空间分辨率退化（单木像素面积缩减约 63%）与纹理减弱条件下的跨尺度迁移能力。
- **单木独立掩膜标注规范**：全数据集采用 50% 重叠率滑动窗口裁剪为 **5,527 个** $512 \times 512$ 像素图像块，人工逐株精细勾绘共 **12,813 个**单木树冠实例。标注时严格保持相邻树冠在二值掩膜中互不连通，使每个前景连通域唯一对应单株树冠，可直接用于语义分割、距离/拓扑能量回归及实例分割任务的训练与评测。

## 2. 数据集划分与统计

| 数据子集 | 影像来源 | 航高 (m) | 地面采样距离 GSD (cm) | 图像块尺寸 (px) | 切片数量 | 树冠实例标注数 | 用途说明 |
| :--- | :--- | :---: | :---: | :---: | :---: | :---: | :--- |
| **训练集 (Train)** | 50 m 正射影像 | 50 | 2.41 | $512 \times 512$ | 3,501 | 6,397 | 模型训练 |
| **验证集 (Val)** | 50 m 正射影像 | 50 | 2.41 | $512 \times 512$ | 804 | 2,096 | 超参数选择与早停监控 |
| **Test1（同域测试集）** | 50 m 正射影像 | 50 | 2.41 | $512 \times 512$ | 817 | 1,703 | 同航高、同分辨率精度评估 |
| **Test2（跨高度测试集）** | 80 m 正射影像 | 80 | 3.98 | $512 \times 512$ | 405 | 2,617 | 跨航高、跨分辨率泛化评估 |
| **合计 (Total)** | 两组航高影像 | 50 / 80 | 2.41 / 3.98 | $512 \times 512$ | **5,527** | **12,813** | — |

---
- **Instance Masks (`instances/`)**: Single-channel 16-bit integer masks derived from connected-component labeling of the instance-disjoint binary masks (`0` for background, `1..N` for individual tree crown instances), ready for instance segmentation training and evaluation.
