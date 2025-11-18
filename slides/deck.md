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
## WebGL Triangle Test

<canvas id="test-canvas" width="400" height="400" style="border: 2px solid #444;"></canvas>

<script>
const canvas = document.getElementById('test-canvas');
const gl = canvas.getContext('webgl') || canvas.getContext('experimental-webgl');

if (!gl) {
  const ctx = canvas.getContext('2d');
  ctx.fillStyle = 'red';
  ctx.fillText('WebGL not supported', 10, 50);
} else {
  // Vertex shader
  const vsSource = `
    attribute vec2 position;
    attribute vec3 color;
    varying vec3 vColor;
    void main() {
      gl_Position = vec4(position, 0.0, 1.0);
      vColor = color;
    }
  `;
  
  // Fragment shader
  const fsSource = `
    precision mediump float;
    varying vec3 vColor;
    void main() {
      gl_FragColor = vec4(vColor, 1.0);
    }
  `;
  
  function createShader(gl, type, source) {
    const shader = gl.createShader(type);
    gl.shaderSource(shader, source);
    gl.compileShader(shader);
    if (!gl.getShaderParameter(shader, gl.COMPILE_STATUS)) {
      console.error('Shader compile error:', gl.getShaderInfoLog(shader));
      gl.deleteShader(shader);
      return null;
    }
    return shader;
  }
  
  const vertexShader = createShader(gl, gl.VERTEX_SHADER, vsSource);
  const fragmentShader = createShader(gl, gl.FRAGMENT_SHADER, fsSource);
  
  const program = gl.createProgram();
  gl.attachShader(program, vertexShader);
  gl.attachShader(program, fragmentShader);
  gl.linkProgram(program);
  
  if (!gl.getProgramParameter(program, gl.LINK_STATUS)) {
    console.error('Program link error:', gl.getProgramInfoLog(program));
  }
  
  // Triangle vertices (position + color)
  const vertices = new Float32Array([
    // x,    y,     r,   g,   b
     0.0,  0.8,   1.0, 0.0, 0.0,  // top (red)
    -0.7, -0.6,   0.0, 1.0, 0.0,  // bottom-left (green)
     0.7, -0.6,   0.0, 0.0, 1.0,  // bottom-right (blue)
  ]);
  
  const buffer = gl.createBuffer();
  
  const positionLoc = gl.getAttribLocation(program, 'position');
  const colorLoc = gl.getAttribLocation(program, 'color');
  
  gl.useProgram(program);
  
  let time = 0;
  function animate() {
    time += 0.02;
    
    // Update colors based on time
    const animatedVertices = new Float32Array([
      // x,    y,     r,   g,   b
       0.0,  0.8,   Math.abs(Math.sin(time)), Math.abs(Math.sin(time + 2)), Math.abs(Math.sin(time + 4)),
      -0.7, -0.6,   Math.abs(Math.sin(time + 1)), Math.abs(Math.sin(time + 3)), Math.abs(Math.sin(time + 5)),
       0.7, -0.6,   Math.abs(Math.sin(time + 2)), Math.abs(Math.sin(time + 4)), Math.abs(Math.sin(time)),
    ]);
    
    gl.bindBuffer(gl.ARRAY_BUFFER, buffer);
    gl.bufferData(gl.ARRAY_BUFFER, animatedVertices, gl.DYNAMIC_DRAW);
    
    gl.vertexAttribPointer(positionLoc, 2, gl.FLOAT, false, 20, 0);
    gl.vertexAttribPointer(colorLoc, 3, gl.FLOAT, false, 20, 8);
    gl.enableVertexAttribArray(positionLoc);
    gl.enableVertexAttribArray(colorLoc);
    
    gl.clearColor(0.1, 0.1, 0.1, 1.0);
    gl.clear(gl.COLOR_BUFFER_BIT);
    gl.drawArrays(gl.TRIANGLES, 0, 3);
    
    requestAnimationFrame(animate);
  }
  
  animate();
  console.log('Triangle animating!');
}
</script>

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

###### Early 2000's: Introduced programmable shaders.  
- DirectX 8
- OpenGL 2.0

###### Modern: Transition to low level APIs
- Vulkan
- Metal
- WebGPU

---
## WebGPU Sprite Rendering Demo