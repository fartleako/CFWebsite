---
title: "Checkpoint Race"
date: 2022-11-07T18:07:37+01:00
draft: false
weight: 10
chapter: false
---

## The Idea
Imagine you were a bird soaring through the world, effortlessly flapping your wings or diving with remarkable speed. 
We wanted to create a small game from this idea. But before we could work on the realisation of the game, we had to think about a few things.

We needed a goal for the small game itself. We wanted to show the manoeuvrability of a birds by constructing a parkour. To complete the level, the player must fly through several checkpoints. For added motivation and replay potential, the time taken to reach the end will be recorded and colliding with obstacles will incur a time penalty which will be included in the final time.

![Bird flying through a checkpoint](/checkpoint_run_showcase.gif)

### Creating a composite world
Without any objects, a world for a bird or its flight would be monotonous. That's why we created a small city with the help of a grind in which we placed buildings. We took the objects themselves from an online database for 3D objects, as well as the bird model.

```cpp
void set_building(float x, float y, float z, int model, int i) {
	SGNTransformation* pTransformSGN = nullptr;
	SGNGeometry* pGeomSGN = nullptr;

	pTransformSGN = new SGNTransformation();
	pTransformSGN->init(&m_BuildingGroupSGN);

	// set to other vector
	pTransformSGN->translation(Vector3f(x, z, y));
	pTransformSGN->scale(m_building_scale);

	pGeomSGN = new SGNGeometry();
	pGeomSGN->init(pTransformSGN, &m_Buildings[model]);

	m_BuildingTransformationSGNs.push_back(pTransformSGN);
	m_BuildingSGNs.push_back(pGeomSGN);

}
```

As you see we just created a function to place an object out of a list of objects. We can change the object type with the ```int model```. In total we had 3 buildings but it would be no problem to implement further models. 

### Setting up the camera
In the normal CrossForge framework the camera can move freely. In our application we thought about a 3rd person view - so that we would follow the bird at all times and see his flight. So we did not want to positon the camera right into the bird but a bit higher up and back. The bird itself would be in the center of the frame at all the time. 

Down below you can see the code for a simple camera that follows the bird at all times.
```cpp
void defaultCameraUpdateBird(VirtualCamera* pCamera, Keyboard* pKeyboard, Mouse* pMouse, Vector3f m, Vector3f posBird, Vector3f up, Matrix3f bird) {
	// the update vector
    Vector3f mm;
    // position for the camer
	Vector3f posC = Vector3f(0.0f, 3.0f, -10.0f); 

    // bird is a matrix and in eigen you access a matrix via (row, col)
	mm.x() = bird(0, 0) * posC.x() + bird(0, 2) * posC.z();
	mm.y() = posC.y();
	mm.z() = bird(2, 0) * posC.x() + bird(2, 2) * posC.z();

    // update the camera
	pCamera->position(posBird + mm);
	pCamera->lookAt(pCamera->position(), posBird, up);
}
```

### Animating the Bird
As we already had several bird models we wanted also to animiate them. The rigging of the bird was done using blender. 
We did not want to play the animation the whole time so we thought to only acivate it while pressing the space key.

```cpp
float AnimationSpeed = 1000 / 10.0f;
if (m_RenderWin.keyboard()->keyPressed(Keyboard::KEY_SPACE, true)) {
	SkeletalAnimationController::Animation* pAnim = m_BipedController.createAnimation(0, AnimationSpeed, 0.0f);
	m_Bird.activeAnimation(pAnim);
}
```

You need load the mesh and the according animation beforehand. 

### Collision detection
In order to recognize a mistake (i.e. flying into a building) or checking if the bird is in a checkpoint we need to use collision detection.
Creating a collision detection for ourselfs was not feasible. So we took an existing library. This library is called fcl ([Flexible-collision-library](https://github.com/flexible-collision-library/fcl)).

As you see our scene is not very complex as well as our models, so it is best to use approximations for the collision and not the meshes itself. This safes computing power and in the future we could implement the meshes at any time.
For the bird we used a sphere as collision shape and for the building a simple box.

Before we implement anything we need to include fcl itself. After that we set fcl up.
```cpp
#include "fcl/narrowphase/collision_object.h"
#include "fcl/narrowphase/distance.h"

// setup for collision detections
auto solver_type = fcl::GJKSolverType::GST_LIBCCD;
using fcl::Vector3;
using Real = typename fcl::constants<float>::Real;
const Real eps = fcl::constants<float>::eps();

fcl::CollisionRequest<float> collision_request(1 /* num contacts */,
				true /* enable_contact */);
collision_request.gjk_solver_type = solver_type;

// the lambda function itself to handle collisons
auto evaluate_collision = [&](
	const fcl::CollisionObject<float>* s1, const fcl::CollisionObject<float>* s2, bool* col) {
	// Compute collision.
	fcl::CollisionResult<float> collision_result;
	std::size_t contact_count = fcl::collide(s1, s2, collision_request, collision_result);

	// Test answers
	if (contact_count >= collision_request.num_max_contacts) {
		std::vector<fcl::Contact<float>> contacts;
		collision_result.getContacts(contacts);
		//GTEST_ASSERT_EQ(contacts.size(), collision_request.num_max_contacts);
		*col = true; // if the color is true then we know there is a collision
	}
}
```

Then we gather all information we need for the bird
```cpp
Eigen::Vector3f BirdPos;
Eigen::Quaternionf BirdRot;
Eigen::Vector3f BirdScale;
m_BirdTransformSGN.buildTansformation(&BirdPos, &BirdRot, &BirdScal);

// need biggest dimesion for the bounding sphere 
auto birdSphere = m_BirdSGN.actor()->boundingVolume().boundingSphere();
m_max_scale_bird = std::max(std::max(BirdScale.x(), BirdScale.y()), BirdScale.z());

// set the radius of the bird
auto bird_collision = std::make_shared<fcl::Sphere<float>>(birdSphere.radius() * m_max_scale_bird);
fcl::Transform3<float> X_WS = fcl::Transform3<float>::Identity();
fcl::CollisionObject<float> bird_collision_geometry(bird_collision, X_WS);

// also need to set the rotation of the bird
fcl::Transform3f bMat = fcl::Transform3f::Identity();
bMat.translate(BirdPos); 
bMat.rotate(BirdRot); 

m_birdTestCollision = bMat.matrix();
bird_collision_geometry.setTransform(bMat);
```

The same we do for the buildings. Here we need to set the bounding box according to the differerent models. Oftentimes the model itself is not in the origin of the objectspace so we need to transform it beforhand. 
If you want to read more, you can do that here: https://github.com/flexible-collision-library/fcl/blob/master/test/test_fcl_sphere_box.cpp