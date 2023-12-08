---
title: "Map-Builder Prototype"
date: 2022-11-07T18:07:37+01:00
draft: false
weight: 20
chapter: false
---

## Why a mapbuilder?
During the creation of the [Checkpoin Race](/showcase/checkpoin_race/) , we wanted to try and create a map builder that would allow you to create and play new maps. We were only able to develop a simple prototype, but we still wanted to show how we did it.


![Bird flying through a checkpoint](/mapbuilder.gif)

---
## Our approach
When starting out, we decided to use a grid-based builder to keep the overall structure simple.

Our aim is to highlight the specific functions we have implemented, starting with the creation of the grid
### Grid

```cpp
void createGrid() {
	// set the starting position for the active tile
	activeTileRow = 0;
	activeTileCol = 0;

	const float tileSize = 10.0f;

	// initialize gridBuildingSGNs to indicate that there is no building on any grid field 
	gridBuildingSGNs.resize(numRows, vector<pair<bool, int>>(numCols, { false, -1 }));
	gridBuildingSGNSs.resize(numRows, vector<pair<SGNTransformation*, SGNGeometry*>>(numCols));



	// loop for creating and placing the grid tiles
	for (int row = 0; row < numRows; ++row) {
		for (int col = 0; col < numCols; ++col) {
			// position for each tile
			float x = col * tileSize;
			float z = row * tileSize;

			// usage of the cube model
			SGNTransformation* pTransformSGN = new SGNTransformation();
			pTransformSGN->init(&m_GridTileGroupSGN);
			pTransformSGN->translation(Vector3f(x, 0.0f, z));
			pTransformSGN->scale(Vector3f(1.0f, 0.5f, 1.0f)); // scale for normal tiles

			SGNGeometry* pGeomSGN = new SGNGeometry();
			pGeomSGN->init(pTransformSGN, &m_Cube);

			// add tile to the scene
			m_GridTilesTransformSGNs.push_back(pTransformSGN);
			m_GridTileGeoSGNs.push_back(pGeomSGN);

			// scale active tile (if needed)
			if (row == activeTileRow && col == activeTileCol) {
				pTransformSGN->scale(Vector3f(1.0f, 1.0f, 1.0f)); // scale for active tile
			}
		}
	}
}
```


### Update active tile

```cpp
void updateActiveTileScaling() {

	for (int i = 0; i < m_GridTilesTransformSGNs.size(); ++i) {
		m_GridTilesTransformSGNs[i]->scale(Vector3f(1.0f, 0.5f, 1.0f));
	}
	// Find the active tile using activeTileRow and activeTileCol
	int activeTileIndex = activeTileRow * numCols + activeTileCol;

	if (activeTileIndex >= 0 && activeTileIndex < m_GridTilesTransformSGNs.size()) {
		// Find the transformation SGN of the active tile
		SGNTransformation* pActiveTileTransform = m_GridTilesTransformSGNs[activeTileIndex];

		// Set the scaling of the active tile
		pActiveTileTransform->scale(Vector3f(1.0f, 1.0f, 1.0f));
	}
}

```

#### Corresponding usage via Up, Down, Left, Right keys

```cpp
//...
if (m_RenderWin.keyboard()->keyPressed(Keyboard::KEY_LEFT, true)) {
	// Move the active tile to the left (reduce col)
	if (activeTileCol > 0) {
		activeTileCol--;
		updateActiveTileScaling();
	}
}
if (m_RenderWin.keyboard()->keyPressed(Keyboard::KEY_RIGHT, true)) {
	// Move the active tile to the right (increase col)
	if (activeTileCol < 3) { // 4 Spalten insgesamt (0, 1, 2, 3)
		activeTileCol++;
		updateActiveTileScaling();
	}
}
//
//...code for handling height adjustment before
//
else if (m_RenderWin.keyboard()->keyPressed(Keyboard::KEY_UP, true)) {
	// Move the active tile upwards (reduce row)
	if (activeTileRow > 0) {
		activeTileRow--;
		updateActiveTileScaling();
	}
}
//
//...code for handling height adjustment before
//
else if (m_RenderWin.keyboard()->keyPressed(Keyboard::KEY_DOWN, true)) {
	// Move the active tile down (increase row)
	if (activeTileRow < 3) { // 4 rows in total (0, 1, 2, 3)
		activeTileRow++;
		updateActiveTileScaling();
	}
}
```
### Handle placement of the buildings
{{% notice info %}}
To save both the geometry and transform node of each tile, we use a 2D grid.\
Each element in the grid is a pair containing an SGNTransformation pointer and an SGNGeometry pointer.\
"vector<vector<pair<SGNTransformation*, SGNGeometry*>>> gridBuildingSGNSs;"
{{% /notice %}}

