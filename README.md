# OpenGL 
# Pipeline

In OpenGL everything is in 3D space, but the screen and window are a 2D array of pixels so a large part of OpenGL's work is about transforming all 3D coordinates to 2D pixels that fit on your screen.<br><br>
The process of transforming 3D coordinates to 2D pixels is managed by the graphics pipeline of OpenGL.<br><br>
The graphics pipeline can be divided into two large parts: the first transforms your 3D coordinates into 2D coordinates and the second part transforms the 2D coordinates into actual colored pixels.
<h4>Graphics pipeline can be divided into several steps where each step requires the output of the previous step as its input.</h4>
<br><br>

# Shader

Graphics cards of today have thousands of small processing cores to quickly process your data within the graphics pipeline by running small programs on the GPU for each step of the pipeline. These small programs are called Shader.<br><br>
Some of these shaders are configurable by the developer which allows us to write our own shaders to replace the existing default shaders.<h4>Shaders are written in the OpenGL Shading Language (GLSL)</h4>

Representation of Pipeline: VERTEX DATA -> <b>VERTEX SHADER</b> -> SHAPE ASSEMBLY -> <b>GEOMETRY SHADER</b> -> RASTERIZATION -> <b>FRAGMENT SHADER</b> -> TESTS AND BLENDING
<br><b>The bold text are configurable by users, which users can inject their own shaders. </b>
