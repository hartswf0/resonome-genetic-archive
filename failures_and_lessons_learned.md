# Failures and Lessons Learned

- `mdeck.html`: Early double deck experiment, no sound, no visuals.
- `mbekitu-11.html`: An attempt to evolve the single-portal concept. While it had some good generative ideas that could inform a future double-decker, it lacked direct control authority and ultimately fell short. The proposed fixes were deemed too complex and untrustworthy.


#AUDIO FAILURES (DOUBLE DECKERS)
my-guy-06.html Audio cannot be started. Please ensure your browser allows autoplay or try refreshing. Error: portalApp.setupUILeseners is not a function

my-guy-05.html Audio cannot be started. Please ensure your browser allows autoplay or try refreshing. Error: droneShaderConfig is not defined

my-guy-04.html Audio cannot be started. Please ensure your browser allows autoplay or try refreshing. Error: portalApp.toggleUI is not a function





# BEST (NESTED FIELD)
mbeck-11.html good sequencer able to control nested fields, react and compose but we have pathology of global control and promise of shaping the visuals and the sound better
×
Modes:

MODE button (top-left) / 'M' key / 2-finger tap to switch.

Composition Mode: Build multi-layered patterns. Drag on canvas to sculpt visuals of Active Layer. Click nodes on canvas or sequencer grid to toggle sounds/effects. Layers & BPM controls available.

Reactive Mode: Free-form sonic/visual exploration. Drag on canvas to sculpt global sound character & visual vibes. Visuals are automatically "sampled" into active layer. Controls are hidden.

Layer Control: 'L' key / 3-finger tap. Select layer for editing. Click its icon again to toggle playback.

BPM: Long-press on canvas / 2-finger tap. Click BPM display to cycle speeds. Use +/- buttons to fine-tune.


#DEAD ON ARRIVAL
mdeck-11.html
Uncaught SyntaxError: Identifier 'audioContext' has already been declared


my-guy-02.html (NESTED FIELD
no audio and sequencer does not run but looks quite nice)
no console errors





﻿MIGHTY MORPH
mighty-morph-14.html (DOUBLE DECKER)
looks good, sequencer very legible but does not run
minimal control of the morphic field on the left
shift click does not work
three.min.js:6 THREE.WebGLProgram: shader error:  0 35715 false gl.getProgramInfoLog Fragment shader is not compiled.
  THREE.WebGLShader: gl.getShaderInfoLog() fragment
