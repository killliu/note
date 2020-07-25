<a href="https://sm.ms/image/3nbeD4dNCJzGqhK" target="_blank" alin="left"><img src="https://i.loli.net/2020/07/24/3nbeD4dNCJzGqhK.gif" ></a>

code from: https://www.shadertoy.com/view/wtdGDs

```csharp
Shader "Unlit/poorWater"
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

            v2f vert (appdata v)
            {
                v2f o;
                o.vertex = UnityObjectToClipPos(v.vertex);
                o.uv = TRANSFORM_TEX(v.uv, _MainTex);
                UNITY_TRANSFER_FOG(o,o.vertex);
                return o;
            }


            float3 mod289(float3 x) {
              return x - floor(x * (1.0 / 289.0)) * 289.0;
            }

            float4 mod289(float4 x) {
              return x - floor(x * (1.0 / 289.0)) * 289.0;
            }

            float4 permute(float4 x) {
                return mod289(((x*34.0)+1.0)*x);
            }

            float4 taylorInvSqrt(float4 r)
            {
              return 1.79284291400159 - 0.85373472095314 * r;
            }

            float snoise(float3 v, out float3 gradient)
            {
              const float2  C = float2(1.0/6.0, 1.0/3.0) ;
              const float4  D = float4(0.0, 0.5, 1.0, 2.0);

            // First corner
              float3 i  = floor(v + dot(v, C.yyy) );
              float3 x0 =   v - i + dot(i, C.xxx) ;

            // Other corners
              float3 g = step(x0.yzx, x0.xyz);
              float3 l = 1.0 - g;
              float3 i1 = min( g.xyz, l.zxy );
              float3 i2 = max( g.xyz, l.zxy );

              //   x0 = x0 - 0.0 + 0.0 * C.xxx;
              //   x1 = x0 - i1  + 1.0 * C.xxx;
              //   x2 = x0 - i2  + 2.0 * C.xxx;
              //   x3 = x0 - 1.0 + 3.0 * C.xxx;
              float3 x1 = x0 - i1 + C.xxx;
              float3 x2 = x0 - i2 + C.yyy; // 2.0*C.x = 1/3 = C.y
              float3 x3 = x0 - D.yyy;      // -1.0+3.0*C.x = -0.5 = -D.y

            // Permutations
              i = mod289(i); 
              float4 p = permute( permute( permute( 
                        i.z + float4(0.0, i1.z, i2.z, 1.0 ))
                      + i.y + float4(0.0, i1.y, i2.y, 1.0 )) 
                      + i.x + float4(0.0, i1.x, i2.x, 1.0 ));

            // Gradients: 7x7 points over a square, mapped onto an octahedron.
            // The ring size 17*17 = 289 is close to a multiple of 49 (49*6 = 294)
              float n_ = 0.142857142857; // 1.0/7.0
              float3  ns = n_ * D.wyz - D.xzx;

              float4 j = p - 49.0 * floor(p * ns.z * ns.z);  //  mod(p,7*7)

              float4 x_ = floor(j * ns.z);
              float4 y_ = floor(j - 7.0 * x_ );    // mod(j,N)

              float4 x = x_ *ns.x + ns.yyyy;
              float4 y = y_ *ns.x + ns.yyyy;
              float4 h = 1.0 - abs(x) - abs(y);

              float4 b0 = float4( x.xy, y.xy );
              float4 b1 = float4( x.zw, y.zw );

              //float4 s0 = float4(lessThan(b0,0.0))*2.0 - 1.0;
              //float4 s1 = float4(lessThan(b1,0.0))*2.0 - 1.0;
              float4 s0 = floor(b0)*2.0 + 1.0;
              float4 s1 = floor(b1)*2.0 + 1.0;
              float4 sh = -step(h, float4(0.0,0,0,0));

              float4 a0 = b0.xzyw + s0.xzyw*sh.xxyy ;
              float4 a1 = b1.xzyw + s1.xzyw*sh.zzww ;

              float3 p0 = float3(a0.xy,h.x);
              float3 p1 = float3(a0.zw,h.y);
              float3 p2 = float3(a1.xy,h.z);
              float3 p3 = float3(a1.zw,h.w);

            //Normalise gradients
              float4 norm = taylorInvSqrt(float4(dot(p0,p0), dot(p1,p1), dot(p2, p2), dot(p3,p3)));
              p0 *= norm.x;
              p1 *= norm.y;
              p2 *= norm.z;
              p3 *= norm.w;

            // Mix final noise value
              float4 m = max(0.6 - float4(dot(x0,x0), dot(x1,x1), dot(x2,x2), dot(x3,x3)), 0.0);
              float4 m2 = m * m;
              float4 m4 = m2 * m2;
              float4 pdotx = float4(dot(p0,x0), dot(p1,x1), dot(p2,x2), dot(p3,x3));

            // Determine noise gradient
              float4 temp = m2 * m * pdotx;
              gradient = -8.0 * (temp.x * x0 + temp.y * x1 + temp.z * x2 + temp.w * x3);
              gradient += m4.x * p0 + m4.y * p1 + m4.z * p2 + m4.w * p3;
              gradient *= 42.0;

              return 42.0 * dot(m4, pdotx);
            }
            // END OF COPY PASTE SIMPLEX NOISE
                fixed4 frag (v2f i) : SV_Target
                {
                            
                // Normalized pixel coordinates (from 0 to 1)
                float2 uv = i.uv;//fragCoord/iResolution.xy;

                float2 t = float2(.5 * _Time.y, .5);
                // sample moving noise in 4 directions, with slower morph
                float3 grad0, grad1, grad2, grad3;

                snoise(float3(2.*i.vertex.xy/_ScreenParams.y + t.xy, .125*t.x), grad0);
                snoise(float3(2.*i.vertex.yx/_ScreenParams.y - t.xy, .125*t.x), grad1);
                snoise(float3(2.*i.vertex.xy/_ScreenParams.y + t.yx, .125*t.x), grad2);
                snoise(float3(2.*i.vertex.yx/_ScreenParams.y - t.yx, .125*t.x), grad3);
                
                // take xy-gradient as surface partial derivative, add together
                float2 grad = grad0.xy + grad1.xy + grad2.xy + grad3.xy;
                
                // light dir
                float3 L = normalize(float3(-1,1,3));
                // camera dir
                float3 C = float3(0,0,1);
                // normal
                float3 N = normalize(float3(grad.xy, 4.));
                
                float lit = pow(max(0.,dot(normalize(L+C), N)), 20.);
                float frac = 1.-max(0., dot(N, C));
                frac *= frac;
                
                float3 b = tex2D(_MainTex, uv + (4./_ScreenParams.xy)*N.xy).xyz;
                
                b = (1.-frac) * b + frac*(10.*lit);
                
                // Output to screen
                return float4(b,1.0);
            }

            ENDCG
        }
    }
}
```

