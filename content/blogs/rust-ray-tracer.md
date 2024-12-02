+++
date = '2024-12-02T12:28:21+05:30'
draft = false
title = 'Rust Ray Tracer'
+++
# Building a Ray Tracer in Rust

Over the past month, I embarked on an exciting journey to build a ray tracer in Rust, inspired by the *"Ray Tracing in One Weekend"* series. The concept of ray tracing has always intrigued me, and finding this guide was the spark I needed to finally dive in. This project became an immersive exploration into the realms of computer graphics, the Rust programming language, and the art of rendering. If you're curious, you can check out the repository [here](https://github.com/Dhull442/rust-ray-tracer).  

In this blog, I’ll walk you through the project's evolution, one scene at a time, highlighting the features implemented and the thought process behind each step.

## Journey:

### Scene 1:
The journey began with laying out the very basic scene setup, creating a fixed camera, creating an image scene and a frame, and implementing the crucial foundation structures of Ray, Vector, Color, etc. Initially, the image output was in `.ppm` format, which basically prints all the pixel values, that is, R, G, and B, separated by a comma. This was fairly simple to execute and generated a `hello world` picture of computer graphics. On discovering that markdown doesn't support rendering `.ppm` images, I switched to creating a `.png` file, which was fairly simple as there was an `Image` crate in Rust. We render this image: <br>
![Scene 1](https://github.com/Dhull442/rust-ray-tracer/blob/3adae2c6b7971585c1915aac416240c4bb260a73/image.png "Scene 1")

### Scene 2:
Now that we had a skeleton for the ray tracer, it was time to dive into the rendering logic. This started with implementing a hard-coded sphere at the center with colors corresponding to the vector of the ray collision point from the center. This generated a colorful sphere at the center. Now, the next structural step was to make a `hittable` class containing the different types of objects, their specific details, and the logic for color generation. This was again a structural step, and some meat was added over the skeleton of the ray-tracer. Doing a basic scene creation with two spheres leads us to this: <br>
![Scene 2](https://github.com/Dhull442/rust-ray-tracer/blob/6213521fcabcba80313a93e1811cb9cdd812cad4/image.png "Scene 2")

### Scene 3:
If you notice carefully, the object edges in the previous scene are a bit too crisp or edgy. We don't want this in a render and introduce aliasing. Now, aliasing is basically saying that instead of just using a single ray per pixel, you have a bunch of rays going through the same pixel, and each of these rays has a little random part to it. We basically say that a ray originates from (x + random(),y+random()) instead of (x,y). Then, we averaged out the color, which led us to create a bit smoother edge. At this point, I was very tired that day and probably introduced a bad bug because the next scene was not something I was expecting: <br>
![Scene 3](https://github.com/Dhull442/rust-ray-tracer/blob/3353f535e2c8d8d1b97421e71839dc21985ae2a0/image.png "Scene 3")<br>
<sup>Welcome to Rainbow Land! Hey, don't laugh. I understand that the image is not ideal, but at least the edges are smoother now, and you get to experience some pretty trippy graphics.  So, not that bad, huh?</sup>

### Scene 3.5:
I understood that the bug before was not something I introduced in the previous step but a design choice made earlier that was simply coming to haunt me. To simplify my initial design and understanding that color can only have values from 0 - 255. I had my color set to u8 type. Any value over 255 would cause it to overflow and lead us to Rainbow Land. Now, this is a crucial step because when you are in the world of computer graphics, the colors are between 0 - 255 only when we are outputting them; inside the system, I will soon have colors equalling 4000. So, instead of u8, I changed my system to have colors as float_64 and wrote a function as_pixel(), which converts the values to the real world and outputs them in an image. This generated the image with no trippy details and surprises, boo, floating points: <br>
![Scene 3.5](https://github.com/Dhull442/rust-ray-tracer/blob/6f7c19211819a68a9a18760ea105767c55003e45/image.png "Scene 3.5")

### Scene 4:
Now that our camera had the ability to perform basic camera stuff, we moved to improving the objects. The first update was creating a material for the objects. This will help us make more detailed scenes and send all the color guessing to the material rather than the object. The object will basically tell its material to tell the Ray guy the color associated with it. To start with, we only work with solid colors, also known as lambertians. Now these lambertians have a characteristic diffusion, where due to the uneven surface, they just send the rays in random directions and grant it some of its color. Implementing this, we get a sad image, mainly because I had set the material color to gray right now: <br>
![Scene 4](https://github.com/Dhull442/rust-ray-tracer/blob/c6a5aff189e74d9086bdaede44e6910abe65d8a4/image.png "Scene 4")

### Scene 5:
Now that we have our lambertians or the solids for the laymen out there, we also need to have a metal object. As Wikipedia describes it, A metal is a material that, when polished or fractured, shows a lustrous appearance and conducts electricity and heat relatively well. These properties are all associated with having electrons available at the Fermi level, as opposed to nonmetallic materials, which do not.
To implement this, we need to create a model of the atom that will have the associated activation levels according to the metal in question and give our ray color based on how the electrons behave at the fermi level. This also involves implementing a fermi-level simulation as well.
OR, we can simply say that the metal is a mirror-like object with some intrinsic color and a fuzz parameter that dictates the reflectance level.
I chose the second route here, but you can definitely go the first route and spend much of your time in physics. Creating a metal object and generating the bounce-back ray was fairly straightforward for a sphere. Creating the metal objects and setting a scene where a Lambertian is being bullied by two metal spheres, we get something like this: <br>
![Scene 5](https://github.com/Dhull442/rust-ray-tracer/blob/55393f949542fbc589876a32b0e9cca98779a862/image.png "Scene 5") <br>
<sup>DISCLAIMER: this blog post does not condone or justify any metals or other materials bullying the fairly simple and cute lambertians. If you are a Lambertian facing such discrimination or hate, please contact the Lambertian bullying helping number listed below. </sup>

### Scene 6:
Now that we have metals and lambertians, there is another material that can be added, that is dielectrics. A glass or refractive material is called a dielectric for all the laymen and women out there. I have only implemented dielectrics without colors; you can argue that glass can also be colored, but that argument is not valid in my world. Replacing the bully metal with a dielectric and to show that my dielectrics can do tricks and is cool beyond comprehension, I have created a bubble inside the dielectric, which is basically filling the glass sphere with another dielectric with a refraction index of 1.0. Rendering this, we get:<br>
![Scene 6](https://github.com/Dhull442/rust-ray-tracer/blob/ee6b73a34b0efefa5d1bab003862b6d08a0e5eb0/image.png "Scene 6") <br>
<sup>PS: For those of you saying that the bubble looks a bit odd, you can go f**k yourself because that is how the physics work in my world. We'll talk when you build a ray tracer.</sup>


### Scene 7:
Now that we have a little more detailed scene creation, we need to work on the camera. The camera, so far, was a fixed hole at the origin and could only produce images that looked in the hard-coded direction. We don't want that. So, I introduce camera vectors, which will help in getting the `u, v, w` vectors for our rendering world. These `u, v, w` vectors are basically `x, y, z` vectors from the camera to the scene. To get these, we set the point from where to look and the point from where to look at, as well as a vector stating which direction is our `y` or the up direction. We then take several cross and dot products to get our 3 directions. I then moved the camera a bit far away and placed it to look down on the sphere. Doing this, we get something like this: <br>
![Scene 7](https://github.com/Dhull442/rust-ray-tracer/blob/7b14024eefc2409f25605a4aa641b6c6ad41fa9d/image.png "Scene 7")

### Scene 7.5:
Did you notice any unexpected behavior Or were you just blown away by the new camera, yes it's a new model and give you cool images. No, but seriously, there is a bug. Can you see that the image is a bit more rounded than before? Do the spheres not look like they are in a straight line? And can you see the curvature of the ground even though the ground sphere was fairly big? Yes, there was a bug, and I had to spend a lot of time debugging everything, only to figure out that my cross product was not correct and the z vector of the cross product was shorter than the others, which led to this rounded world. This also helped me understand that I could basically alter these vectors to introduce the iPad filters in my ray tracer. So it was more like a feature than a bug. Anyway, I wasn't able to detect this bug until way after, and the next few images will be a bit distorted. Beer with me?

### Scene 8:
We are unaware of the bug I talked about previously and would go on to implement another important feature: depth of field. This depth of field would lead to our images resembling the real world a bit more and give our rendering more depth. To do this, you basically need to understand that a camera is not a point object and actually captures the image using a circle, the wideness of this circle dictates our depth of field and the distance between look from and look at point will give us our focal dist. Now that we have established how real-world cameras work, we just need to say that, okay, the ray might also be coming from a disk rather than the camera point. The depth of field determines the size of the disk. This gives us a bit more realistic yet distorted image: <br>
![Scene 8](https://github.com/Dhull442/rust-ray-tracer/blob/3a30e3d6f6b3543e5d44e16da74d56bd75275391/image.png "Scene 8") <br>
<sup>Notice that the ground is bit more fuzzy at the top? That is because of this depth of field. You'll be able to witness it better in our demo render</sup>

### Scene 9:
Now that we have a fairly complicated ray tracer, it was time to build a demo image to show off basically. To create this, we created 3 spheres of 3 different materials that we introduced before and added several small balls to make the scene look cool. We get this: <br>
![Scene 9](https://github.com/Dhull442/rust-ray-tracer/blob/88c249d570a4a96e404273abf22cbf6fad619bbf/image.png "Scene 9")<br>

### Scene 9.5:
Can you notice the distortion now? Yeah, this was the first time that I realized that there was definitely a bug in the program and started doing what I mentioned in **Scene 7.5**. Killing the bug, we get a cool-looking image: <br>
![Scene 9.5](https://github.com/Dhull442/rust-ray-tracer/blob/220096fbc260fe6669f406c389f3f2e15c038377/image.png "Scene 9.5")<br>
<sup>Will you look at that? Isn't that a state-of-the-art, extra cool image? Can you tell that I didn't take this using a camera? 
I'm sure proud as hell and will be going out to celebrate this.</sup>

### Scene 10:
After rendering the same image multiple times and drooling over it for days, I thought about moving forward. Now, the pain point about the above render was its rendering speed. The above render took me about an hour to generate, and I realized there were several suboptimal parts. One was that I was basically doing a single-thread implementation where I could very easily do a multi-threaded one. I researched this, and since I wanted to see what the author would do, I went in the direction of creating BVH. A BVH is basically a Bouding Volume Hierarchy. Think of it as a tree of bounding boxes of all the objects in the scene. So whenever I want to check the objects my ray hits, instead of cycling through the entire list, I can look at the objects I want. Since the rays bounce off a lot and the image size is huge, we can get a lot of speed-up. When I was creating this tree, everything was going fine until I realized that Rust is not cpp and there is no `nullptr` in Rust as there is in cpp. So how can I go about implementing a tree? This was not a good time to be advocating rust, and I kind of wanted to stop working, thinking that I needed to make a whole new design and understand how Rust goes about creating trees. It turns out that it was much simpler than I had thought. So Rust does have a null pointer, but instead of using it like it's done in CPP, Rust doesn't allow you to just create a pointer to anywhere. This is one of the reasons rust is so good: it's very hard for the user to make an injection like they can do in CPP. Anyway, I implemented this whole structure and tried to render the scene with it, but for some reason, it was not working like it was supposed to. Understanding that this was just a performance issue, I scheduled this task for the future. Here, you can have a render created using BVH Node: <br>
![Scene 10](https://github.com/Dhull442/rust-ray-tracer/blob/2bb3f54b6a813f22032434cf5191cae0cf165955/image.png "Scene 10")


### Scene 11:
Saying bye-bye-bye to BVH, we move on to actually working on the ray tracer and not caring about those false speed-up claims. 
Now things are about to change at the speed of light. I introduced a texture class that would take on the task of creating various textures from the object. Basically, Lambertians can now have textures instead of solid colors. This texture can take on the types of Solid Color, Checkered texture, Image Texture, and Perlin Noise. I have implemented all the previous textures, trust me, but I will only show you my image for perlin noise texture. This was a very cool thing to implement. I also modified the Perlin noise a bit to make it look like marble but forgot to commit it at that point. Here is the image: <br>
![Scene 11](https://github.com/Dhull442/rust-ray-tracer/blob/95dc1bb93ffd373ccb118213bd0acc20b1f50a28/image.png "Scene 11")

### Scene 12:
Now, I have touched the project after a long time due to some prior commitments. Yes, traveling around Goa is a commitment, and I won't work on the project if I don't feel like it. Understood? Sorry about that. Anyway, So now I kind of did a lot of stuff in a single commit, including creating quadrilaterals, adding boxes, creating a light object, and adding a new scene. This scene is called a Cornell box. This was introduced by Mr. Cornell in 1984. I also added a background color instead of generating the color from the ray class. The resulting render has a lot of noise because of reasons that we will discuss later, but the render now looks like this: <br>
![Scene 12](https://github.com/Dhull442/rust-ray-tracer/blob/65340829b51a7f479941ae1d803cd0ed42c8aac5/image.png "Scene 12")


### Scene 13:
Now that we have a fairly complicated build, we can go on and generate a more advanced demo render where we will demonstrate all of our added objects and their traits. I also introduced a medium object, which can be used to add fog, smoke, or clouds. Due to the massive number of objects in the scene and my using a Macbook for ray tracing, the resulting image has a lot of noise: <br>
![Scene 13](https://github.com/Dhull442/rust-ray-tracer/blob/0e547f3f5e602528e82b56b748902ec586ab16e4/image.png "Scene 13")

### Scene 13.5:
Again, since this scene kind-of had everything we wanted, I wanted to generate a fairly smooth image. To do this, I tried to increase the samples per pixel to 10K, which showed that my image would render in 3 days. I then again turned to BVH to fasten my render. After boxing around with BVH, I had to walk away due to multiple punches and empty renders. I then decided to pursue the line of parallel programming and introduced a parallel iteration over the array. This led to the render times being reduced by 10 times, and I was able to get a very good render: <br>
![Scene 14](https://github.com/Dhull442/rust-ray-tracer/blob/956177b5cfc7434c1fcc2368219c7d486b86492c/demo_image_4.png "Scene 13.5")

### Scene 14:
Now that our demo render is good. We can start working on the problem of the weird noise in the Cornell box. The noise is so weird because our renderer doesn't care more about the light than the other objects, and there is no preference. If we introduce a probability distribution function and change the world so that we can list the lights in another list so more rays lead to the light, we get a little bit less noise.<br>
![Scene 14](https://github.com/Dhull442/rust-ray-tracer/blob/8e960c3630c255ba02cb7c6934053aa3aaac00b7/image.png "Scene 14")

### Scene 15:
Making some more technical changes and adding a sphere object in the Cornell box, we get our final render: <br>
![Scene 15](https://github.com/Dhull442/rust-ray-tracer/blob/341c27be3be300fc4b51232bb2589658416ca5e1/image.png "Scene 15")

### Conclusion:
This was more or less the journey I took into building a ray tracer in rust. This helped me understand the basic differences between CPP and Rust. I'd be happy to hear your thoughts about this. Feel free to borrow the project for your own development.


#### How to Use

If you’d like to try the ray tracer yourself:
```bash
git clone https://github.com/Dhull442/rust-ray-tracer.git
cd rust-ray-tracer
cargo run
