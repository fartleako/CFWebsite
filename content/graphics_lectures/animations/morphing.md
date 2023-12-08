---
title: "Morphing"
date: 2022-11-20T12:00:08+01:00
draft: false
weight: 20
---

#### Automatically transforming a form/model to another through a seamless transition by geometric interpolation is called Morphing. There are various applications for this, the most typical ones are face animations, creating variations or combinations and some other simple animations.

![MorphingDisgust](/morph_target.gif)

With Examples/exampleMorphTargetAnimation.hpp we will show you how Crossforge implements Morphing with face animations.

---

The basis of the Morph Target Animation consists of two classes: **MorphTargetActor** and **MorphTargetAnimationController**

```cpp
MorphTargetActor m_Face;
MorphTargetAnimationController m_MTController;
```

For this to work we first need a base model, which is a male face.

```cpp
// load face model
SAssetIO::load("Assets/ExampleScenes/FaceGenMale/MaleFace.obj", &M);
setMeshShader(&M, 0.5f, 0.04f);
M.computePerVertexNormals();
// build the morph targets
buildMTModel(&M);
```

After that we need forms we want to morph into, the face with different expressions in our case. We add these in the function **buildMTModel** with the help of the class **MorphTargetModelBuilder** which lets us configure the **T3DMesh** more easily for our use:

```cpp
void buildMTModel(T3DMesh<float>* pBaseMesh) {
    if (nullptr == pBaseMesh) throw NullpointerExcept("pBaseMesh");

    // create morph target build and initialize with base mesh
    MorphTargetModelBuilder MTBuilder;
    MTBuilder.init(pBaseMesh);

    // define models we want to add
    // models have to bin in full vertex correspondence (same number of vertices, each having the same meaning)
    vector<pair<string, string>> MTList;
    MTList.push_back(pair("Anger", "Assets/ExampleScenes/FaceGenMale/MaleFace_ExpressionAnger.obj"));
    MTList.push_back(pair("ChinRaised", "Assets/ExampleScenes/FaceGenMale/MaleFace_ExpressionChinRaised.obj"));
    MTList.push_back(pair("Disgust", "Assets/ExampleScenes/FaceGenMale/MaleFace_ExpressionDisgust.obj"));
    MTList.push_back(pair("Fear", "Assets/ExampleScenes/FaceGenMale/MaleFace_ExpressionFear.obj"));
    MTList.push_back(pair("Frown", "Assets/ExampleScenes/FaceGenMale/MaleFace_ExpressionFrown.obj"));
    MTList.push_back(pair("Kiss", "Assets/ExampleScenes/FaceGenMale/MaleFace_ExpressionKiss.obj"));
    MTList.push_back(pair("PuffCheeks", "Assets/ExampleScenes/FaceGenMale/MaleFace_ExpressionPuffCheeks.obj"));
    MTList.push_back(pair("Surprise", "Assets/ExampleScenes/FaceGenMale/MaleFace_ExpressionSurprise.obj"));

    // load models and add to builder as targets
    T3DMesh<float> M;
    for (auto i : MTList) {
        M.clear();
        try {
            AssetIO::load(i.second, &M);
            MTBuilder.addTarget(&M, i.first);
        }
        catch (const CrossForgeException& e) {
            SLogger::logException(e);
        }
    }//for[all files]

    // build morph targets and retrieve them
    MTBuilder.build();
    MTBuilder.retrieveMorphTargets(pBaseMesh);                      
    //the class T3DMesh contains properties for Morphing, so we can save the base model together with the morph targets in a single T3DMesh
}//buildMTModel
```

Now that we have the models all set up it's time for the animations. For that we use our previously declared controller and initialize it with our saved morph models:

```cpp
m_MTController.init(&M);
buildMTSequences(&m_MTController);
```

In **buildMTSequences** we create the animation sequences in the form of the class **MorphTargetAnimationController::AnimationSequence**:

```cpp
void buildMTSequences(MorphTargetAnimationController* pController) {
    if (nullptr == pController) throw NullpointerExcept("pController");

    MorphTargetAnimationController::AnimationSequence Seq;

    // for every morph target we create a sequence
    // 1.25 seconds to activate the target, 0.5 seconds hold, and 1.25 seconds to go back to base mesh
    for (uint32_t i = 0; i < pController->morphTargetCount(); ++i) {
        Seq.clear();
        MorphTargetAnimationController::MorphTarget* pMT = pController->morphTarget(i);
        Seq.Name = pMT->Name;
        Seq.Targets.push_back(pMT->ID);
        Seq.Targets.push_back(pMT->ID);
        Seq.Targets.push_back(pMT->ID);
        Seq.Parameters.push_back(Vector3f(0.0f, 1.0f, 1.25f));
        Seq.Parameters.push_back(Vector3f(1.0f, 1.0f, 0.5f));
        Seq.Parameters.push_back(Vector3f(1.0f, 0.0f, 1.25f));
        pController->addAnimationSequence(&Seq);
    }//for[all morph targets

}//buildMTSequences
```

And finally we initialize the MorphTargetActor and add it to the scene graph

```cpp
m_Face.init(&M, &m_MTController);
M.clear();

// load skydome
SAssetIO::load("Assets/ExampleScenes/SimpleSkydome.fbx", &M);
setMeshShader(&M, 0.8f, 0.04f);
M.computePerVertexNormals();
m_Skydome.init(&M);                                                                 //staticActor
M.clear(); 

// build scene graph			
m_RootSGN.init(nullptr);                                                            //SGNTransformation
m_SG.init(&m_RootSGN);                                                              //SceneGraph

// add skydome		
m_SkydomeSGN.init(&m_RootSGN, &m_Skydome);                                          //SGNGeometry
m_SkydomeSGN.scale(Vector3f(5.0f, 5.0f, 5.0f));

// add cube		
m_FaceTransformSGN.init(&m_RootSGN, Vector3f(0.0f, 3.0f, 0.0f));                    //SGNTransformation
m_FaceSGN.init(&m_FaceTransformSGN, &m_Face);                                       //SGNGeometry
m_FaceSGN.scale(Vector3f(0.01f, 0.01f, 0.01f));
```

To actually play the animation we have to update the controller and add the animation to our **MorphTargetActor** in the main loop (run).

```cpp
// progres morph target animations
m_MTController.update(60.0f/m_FPS);

//... input

MorphTargetAnimationController::ActiveAnimation* pAnim = m_MTController.play(PlayMTAnimation, MTAnimationSpeed);
//MorphTargetAnimationController::play(animationindex, speed)
m_Face.addAnimation(pAnim);
```

You should already know how to process input after reading [Basic Scene](/my_first_scene/basic_scene#input), so the last step for you is to add inputs to play the different animations. Dont forget, that if there is no input then it - of course - won't play an animation ;).