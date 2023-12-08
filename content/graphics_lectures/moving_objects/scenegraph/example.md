---
title: "Solarsystem 2"
date: 2022-11-20T12:00:39+01:00
draft: false
weight: 20
---

#### This time we will only use the scene graph to render our solarsystem and add some details: we add the moon and let it orbit around the earth and on top of orbiting we add rotation around their own axis to all the planets.

![SolarsystemScenegraph](/transformSG.gif)

---

Add the moon:
```cpp
T3DMesh<float> M;
//StaticActor m_Moon
SAssetIO::load("Assets/ExampleScenes/Solar/moon/scene.gltf", &M);
setMeshShader(&M, 0.4f, 0.02f);
M.computePerVertexNormals();
m_Moon.init(&M);
M.clear();
```

Now to the changed scene graph: we will add **Transformation Nodes** for every planet. The earth is a bit special, because we split translation and rotation about own axis into two Nodes, which is done in one and the same node for other planets, because the origin of the moon will always be the earth, but we dont want the moon to orbit in the same speed as the earth rotates around its own axis. The scene graph will look like this.

{{<mermaid align="middle">}}
    graph TD;
    root([m_RootSGN])
    solarTransform([m_SolarsystemSGN])
    sunTransform([m_SunTransformSGN])
    sunGeo([m_SunSGN])
    earthTranslate([m_EarthTranslateSGN])
    earthOrbit([m_EarthOrbitSGN])
    earthRotation([m_EarthRotationSGN])
    earthGeo([m_EarthSGN])
    moonOrbit([m_MoonOrbitSGN])
    moonTransform([m_MoonTransformSGN])
    moonGeo([m_MoonSGN])
    planetOrbit([m_PlanetOrbitSGN])
    planetTransform([m_PlanetTransformSGN])
    planetGeo([m_PlanetSGN])

    subgraph Sun [ ]
    sunTransform
    sunGeo
    end

    subgraph Earth [ ]
    earthTranslate
    earthOrbit
    earthRotation
    earthGeo
   
    end

    subgraph Moon [ ]
    moonOrbit
    moonTransform
    moonGeo
    end

    subgraph Planet [ ]
    planetOrbit
    planetTransform
    planetGeo
    end

    root ==> solarTransform

    solarTransform ==> |Sun| sunTransform
    solarTransform ==> |Earth| earthOrbit
    solarTransform -.-> |Planet| planetOrbit

    sunTransform ==> sunGeo

    earthOrbit ==> earthTranslate

    earthTranslate ==> earthRotation
    earthTranslate ==> |Moon| moonOrbit

    earthRotation ==> earthGeo

    moonOrbit ==> moonTransform

    moonTransform ==> moonGeo

    planetOrbit -.-> planetTransform

    planetTransform -.-> planetGeo

{{< /mermaid >}}

And this how it is created in the code:
```cpp
// build scene graph
m_RootSGN.init(nullptr);
m_SG.init(&m_RootSGN);

// Solarsystem
m_SolarsystemSGN.init(&m_RootSGN, Vector3f(0.0f, 2.0f, 0.0f));

//Sun
m_SunTransformSGN.init(&m_SolarsystemSGN);
m_SunSGN.init(&m_SunTransformSGN, &m_SunBody);
m_SunSGN.scale(Vector3f(0.7f, 0.7f, 0.7f));

// rotation
Quaternionf R;
R = AngleAxisf(CForgeMath::degToRad(20.0f / 120.0f), Vector3f::UnitY());
m_SunTransformSGN.rotationDelta(R);
```

```cpp
// Earth
m_EarthOrbitSGN.init(&m_SolarsystemSGN);
m_EarthTranslateSGN.init(&m_EarthOrbitSGN, Vector3f(3.6f, 0.0f, 0.0f));
m_EarthRotationSGN.init(&m_EarthTranslateSGN);
m_EarthSGN.init(&m_EarthRotationSGN, &m_Earth);
m_EarthSGN.scale(Vector3f(0.2f, 0.2f, 0.2f));

    // orbiting
R = AngleAxisf(CForgeMath::degToRad(40.0f / 120.0f), Vector3f::UnitY());
m_EarthOrbitSGN.rotationDelta(R);

    // rotation
R = AngleAxisf(CForgeMath::degToRad(45.0f / 120.0f), Vector3f::UnitY());
m_EarthRotationSGN.rotationDelta(R);

// Moon
m_MoonOrbitSGN.init(&m_EarthTranslateSGN);
m_MoonTransformSGN.init(&m_MoonOrbitSGN, Vector3f(0.3f, 0.0f, 0.0f));
m_MoonSGN.init(&m_MoonTransformSGN, &m_Moon);
m_MoonSGN.scale(Vector3f(0.04f, 0.04f, 0.04f));

    // orbiting
R = AngleAxisf(CForgeMath::degToRad(120.0f / 120.0f), Vector3f::UnitY());
m_MoonOrbitSGN.rotationDelta(R);
```

By chaining transformation nodes, we get the result we want: we translate the object, but we rotate around the original y-axis and not the translated one. As for the moon, it moves with the Earth and rotates around the Earth on it's own, meaning it rotates around the y-Axis of the objectspace of the earth. 
The **rotationDelta()** applies rotation everytime the scene graph is updated (**m_SG.update()**). So don't forget to add it to the main loop.
```cpp
m_SG.update(60.0f / m_FPS);
```

And with that we've got a bit more detailed version of the solarsystem using a scene graph. Make sure to read the complete source code in /Examples/exampleTransformationSG.hpp to get a better understanding.