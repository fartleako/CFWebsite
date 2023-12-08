---
title: "Physically Based Shading"
date: 2022-10-22T18:53:54+02:00
draft: false
weight: 10
math: true
---

#### Physically based shading (PBS) is a rendering technique used in computer graphics to create realistic materials and lighting in 3D scenes. Unlike traditional rendering techniques, which rely on ad hoc methods to simulate lighting and material properties, physically based shading is based on the laws of physics and provides a more accurate and consistent way to render objects.

## Principles of Physically Based Shading
At the heart of physically based shading is the idea that light interacts with surfaces in a physically accurate way. This means that the way a surface reflects and refracts light should be based on its physical properties, such as its roughness, metallicness, and index of refraction.

Physically based shading relies on mathematical models to accurately simulate these physical properties. For example, the Cook-Torrance model is a commonly used model for simulating roughness in metallic surfaces, while the Fresnel equations are used to simulate the way light is reflected and refracted at the surface of an object.

## Implementing Physically Based Shading
Implementing physically based shading involves using these mathematical models to calculate the way light interacts with surfaces in a 3D scene. This can be done using a variety of shading models, such as the Disney principled shading model or the physically based standard shading model.

To implement physically based shading, you need to define the physical properties of each material in your scene, such as its roughness, metallicness, and index of refraction. You also need to define the properties of each light source, such as its color, intensity, and position.

Once you have defined these properties, you can use them to calculate the amount of light that each pixel in the scene receives, taking into account the physical properties of the materials and the lighting conditions in the scene.

## Benefits of Physically Based Shading
Physically based shading provides several benefits over traditional rendering techniques. For example:

- More realistic lighting: Because physically based shading is based on the laws of physics, it provides a more accurate and consistent way to simulate lighting in a scene.

- More realistic materials: Physically based shading allows you to accurately simulate the physical properties of different materials, such as metal, plastic, and glass.

- Greater artistic control: Physically based shading provides a flexible framework that allows artists to create a wide range of materials and lighting conditions, while still maintaining a high degree of realism.

## Conclusion
Physically based shading is a powerful rendering technique that allows you to create more realistic and visually appealing 3D graphics. By using mathematical models to accurately simulate the physical properties of materials and lighting, physically based shading provides a more accurate and consistent way to render objects. With its ability to create more realistic lighting and materials, physically based shading is becoming an increasingly popular technique in the field of computer graphics.

---

{{% notice info %}}
This chapter was written on a test basis with the text AI ChatGPT.
{{% /notice %}}