```cpp
void placeBuilding(int variant, float x, float y, float z) {
	// Create transformations for the building
	SGNTransformation* pBuildingTransform = new SGNTransformation();
	pBuildingTransform->init(&m_BuildingGroupSGN);
	pBuildingTransform->translation(Vector3f(x, y, z));
	pBuildingTransform->scale(m_building_scale);

	
	if (variant == 2) pBuildingTransform->scale(Vector3f(2.5f, 5.0f, 2.5f));

	// Create geometry node for the building
	SGNGeometry* pBuildingGeometry = new SGNGeometry();
	pBuildingGeometry->init(pBuildingTransform, &m_Buildings[variant]);
    // save the geometry and transform nodes
	gridBuildingSGNSs[activeTileRow][activeTileCol].first = pBuildingTransform;
	gridBuildingSGNSs[activeTileRow][activeTileCol].second = pBuildingGeometry;
}
```
#### Replace in case there is already a building on the active tile

```cpp
void replaceBuilding(int variant) {
	if (variant == 2) {
		gridBuildingSGNSs[activeTileRow][activeTileCol].first->scale(Vector3f(2.5f, 5.0f, 2.5f));
	}
	else {
		gridBuildingSGNSs[activeTileRow][activeTileCol].first->scale(Vector3f(5.0f, 5.0f, 5.0f));
	}
	gridBuildingSGNSs[activeTileRow][activeTileCol].second->init(gridBuildingSGNSs[activeTileRow][activeTileCol].first, &m_Buildings[variant]);
}
```

### Placement at active tile
{{% notice info %}}
Furthermore we save the state of each tile, i.e. whether a building is placed and which variant in the following 2D grid:\
"vector<vector<pair<bool, int>>> gridBuildingSGNs;"
{{% /notice %}}
```cpp
void placeBuildingAtActiveTile() {
	// Check whether the active tile is valid
	if (activeTileRow >= 0 && activeTileRow < numRows && activeTileCol >= 0 && activeTileCol < numCols) {
		// Check whether there is already a building on this tile
		if (gridBuildingSGNs[activeTileRow][activeTileCol].first) {
			// Place the building on the active tile
			selectedBuildingVariant = gridBuildingSGNs[activeTileRow][activeTileCol].second;
			selectedBuildingVariant = (selectedBuildingVariant + 1) % 3;
			// replace current building
			replaceBuilding(selectedBuildingVariant);
			
			// set new variant inside the 2D grid for active tile
			gridBuildingSGNs[activeTileRow][activeTileCol].second = selectedBuildingVariant;
		}
		else {
			placeBuilding(0, activeTileCol * 10.0f + 2.5f, 1.0f, activeTileRow * 10.0f);
			// Update gridBuildingSGNs for the current active tile
			gridBuildingSGNs[activeTileRow][activeTileCol].first = true;
			gridBuildingSGNs[activeTileRow][activeTileCol].second = 0;
		}

	}
}
```
#### Corresponding keys
```cpp
void handleBuildingPlacementInput() {
	// Check whether a key has been entered for the building placement
	if (m_RenderWin.keyboard()->keyPressed(Keyboard::KEY_B, true)) {
		// Place the selected building on the active tile
		placeBuildingAtActiveTile();

		// Increase the index of the selected building variant for the next building
		selectedBuildingVariant = (selectedBuildingVariant + 1) % 3; // Assumption: There are 3 building variants
	}
}
```

### Height adjustment for current building
To adjust the height on an active tile with a building placed:
```cpp
//...
if (m_RenderWin.keyboard()->keyPressed(Keyboard::KEY_LEFT_CONTROL) && m_RenderWin.keyboard()->keyPressed(Keyboard::KEY_UP, true)) {
    //Only if there is a building on the active tile :)
	if (gridBuildingSGNs[activeTileRow][activeTileCol].first == true) {
		float currentX = gridBuildingSGNSs[activeTileRow][activeTileCol].first->translation().x();
		float currentY = gridBuildingSGNSs[activeTileRow][activeTileCol].first->translation().y();
		float currentZ = gridBuildingSGNSs[activeTileRow][activeTileCol].first->translation().z();

		gridBuildingSGNSs[activeTileRow][activeTileCol].first->translation(Vector3f(currentX, currentY + 1.0f, currentZ));
	}
	
}
//
//...following is the case for only pressing KEY_UP
//
if (m_RenderWin.keyboard()->keyPressed(Keyboard::KEY_LEFT_CONTROL) && m_RenderWin.keyboard()->keyPressed(Keyboard::KEY_DOWN, true)) {
    //Only if there is a building on the active tile :)
	if (gridBuildingSGNs[activeTileRow][activeTileCol].first == true) {
	float currentX = gridBuildingSGNSs[activeTileRow][activeTileCol].first->translation().x();
	float currentY = gridBuildingSGNSs[activeTileRow][activeTileCol].first->translation().y();
	float currentZ = gridBuildingSGNSs[activeTileRow][activeTileCol].first->translation().z();

	gridBuildingSGNSs[activeTileRow][activeTileCol].first->translation(Vector3f(currentX, currentY - 1.0f, currentZ));
	}
}
//
//...following is the case for only pressing KEY_DOWN
//
```