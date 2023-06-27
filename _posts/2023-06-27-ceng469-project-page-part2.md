# Ceng469 Final Project: Temporal Tone Mapping for Simulating Human Visual System: Part 2: Implementation

My project aims to model how the Human Visual System (HVS) adjusts to cahnges in light level. I plan to achieve this by using temporal tonemapping operators. To be more spesific, I will be modifying Reinhard's Global Operator as a rolling average between frames, both as a aritmatic and geometric average to compare the results.

## Navigation

1. <a href="{{site.url}}/2023/06/27/ceng469-project-page-part1">Part 1: Theory</a>
2. <a href="{{site.url}}/2023/06/27/ceng469-project-page-part2">Part 2: Implementation</a>
3. <a href="{{site.url}}/2023/06/27/ceng469-project-page-part3-0">Part 3: Results</a>
   1. <a href="{{site.url}}/2023/06/27/ceng469-project-page-part3-1">Part 3.1: Formula 1</a>
   2. <a href="{{site.url}}/2023/06/27/ceng469-project-page-part3-2">Part 3.2: Formula 2</a>
   3. <a href="{{site.url}}/2023/06/27/ceng469-project-page-part3-3">Part 3.3: Formula 3</a>
   4. <a href="{{site.url}}/2023/06/27/ceng469-project-page-part3-4">Part 3.4: Formula 4</a>
   5. <a href="{{site.url}}/2023/06/27/ceng469-project-page-part3-5">Part 3.5: Comparing Results</a>
4. <a href="{{site.url}}/2023/06/27/ceng469-project-page-part4">Final Remarks</a>

## Implementation

I implemented my project using C++ and OpenGL, with the help of stb_image, GLM, GLEW, GLFW, and FAGL (my own library) libraries. 

Like most OpenGL applications, it can be divided into 2 parts, setup and main loop. In the setup part, I initiliza the window, shaders, models, vertex array objects, frame buffers and keyboard controls using FAGL. Afterwards, I modify any of the already initilized variables according to arguments given, if any. Then I implement an input manager to press the same keys at the same frames during testing (which can be turned off by default). Lastly, I create a Time Manager, also from FAGL, to limit frames per second to 30. I limited frame per secondsbecuase the weighted average method I use is a framewise operation, meaning it different frame rates would create different visual effects. I choosed 30 frames per second as to make frame operations more visible while still keeping fluid movement. Also 30 frame per second is a very easy target to hit, meaining performance dips shouldn't be able to affect the consistency of the results.

In the main loop, first I draw a shadow map for each point light, implemented as such:

```c++
for (unsigned int i = 0; i < LIGHT_AMOUNT; i++) {
	float near_plane = 1.0f;
	float far_plane = 100.0f;
	glm::mat4 shadowProj = glm::perspective(glm::radians(90.0f), (float)w / (float)h, near_plane, far_plane);
	std::vector<glm::mat4> shadowTransforms;
	shadowTransforms.push_back(shadowProj * glm::lookAt(lights[i].pos, lights[i].pos + glm::vec3(1.0f, 0.0f, 0.0f), glm::vec3(0.0f, -1.0f, 0.0f)));
	shadowTransforms.push_back(shadowProj * glm::lookAt(lights[i].pos, lights[i].pos + glm::vec3(-1.0f, 0.0f, 0.0f), glm::vec3(0.0f, -1.0f, 0.0f)));
	shadowTransforms.push_back(shadowProj * glm::lookAt(lights[i].pos, lights[i].pos + glm::vec3(0.0f, 1.0f, 0.0f), glm::vec3(0.0f, 0.0f, 1.0f)));
	shadowTransforms.push_back(shadowProj * glm::lookAt(lights[i].pos, lights[i].pos + glm::vec3(0.0f, -1.0f, 0.0f), glm::vec3(0.0f, 0.0f, -1.0f)));
	shadowTransforms.push_back(shadowProj * glm::lookAt(lights[i].pos, lights[i].pos + glm::vec3(0.0f, 0.0f, 1.0f), glm::vec3(0.0f, -1.0f, 0.0f)));
	shadowTransforms.push_back(shadowProj * glm::lookAt(lights[i].pos, lights[i].pos + glm::vec3(0.0f, 0.0f, -1.0f), glm::vec3(0.0f, -1.0f, 0.0f)));

	glViewport(0, 0, w, h);
	faglBindFramebuffer(shadowFBO[i]);
	glClear(GL_DEPTH_BUFFER_BIT);
		
	for (unsigned int j = 0; j < MODEL_AMOUNT; j++) {
		models[j].DrawShadow(glm::value_ptr(modelMatrix), value_ptr(lights[i].pos), shadowTransforms, far_plane);
	}
	faglUnbindFramebuffer();
}
```

