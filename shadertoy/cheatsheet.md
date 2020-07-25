| shadertoy             | unityshader                        |
| --------------------- | ---------------------------------- |
| vec<n>                | float<n>                           |
| mat<n>                | float<n>x<n>                       |
| fract                 | frac                               |
| mix                   | lerp                               |
| mod                   | x-y*floor(x/y)                     |
| atan(x,y)             | atan2(y,x)                         |
| texture               | tex2D                              |
| vec3(1) ok            | float3(1)not ok float3(1,1,1) or 1 |
| vec3 a = 1 not ok     | float3 a = 1 ok                    |
| vec3 up = vec3(0,1,0) | static float3 up = float3(0,1,0)   |
| use *                 | use mul()                          |
| mainImage             | frag or surf                       |
| fragCoor/iResolution  | i.uv or IN.uv_MainTex              |
| iTime                 | _Time.y                            |
| iMouse                | have to manually add               |
| fragColor = color     | return color                       |
| iChannel<n>           | have to manually add               |
| None                  | quite a lot                        |
| mat<n> * vec<n>       | mul(float<n>, float<n>x<n>)        |

fragCoord.xy / iResolution.xy 会将坐标转换到 [0, 1] 之间

iResolution 表示画布像素高宽