---

title : "Ardupilot"

published : false

---

Ardupilot is another open-source project.


### ground control

ArduPilot can be used with many different ground stations.




### github libraries structure

1. Top-Level Vehicle Directories
- `ArduCopter/` → Multicopter flight control logic  
- `ArduPlane/` → Fixed-wing aircraft  
- `ArduSub/` → Submarines/ROVs  
- `Rover/` → Ground rovers, cars, boats  





2. Shared Libraries
- `libraries/`  
  - Core functionality shared by all vehicles:  
    - `AP_Math` (math utilities)  
    - `AP_NavEKF` (state estimation, sensor fusion)  
    - `AP_HAL` (Hardware Abstraction Layer: drivers for boards, sensors, IO)  
    - `AP_Mission` (mission planning)  
    - `AP_GPS`, `AP_Baro`, `AP_Compass` (sensor drivers)  


### waf

**Waf** is a **Python-based build system**. It automates tasks like configuring, compiling, and installing software projects.  
ArduPilot and some other large C++ projects use it instead of `make` or `CMake`.


- Written in Python (portable, no extra dependencies).  
- Supports multiple toolchains and platforms.  
- Faster and more flexible than legacy `make`.  
- Handles **parallel builds**.  
- Extensible with Python scripts.  
- Used by ArduPilot to manage complex targets (Copter, Plane, Rover, Sub, SITL).  



### ardupilot 3.6.9

Company f450 drone with ardupilot 3.6.9.

