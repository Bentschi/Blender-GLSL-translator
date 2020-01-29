# Blender-GLSL-translator
Proof of concept: compiling material nodes into GLSL shaders

I am currently working on a game engine that uses Vulkan.
I thought it would be great if you could just export the materials from Blender into a GLSL shader.
This script supports all the nodes I used and can not only translate the nodes, but also compile the translation without any errors.
However, I decided to scarp the Idea, because I will be faster writing my shaders by hand instead of finishing this project.

That being said, I want to give this "proof of concept" project to anyone who wants to finish it or use it as a base for a similar project.

Here is an example command line (debug) output that script generates, from one of the materials I use:

```GLSL
-----------------------
#version 440

struct material
{
	vec4 color;
	float metallic;
	vec4 specular;
	float roughness;
	float ior;
	vec4 emission;
	float alpha;
	vec3 normal;
};

layout(location=0) in vec3 v_position;
layout(location=1) in vec3 v_normal;
layout(location=2) in vec3 v_objectNormal;
layout(location=3) in vec2 v_texCoord;
layout(binding=0) uniform sampler2D v_textures[3];
layout(location=0) out vec4 fragColor;

mat4 buildTransform(vec3 l, vec3 r, vec3 s) {vec3 a=sin(r); vec3 b=cos(r); return mat4(vec4(b.y*b.z+a.x*a.z*s.x,b.y*a.z-a.x*a.y*b.z,b.x*a.y,0.0),vec4(-b.x*a.z,b.x*b.z*s.y,a.x,0.0),vec4(a.x*b.y*a.z-a.y*b.z,-a.y*a.z-a.x*b.y*b.z,b.x*b.y*s.z,0.0),vec4(l.xyz,1.0));}
vec3 mapping(vec3 vec, vec3 loc, vec3 rot, vec3 sca) {return (vec4(vec,1.0)*buildTransform(loc,rot,sca)).xyz;}
float rgb2bw(vec4 c) {return (c.r+c.g+c.b)/3.0;}
float colorMixOverlayC(float a, float b) {return (a<0.5)?(2.0*b*a):(1.0-2.0*(1.0-a)*(1.0-b));}
vec4 colorMixOverlay(vec4 a, vec4 b, float f) {return mix(a,vec4(colorMixOverlayC(a.r,b.r),colorMixOverlayC(a.g,b.g),colorMixOverlayC(a.b,b.b),colorMixOverlayC(a.a,b.a)),f);}

void main()
{

	/* Group Input -> GROUP_INPUT@Scale */
	/* [main->Group.004]->Group Input~Input_5 */
	/* 30 */ float var29 = 0.00050;

	/* Group Input -> GROUP_INPUT@Scale */
	/* [main->Group.004->Group]->Group Input~Input_9 */
	/* 31 */ float var28 = var29;

	/* Texture Coordinate -> TEX_COORD@UV */
	/* [main->Group.004]->Texture Coordinate~UV */
	/* 32 */ vec3 var25 = vec3(v_texCoord,0.0);

	/* Group Input -> GROUP_INPUT@Vector */
	/* [main->Group.004->Group]->Group Input~Input_17 */
	/* 33 */ vec3 var24 = var25;

	/* Math -> MATH@Value (MULTIPLY) */
	/* [main->Group.004->Group]->Math.009~Value */
	/* 34 */ float var41 = (var28*250.00000);

	/* Group Input -> GROUP_INPUT@Scale */
	/* [main->Group.004->Group->Group.003]->Group Input~Input_5 */
	/* 35 */ float var40 = var41;

	/* Group Input -> GROUP_INPUT@Vector */
	/* [main->Group.004->Group->Group.003]->Group Input~Input_11 */
	/* 36 */ vec3 var39 = var24;

	/* Mapping -> MAPPING@Vector */
	/* [main->Group.004->Group->Group.003]->Mapping.005~Vector */
	/* 37 */ vec3 var38 = mapping(var39,vec3(0.00000,0.00000,0.00000),vec3(0.00000,0.00000,0.00000),vec3(var40));

	/* Math -> MATH@Value (ADD) */
	/* [main->Group.004->Group]->Math.008~Value */
	/* 38 */ float var35 = (var28+30.00000);

	/* Group Input -> GROUP_INPUT@Scale */
	/* [main->Group.004->Group->Group.001]->Group Input~Input_5 */
	/* 39 */ float var34 = var35;

	/* Group Input -> GROUP_INPUT@Vector */
	/* [main->Group.004->Group->Group.001]->Group Input~Input_11 */
	/* 40 */ vec3 var33 = var24;

	/* Mapping -> MAPPING@Vector */
	/* [main->Group.004->Group->Group.001]->Mapping.005~Vector */
	/* 41 */ vec3 var32 = mapping(var33,vec3(0.00000,0.00000,0.00000),vec3(0.00000,0.00000,0.00000),vec3(var34));

	/* Group Input -> GROUP_INPUT@Type */
	/* [main->Group.004]->Group Input~Input_7 */
	/* 42 */ float var15 = 0.65000;

	/* Group Input -> GROUP_INPUT@Type */
	/* [main->Group.004->Group]->Group Input~Input_11 */
	/* 43 */ float var14 = var15;

	/* Image Texture -> TEX_IMAGE@Color (noise-soft-normal) */
	/* [main->Group.004->Group->Group.003]->Image Texture.007~Color */
	/* 44 */ vec4 var50 = texture(v_textures[2],var38.xy);

	/* Normal Map -> NORMAL_MAP@Normal */
	/* [main->Group.004->Group->Group.003]->Normal Map~Normal */
	/* 45 */ vec3 var49 = normalize(v_normal*(var50.xyz*1.00000)*0.5+0.5);

	/* Group -> GROUP@Normal */
	/* [main->Group.004->Group]->Group.003~Output_9 */
	/* 46 */ vec3 var48 = var49;

	/* Image Texture -> TEX_IMAGE@Color (noise-soft-normal) */
	/* [main->Group.004->Group->Group.001]->Image Texture.007~Color */
	/* 47 */ vec4 var47 = texture(v_textures[2],var32.xy);

	/* Normal Map -> NORMAL_MAP@Normal */
	/* [main->Group.004->Group->Group.001]->Normal Map~Normal */
	/* 48 */ vec3 var46 = normalize(v_normal*(var47.xyz*1.00000)*0.5+0.5);

	/* Group -> GROUP@Normal */
	/* [main->Group.004->Group]->Group.001~Output_9 */
	/* 49 */ vec3 var45 = var46;

	/* Mix -> MIX_RGB@Color (MIX) */
	/* [main->Group.004->Group]->Mix.007~Color */
	/* 50 */ vec4 var44 = mix(vec4(var45,1.0),vec4(var48,1.0),var14);

	/* Group -> GROUP@Normal */
	/* [main->Group.004]->Group~Output_29 */
	/* 51 */ vec3 var43 = var44.xyz;

	/* Group -> GROUP@Normal */
	/* [main]->Group.004~Output_13 */
	/* 52 */ vec3 var42 = var43;

	/* Image Texture -> TEX_IMAGE@Color (noise-soft) */
	/* [main->Group.004->Group->Group.003]->Image Texture.006~Color */
	/* 53 */ vec4 var37 = texture(v_textures[1],var38.xy);

	/* Group -> GROUP@Color */
	/* [main->Group.004->Group]->Group.003~Output_7 */
	/* 54 */ vec4 var36 = var37;

	/* Image Texture -> TEX_IMAGE@Color (noise-soft) */
	/* [main->Group.004->Group->Group.001]->Image Texture.006~Color */
	/* 55 */ vec4 var31 = texture(v_textures[1],var32.xy);

	/* Group -> GROUP@Color */
	/* [main->Group.004->Group]->Group.001~Output_7 */
	/* 56 */ vec4 var30 = var31;

	/* Math -> MATH@Value (MULTIPLY) */
	/* [main->Group.004->Group]->Math.007~Value */
	/* 57 */ float var27 = (var28*16.00000);

	/* Group Input -> GROUP_INPUT@Scale */
	/* [main->Group.004->Group->Group.002]->Group Input~Input_9 */
	/* 58 */ float var26 = var27;

	/* Group Input -> GROUP_INPUT@Vector */
	/* [main->Group.004->Group->Group.002]->Group Input~Input_13 */
	/* 59 */ vec3 var23 = var24;

	/* Mapping -> MAPPING@Vector */
	/* [main->Group.004->Group->Group.002]->Mapping.004~Vector */
	/* 60 */ vec3 var22 = mapping(var23,vec3(0.00000,0.00000,0.00000),vec3(0.00000,0.00000,0.00000),vec3(var26));

	/* Image Texture -> TEX_IMAGE@Color (noise) */
	/* [main->Group.004->Group->Group.002]->Image Texture.005~Color */
	/* 61 */ vec4 var21 = texture(v_textures[0],var22.xy);

	/* RGB to BW -> RGBTOBW@Val */
	/* [main->Group.004->Group->Group.002]->RGB to BW~Val */
	/* 62 */ float var20 = rgb2bw(var21);

	/* Group -> GROUP@Val */
	/* [main->Group.004->Group]->Group.002~Output_15 */
	/* 63 */ float var19 = var20;

	/* Group Input -> GROUP_INPUT@Soft */
	/* [main->Group.004]->Group Input~Input_9 */
	/* 64 */ float var18 = 0.58333;

	/* Group Input -> GROUP_INPUT@Soft */
	/* [main->Group.004->Group]->Group Input~Input_13 */
	/* 65 */ float var17 = var18;

	/* Mix -> MIX_RGB@Color (MIX) */
	/* [main->Group.004->Group]->Mix.004~Color */
	/* 66 */ vec4 var16 = mix(vec4(vec3(var19),1.0),var30,var17);

	/* Mix -> MIX_RGB@Color (MIX) */
	/* [main->Group.004->Group]->Mix.005~Color */
	/* 67 */ vec4 var13 = mix(var16,var36,var14);

	/* RGB to BW -> RGBTOBW@Val */
	/* [main->Group.004->Group]->RGB to BW.001~Val */
	/* 68 */ float var12 = rgb2bw(var13);

	/* RGB -> RGB@Color */
	/* [main]->RGB~Color */
	/* 69 */ vec4 var11 = vec4(0.03000,0.01000,0.00800,1.00000);

	/* Group Input -> GROUP_INPUT@Color */
	/* [main->Group.004]->Group Input~Input_1 */
	/* 70 */ vec4 var10 = var11;

	/* Group Input -> GROUP_INPUT@Color */
	/* [main->Group.004->Group]->Group Input~Input_1 */
	/* 71 */ vec4 var9 = var10;

	/* Group Input -> GROUP_INPUT@Mix */
	/* [main->Group.004]->Group Input~Input_3 */
	/* 72 */ float var8 = 1.00000;

	/* Group Input -> GROUP_INPUT@Mix */
	/* [main->Group.004->Group]->Group Input~Input_3 */
	/* 73 */ float var7 = var8;

	/* Mix -> MIX_RGB@Color (OVERLAY) */
	/* [main->Group.004->Group]->Mix.006~Color */
	/* 74 */ vec4 var6 = colorMixOverlay(var9,vec4(vec3(var12),1.0),var7);

	/* Group -> GROUP@Color */
	/* [main->Group.004]->Group~Output_23 */
	/* 75 */ vec4 var5 = var6;

	/* Group -> GROUP@Color */
	/* [main]->Group.004~Output_11 */
	/* 76 */ vec4 var4 = var5;

	/* Group Input -> GROUP_INPUT@Color */
	/* [main->Group.007]->Group Input~Input_15 */
	/* 77 */ vec4 var3 = var4;

	/* Group -> GROUP@Color */
	/* [main]->Group.007~Output_17 */
	/* 78 */ vec4 var2 = var3;

	/* Principled BSDF -> BSDF_PRINCIPLED@BSDF */
	/* [main]->Principled BSDF~BSDF */
	/* 79 */ material var1 = material(var2,0.32500,vec4(vec3(0.50000),1.0),0.90000,1.45000,vec4(0.00000,0.00000,0.00000,1.00000),1.00000,var42);

	/* compiled output */
	/* main */
	/* 80 */ material mat = var1;
	/* 81 */ 
	/* 82 */ fragColor = vec4(mat.color.xyz, mat.alpha);
	/* 83 */ 
}

/* Translation Errors: 0 */

-----------------------
```
