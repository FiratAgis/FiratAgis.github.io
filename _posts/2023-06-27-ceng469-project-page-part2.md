# Ceng469 Final Project: Temporal Tone Mapping for Simulating Human Visual System: Part 2: Implementation

My project aims to model how the Human Visual System (HVS) adjusts to cahnges in light level. I plan to achieve this by using temporal tonemapping operators. To be more spesific, I will be modifying Reinhard's Global Operator as a rolling average between frames, both as a aritmatic and geometric average to compare the results.

## Navigation


## Implementation

I implemented my project using C++ and OpenGL, with the help of stb_image, GLM, GLEW, GLFW, and FAGL (my own library) libraries. Like most OpenGL applications, it can be divided into 2 parts, setup and main loop. In main loop,


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

### Render Shaders

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
