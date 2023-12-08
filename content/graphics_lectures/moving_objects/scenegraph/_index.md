---
title: "Scenegraph"
date: 2022-11-20T12:00:39+01:00
draft: false
weight: 20
---

#### A scenegraph is a data structure which allows inheritance of data manipulation (for instance rotation, scaling or transformation). It is often used in frameworks and engines to easily display objects. For example you can "connect" the tires with the rest of a car. Therefore you can scale the tires diffently then the chassis. In this manner you can build your whole scene - that's why it is called scenegraph.

Before we can connect models via a scenegraph, we have to create the root of the scenegraph itself - we can do that via a **SGNTransformation**-Node. It is allways good to remember which node is the root itself. For that reason we called it in our example **m_RootSGN**.

After that we can initialze the root and connect to the scengraph. An instance of a scenegraph is already defined in the exampleSceneBase.hpp, so we do not have to define it ourselfs. 

```cpp
SGNTransformation m_RootSGN;
...
m_RootSGN.init(nullptr);
m_SG.rootNode(&m_RootSGN);
```

Before we can connect any further nodes we need to load the according models. 

```cpp
SAssetIO::load("Assets/ExampleScenes/TexturedGround.gltf", &M);
...
SAssetIO::load("Assets/ExampleScenes/Trees/LowPolyTree_01.glb", &M);
...
```
As you now have guessed the root is initialized but till now we do not display any model. 
There are two important nodes which you need to know in order to understand the behaviour of the scengraph. 

- In first that is the **SGNTransformation**. It is added first to the scengraph. Trough an instance of this node you can scale, transform (i.e. move) an object and even rotate it. You can even connect multible transform nodes together to "add" the transformations. 
- Secondly there is the **SGNGeometry**. This is more or lesse the "real" geometry of the object. For example, you want to set a tree initial to the position (3,2,6) and you also want to scale it to halfe the size, you can do that. This node is allways connect to a transform node - otherwise you can not change a paramter afterwards. 

Now we connect the ground node to the root itself. The **.init** functions holds two parameters, the first is the parent - the transformation node which you want to connect to and the second parameter is the renderable object. In our case that is **m_Ground**.

```cpp
StaticActor m_Ground;
SGNGeometry m_GroundSGN;
SGNTransformation m_GroundTransformSGN;
...
// initialize ground transformation and geometry scene graph node
m_GroundTransformSGN.init(&m_RootSGN);
m_GroundSGN.init(&m_GroundTransformSGN, &m_Ground);
m_GroundSGN.scale(Vector3f(15.0f, 15.0f, 15.0f));
```
Till now our scengraph looks a bit like this: 
{{<mermaid align="middle">}}
graph TD;
    root([m_RootSGN])
    groundTransform([m_GroundTransformSGN])
    groundSGN([m_GroundSGN])
    root ==> groundTransform
    groundTransform ==> groundSGN 
{{< /mermaid >}}

In our example we want to display a group of trees. It would be to much work to connect 10 trees by hand let alone 100 or more. So we just create one transformation node for all further trees which we call m_TreeGroupSGN and connect it to the root. After that we implement a for-loop where we specify the number of trees we want to display. 

```cpp
// sceen graph node that holds our forest
m_TreeGroupSGN.init(&m_RootSGN);
float Area = 500.0f;	// square area [-Area, Area] on the xz-plane, where trees are planted
float TreeCount = 900;	// number of trees to crea

for (uint32_t i = 0; i < TreeCount; ++i){
```

In the for-loop itself we create the nodes itself, initalize them and connect them to m_TreeGroupSGN node. For this reason we can controll the attribute of each tree individually. We could make the trees with each itteration of the loop bigger, smaller or put a bit of randomness into the game. So we can scale them between two values and set them anywhere in the world via a pseudo random number generator.

```cpp
// create the scene graph nodes
SGNGeometry* pGeomSGN = nullptr;
SGNTransformation* pTransformSGN = nullp

// initialize position and scaling of the tree
pTransformSGN = new SGNTransformation();
pTransformSGN->init(&m_TreeGroupSGN);
```

We set the world to a size of 1000x1000 with the area. In this area we can randomly set the trees now. We just generate two random numbers for the coordinates of the tree. Remeber we do that for each tree, so each time we load up the programm the forest is different. 

```cpp
Vector3f TreePos = Vector3f::Zero();
TreePos.x() = CoreUtility::randRange(-Area, Area);
TreePos.z() = CoreUtility::randRange(-Area, Area);
pTransformSGN->translation(TreePos);
```