ERROR: 0:128: 'color' : undeclared identifier
ERROR: 0:128: 'assign' : l-value required (can't modify a const)
ERROR: 0:128: '=' : dimension mismatch
ERROR: 0:128: 'assign' : cannot convert from 'highp 3-component vector of float' to 'const highp float'
ERROR: 0:141: 'color' : undeclared identifier
ERROR: 0:141: 'assign' : l-value required (can't modify a const)
ERROR: 0:141: 'assign' : cannot convert from 'highp 3-component vector of float' to 'const highp float'
ERROR: 0:146: 'color' : undeclared identifier
ERROR: 0:146: 'constructor' : not enough data provided for construction
1: #version 300 es
2: #define varying in
3: out highp vec4 pc_fragColor;
4: #define gl_FragColor pc_fragColor
5: #define gl_FragDepthEXT gl_FragDepth
6: #define texture2D texture
7: #define textureCube texture
8: #define texture2DProj textureProj
9: #define texture2DLodEXT textureLod
10: #define texture2DProjLodEXT textureProjLod
11: #define textureCubeLodEXT textureLod
12: #define texture2DGradEXT textureGrad
13: #define texture2DProjGradEXT textureProjGrad
14: #define textureCubeGradEXT textureGrad
15: precision highp float;
16: precision highp int;
17: #define HIGH_PRECISION
18: #define SHADER_NAME ShaderMaterial
19: #define GAMMA_FACTOR 2
20: uniform mat4 viewMatrix;
21: uniform vec3 cameraPosition;
22: uniform bool isOrthographic;
23: 
24: vec4 LinearToLinear( in vec4 value ) {
25: 	return value;
26: }
27: vec4 GammaToLinear( in vec4 value, in float gammaFactor ) {
28: 	return vec4( pow( value.rgb, vec3( gammaFactor ) ), value.a );
29: }
30: vec4 LinearToGamma( in vec4 value, in float gammaFactor ) {
31: 	return vec4( pow( value.rgb, vec3( 1.0 / gammaFactor ) ), value.a );
32: }
33: vec4 sRGBToLinear( in vec4 value ) {
34: 	return vec4( mix( pow( value.rgb * 0.9478672986 + vec3( 0.0521327014 ), vec3( 2.4 ) ), value.rgb * 0.0773993808, vec3( lessThanEqual( value.rgb, vec3( 0.04045 ) ) ) ), value.a );
35: }
36: vec4 LinearTosRGB( in vec4 value ) {
37: 	return vec4( mix( pow( value.rgb, vec3( 0.41666 ) ) * 1.055 - vec3( 0.055 ), value.rgb * 12.92, vec3( lessThanEqual( value.rgb, vec3( 0.0031308 ) ) ) ), value.a );
38: }
39: vec4 RGBEToLinear( in vec4 value ) {
40: 	return vec4( value.rgb * exp2( value.a * 255.0 - 128.0 ), 1.0 );
41: }
42: vec4 LinearToRGBE( in vec4 value ) {
43: 	float maxComponent = max( max( value.r, value.g ), value.b );
44: 	float fExp = clamp( ceil( log2( maxComponent ) ), -128.0, 127.0 );
45: 	return vec4( value.rgb / exp2( fExp ), ( fExp + 128.0 ) / 255.0 );
46: }
47: vec4 RGBMToLinear( in vec4 value, in float maxRange ) {
48: 	return vec4( value.rgb * value.a * maxRange, 1.0 );
49: }
50: vec4 LinearToRGBM( in vec4 value, in float maxRange ) {
51: 	float maxRGB = max( value.r, max( value.g, value.b ) );
52: 	float M = clamp( maxRGB / maxRange, 0.0, 1.0 );
53: 	M = ceil( M * 255.0 ) / 255.0;
54: 	return vec4( value.rgb / ( M * maxRange ), M );
55: }
56: vec4 RGBDToLinear( in vec4 value, in float maxRange ) {
57: 	return vec4( value.rgb * ( ( maxRange / 255.0 ) / value.a ), 1.0 );
58: }
59: vec4 LinearToRGBD( in vec4 value, in float maxRange ) {
60: 	float maxRGB = max( value.r, max( value.g, value.b ) );
61: 	float D = max( maxRange / maxRGB, 1.0 );
62: 	D = clamp( floor( D ) / 255.0, 0.0, 1.0 );
63: 	return vec4( value.rgb * ( D * ( 255.0 / maxRange ) ), D );
64: }
65: const mat3 cLogLuvM = mat3( 0.2209, 0.3390, 0.4184, 0.1138, 0.6780, 0.7319, 0.0102, 0.1130, 0.2969 );
66: vec4 LinearToLogLuv( in vec4 value ) {
67: 	vec3 Xp_Y_XYZp = cLogLuvM * value.rgb;
68: 	Xp_Y_XYZp = max( Xp_Y_XYZp, vec3( 1e-6, 1e-6, 1e-6 ) );
69: 	vec4 vResult;
70: 	vResult.xy = Xp_Y_XYZp.xy / Xp_Y_XYZp.z;
71: 	float Le = 2.0 * log2(Xp_Y_XYZp.y) + 127.0;
72: 	vResult.w = fract( Le );
73: 	vResult.z = ( Le - ( floor( vResult.w * 255.0 ) ) / 255.0 ) / 255.0;
74: 	return vResult;
75: }
76: const mat3 cLogLuvInverseM = mat3( 6.0014, -2.7008, -1.7996, -1.3320, 3.1029, -5.7721, 0.3008, -1.0882, 5.6268 );
77: vec4 LogLuvToLinear( in vec4 value ) {
78: 	float Le = value.z * 255.0 + value.w;
79: 	vec3 Xp_Y_XYZp;
80: 	Xp_Y_XYZp.y = exp2( ( Le - 127.0 ) / 2.0 );
81: 	Xp_Y_XYZp.z = Xp_Y_XYZp.y / value.y;
82: 	Xp_Y_XYZp.x = value.x * Xp_Y_XYZp.z;
83: 	vec3 vRGB = cLogLuvInverseM * Xp_Y_XYZp.rgb;
84: 	return vec4( max( vRGB, 0.0 ), 1.0 );
85: }
86: vec4 linearToOutputTexel( vec4 value ) { return LinearToLinear( value ); }
87: 
88: 
89:                 uniform float time;
90:                 uniform float audioLevel;
91:                 uniform float beatPulse;
92:                 uniform vec2 mouse; // Global mouse UV
93:                 uniform vec3 gridColor; // For the grid lines
94: 
95:                 varying vec2 vUv;
96:                 varying vec3 vPosition;
97:                 varying float vDistortion;
98:                 
99:                 vec3 hsv2rgb(vec3 c) {
100:                     vec4 K = vec4(1.0, 2.0 / 3.0, 1.0 / 3.0, 3.0);
101:                     vec3 p = abs(fract(c.xxx + K.xyz) * 6.0 - K.www);
102:                     return c.z * mix(K.xxx, clamp(p - K.xxx, 0.0, 1.0), c.y);
103:                 }
104:                 
105:                 void main() {
106:                     vec2 center = vUv - 0.5;
107:                     float dist = length(center);
108:                     
109:                     float hue = 0.55 + time * 0.02 + vDistortion * 2.0 + audioLevel * 0.3;
110:                     float sat = 0.7 + vDistortion * 0.3;
111:                     float brightness = 0.15 + vDistortion * 1.5 + audioLevel * 0.8 + beatPulse * 0.5;
112: 
113:                     // Pulsating grid lines directly drawn in the shader (for overall background grid)
114:                     vec2 gridCoords = vUv * vec2(8.0, 4.0); // 8 columns, 4 rows on this plane
115:                     vec2 gridFract = fract(gridCoords);
116: 
117:                     float lineThickness = 0.02; 
118:                     float horizontalLine = smoothstep(lineThickness, lineThickness + 0.01, gridFract.y) - smoothstep(1.0 - lineThickness, 1.0 - lineThickness + 0.01, gridFract.y);
119:                     float verticalLine = smoothstep(lineThickness, lineThickness + 0.01, gridFract.x) - smoothstep(1.0 - lineThickness, 1.0 - lineThickness + 0.01, gridFract.x);
120:                     
121:                     float gridEffect = max(horizontalLine, verticalLine);
122:                     gridEffect = max(gridEffect, 1.0 - smoothstep(0.98, 1.0, gridFract.x)); // Rightmost vertical line
123:                     gridEffect = max(gridEffect, 1.0 - smoothstep(0.98, 1.0, gridFract.y)); // Bottommost horizontal line
124: 
125: 
126:                     vec3 baseColor = hsv2rgb(vec3(hue, sat, brightness));
127:                     vec3 gridLineColor = gridColor * (1.0 + beatPulse * 1.5);
128:                     color = mix(baseColor, gridLineColor, gridEffect);
129:                     
130:                     // Add visual cursor glow (only for right half of the screen)
131:                     vec2 globalMouse = mouse;
132:                     float cursorDist;
133:                     if (globalMouse.x > 0.5) { // Only show glow on right half
134:                         vec2 localMouse = vec2((globalMouse.x - 0.5) * 2.0, globalMouse.y);
135:                         cursorDist = distance(vUv, localMouse);
136:                     } else {
137:                         cursorDist = 100.0; // Effectively turn off glow on left
138:                     }
139:                    
140:                     float cursorGlow = smoothstep(0.1, 0.0, cursorDist) * (0.5 + audioLevel * 1.0); 
141:                     color += vec3(0.1, 0.2, 0.3) * cursorGlow; 
142:                     
143:                     float alpha = 0.9 - dist * 0.5 + vDistortion + audioLevel * 0.1;
144:                     alpha = clamp(alpha, 0.0, 1.0); 
145:                     
146:                     gl_FragColor = vec4(color, alpha);
147:                 }
148:             

favicon.ico:1 
 Failed to load resource: the server responded with a status of 404 (Not Found)
FIXING THIS PROBLEM MAKES THE SEQUENCER LOOK WORSE
CORE MECHANICS MISSIING
DO NOT PASS GO
DO NOT COLLECT 200
GO BACKWARDS TO FIND THE ANCESTRAL SOLUTION



BIG FAIL
mbeck-10.html (NESTED FIELD)
FAIL! Audio cannot be started. Please ensure your browser allows autoplay or try refreshing.


MIGHTY MORPH
mighty-morph-09.html (DOUBLE DECKER)
looks good more minimal than others but the sequencer is a little off and might cause a seizure no clear sample mechanic but it does sound like a drone and does seem to be connected to it. the main issue is that the sequencer seems like a secondary thought the actual boxes to press compared to the graphic on top. would be better if the sequencer was the same as the morphic field and not offcentered. also would be better if sampler was more directly and clearly part of what was being sequenced. its unclear in the sequencer how it is able to control the sound if it is not being interacted with by me. what is the relationship between the sequencer and the morphic field? there's alot going on visually in the sequencer and we need more direct control of the morphic field and the sequencer. 

### Lessons from `mighty-morph-09.html` Failure (July 23, 2025)

1.  **The "Sledgehammer" approach is counterproductive.** Attempting to replace a large, complex script in a single step is fragile and prone to timeouts or other failures. It's an all-or-nothing strategy that offers no path for incremental success.
2.  **Isolate logic before debugging.** When faced with a large, broken inline `<script>`, the first step should be to extract it into a separate `.js` file. This separates concerns and makes the code much easier to analyze and debug in a controlled environment.
3.  **A "Steady" approach requires a full diagnosis first.** Small, incremental changes are the correct path, but they will fail if the underlying diagnosis is wrong. I must ensure I have the full context and understand the scope of the errors *before* beginning incremental fixes, rather than discovering larger issues midway through.
 MORE SO, IF YOU WANT TO MAKE A CHANGE TO A LARGE CODE BASE, YOU MUST UNDERSTAND THE CODE BASE FIRST. YOU MUST UNDERSTAND THE CODE BASE BEFORE YOU BEGIN TO CHANGE IT. YOU MUST UNDERSTAND THE CODE BASE BEFORE YOU BEGIN TO DEBUG IT. YOU MUST UNDERSTAND THE CODE BASE BEFORE YOU BEGIN TO TEST IT. YOU MUST UNDERSTAND THE CODE BASE BEFORE YOU BEGIN TO DEPLOY IT.

 A SMALL CHANGE TO A LARGE CODE IS THE MOST DANGEROUS OF CHANGES. LEARNING TO LET GO OF LEGACY PROBLEMS OR DIAGNOSING BRITTLENESS IS CRITICAL TO SUCCESS.


 