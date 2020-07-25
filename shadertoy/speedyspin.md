<a href="https://sm.ms/image/5rnZFKpH4NJPT1j" target="_blank" align="left"><img src="https://i.loli.net/2020/07/24/5rnZFKpH4NJPT1j.gif" ></a>

code from: https://www.shadertoy.com/view/tsSyDD

```csharp
Shader "Unlit/crystal"
{
    Properties
    {
        _MainTex ("Texture", 2D) = "white" {}
    }
    SubShader
    {
        Tags { "RenderType"="Opaque" }
        LOD 100

        Pass
        {
            CGPROGRAM
            #pragma vertex vert
            #pragma fragment frag
            // make fog work
            #pragma multi_compile_fog

            #include "UnityCG.cginc"

            struct appdata
            {
                float4 vertex : POSITION;
                float2 uv : TEXCOORD0;
            };

            struct v2f
            {
                float2 uv : TEXCOORD0;
                UNITY_FOG_COORDS(1)
                float4 vertex : SV_POSITION;
            };

            sampler2D _MainTex;
            float4 _MainTex_ST;
            
            #define NUM_OCTAVES 5
            float rand(float2 n) { 
                return frac(sin(dot(n, float2(12.9898, 4.1414))) * 43758.5453);
            }
            
            float noise(float2 p){
                float2 ip = floor(p);
                float2 u = frac(p);
                u = u*u*(3.0-2.0*u);
                
                float res = lerp(
                    lerp(rand(ip),rand(ip+float2(1.0,0.0)),u.x),
                    lerp(rand(ip+float2(0.0,1.0)),rand(ip+float2(1.0,1.0)),u.x),u.y);
                return res*res;
            }
            
            float fbm(float2 x) {
                float v = 0.0;
                float a = 0.5;
                float2 shift = float2(100,0);
                // Rotate to reduce axial bias
                float2x2 rot = float2x2(cos(0.5), sin(0.5), -sin(0.5), cos(0.50));
                for (int i = 0; i < NUM_OCTAVES; ++i) {
                    v += a * noise(x);
                    x = mul(x, rot) * 2.0 + shift;
                    a *= 0.5;
                }
                return v;
            }
            
            float pattern( in float2 p )
            {
                float2 q = float2( fbm( p + float2(0.0,0.0) ),
                               fbm( p + float2(5.2,1.3) ) );
            
                return fbm( p + 4.0*q );
            }
            
            float3 colour(float x) {
                float3 a = float3(0.5, 0.5, 0.5);
                float3 b = float3(0.5, 0.5, 0.5);
                float3 c = float3(1.0, 1.0, 1.0);
                float3 d = float3(0.0, 0.0, 0.0);
                
                return a + b * cos(6.28318*(c*x+d));
            }
            
            float2 rotate(float2 v, float a) {
                float s = sin(a);
                float c = cos(a);
                float2x2 m = float2x2(c, -s, s, c);
                return mul(v, m);
            }

            v2f vert (appdata v)
            {
                v2f o;
                o.vertex = UnityObjectToClipPos(v.vertex);
                o.uv = TRANSFORM_TEX(v.uv, _MainTex);
                UNITY_TRANSFER_FOG(o,o.vertex);
                return o;
            }

            fixed4 frag (v2f i) : SV_Target
            {              
                float2 uv = i.uv;
                uv = 0.7*abs(2.0*(uv-0.5));
                uv.y += uv.x / 10.0;
            
                float noise = fbm(_Time.y * 0.2 + uv * 10.0) / 4.0;
                float noiseTex = pattern(rotate(uv*10.0*(1.0+(0.05*sin(_Time.y*0.5))), _Time.y*0.01)) * 0.5+cos(0.2*_Time.y+sin(_Time.y)*0.2);
                
                float inside = (sin((uv.x+uv.y))*10.0) + (uv.y*10.0+uv.x*10.0);
                float mask = (uv.y*2.0 + sin(inside)) / 1.5;
                
                float3 col = colour(noiseTex+mask);
                col.rg += -abs(rotate(uv, 0.3*_Time.y+noise*10.0).xy);
            
                return float4(col,1.0);
            }
            ENDCG
        }
    }
}
```

