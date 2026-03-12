# UE5 Footstep Sounds Using Physical Materials

## 🖼️ Preview

![Footstep System](Media/1.gif)

## 🧱 Features

**Physical Surface Detection**

- Uses **Line Trace by Channel** inside an Anim Notify to detect the surface beneath the player
- Retrieves the hit surface using **Get Surface Type**
- Uses **Switch on EPhysicalSurface** to determine which audio to trigger
- Footstep sound automatically adapts to the environment

**Custom Footstep Anim Notify**

- Custom **AN_Footsteps** blueprint created from the **AnimNotify** class
- Runs detection logic exactly when the character's foot contacts the ground
- Uses **Mesh Comp → Get Owner** to identify the player character
- Ignores the character during the trace using **Actors to Ignore**

**Surface Detection Line Trace**

- Trace starts at the character location
- Uses **Get Actor Up Vector** multiplied by a negative value to trace downward
- Trace length set to **150 units** to reliably detect ground surfaces
- Uses **Break Hit Result** to access the exact impact point for sound playback

**Dynamic Footstep Sound System**

- Three unique surface types implemented:
  - **Water**
  - **Rock**
  - **Zap (Hard Surface)**
- Uses **Play Sound at Location** to trigger audio at the exact footstep location
- Each surface triggers a unique Sound Cue

**Randomized Sound Cues**

- Footstep audio controlled through **Sound Cue graphs**
- Multiple audio clips connected to a **Random node** to prevent repetition
- **Modulator node** adds pitch and volume variation
  - Pitch: **0.8 – 1.2**
  - Volume: **0.85 – 1.0**
- Creates more natural sounding footsteps

**Physical Materials System**

- Custom physical materials created:
  - **PM_Rock**
  - **PM_Water**
  - **PM_Zap**
- Each material assigned a **Surface Type** in Project Settings
- Materials applied to world surfaces using **Phys Material Override**

**Animation Driven Footsteps**

- Footsteps triggered using animation notifies instead of movement speed
- **AN_Footsteps** placed on the **Surface_Footsteps notify track**
- Notifies aligned with **L and R foot contact markers**
- Ensures footsteps stay perfectly synchronized with animation

**Surface Testing Environment**

- Simple test level created using multiple **plane meshes**
- Each plane assigned a different surface material
- Allows easy testing of the dynamic footstep system across different surfaces

## 🚀 Result

The finished system dynamically detects the surface the player is walking on and plays the appropriate footstep sound in sync with the animation.

Because the system is built using **Physical Materials and Anim Notifies**, it is fully scalable. Additional surfaces, sounds, or effects like **particles, decals, or footprints** can be added without changing the core system, making it a flexible foundation for more advanced audio and gameplay interactions.