DrawShadow function uses the shadow shaders given at the end of this part. After this, I start the regular scene rendering. For regular rendering, I use the Render Shaders (Model/Light) also given at the end of this part. 

Finally I calculate the $$\dot{L}_w$$ for the frame. After calculating it with the following code block, I use LDR to HDR shaders to complete mey rendering. Then I handle keyboard inputs etc. concluding my main loop.

```c++
void DrawQuad() {
	glActiveTexture(GL_TEXTURE0);
	faglBindTexture(fagl::BIND_TEXTURE_TARGET::TEXTURE_2D, colorBuffer);
	unsigned int cell_count = w * h;
	GLfloat* data = new GLfloat[cell_count * 4];
	faglReadPixels(w, h, fagl::READ_PIXEL_FORMAT::RGBA, fagl::READ_PIXEL_TYPE::FLOAT, data);

	GLenum err;
	while ((err = glGetError()) != GL_NO_ERROR)
	{
		std::cout << "err: " << err << std::endl;
	}

	float average = 0;

	for (unsigned int i = 0; i < cell_count; i++) {
		float lum = data[i * 4] * 0.2126f + data[i * 4 + 1] * 0.7152f + data[i * 4 + 2] * 0.0722;

		average += log(FLT_EPSILON + lum);
	}

	if (formula == 1) {
		calculatedAverageValue = alpha / exp(average / cell_count);
		if (rolledAverageValue > 0)
			rolledAverageValue = mixValue * calculatedAverageValue + (1 - mixValue) * rolledAverageValue;
		else
			rolledAverageValue = calculatedAverageValue;

		faglUnbindFramebuffer();

		ClearDisplay();

		quad.Draw(rolledAverageValue);
	}
	else if (formula == 2) {
		calculatedAverageValue = exp(average / cell_count);
		if (rolledAverageValue > 0)
			rolledAverageValue = mixValue * calculatedAverageValue + (1 - mixValue) * rolledAverageValue;
		else
			rolledAverageValue = calculatedAverageValue;

		faglUnbindFramebuffer();

		ClearDisplay();

		quad.Draw(alpha / rolledAverageValue);
	}
	else if (formula == 3) {
		calculatedAverageValue = alpha / exp(average / cell_count);
		if (rolledAverageValue > 0)
			rolledAverageValue = exp(mixValue * log(calculatedAverageValue) + (1 - mixValue) * log(rolledAverageValue));
		else
			rolledAverageValue = calculatedAverageValue;

		faglUnbindFramebuffer();

		ClearDisplay();

		quad.Draw(rolledAverageValue);
	}
	else if (formula == 4) {
		calculatedAverageValue = exp(average / cell_count);
		if (rolledAverageValue > 0)
			rolledAverageValue = exp(mixValue * log(calculatedAverageValue) + (1 - mixValue) * log(rolledAverageValue));
		else
			rolledAverageValue = calculatedAverageValue;

		faglUnbindFramebuffer();

		ClearDisplay();

		quad.Draw(alpha / rolledAverageValue);
	}
	
	delete[] data;
}
```

### Shadow Shaders

#### Vertex Shader

```c++
#version 460 core

layout (location = 0) in vec3 inVertex;
layout (location = 1) in vec3 inNormal;

uniform mat4 modelingMatrix;

void main()
{
    gl_Position = modelingMatrix * vec4(inVertex, 1.0);
}
```

