+++
date = '2024-12-02T12:28:21+05:30'
draft = false
title = 'Rust Ray Tracer'
+++
# Building a Ray Tracer in Rust

Over the past month, I’ve been working on a **ray tracer in Rust**, inspired by the "Ray Tracing in One Weekend" series. This project has been a deep dive into computer graphics, Rust programming, and the intricacies of rendering. You can explore the repository [here](https://github.com/Dhull442/rust-ray-tracer).  

In this blog, I’ll take you through the major milestones of the project, discussing the features implemented and the work behind them.

## What’s New?

### 1. **Initial Scene Rendering**
The journey began with the basics: rendering a simple scene consisting of spheres and planes. This involved implementing a ray-sphere intersection algorithm and introducing basic Lambertian shading for diffuse materials. The groundwork laid here set the stage for all subsequent features.

**Demo Render:**  
![Basic Scene](https://github.com/Dhull442/rust-ray-tracer/raw/main/images/basic-render.png)

---

### 2. **Adding Reflective Surfaces**
With the basics in place, the next step was to introduce reflective materials. This required implementing reflection calculations in the shading model. The result was the ability to create realistic metallic surfaces that interact naturally with light.

**Demo Render:**  
![Reflection Render](https://github.com/Dhull442/rust-ray-tracer/raw/main/images/reflection-render.png)

---

### 3. **Improving the Camera**
A more realistic camera model was introduced, featuring depth-of-field effects and adjustable aperture. These enhancements allow for photorealistic rendering by simulating the blur seen in real-world optics when focusing on objects at varying distances.

**Demo Render:**  
![Camera Effects](https://github.com/Dhull442/rust-ray-tracer/raw/main/images/camera-effects.png)

---

### 4. **Lighting and Shadow Enhancements**
To improve the visual quality of the renders, I added support for shadow rays and area lights. This upgrade enabled soft shadows, creating smoother lighting transitions and making scenes look more natural.

**Demo Render:**  
![Lighting Improvements](https://github.com/Dhull442/rust-ray-tracer/raw/main/images/lighting.png)

---

### 5. **Advanced Materials: Glass and Refraction**
One of the most exciting updates was the addition of dielectric materials, enabling realistic rendering of glass and other transparent objects. This involved implementing Snell's law for light refraction, combined with reflection to capture the unique properties of glass.

**Demo Render:**  
![Glass Material](https://github.com/Dhull442/rust-ray-tracer/raw/main/images/glass-material.png)

---

### 6. **Monte Carlo Path Tracing**
The final major update so far has been the implementation of Monte Carlo path tracing. This technique simulates the global illumination of light bouncing off surfaces in a physically accurate manner, producing lifelike renders. It marked a shift from simple ray tracing to a more comprehensive rendering solution.

**Demo Render:**  
![Global Illumination](https://github.com/Dhull442/rust-ray-tracer/raw/main/images/global-illumination.png)

---

## How to Use

If you’d like to try the ray tracer yourself:
```bash
git clone https://github.com/Dhull442/rust-ray-tracer.git
cd rust-ray-tracer
cargo run
