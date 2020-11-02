# UnityToonShader
 The Toon Shader creating Cartoon Effect


Toon Shader is one of the artistic effects that converts the realistic world to photo like world.

Firstly,
Use normal procedure to create a standard shader, (Receive World Directional Light, Add normal, Dot Product to Calculate light strength and convert it to screen space.)


Then,
var lightStrength
As Cartoon-like world may not have progressively changed light, change it to dark / light only or have four degree of brightness with ambient light added in while we use “light = lightStrengh * _LightColor0” to calculate world light.




Thirdly, Blinn-Phong Reflection
To create a Specular Effect, Pass TextureCoordinate and calculate the vertex’s position to camera’s position.
Blinn-Phong Reflection essentially needs the effective light direction, a project to the vector of the camera’s view. So, Calculate Var HalfVector as an effective light directly to the camera view. 
var HalfVector: the direction of the reflection as it is the combination of view angle and light source direction.
The final value is a float calculated through the dot product between HalfVector and Normal and isLit, which suggests the final amount of light and using “isLit” to calculate if it is light up right now. The ‘Glossiness’ is the power/ The strength of the reflection.
Finally, use smoothstep() function to make the reflection changes suddenly between pixels to make the artistic effects.





RimEffect:

To make the object pop out, I wanna add light to the edge of the object, how to do that?
Simply Added more light to the pixels that face away from the view direction.
Calculate var rimLight = 1 - dot(viewDir, normal).
Simply add rimLight to the final Coordinate, get following effect:



Alternative:

Also if  rimLight = dot(viewDir, normal), the light is actually reduced at the edge of the object since the dot product ranged between (-1,1), can be negative:





Rim Shine only on bright side:

Use var rimIntensity as the intensity of rim effect, we use smoothstep() again to make the effects that the pixels are jumped suddenly (sudden bright or dark).
Use rimLight * pow(BlinnStrength, _RimThreshold);  Set up the threshold and Reflection amount by quadratic.




Shadows:

Casting Shadows are Enabled by adding: 
UsePass "Legacy Shaders/VertexLit/SHADOWCASTER"
This line uses another shader’s pass from unity’s vertexLit Shader Library.

Use Unity’s standard Library and built in Variable
var SHADOW_ATTENUATION(i).
This returns 0 - 1 depending on whether the object is shadowed.
Not only multiply this with light factor to create shadows, 
Also multiply this factor with reflection light so it doesn’t generate reflection under Shadows.