#### Geometry Shader

```c++
#version 460 core
layout (triangles) in;
layout (triangle_strip, max_vertices=18) out;

uniform mat4 shadowMatrices[6];

out vec4 fragWorldPos;

void main()
{
    for(int face = 0; face < 6; ++face)
    {
        gl_Layer = face; 
        for(int i = 0; i < 3; ++i) 
        {
            fragWorldPos = gl_in[i].gl_Position;
            gl_Position = shadowMatrices[face] * fragWorldPos;
            EmitVertex();
        }    
        EndPrimitive();
    }
} 
```

#### Fragment Shader

```c++
#version 460 core
in vec4 fragWorldPos;

uniform vec3 lightPos;
uniform float farPlane;

void main()
{
    float lightDistance = length(fragWorldPos.xyz - lightPos);

    lightDistance = lightDistance / farPlane;
    
    gl_FragDepth = lightDistance;
}
```

### Render Shaders (Model)

#### Vertex Shader

```glsl
#version 460 core

uniform mat4 modelingMatrix;
uniform mat4 viewingMatrix;
uniform mat4 projectionMatrix;

layout(location=0) in vec3 inVertex;
layout(location=1) in vec3 inNormal;

out vec4 fragWorldPos;
out vec3 fragWorldNor;

void main(void)
{
	fragWorldPos = modelingMatrix * vec4(inVertex, 1);
	fragWorldNor = inverse(transpose(mat3x3(modelingMatrix))) * inNormal;

    gl_Position = projectionMatrix * viewingMatrix * fragWorldPos;
}
```

#### Fragment Shader

```glsl
#version 460 core

uniform vec3 kd;
uniform vec3 ka;
uniform vec3 ks;

uniform float ns;

uniform vec3 Iamb;
uniform vec3 I[5]; 
uniform vec3 lightPos[5];
uniform int lightNo;

uniform vec3 eyePos;

uniform float farPlane;

uniform samplerCube light0Shadow;
uniform samplerCube light1Shadow;

uniform bool shadowEnabled;

in vec4 fragWorldPos;
in vec3 fragWorldNor;

out vec4 fragColor;

vec3 gridSamplingDisk[20] = vec3[]
(
   vec3(1, 1,  1), vec3( 1, -1,  1), vec3(-1, -1,  1), vec3(-1, 1,  1), 
   vec3(1, 1, -1), vec3( 1, -1, -1), vec3(-1, -1, -1), vec3(-1, 1, -1),
   vec3(1, 1,  0), vec3( 1, -1,  0), vec3(-1, -1,  0), vec3(-1, 1,  0),
   vec3(1, 0,  1), vec3(-1,  0,  1), vec3( 1,  0, -1), vec3(-1, 0, -1),
   vec3(0, 1,  1), vec3( 0, -1,  1), vec3( 0, -1, -1), vec3( 0, 1, -1)
);

float ShadowCalculation(uint light_no)
{
    if(!shadowEnabled){
        return 0.0;
    }
    vec3 fragToLight = vec3(fragWorldPos) - lightPos[light_no];
    float currentDepth = length(fragToLight);
    
    float shadow = 0.0;
    float bias = 0.15;
    int samples = 20;
    float diskRadius = (1.0 + (length(eyePos - vec3(fragWorldPos)) / farPlane)) / 25.0;
    if(light_no == 0){
        for(int i = 0; i < samples; ++i)
        {
            if(currentDepth - bias > texture(light0Shadow, fragToLight + gridSamplingDisk[i] * diskRadius).r * farPlane){
                shadow += 1.0;
            }
        }
    }
    else if(light_no == 1){
        for(int i = 0; i < samples; ++i)
        {
            if(currentDepth - bias > texture(light1Shadow, fragToLight + gridSamplingDisk[i] * diskRadius).r * farPlane)
                shadow += 1.0;
        }
    }
    
    shadow /= float(samples);
        
    return shadow;
}

void main(void)
{
	vec3 diffuseColor = vec3(0, 0, 0);
	vec3 specularColor = vec3(0, 0, 0);
	
	vec3 V = normalize(eyePos - vec3(fragWorldPos));
	vec3 N = normalize(fragWorldNor);

	for(int i = 0; i < lightNo; i++){

		float dist = length(vec3(fragWorldPos) - lightPos[i]);

		vec3 L = normalize(lightPos[i] - vec3(fragWorldPos));
		vec3 H = normalize(L + V);

		float NdotL = dot(N, L);
		float NdotH = dot(N, H);

        float shadow = 1.0 - ShadowCalculation(i);

		diffuseColor += (I[i] / (dist *dist)) * kd * max(0, NdotL) * shadow;
		specularColor += (I[i] / (dist *dist))  * ks * pow(max(0, NdotH), ns) * shadow;
	}

	vec3 ambientColor = Iamb * ka;

	fragColor = vec4(diffuseColor + specularColor + ambientColor, 1);
}
```

