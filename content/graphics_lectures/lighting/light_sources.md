---
title: "Light Sources"
date: 2022-10-22T18:53:54+02:00
draft: false
weight: 20
math: true
---

#### Light sources are an important aspect of computer graphics as they play a crucial role in creating a realistic scene. In this chapter, we will discuss the basics of implementing light sources in C++ to place them in a scene.

## Types of Light Sources
There are several types of light sources that can be used in computer graphics, each with their own properties and characteristics. The most common types of light sources include:

- Point Light Sources: A point light source emits light uniformly in all directions from a single point in space. It is typically used to simulate light sources such as light bulbs or candle flames.

- Directional Light Sources: A directional light source emits light in a single direction from an infinite distance away. It is typically used to simulate light sources such as the sun or moon.

- Spot Light Sources: A spot light source emits light in a cone-shaped beam from a single point in space. It is typically used to simulate light sources such as flashlights or stage lights.

## Implementing Light Sources in C++
In order to implement light sources in C++, we need to create a data structure that represents the properties of each light source. This data structure should include information such as the position of the light source, its color and intensity, and its type.

Once we have created the data structure, we can place the light source in the scene by setting its position and orientation. We can then calculate the amount of light that each object in the scene receives from the light source using various lighting models such as the Phong lighting model.

Here's an example of how we might implement a point light source in C++:
```cpp
struct PointLight {
  glm::vec3 position;
  glm::vec3 color;
  float intensity;
};

PointLight light;
light.position = glm::vec3(0.0f, 2.0f, 0.0f);
light.color = glm::vec3(1.0f, 1.0f, 1.0f);
light.intensity = 1.0f;
```
In this example, we have created a **PointLight** struct that includes the position, color, and intensity of the light source. We have then set the position of the light source to (0, 2, 0), its color to white, and its intensity to 1.0.

We can then use this **PointLight** struct to calculate the amount of light that each object in the scene receives from the light source.

## Conclusion
Light sources are an important aspect of computer graphics and play a crucial role in creating a realistic scene. By implementing light sources in C++, we can place them in a scene and use them to calculate the amount of light that each object in the scene receives. By understanding the basics of light sources and how to implement them in C++, we can create more realistic and visually appealing 3D graphics.

---

{{% notice info %}}
This chapter was written on a test basis with the text AI ChatGPT.
{{% /notice %}}