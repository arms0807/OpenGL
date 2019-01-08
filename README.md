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

The first VERTEX DATA is the input from user, which should be vertex type. The vertex is the attribute of shape containing 3D position and color value. <br>We can use some hints like GL_POINTS, GL_TRIANGLES AND GL_LINE_STRIP to draw the exact types or shapes we want.
<br><br>
The <b>VERTEX SHADER</b> is to transform 3D coordinates into different 3D coordinates and the vertex shader allows us to do some basic processing on the vertex attributes.
<br><br>
The <b>PRIMITIVE ASSEMBLY</b> takes as input all the vertices/vertex from the vertex shader that form a primitive and assembles all the point(s) in the primitive shape given.
<br><br>
The <b>GEOMETRY SHADER</b> takes the output of <b>PRIMITIVE ASSEMBLY</b> as input. In this step, it forms a primitive and has the ability to generate other shapes by emitting new vertices to form new primitive(s). 
<br><br>
The <b>RASTERIZATION STAGE</b> maps the resulting primitive(s) to the corresponding pixels on the final screen, resulting the fragments for the fragment shader to use. Before the <b>FRAGMENT SHADERS</b> run, clipping is performed. <b>Clipping</b> discards all fragments that are outside your view, increasing performance.
<br><br>
<b>A fragment in OpenGL is all the data required for OpenGL to render a single pixel</b>
<br><br>
The main purpose of <b>FRAGMENT SHADER</b> is to calculate the final color of a pixel. Usually the shader contains data about the 3D scene that it can use to calculate the final pixel color.
<br><br>
The final stage <b>ALPHA TEST AND BLENDING</b> checks the corresponding depth value of the fragment and uses those to check if the resulting fragment is in front or behind other objects and should be discarded accordingly. This stage also checks for alpha values and blends the objects accordingly. Therefore even if a pixel output color is calculated in the fragment shader, the final pixel color could still be something entirely different when rendering multiple triangles.
<br><br>
Modern OpenGL users are required to define at least a vertex and fragment shader of our own because there are no default vertex/fragment shaders on the GPU.
<br><br>
Refernece : https://learnopengl.com/Getting-started/Hello-Triangle