### Render Shaders (Light)

#### Vertex Shader

```c++
#version 460 core

layout(location=0) in vec3 inVertex;

uniform vec3 lightPos;
uniform vec3 lightColor;

uniform mat4 modelingMatrix;
uniform mat4 viewingMatrix;
uniform mat4 projectionMatrix;

out vec4 fragWorldPos;
out vec3 fragWorldNor;

void main(void)
{
	fragWorldPos = modelingMatrix * vec4(inVertex, 1);
	fragWorldNor = normalize(vec3(fragWorldPos) - lightPos);

    gl_Position = projectionMatrix * viewingMatrix * fragWorldPos;
}
```

#### Fragment Shader

```c++
#version 460 core

in vec4 fragWorldPos;
in vec3 fragWorldNor;

uniform vec3 lightPos;
uniform vec3 lightColor;

uniform vec3 Iamb;
uniform vec3 ka;

uniform vec3 eyePos;

uniform float colorMultiplier;

out vec4 fragColor;

void main(void)
{
	vec3 V = normalize(eyePos - vec3(fragWorldPos));
	vec3 N = normalize(fragWorldNor);

	float dist = length(vec3(fragWorldPos) - lightPos);


	float dotP = dot(N, V);

	vec3 diffuseColor = ((lightColor * colorMultiplier) / (dist * dist)) * max(0, dotP);
	

	vec3 ambientColor = Iamb * ka;

	fragColor = vec4(diffuseColor + ambientColor, 1);
}
```

### HDR to LDR Shaders

#### Vertex Shader

```glsl
#version 460 core

layout(location=0) in vec3 inVertex;
layout(location=1) in vec2 inTexture;

uniform float average;
uniform mat4 modelingMatrix;
uniform mat4 viewingMatrix;
uniform mat4 projectionMatrix;

uniform vec3 eyePos;

out vec4 fragWorldPos;
out vec2 fragWorldTex;

void main(void)
{
	fragWorldPos = vec4(inVertex, 1);
	fragWorldTex = inTexture;

    gl_Position = fragWorldPos;
}
```

#### Fragment Shader

```glsl
#version 460 core

in vec4 fragWorldPos;
in vec2 fragWorldTex;

uniform sampler2D hdrBuffer;
uniform float average;

out vec4 fragColor;

vec3 tonemap(vec3 hdrColor) {
	float lum = hdrColor.r * 0.2126 + hdrColor.g * 0.7152 + hdrColor.b * 0.0722;
	float lumScaled = lum * average;
	float lumTM = lumScaled / (lumScaled + 1);
	vec3 tmColor = clamp((lumTM / lum) * hdrColor, vec3(0, 0, 0), vec3(1, 1, 1));
	float p = 1. / 2.2;
	return pow(tmColor, vec3(p, p, p));
}

void main(void){
	 vec3 hdrColor = texture(hdrBuffer, fragWorldTex).rgb;
	 fragColor = vec4(tonemap(hdrColor), 1);
}
```
