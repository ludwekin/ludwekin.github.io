---

title : "quadcopter"

published : false

---

This article may be visited in companion with [article-fly-control](ludwekin.github.io/posts/fly-control).

### f450

Let's talk about "f450" first.

What is the F450?

FlameWheel 450 is f450.

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


### back emf

When the magnet rotor of a brushless motor rotates next to the stator coils, according to Faraday’s law of electromagnetic induction, a voltage is induced in the stator coils. This induced voltage is called back electromotive force (back EMF).  

How ESC Detects Back EMF?





Modern ESCs (running Field Oriented Control) operate in a “dual-task” mode:

1. **Most of the time**: Actively drive current into the motor windings to generate torque.  
2. **Extremely short intervals**: Momentarily stop driving one winding and, during this “idle window,” measure the back EMF voltage on that coil.  

This cycle repeats at very high frequency (thousands to tens of thousands of times per second). The ESC’s MCU uses dedicated **ADC hardware** to sample these voltages quickly and accurately.

### From Back EMF to RPM 

1. **Measure period**  
   - The MCU times the interval between two zero-crossings of the back EMF.  
   - This interval is the electrical period (T).  

2. **Compute frequency**  
   - Electrical frequency: `f = 1 / T`  

3. **Convert to RPM**  
   - With the motor’s pole pair count known, electrical frequency converts to mechanical RPM.  
   - Formula:  
     ```
     RPM = (f * 60) / Pole_Pairs
     ```  
   - Explanation:  
     - Multiply by 60 to convert from revolutions per second to per minute.  
     - Divide by pole pairs because one mechanical revolution produces multiple electrical cycles (equal to the number of pole pairs).  


### reciver, fpv glasses

What's rk5808? Can a fpv simulate glassed be made by myself?


### elrs & 2.4g

What fuck? elrs is lora technology.




### rough flight f103

This link is a video to fly with a stm32.

[f450-f103](https://www.bilibili.com/video/BV1Us4y1U7H1/?spm_id_from=333.1387.homepage.video_card.click&vd_source=bdb655fa0a3eb3c043d2c9524d6aa41d)


