---
title: "Skeletal Animation"
date: 2022-11-20T12:01:15+01:00
draft: false
weight: 40
---

Skeletal Animation, also called rigging, is appliable when an object consists of two parts: a surface and jointed, but individually rigid bodies called bones. The entirety of these bones is called a skeleton hence skeletal animation. Given the nature of these bones they can be animated seperately while still influencing other bones in their position and/or orientation, which is why the skeleton can also be seen as a hierarchy/tree, since transformation in a higher node/bone will also apply to the child nodes/bones. The vertices of the surface are all enveloped to their appropriate bone, which means if the bone moves, the appointed vertices move accordingly. As the name was bestowed, Skeletal Animation's initial and now main purpose is to animate humans, but can also be used to animate functionally similar organisms or objects like animals, insects or even machinery.

Here it is used to create a walking animation for a human:
![skeleton_anim](/skeletal_animation.gif)

Crossforge can work with skeletal animations and using Examples/exampleSkeletalAnimation.hpp we will show you how easy it is!

{{% notice info %}}
We will not show you, how you can create a rigged object animation, but how you can insert one into your scene! \
\
Crossforge supports the formats glb and fpx.
{{% /notice %}}

---

Like every object we first need to load the asset:

```cpp
SAssetIO::load("Assets/tmp/MuscleManWalking.glb", &M);
setMeshShader(&M, 0.7f, 0.04f);
M.computePerVertexNormals();
```

Similar to Morphing, Skeletal Animation works with 2 classes: **SkeletalActor** and **SkeletalAnimationController**. With the init function of the controller we can extract all the animation information of the file/mesh, while the init function of the actor reads the object itself(geometry) and assigns the controller to it.

```cpp
//SkeletalActor m_MuscleMan;
//SkeletalAnimationController m_BipedController;
m_BipedController.init(&M);
m_MuscleMan.init(&M, &m_BipedController);
M.clear();
```

Also like every object we have to add it to the Scene Graph:

```cpp
//SGNGeometry m_MuscleManSGN;
//SGNTransformation m_MuscleManTransformSGN;

// add skeletal actor to scene graph (Eric)			
m_MuscleManTransformSGN.init(&m_RootSGN, Vector3f(0.0f, -0.06f, 0.0f));
m_MuscleManSGN.init(&m_MuscleManTransformSGN, &m_MuscleMan);
m_MuscleManSGN.scale(Vector3f(0.025f, 0.025f, 0.025f));
```

Finally we need to add the update function to the main loop (run):

```cpp
// this will progress all active skeletal animations for this controller
m_BipedController.update(60.0f / m_FPS);
``` 

To play the animation and maybe change the animation speed you can set some hotkeys while also adding this to the main loop:

```cpp
SkeletalAnimationController::Animation* pAnim = m_BipedController.createAnimation(0, AnimationSpeed, 0.0f);
m_MuscleMan.activeAnimation(pAnim);
```