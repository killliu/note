MeshComplier.cs 将要收集的场景mesh数据改为unity生成的navmesh

```c#
public static GameObject o;
private bool Load(InputBuildContext context)
{
    context.info.loaderCount++;
    //int count = context.LoadFromScene<MeshFilter>();
    //context.Log(string.Format("{0}: Loaded {1} MeshFilters", name, count), this);
    GameObject ma = GameObject.Find("#ForNavmesh#");
    if (null != ma)
        o = Instantiate(ma);
    else
    {
        o = new GameObject("temp");
        MeshFilter mf = o.AddComponent<MeshFilter>();
        NavMeshTriangulation mt = NavMesh.CalculateTriangulation();
        Mesh m = new Mesh();
        m.vertices = mt.vertices;
        m.triangles = mt.indices;
        m.RecalculateNormals();
        m.RecalculateTangents();
        mf.sharedMesh = m;

        string p = GetPath("map", "asset");
        string rp = Application.dataPath.Replace("Assets", p);
        if (File.Exists(rp))
            File.Delete(rp);
        AssetDatabase.CreateAsset(o.GetComponent<MeshFilter>().sharedMesh, p);
        AssetDatabase.SaveAssets();
        AssetDatabase.Refresh();
    }
    int count = context.LoadFromScene(new MeshFilter[1] { o.GetComponent<MeshFilter>() });
    context.Log(string.Format("{0}: Loaded {1} MeshFilters", name, count), this);
    int mts = o.GetComponent<MeshFilter>().sharedMesh.triangles.Length;
    int mvs = o.GetComponent<MeshFilter>().sharedMesh.vertices.Length;
    Debug.Log("<color='green'>triangles: " + mts + ", vertices: " + mvs + " </color>");
    return true;
}

private string GetPath(string name, string extion)
{
    string p = SceneManager.GetActiveScene().path;
    if (string.IsNullOrEmpty(p))
        return "Assets";
    string[] s = p.Split('/');
    return p.Replace(s[s.Length - 1], name + "." + extion);
}
```

NavmeshBuildEditor.cs 的 OnGUIStandard 函数中修改为如下：

```c#
if (GUILayout.Button("Build & Bake"))
{
    //NavmeshBuildHelper helper = new NavmeshBuildHelper(build);
    //helper.Build();
    EditorApplication.delayCall += () =>
    {
        NavmeshBuildHelper helper = new NavmeshBuildHelper(build);
        helper.Build();
        if (null != MeshCompiler.o)
            GameObject.DestroyImmediate(MeshCompiler.o);
    };
}
```

NavmeshBuild.asset 面板中参数修改 参考NMGenParams.cs ：

1. Walkable Radius	: 0        <font color="darkgray">  ( navgation 已经是计算好了的可行走区域)</font>

2. XZ Cell Size		: 0.01      <font color="darkgray"> ( navmesh 最小化计算)</font>

3. Y Cell Size       : 0.01

4. Tile Size        : 0        <font color="darkgray"> ( 0 不划分区域，eg. 500 -> 5, 划分区域)</font>  

     <font color="darkgray">5 = 500 * 0.01 即 vx = Tile Size * XZ Cell Size</font>