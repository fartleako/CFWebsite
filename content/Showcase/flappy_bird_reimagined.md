---
title: "Flappy Bird Reimagined"
date: 2022-11-07T18:07:37+01:00
draft: false
weight: 20
chapter: false
---

## What is Flappy Bird Reimagined?
With the original hit Flappy Bird as inspiration, we have used the Crossforge engine to bring Flappy Bird into 3 dimensions, our protagonist in this case is a hummingbird who maneuvers his way through the endless rows of buildings.

![Bird flying through a checkpoint](/flappybird.gif)

---
## How it was made
In the following sections, we will briefly introduce the parts that make up Flappy Bird Reimagined

### Our "protagonist" / actor
For our main character, we used a hummingbird model from [Sketchfab](https://sketchfab.com/3d-models/hummingbird-flying-4e6d591f6e50493aa5e31355084fc4e8) with pre-made animations that we customized and tailored to our needs. ([Licenses](/showcase/licenses_showcases/))

The following code shows the integration of the gltf model and the use of the animation controller.
```cpp
void init(void) override {
//...

T3DMesh<float> M;
void init(void) override {
SAssetIO::load("MyAssets/Kolibri/Kolibri.gltf", &M);
setMeshShader(&M, 0.7f, 0.04f);
M.computePerVertexNormals();
m_BipedController.init(&M); //SkeletalAnimationController m_BipedController;
m_Bird.init(&M, &m_BipedController); //SkeletalActor m_Bird;
M.clear();
m_RepeatAnimation = true; //bool m_RepeatAnimation = true;

//...

float AnimationSpeed = 1000 / 60.0f;
SkeletalAnimationController::Animation* pAnim = m_BipedController.createAnimation(0, AnimationSpeed, 0.0f);
m_Bird.activeAnimation(pAnim);

//...
}

```

Inside the mainloop we make sure the animation is properly repeated

```cpp
void mainLoop(void)override {
//...
// this will progress all active skeletal animations for this controller
m_BipedController.update(60.0f / m_FPS);
if (m_RepeatAnimation && nullptr != m_Bird.activeAnimation()) {
    auto* pAnim = m_Bird.activeAnimation();
    if (pAnim->t >= pAnim->Duration) pAnim->t -= pAnim->Duration;
    }
//...
}
```

### The environment ([Licenses](/showcase/licenses_showcases/))
#### Skybox 
The process of implementing the skyboxes is already documented [here](/graphics_lectures/fun_with_textures/skybox/), so in short:
```cpp
void init(void) override {
//...
/// gather textures for the skybox
m_City1.push_back("MyAssets/FlappyAssets/skybox1/right.bmp");
m_City1.push_back("MyAssets/FlappyAssets/skybox1/left.bmp");
m_City1.push_back("MyAssets/FlappyAssets/skybox1/up.bmp");
m_City1.push_back("MyAssets/FlappyAssets/skybox1/down.bmp");
m_City1.push_back("MyAssets/FlappyAssets/skybox1/back.bmp");
m_City1.push_back("MyAssets/FlappyAssets/skybox1/front.bmp");

// initialize actor
m_Skybox.init(m_City1[0], m_City1[1], m_City1[2], m_City1[3], m_City1[4], m_City1[5]); //SkyboxActor m_Skybox;

// set initialize color adjustment values
m_Skybox.brightness(1.15f);
m_Skybox.contrast(1.1f);
m_Skybox.saturation(1.2f);

// create scene graph for the Skybox
m_SkyboxTransSGN.init(nullptr);
m_SkyboxGeomSGN.init(&m_SkyboxTransSGN, &m_Skybox);
m_SkyboxSG.init(&m_SkyboxTransSGN);
//...
}

void mainLoop(void)override {
//...
m_SkyboxSG.update(60.0f / m_FPS);
//...
// Skybox should be last thing to render
m_SkyboxSG.render(&m_RenderDev);
}
```
#### Textured Ground
```cpp
void init(void) override {
//...
SAssetIO::load("MyAssets/Ground/cloud.gltf", &M);
setMeshShader(&M, 0.8f, 0.04f);
for (uint8_t i = 0; i < 4; ++i) M.textureCoordinate(i) *= 50.0f;
M.computePerVertexNormals();
M.computePerVertexTangents();
m_Ground.init(&M); //StaticActor m_Ground;
M.clear();
//...
}
```
#### Starting Area
```cpp

void ExampleFlappyBird::createStartingArea(float xOffset) {
	////////////////////////////////////////////////////////////////////////////////
	// Create ground transformation SGN
	SGNTransformation* GroundTransformSGN = new SGNTransformation();
	GroundTransformSGN->init(&m_RootSGN);
	GroundTransformSGN->translation(Vector3f(0.0f, -0.5f, xOffset));
	m_BuildingSGNs.push_back(GroundTransformSGN);
	// Create ground geometry SGN
	SGNGeometry* GroundGeoSGN = new SGNGeometry();
	GroundGeoSGN->init(GroundTransformSGN, &m_Ground);
	GroundGeoSGN->scale(Vector3f(0.75f, 0.75f, 0.835f)); 
	m_BuildingGeoSGNs.push_back(GroundGeoSGN);
	////////////////////////////////////////////////////////////////////////////////
	// Create left side wall transformation SGN
	SGNTransformation* leftSidewallTransformSGN = new SGNTransformation();
	leftSidewallTransformSGN->init(&m_RootSGN);
	leftSidewallTransformSGN->translation(Vector3f(17.0f, 0.0f, xOffset));
	m_BuildingSGNs.push_back(leftSidewallTransformSGN);
	// Create geometry SGN for the left side wall
	SGNGeometry* leftSidewallGeoSGN = new SGNGeometry();
	leftSidewallGeoSGN->init(leftSidewallTransformSGN, &building_2);
	leftSidewallGeoSGN->scale(Vector3f(5.0f, 5.0f, 50.0f)); 
	m_BuildingGeoSGNs.push_back(leftSidewallGeoSGN);
	////////////////////////////////////////////////////////////////////////////////
	// Create right side wall transformation SGN
	SGNTransformation* rightSidewallTransformSGN = new SGNTransformation();
	rightSidewallTransformSGN->init(&m_RootSGN);
	rightSidewallTransformSGN->translation(Vector3f(-11.0f, 0.0f, xOffset));
	m_BuildingSGNs.push_back(rightSidewallTransformSGN);
	// Create geometry SGN for the right side wall
	SGNGeometry* rightSidewallGeoSGN = new SGNGeometry();
	rightSidewallGeoSGN->init(rightSidewallTransformSGN, &building_2);
	rightSidewallGeoSGN->scale(Vector3f(5.0f, 5.0f, 50.0f)); 
	m_BuildingGeoSGNs.push_back(rightSidewallGeoSGN);
}

```
#### Generated Rows with side buildings
```cpp

void ExampleFlappyBird::createBuildingRow(float xOffset) {
	float buildingSpacing = 5.0f; // Distance between the buildings in a row
	float buildingWidth = 3.0f; // Width of a building
	// Calculate total row width
	float totalRowWidth = 3 * buildingWidth + 2 * buildingSpacing;
	// Randomly select which position the building of type building_2 should have
	int building2Position = rand() % 3;
	////////////////////////////////////////////////////////////////////////////////
	// Create ground transformation SGN
	SGNTransformation* GroundTransformSGN = new SGNTransformation();
	GroundTransformSGN->init(&m_RootSGN);
	GroundTransformSGN->translation(Vector3f(0.0f, -0.5f, xOffset));
	m_BuildingSGNs.push_back(GroundTransformSGN);
	// Create ground geometry SGN
	SGNGeometry* GroundGeoSGN = new SGNGeometry();
	GroundGeoSGN->init(GroundTransformSGN, &m_Ground);
	GroundGeoSGN->scale(Vector3f(0.75f, 0.75f, 0.835f)); // Scaling along the z-axis only
	m_BuildingGeoSGNs.push_back(GroundGeoSGN);
	////////////////////////////////////////////////////////////////////////////////
	// Create left side wall transformation SGN
	SGNTransformation* leftSidewallTransformSGN = new SGNTransformation();
	leftSidewallTransformSGN->init(&m_RootSGN);
	leftSidewallTransformSGN->translation(Vector3f(17.0f, 0.0f, xOffset));
	m_BuildingSGNs.push_back(leftSidewallTransformSGN);
	// Create geometry SGN for the left side wall
	SGNGeometry* leftSidewallGeoSGN = new SGNGeometry();
	leftSidewallGeoSGN->init(leftSidewallTransformSGN, &building_2);
	leftSidewallGeoSGN->scale(Vector3f(5.0f, 5.0f, 50.0f)); // Scaling along the z-axis only
	m_BuildingGeoSGNs.push_back(leftSidewallGeoSGN);
	////////////////////////////////////////////////////////////////////////////////
	// Create right side wall transformation SGN
	SGNTransformation* rightSidewallTransformSGN = new SGNTransformation();
	rightSidewallTransformSGN->init(&m_RootSGN);
	rightSidewallTransformSGN->translation(Vector3f(-11.0f, 0.0f, xOffset));
	m_BuildingSGNs.push_back(rightSidewallTransformSGN);
	// Create geometry SGN for the right side wall
	SGNGeometry* rightSidewallGeoSGN = new SGNGeometry();
	rightSidewallGeoSGN->init(rightSidewallTransformSGN, &building_2);
	rightSidewallGeoSGN->scale(Vector3f(5.0f, 5.0f, 50.0f)); // Scaling along the z-axis only
	m_BuildingGeoSGNs.push_back(rightSidewallGeoSGN);
    ////////////////////////////////////////////////////////////////////////////////

	for (int i = 0; i < 3; ++i) {
		// Create buildings for a row here and save them in the list
		SGNTransformation* buildingTransformSGN = new SGNTransformation();
		buildingTransformSGN->init(&m_RootSGN);
		
		if (i == building2Position) {
			int Version = rand() % 3;
			if (Version == 1) {
				Quaternionf querRot;
				querRot = AngleAxisf(CForgeMath::degToRad(90.0f), Vector3f::UnitZ());
				buildingTransformSGN->translation(Vector3f(i * (buildingWidth + buildingSpacing) - 5.0, 5.0f, xOffset));
				buildingTransformSGN->rotation(querRot);
			}
			else if(Version == 2) {
				Quaternionf querRot;
				querRot = AngleAxisf(CForgeMath::degToRad(90.0f), Vector3f::UnitZ());
				buildingTransformSGN->translation(Vector3f(i * (buildingWidth + buildingSpacing) - 3.0, 12.0f, xOffset));
				buildingTransformSGN->rotation(querRot);
				buildingTransformSGN->scale(Vector3f(0.75f, 0.75f, 0.75f));
			}
			else {
				buildingTransformSGN->translation(Vector3f(i * (buildingWidth + buildingSpacing) - 5.0f, -10.0f, xOffset));
			}
		}
		else {
			buildingTransformSGN->translation(Vector3f(i * (buildingWidth + buildingSpacing) - 5.0f, 0.0f, xOffset));
		}


		// Add the transformations for the current building to the list
		m_BuildingSGNs.push_back(buildingTransformSGN);

		// Create the geometry SGN for the building here and save it in the list
		SGNGeometry* buildingGeoSGN = new SGNGeometry();

		// Decide whether the building is building_2 or not
		if (i == building2Position) {
			buildingGeoSGN->init(buildingTransformSGN, &building_2);
		}
		else {
			buildingGeoSGN->init(buildingTransformSGN, &building_1);
		}

		buildingGeoSGN->scale(Vector3f(5.0f, 5.0f, 5.0f));

		// add geometry SGN to list
		m_BuildingGeoSGNs.push_back(buildingGeoSGN);
	}
}

```