It is good to set the trees to different postions - but trees are also never the same hight. Therefore we can set the scale of the trees randomly as well. 

In the last step we need to connect the geometry itself to the forest. To make the forest a bit more interesting we also choose on of three tree models randomly. 

```cpp
// initialize geometry
// choose one of the trees randomly
pGeomSGN = new SGNGeometry();
uint8_t TreeType = CoreUtility::rand() % 3;
pGeomSGN->init(pTransformSGN, &m_Trees[TreeType]);
```

If you want, you can add a tree model by yourself and tinker around. Don't forget to change the value to `rand() % 4` afterwards or higher depending how many models you added. 

The scengraph should now look a bit like this but with 900 trees not just two: 
{{<mermaid align="middle">}}
    graph TD;
    root([m_RootSGN])
    groundTransform([m_GroundTransformSGN])
    groundSGN([m_GroundSGN])
    forestTransform([m_TreeGroupSGN])
    
    tree1transform([Tree-1-Transform])
    tree1SGN([Tree-1-SGN])
    
    tree_I_transform([Tree-i-Transform])
    tree_I_SGN([Tree-i-SGN])
    
    %%tree_N_transform([Tree-n-Transform])
    %%tree_N_SGN([Tree-n-Transform])

    %%subgraph treeN [ ]
    %%tree_N_transform
    %%tree_N_SGN
    %%end

    subgraph treei [ ]
    tree_I_transform
    tree_I_SGN
    end

    subgraph tree1 [ ]
    tree1transform
    tree1SGN
    end

    groundTransform ==> groundSGN 

    root ==> groundTransform
    root ==> forestTransform

    forestTransform ==>|Tree 1| tree1transform
    forestTransform -.->|Tree i| tree_I_transform
    %%forestTransform -.->|Tree n| tree_N_transform

    tree1transform --> tree1SGN
    tree_I_transform --> tree_I_SGN
    %%tree_N_transform --> tree_N_SGN

    %% This link is a picture of this specific flowchart
    %% [![](https://mermaid.ink/img/pako:eNp1lM9vgyAUgP8VQmKyJWWJV5f21KTZYR5Wb7VpWGEtWQCDeFhq__eBigiqB388vvehjycPeJWEwgyCm8LVHRT791IAcygp9cuJX77M9XjIz699-KZkI0ihsKh_pOKWOIShGWwCHpuMGpjWOlAVilLLVROsP2szkmrPWhKlaMx2dMd1Mw7EounycYldbNllSG9jc1uSdFQ-84m5b2S9MaIcVzff_XLYhBycwHlttrnZRaggThjomNfFpYg_u39eE6WhKF3QpEuSqIXAdrvznQIcZftvMlSE8nE0aiKXHYUt2tqCg7RdfNmYR29oSGDtSpmSZD1JDEnBQi2VCiC0iyoVzzYi0zWZN4LHhi6AG8ip4pgR83c_bFoJ9Z1yWsLM3BKsfktYiqfhcKPl8U9cYaZVQzewqQjWdM-wWWzugpQwLdVnv1t0m8bzH2bwad0?type=png)](https://mermaid.live/edit#pako:eNp1lM9vgyAUgP8VQmKyJWWJV5f21KTZYR5Wb7VpWGEtWQCDeFhq__eBigiqB388vvehjycPeJWEwgyCm8LVHRT791IAcygp9cuJX77M9XjIz699-KZkI0ihsKh_pOKWOIShGWwCHpuMGpjWOlAVilLLVROsP2szkmrPWhKlaMx2dMd1Mw7EounycYldbNllSG9jc1uSdFQ-84m5b2S9MaIcVzff_XLYhBycwHlttrnZRaggThjomNfFpYg_u39eE6WhKF3QpEuSqIXAdrvznQIcZftvMlSE8nE0aiKXHYUt2tqCg7RdfNmYR29oSGDtSpmSZD1JDEnBQi2VCiC0iyoVzzYi0zWZN4LHhi6AG8ip4pgR83c_bFoJ9Z1yWsLM3BKsfktYiqfhcKPl8U9cYaZVQzewqQjWdM-wWWzugpQwLdVnv1t0m8bzH2bwad0)

{{< /mermaid >}}

If we start the framework now, it could look like this:
![scnegraph](/scenegraph.JPG)


