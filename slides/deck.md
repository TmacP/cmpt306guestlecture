---
marp: true
title: CMPT 306 Guest Lecture
author: Tristen MacPherson
theme: default
paginate: true
footer: "Graphics and Shaders"
---
<!-- _class: lead -->
# Graphics and Shaders
### Guest Lecturer Tristen MacPherson

---
## Agenda
1. GPU
2. Compositing
3. Godot
4. Shaders

---
## Textures
- Typically a 2D image:
    - Color buffer
    - Raw or compressed
    - Float or int
- Properties:
    - Size
    - Channels ( RGBA )
    - Bit depth ( bits per channel )
    - Color space
    - Format ( raw, channel order, or compressed formats )

---
## The GPU
- GPU:
    - Specialized processor to accelerate mathematical calculations, particularly those related to graphics and parallel processing.
    - Originally specialized to computer graphics, but now generalized.
        - Massively parallel data processing
        - SIMD and SIMT
            - Single instruction multiple data
            - Single instruction multiple threads
    - Different vendors have different terms, but the same general ideas.   

---
## The GPU
- Nvidia:
    - CUDA core 
        - basic processing units within an NVIDIA GPU.
    - Streaming multiprocessor (SM)
        - Group of CUDA cores
- AMD:
    - Stream processor
        - basic execution units within AMD GPUs.
    - Compute Units
        - A Compute Unit in AMD's architecture is analogous to NVIDIA's Streaming Multiprocessor (SM).

---
## The GPU
CPU:
‘Core’ count
Big/LITTLE
‘Hyperthreading’ exception
Generally, core count = execution thread count
Only way to exceed parallel multiprocessing constraints is to leverage CPU & ISA specific SIMD instructions:
SSE2 (x86), SSE3 (x86), SSE4 (x86), AVX (x86), NEON (arm)

---
## GPU vs CPU

GPU:
NVIDIA GeForce RTX 4090 specs:
Specs:
128 SMs
128 CUDA cores / SM
= 16,384 CUDA cores
Each CUDA core includes at least one ALU, it can be inferred that the RTX 4090 has 16,384 ALUs, at a minimum.

---
## Compositing

---
## GPU API
###### Modern GPU architecture: 
Can be thought of a black box which you submit work or jobs to and retrieve the results. Communicated via graphics APIs

###### Early 2000’s: Introduced programmable shaders.  
- DirectX 8
- OpenGL 2.0

###### Modern: Transition to low level APIs
- Vulkan
- Metal
- WebGPU

---