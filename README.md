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

# Spawn Items From Breakable Objects

## 🖼️ Preview

![Spawn Items From Breakable Objects](Media/2.gif)

## 🧱 Features

**Chaos Geometry Collection**

- Cube mesh converted into a Geometry Collection using Fracture Mode
- Uniform Fracture applied multiple times to generate breakable fragments
- Bone color visualization disabled for a normal mesh appearance
- Damage Threshold values configured to create layered break behavior
  - Index 0 → 1000
  - Index 1 → 100
  - Index 2 → 10
- Object positioned above the ground so gravity triggers a fracture on impact

**Breakable Actor Blueprint**

- Actor Blueprint created to control the breakable object logic
- Geometry Collection component added to the Blueprint
- Rest Collection assigned to the fractured Geometry Collection asset
- Chaos Physics **Notify Breaks** enabled to allow Blueprint event detection

**Spawnable Item Actor**

- Separate Actor Blueprint created to represent the item revealed after the object breaks
- Static Mesh component added to represent the spawned object
- Cylinder mesh used for demonstration
- Mesh scaled to fit the size of the fractured cube
- Collision preset set to **Overlap All** so the item does not interfere with debris or the player

**Chaos Break Event Logic**

- **On Chaos Break Event** used to detect when the Geometry Collection fractures
- **Do Once** node added to prevent multiple spawn triggers from fragment events
- **Spawn Actor From Class** node used to spawn the item Blueprint
- Collision handling set to **Always Spawn, Ignore Collisions**
- Break event data used to determine spawn location
- **Break Chaos Break Event** node extracts the fracture location
- Location converted using **Make Transform**
- Transform connected to the Spawn Actor node to place the item at the exact break position

**Debris Collision Handling**

- Custom **Debris** collision preset created in Project Settings
- Collision Enabled set to **Physics Only (No Query Collision)**
- Object Type set to **Destructible**
- Visibility, Camera, and Pawn traces set to **Ignore**
- Allows debris to simulate physics without blocking the player or camera

**Collision Profile Per Level**

- Geometry Collection configured with collision profiles per fracture level
  - Index 0 → None
  - Index 1 → Debris
  - Index 2 → Debris
- Ensures fractured pieces do not block player movement after the object breaks


## 🚀 Result

When the Geometry Collection fractures during gameplay, a Blueprint actor is spawned automatically at the exact location where the break occurred. This simple system can be expanded to reveal pickups, collectibles, or gameplay objects whenever destructible items break in the environment.