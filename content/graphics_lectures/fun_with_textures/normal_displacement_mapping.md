---
title: "Normal and Displacement Mapping"
date: 2022-10-22T18:53:54+02:00
draft: false
weight: 10
---

#### Normal and displacement mapping are techniques used in 3D computer graphics to add detail and realism to surfaces without actually increasing the number of polygons in the model. These techniques are widely used in video games and visual effects to create more realistic and detailed objects and environments.

## Normal Mapping
Normal mapping is a technique used to simulate the appearance of bumps and crevices on the surface of a model without actually changing its shape. This is accomplished by using a normal map, which is an image that encodes the direction of surface normals for each pixel on the surface of the model.

When the model is rendered, the normal map is used to calculate the lighting and shading on the surface, creating the illusion of depth and texture. This technique is often used to add detail to low-polygon models or to create the appearance of fine surface details that would be difficult or impossible to model directly.

## Displacement Mapping
Displacement mapping is a technique used to actually deform the surface of a model, creating the appearance of depth and texture. This is accomplished by using a displacement map, which is an image that encodes the amount and direction of displacement for each pixel on the surface of the model.

When the model is rendered, the displacement map is used to displace the surface of the model, creating the appearance of bumps and crevices. This technique is often used to add fine surface details to high-polygon models or to create the appearance of complex geometry that would be difficult or impossible to model directly.

## Implementation
Both normal and displacement mapping are implemented in a similar way in 3D computer graphics. The process involves:

1. Creating a high-resolution model or texture that contains the fine surface details to be added to the surface of the model.

2. Generating a normal or displacement map that encodes the direction or amount of displacement for each pixel on the surface of the model.

3. Applying the normal or displacement map to the surface of the model during rendering to create the appearance of depth and texture.

## Benefits
The benefits of using normal and displacement mapping in 3D computer graphics are numerous. These techniques allow for:

- More efficient use of resources: By adding detail to surfaces without increasing the number of polygons in the model, normal and displacement mapping allow for more efficient use of computing resources and memory.

- More realistic and detailed surfaces: Normal and displacement mapping allow for the creation of more realistic and detailed surfaces, adding depth and texture to objects and environments.

- More flexibility in design: By adding detail to surfaces through mapping rather than modeling directly, designers have more flexibility in the design process, allowing for faster iteration and experimentation.

## Conclusion
Normal and displacement mapping are powerful techniques used in 3D computer graphics to add detail and realism to surfaces without actually increasing the number of polygons in the model. By using normal or displacement maps to simulate the appearance of bumps and crevices on the surface of a model, designers can create more realistic and detailed objects and environments. With their ability to save computing resources, provide more flexibility in design, and enhance the visual appeal of a scene, normal and displacement mapping are valuable tools for anyone working in 3D computer graphics.

---

{{% notice info %}}
This chapter was written on a test basis with the text AI ChatGPT.
{{% /notice %}}