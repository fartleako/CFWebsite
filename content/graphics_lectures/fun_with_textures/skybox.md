---
title: "Skybox"
date: 2022-11-20T12:00:55+01:00
draft: false
weight: 10
---

#### When creating seemingly endless worlds, one way of achieving this feeling is by using a concept called Skybox.

![Example Skybox](/duck_skybox.png)


In short a Skybox is either a cube or a sphere, so called skydome, that surrounds the world.
The sky and other unreachable objects are projected onto the inner faces.

----


We use the **SkyboxActor** class when handling with skyboxes.
```cpp
SkyboxActor m_Skybox;
```
1) First we need to load textures for the 6 faces of the Cube. We use the clear sky version as an example.  
For that we can create a string vector to store the path for all face textures.
```cpp 
vector<string> m_ClearSky;
m_ClearSky.push_back("Assets/ExampleScenes/skybox/vz_clear_right.png");
m_ClearSky.push_back("Assets/ExampleScenes/skybox/vz_clear_left.png");
m_ClearSky.push_back("Assets/ExampleScenes/skybox/vz_clear_up.png");
m_ClearSky.push_back("Assets/ExampleScenes/skybox/vz_clear_down.png");
m_ClearSky.push_back("Assets/ExampleScenes/skybox/vz_clear_back.png");
m_ClearSky.push_back("Assets/ExampleScenes/skybox/vz_clear_front.png");
```

2) To initialize those faces now we use the **init**-function, which loads all the textures and assigns them to the corresponding face

```cpp
m_Skybox.init(m_ClearSky[0], m_ClearSky[1], m_ClearSky[2], m_ClearSky[3], m_ClearSky[4], m_ClearSky[5]);
```
3) Furthermore we can initialize color adjustment values.

```cpp
m_Skybox.brightness(1.15f);
m_Skybox.contrast(1.1f);
m_Skybox.saturation(1.2f);
```
4) After initializing the Skybox we should create a scene graph for the skybox.

```cpp
SceneGraph m_SkyboxSG;
SGNTransformation m_SkyboxTransSGN;
SGNGeometry m_SkyboxGeomSGN;
```
5) And add the skybox

```cpp
m_SkyboxTransSGN.init(nullptr);
m_SkyboxGeomSGN.init(&m_SkyboxTransSGN, &m_Skybox);
m_SkyboxSG.init(&m_SkyboxTransSGN);
```

6) The skybox scenegraph must then be rendered in the mainloop