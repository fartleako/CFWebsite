---
title: "Loading and Storing Assets"
date: 2022-11-01T21:49:43+01:00
draft: false
weight: 40
---

To handle import and export of meshes or image files - Assets - CrossForge implements the class **SAssetIO**.
To load an Asset, simply use the load function in combination with the class **T3DMesh** or **T2DImage** depending on the asset to store it:
```cpp
T3DMesh<float> M;
T2DImage<uint8_t> I;
SAssetIO::load("Assets/path/mesh.meshformat", &M);
SAssetIO::load("Assets/path/image.imageformat", &I);
```