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
<br>
<b>The bold text are configurable by users, which users can inject their own shaders. </b>

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
<b>RModern OpenGL users are required to define at least a vertex and fragment shader of our own because there are no default vertex/fragment shaders on the GPU.</b>
<br><br>
# Vertex Input
OpenGL is 3D graphics library so all coordinates that we specify in OpenGL are in 3D(x, y, and z coordinate). OpenGL doesn't simply transform all your 3D coordinates to 2D pixels on your screen; OpenGL only processes 3D coordinates when they're in a specific range between -1.0 and 1.0 on all 3 axes(x, y and z). All coordinates within this so called normalized device coordinates range will end up visible on your screen (and all coordinates outside this region won't).
<br><br>
<b>Normalized Device Coordinates(NDC)</b> 
<br>Unlike usual screen coordinates the positive y-axis points in the up-direction and the (0, 0) coordinates are at the center of the graph, instead of top-left. NDC coordinates will then be transformed to screen-space coordinates via the viewport transform using the data you provided with glViewport.
<br><br>
Send the vertex data to the input of the <b>VERTEX SHADER</b>. This is done by creating memory on the GPU where we store the vertex data.
<br>Users manage this memory via so called vertex buffer objects(VBO) that can store a large number of vertices in the GPU's memory. The advantage of using VBO is we can send large batches of data all at once. Sending data to the graphics card from the CPU is relatively slow so it tries to send as much data as possible at once. Once the data is in the graphics card's memory the vertex shader has almost instant access to the vertices making it extremely fast.
<br><br>
VBO is like any object in OpenGL, having a unique ID corresponding to that buffer, so we can generate one with a buffer  ID using the glGenBuffers function: 
<br><b>Example</b>
<br>unsigned int VBO;
<br>glGenBuffers(1, &VBO);
<br><br>
OpenGL has many types of buffer objects. For he vertex buffer object is <b>GL_ARRAY_BUFFER</b>. We can useglBindBuffer function to bind the newly created buffer.
<br><b>Example</b>
<br>glBindBuffer(GL_ARRAY_BUFFER, VBO);
<br><br>
After the last step, we can use glBufferData function that copies the previously defined vertex data into the buffer's memory
<br><b>Example</b>
<br>glBufferData(GL_ARRAY_BUFFER, sizeof(vertices, vertices, GL_STATIC_DRAW);
<br>
glBufferData is a function specifically targeted to ccopy user-defined data into the currently bound buffer. 
<br>The <b>first</b> argument is the type of the buffer we want to copy data into. 
<br>The <b>second</b> argument specifies the size of the data( in bytes) we want to pass to the buffer. 
<br>The <b>third</b> parameter is the actual data we want to send. 
<br>The <b>fourth</b> parameter specifies how we want the graphics card to manage the given data. This take 3 forms:<br><b>GL_STATIC_DRAW</b>: the data will most likely not change at all or very rarely.<br><b>GL_DYNAMIC_DRAW</b>: the data is likely to change a lot.<br><b>GL_STREAM_DRAW</b>: the data will change every time it is drawn.
<br> The first form will stay the position where it created.
<br> The last two forms will make the graphics card place the data in memory that allows for faster writes.
<h3> In this step <b>VERTEX INPUT</b> we stored the vertex data within memory on the graphics card as managed by a vertex buffer object named VBO. Next we want to create a vertex and fragment shader that actually processes this data</h3>
<br>

# VERTEX SHADER
At least two SHADERS are required by Modern OpenGL, VERTEX and FRAGMENT SHADERS, from users. VERTEX SHADER is written in shader language GLSL( OpenGL shading language) and then compile this shader.
<br><b>Example</b>
<br>#version 330 core
<br>layout (location = 0) in vec3 aPos;
<br>void main(){gl_Position = vec4(aPos.x, aPos.y, aPos.z, 1.0);}
<br><br>
The first line <b>#version 330 core</b> is to clarify the version of shader.<br>
The second line is to declare all the input vertex attributes in the vertex shader with the in keyword.<br>
The <b>vec3</b> is a vector type which contains 3 parameters. In this case we initiate aPos as a variable.<br>
The layout (location = 0) is to set the location of the input variable.<br>
<br>
Note: A vector in GLSL has a maximum size of 4 and each of its values can be retrieved via vec.x, vec.y, vec.z, vec.w. The first three are position and the last one is called perspective division.
<br>
The output of vertex shader we use gl_Position to complete it. The vector size is 4 (<b>vec4</b>), however, we only got 3 parameteres. Therefore, the last one we set it into 1.0f.
<br>
Note: In real applications the input data is not usually not already in normalized device coordinates so we first have to transform the input data to coordinates that fall wihtin OpenGL's visible region.
<br>

# COMPILING A SHADER
After wrote the source code for the vertex shader, in order for OpenGL to use the shader it has to dynamically compile it at run-time from its source code.<br>
First of all, users need to create a shader object, again referenced by an ID.
We store the vertex shader as an unsigned int and create the shader with <b>glCreateShader</b><br>
<b>Example</b>
<br>unsigned int vertexShader;<br>
vertexShader = glCreateShader(GL_VERTEX_SHADER);
<br>
The argument in glCreateShader is the type of shader.<br>
Next attach the shader source code to the shader object and compile the shader<br>
<b>Example</b>
<br>glShaderSource(vertexShader, 1, &vertexShaderSource, NULL);
<br>glCompileShader(vertexShader);
<br>
The glShaderSource function takes the shader object to compile to as its first argument. <br>
The second argument speicifies how many strings we're passing as source code. <br>
The third argument is the actual source code of the vertex shader.<br>
<br>
The following is to check if compilation was successful after the call to glCompileShader.
<br>
int success; // this is for indicating success<br>
char infoLog[512]; // container for the error messages<br>
glGetShaderiv(vertexShader, GL_COMPILE_STATUS, &success); //check if compilation was successful<br>
<br>
if(!success){<br>
glGetShaderInfoLog(vertexShader, 512, NULL, infoLog); // glGetShaderInfoLog for retrieving the error message<br>
cout<<"Error"<<infoLog<<endl;}
<br>

#  FRAGMENT SHADER
This step is all about calculating the color output of your pixels. Fragment shader will always output an orangeish color. <br>
Note: Colors in computer graphics are represented as an array of 4 values: Red, Green, Blue and alpha(opacity). The range of value will be 0.0f~1.0f.
<br>
<b>EXAMPLE</b><br>
#version 330 core // declare the version<br>
out vec4 FragColor; // declare output values with the out keyword<br><br>
void main(){<br>
FragColor = vec4(1.0f, 0.5f, 0.2f, 1.0f); // <br>
}<br>
Note: The process for compiling a fragment shader is similar to the vertex shader, but the function will be used GL_FRAGMENT_SHADER<br>
<b>EXAMPLE</b>
<br>unsigned int fragmentShader;<br>
fragmentShader = glCreateShader(GL_FRAMGMENT_SHADER);<br>
glShaderSource(fragmentShader, 1, &fragmentShaderSource, NULL);<br>
glCompileShader(fragmentShader);<br>
Note: Both Shaders are now compiled and the last thing is to link both shader objects into a shader program which we can use for rendering<br>

# SHADER PROGRAM
This is the last step. To use the recently compiled shaders we have to link them to a shader program object and then activate this shader program when rendering objects. The activated shader program's shaders will be used when we issue render calls.<br>
The linking means the output of previous step need to attach to the input of current step.<br>
First create program object.<br>
<b>EXAMPLE</b>
<br>unsigned int shaderProgram;<br>
shaderProgram = glCreateProgram(); // The glCreateProgram creates a program and returns the ID reference to the newly created program object<br>
Now we need to attach the previously compiled shaders to the program object<br>
<b>EXAMPLE</b>
<br>glAttachShader(shaderProgram, vertexShader);
<br>glAttachShader(shaderProgram, fragmentShader);
<br>glLinkProgram(shaderProgram);<br>
Note: Like shader compilation, we can check if it is success or not by using glGetShaderiv and glGetShaderInfoLog.<br>
Finally, we have to activate the program object by calling glUseProgram<br>
<b>EXAMPLE</b>
<br>glUseProgram(shaderProgram); // Every shader and rendering call after glUseProgram will now use this program object<br>
After glUseProgram called, we can delete the shaders we created previously.
<b>EXAMPLE</b>
<br>glDeleteShader(vertexShader);<br>
glDeleteShader(fragmentShader); <br>
<b>Note: Right now we sent the input vertex data to the GPU and instructed the GPU how it should process the vertex data within a vertex and fragment shader. But OpenGL does not yet know how it should interpret the vertex data in memory and how it should connect the vertex data to the vertex shader's attributes.</b>
