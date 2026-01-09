# CLAUDE.md - Unity Project Guidelines

This file provides guidance for AI assistants (like Claude) working on this Unity project.

## Unity MCP Integration

This project uses the Unity MCP (Model Context Protocol) server for direct Unity Editor integration. Always prefer MCP tools over manual file editing when possible.

### Key MCP Tools

- **manage_gameobject**: Create, modify, delete, and query GameObjects
- **manage_components**: Add, remove, and configure components
- **manage_scene**: Scene operations and hierarchy management
- **manage_material**: Material creation and property modification
- **manage_script**: Script creation and editing (prefer `script_apply_edits` for structured changes)
- **manage_vfx**: Visual effects (particles, VFX Graph, line/trail renderers)
- **manage_asset**: Asset import, search, and management

### MCP Best Practices

1. **Use batch_execute for multiple operations**: When creating multiple objects or making several changes, use `batch_execute` to reduce latency and token costs by 10-100x
2. **Search before creating**: Use `find_gameobjects` to check if objects already exist before creating duplicates
3. **Prefer instance IDs**: When targeting GameObjects, use instance IDs (`search_method="by_id"`) for reliability over names or paths
4. **Read before editing**: Use the `view` tool on files before making edits to verify exact content
5. **Validate after changes**: Check console output with `read_console` after operations to catch errors early

## Unity Architecture Principles

### GameObject Usage

**DO:**
- Use standard Unity primitive types (Cube, Sphere, Cylinder, Capsule, Plane, Quad)
- Leverage built-in components (Rigidbody, Collider, Renderer, etc.)
- Use prefabs for reusable objects
- Follow Unity naming conventions (PascalCase for scripts, descriptive names for GameObjects)

**DON'T:**
- Create custom GameObject types when built-in primitives suffice
- Reinvent built-in functionality (physics, rendering, input)
- Create overly complex hierarchies when flat structures work

### Component Design

**Favor composition over inheritance:**
```csharp
// GOOD - Small, focused components
public class Health : MonoBehaviour { }
public class Movement : MonoBehaviour { }
public class Damage : MonoBehaviour { }

// AVOID - Monolithic components
public class PlayerEverything : MonoBehaviour { }
```

**Use standard component patterns:**
- MonoBehaviour for game logic
- ScriptableObjects for data/configuration
- Static classes for utilities (sparingly)

### Script Organization

```
Assets/
├── Scripts/
│   ├── Core/           # Essential game systems
│   ├── Gameplay/       # Game-specific logic
│   ├── UI/             # User interface
│   ├── Utilities/      # Helper classes
│   └── Data/           # ScriptableObjects
├── Prefabs/
├── Materials/
├── Scenes/
└── Resources/          # Use sparingly
```

## Code Standards

### Namespaces
```csharp
namespace YourProjectName.Gameplay
{
    // Your code here
}
```

### Serialization
```csharp
// GOOD - Expose in Inspector with SerializeField
[SerializeField] private float speed = 5f;

// AVOID - Public fields (unless intentionally exposing API)
public float speed = 5f;
```

### Unity Lifecycle Methods Order
```csharp
// Standard order
private void Awake() { }
private void OnEnable() { }
private void Start() { }
private void Update() { }
private void FixedUpdate() { }
private void LateUpdate() { }
private void OnDisable() { }
private void OnDestroy() { }
```

## Performance Considerations

1. **Cache component references** in Awake/Start, not in Update
2. **Use object pooling** for frequently instantiated objects
3. **Minimize GetComponent calls** - cache results
4. **Use tags and layers** efficiently for raycasts and physics
5. **Profile before optimizing** - don't prematurely optimize

## Common Unity Patterns

### Singleton (use sparingly)
```csharp
public class GameManager : MonoBehaviour
{
    public static GameManager Instance { get; private set; }
    
    private void Awake()
    {
        if (Instance != null && Instance != this)
        {
            Destroy(gameObject);
            return;
        }
        Instance = this;
        DontDestroyOnLoad(gameObject);
    }
}
```

### ScriptableObject Configuration
```csharp
[CreateAssetMenu(fileName = "NewConfig", menuName = "Game/Config")]
public class GameConfig : ScriptableObject
{
    public float playerSpeed = 5f;
    public int maxHealth = 100;
}
```

### Events (UnityEvent or C# events)
```csharp
using UnityEngine.Events;

public class Health : MonoBehaviour
{
    public UnityEvent<int> OnHealthChanged;
    
    private int currentHealth;
    
    public void TakeDamage(int amount)
    {
        currentHealth -= amount;
        OnHealthChanged?.Invoke(currentHealth);
    }
}
```

## Testing

- Use Unity Test Framework for unit and integration tests
- Place tests in `Assets/Tests/` (Editor and Play Mode)
- Keep game logic testable - avoid tight coupling to MonoBehaviour when possible

## Version Control

- Always include `.gitignore` for Unity (Library, Temp, Obj folders)
- Commit `.meta` files (essential for Unity)
- Use text serialization for scenes and prefabs (Editor Settings)
- Use Git LFS for large binary assets

## Scene Management

- Keep scenes focused and modular
- Use additive scene loading for complex projects
- Maintain a consistent scene structure:
  - `_Managers` (GameManager, etc.)
  - `_Environment` (terrain, props)
  - `_Lighting` (lights, reflection probes)
  - `_Gameplay` (players, enemies, objectives)
  - `_UI` (canvas elements)

## Asset Management

- Use consistent naming: `[Type]_[Name]_[Variant]`
  - Example: `MAT_Metal_Rusty`, `PREFAB_Enemy_Zombie`
- Keep asset folders organized by type and purpose
- Use AssetBundles or Addressables for runtime asset loading
- Optimize textures and models before import

## When to Create Custom Solutions

Create custom implementations when:
- Built-in solutions don't meet specific requirements
- Performance requires specialized handling
- Domain logic is complex and warrants abstraction

Avoid custom solutions for:
- Basic math operations (use Unity.Mathematics or Mathf)
- Object pooling (use Unity's Object Pooling API)
- Serialization (use Unity's serialization or JSON)

## Communication

When working with AI assistance:
1. **Specify Unity version** if targeting specific features
2. **Describe the desired behavior** rather than implementation
3. **Mention existing systems** to maintain consistency
4. **Request explanations** for unfamiliar patterns
5. **Ask for alternatives** when uncertain about approach

## Resources

- Unity Manual: https://docs.unity3d.com/Manual/
- Unity Scripting API: https://docs.unity3d.com/ScriptReference/
- Unity Best Practices: https://unity.com/how-to

---

*Last Updated: 2025-01-09*
*Unity Version: [Specify your version]*