---

title : "quadcopter"

published : false

---

This article may be visited in companion with [article-fly-control](ludwekin.github.io/posts/fly-control).

### f450

Let's talk about "f450" first.

What is the F450?

The F450 is a quadcopter frame kit, originally manufactured by DJI. It's a relatively simple, affordable, and well-documented platform that has become a standard for learning drone development and flight controller programming.

The number "450" refers to the diagonal motor-to-motor distance in millimeters. This is a standard naming convention in the multirotor community:

    - The diagonal wheelbase (distance between opposite motors) is 450mm
    - This puts it in the "medium" size category - not too small to be fragile, not too large to be unwieldy
    - Other common sizes follow the same pattern: F330 (330mm), F550 (550mm), etc.

### download a f450 chasis example

This is a link to grabcad. Where you can download a step file of f450 chasis.[f450-whole-step](https://grabcad.com/library/f450-quadcopter-frame-with-pixhawk-2-4-8-flight-controller-1).

Or you can alternatively download my [archice file](/assets/f450/F450%20Quadcopter%20Frame%20with%20Pixhawk%202.4.8%20Flight%20Controller.step).

### TORAYCA & Toray Industries

By the way, TORAYCA® is the trademark for **carbon fibers and composite materials** developed and produced by **Toray Industries, Inc.**, a Japanese multinational company. It is widely used in aerospace, automotive, sports equipment, industrial machinery, and civil engineering.

In the field of quadcopter, carbon fibers are very common in chasis design.

### DXF format

DXF is a CAD data file format** developed by Autodesk in 1982 to allow interoperability between AutoCAD and other design programs.  

    - It stores 2D and 3D drawings in a text-based or binary format.  
    - Acts as a neutral exchange format compared to the proprietary DWG format.

Usually, we prepare these files for CNC machining, 3D printing, or laser cutting.  

Now you know that you can just download some DXF files to make your own carbon fiber chasis.

### about 3D printed chasis

Using 3D printed chasis is not recommanded in real flight machines.

As [cjdavies](https://www.reddit.com/r/diydrones/comments/1i3dkyp/comment/m7miokr/?utm_source=share&utm_medium=web3x&utm_name=web3xcss&utm_term=1&utm_content=share_button)replys:
```markdown
The main reason why we as a community generally try to discourage people from 3D printing frames is because there is such a widespread tendency to simply copy an existing flat CFRP sheet design using 3D printing, with little or no consideration to the drastically different material properties of CFRP vs printable thermoplastics/polymers.

The frame in the video you linked is a perfect example of this. If you were to simply copy a frame design like that using a 3D printer, it would have substantially less stiffness than the original CFRP version - even in PA12. Stiffness is one of the most important properties when it comes to the flight performance of a drone frame.

Now that’s not to say that you can’t 3D print a good frame. However it would have to be designed from the ground up to take into account the properties of the material that you are using. If you take a look at DJI’s consumer drones, they are all plastic with little or no carbon fiber reinforcement. But they are designed as monocoques, a completely different approach to flat pieces of CFRP.

One option that you may not have considered is to use your access to 3D printing facilities to produce prototypes of a design that you will ultimately have CNC cut from CFRP. This is what I’ve done for all of my own frame designs - I printed off prototypes in PLA & used them to check fitment, clearances, etc. before sending the final design to a CNC factory to produce the real thing to fly.
```

### a cool design 

However, I find one that is very suitbale for 3D print.

[Aether](https://makerworld.com.cn/zh/models/427062-bm-yi-tai-4-4-ying-cun-yi-ti-shi-fpv-wu-ren-ji-ji?from=search#profileId-339009). This one is designed as a whole which is brain-new. In case that the link gets invaild, you can download from my [site](/assets/Aether/Aether4.stl). 

By the way, this design is very interesting that I just want to create a new [post](ludwekin.github.io/aether) for it.