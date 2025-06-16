# VS Code 3D File Format Proposal: .vs3d Format

## Executive Summary

For **VS Code 3D**, we propose the **`.vs3d`** format - a JSON-based 3D scene format specifically designed for AI manipulation while maintaining full compatibility with three.js. This format combines the best aspects of glTF with enhanced AI-readability and VS Code integration.

## Format Design Philosophy

### Core Principles
1. **AI-First Design**: Structured for easy parsing and manipulation by AI assistants
2. **Human-Readable**: JSON-based with clear, descriptive property names
3. **Three.js Native**: Direct compatibility with three.js without conversion
4. **Version Control Friendly**: Text-based format that works well with Git
5. **Extensible**: Plugin architecture for custom features
6. **Performance Optimized**: Efficient loading and rendering

### Why Not Existing Formats?

| Format | Pros | Cons for AI |
|--------|------|-------------|
| **glTF/GLB** | Industry standard, three.js compatible | Complex structure, binary components, verbose |
| **OBJ** | Simple, text-based | Limited features, no scene hierarchy |
| **FBX** | Feature-rich | Binary format, not AI-readable |
| **Three.js JSON** | three.js native | Deprecated, limited features |

## .vs3d Format Specification

### File Structure
```json
{
  "metadata": {
    "version": "1.0",
    "generator": "VS Code 3D",
    "created": "2024-01-15T10:30:00Z",
    "description": "AI-friendly 3D scene format"
  },
  "scene": {
    "name": "My 3D Scene",
    "background": "#87CEEB",
    "environment": null,
    "nodes": []
  },
  "assets": {
    "geometries": {},
    "materials": {},
    "textures": {},
    "animations": {}
  },
  "aiMetadata": {
    "intent": "architectural visualization",
    "style": "modern",
    "complexity": "medium",
    "tags": ["building", "interior", "lighting"]
  }
}
```

### Node Structure (AI-Optimized)
```json
{
  "id": "unique-node-id",
  "name": "Living Room Chair",
  "type": "mesh",
  "description": "Modern ergonomic chair with blue fabric",
  "transform": {
    "position": [0, 0, 0],
    "rotation": [0, 0, 0],
    "scale": [1, 1, 1]
  },
  "geometry": "chair-geometry-id",
  "material": "fabric-material-id",
  "visible": true,
  "castShadow": true,
  "receiveShadow": true,
  "aiContext": {
    "category": "furniture",
    "function": "seating",
    "style": "modern",
    "material": "fabric",
    "color": "blue"
  }
}
```

### Geometry Definition (Simplified)
```json
{
  "chair-geometry-id": {
    "type": "BufferGeometry",
    "description": "Chair mesh with 1,234 vertices",
    "attributes": {
      "position": {
        "type": "Float32Array",
        "itemSize": 3,
        "data": [/* vertex positions */],
        "count": 1234
      },
      "normal": {
        "type": "Float32Array",
        "itemSize": 3,
        "data": [/* normals */]
      },
      "uv": {
        "type": "Float32Array",
        "itemSize": 2,
        "data": [/* texture coordinates */]
      }
    },
    "index": [/* triangle indices */],
    "boundingBox": {
      "min": [-1, 0, -1],
      "max": [1, 2, 1]
    }
  }
}
```

### Material System (PBR + AI Context)
```json
{
  "fabric-material-id": {
    "type": "MeshStandardMaterial",
    "name": "Blue Fabric",
    "description": "Soft blue upholstery fabric with subtle texture",
    "properties": {
      "baseColor": "#4169E1",
      "roughness": 0.8,
      "metalness": 0.0,
      "opacity": 1.0
    },
    "textures": {
      "baseColorMap": "fabric-diffuse-texture-id",
      "normalMap": "fabric-normal-texture-id",
      "roughnessMap": "fabric-roughness-texture-id"
    },
    "aiContext": {
      "materialType": "fabric",
      "feel": "soft",
      "pattern": "solid",
      "durability": "high"
    }
  }
}
```

## AI-Friendly Features

### 1. Semantic Descriptions
Every object includes human-readable descriptions that help AI understand intent:
```json
{
  "description": "A modern office chair with ergonomic back support",
  "aiContext": {
    "purpose": "provide comfortable seating for work",
    "targetUser": "office worker",
    "useCase": "8-hour workday"
  }
}
```

### 2. Hierarchical Organization
Clear parent-child relationships with semantic grouping:
```json
{
  "name": "Office Setup",
  "children": [
    {
      "name": "Desk Area",
      "children": ["desk", "chair", "monitor", "keyboard"]
    },
    {
      "name": "Storage",
      "children": ["bookshelf", "filing-cabinet"]
    }
  ]
}
```

### 3. Constraint System
Define relationships and constraints that AI can understand and maintain:
```json
{
  "constraints": {
    "chairPosition": {
      "type": "relativeTo",
      "target": "desk",
      "offset": [0, 0, -0.8],
      "description": "Chair should be positioned in front of desk"
    }
  }
}
```

### 4. Modification History
Track changes for AI context and version control:
```json
{
  "modifications": [
    {
      "timestamp": "2024-01-15T10:30:00Z",
      "action": "moved",
      "target": "chair",
      "description": "Moved chair closer to desk for better ergonomics",
      "agent": "AI Assistant"
    }
  ]
}
```

## Three.js Integration

### Direct Loading
```javascript
// VS Code 3D Loader
import { VS3DLoader } from './loaders/VS3DLoader.js';

const loader = new VS3DLoader();
loader.load('scene.vs3d', (scene) => {
  // Scene is ready to use with three.js
  renderer.render(scene, camera);
});
```

### Real-time Synchronization
```javascript
// Bi-directional sync between editor and three.js
const scene3D = VS3DManager.loadScene('project.vs3d');

// Changes in 3D viewport update the file
scene3D.addEventListener('objectMoved', (event) => {
  VS3DManager.updateNode(event.object.id, {
    transform: { position: event.newPosition }
  });
});

// File changes update the 3D scene
VS3DManager.addEventListener('fileChanged', (event) => {
  scene3D.updateFromFile(event.changes);
});
```

## AI Assistant Integration Examples

### 1. Natural Language Scene Manipulation
```typescript
// AI can easily parse and modify scenes
interface VS3DScene {
  metadata: SceneMetadata;
  scene: SceneGraph;
  assets: AssetLibrary;
  aiMetadata: AIContext;
}

// Example AI operation
function addChairNearDesk(scene: VS3DScene): void {
  const desk = scene.scene.nodes.find(n => n.aiContext.category === 'desk');
  const chairGeometry = scene.assets.geometries['standard-chair'];

  const newChair = {
    id: generateId(),
    name: 'Office Chair',
    type: 'mesh',
    description: 'Ergonomic office chair placed near desk',
    transform: {
      position: [desk.transform.position[0], 0, desk.transform.position[2] - 1]
    },
    geometry: 'standard-chair',
    material: 'office-chair-material',
    aiContext: {
      category: 'furniture',
      function: 'seating',
      relationship: 'paired-with-desk'
    }
  };

  scene.scene.nodes.push(newChair);
}
```

### 2. Style and Theme Application
```json
{
  "themes": {
    "modern-office": {
      "colorPalette": ["#FFFFFF", "#333333", "#4169E1"],
      "materials": {
        "primary": "brushed-metal",
        "secondary": "white-matte",
        "accent": "royal-blue"
      },
      "lighting": "bright-neutral"
    }
  }
}
```

### 3. Automated Optimization
```javascript
// AI can analyze and optimize scenes
function optimizeScene(scene) {
  // Merge similar materials
  const materials = Object.values(scene.assets.materials);
  const duplicates = findSimilarMaterials(materials);

  // Simplify geometry for distant objects
  scene.scene.nodes.forEach(node => {
    if (node.aiContext.importance === 'background') {
      node.geometry = simplifyGeometry(node.geometry);
    }
  });

  // Optimize lighting setup
  optimizeLighting(scene.scene.lights);
}
```

## Development Workflow

### 1. Creation
```bash
# Create new 3D project
VS Code 3D: Create Project
> Choose template: "Empty Scene"
> Creates: project.vs3d
```

### 2. Editing
- **Visual Editor**: Drag-and-drop 3D modeling
- **Code Editor**: Direct JSON editing with IntelliSense
- **AI Assistant**: Natural language modifications

### 3. Version Control
```bash
git add project.vs3d
git commit -m "Added office furniture layout"
git diff HEAD~1 project.vs3d  # Shows human-readable changes
```

### 4. Export
```javascript
// Export to other formats
VS3D.export('project.vs3d', {
  format: 'gltf',
  options: { includeAnimations: true }
});
```

## File Size and Performance

### Optimization Strategies
1. **Asset References**: Large binary data stored separately
2. **Compression**: Optional gzip compression for production
3. **LOD Support**: Multiple detail levels for geometry
4. **Streaming**: Progressive loading for large scenes

### Example Asset Structure
```
project/
├── project.vs3d          # Main scene file (text)
├── assets/
│   ├── textures/         # Image files
│   ├── models/           # Binary geometry data
│   └── materials/        # Shared material definitions
└── exports/              # Generated formats (gltf, obj, etc.)
```

## Implementation Roadmap

### Phase 1: Core Format
- [x] JSON schema definition
- [ ] Basic three.js loader
- [ ] VS Code integration
- [ ] File validation

### Phase 2: AI Features
- [ ] AI metadata system
- [ ] Natural language API
- [ ] Constraint system
- [ ] Auto-optimization

### Phase 3: Advanced Features
- [ ] Animation support
- [ ] Physics integration
- [ ] Multi-user collaboration
- [ ] Cloud synchronization

## Conclusion

The **`.vs3d`** format provides the perfect balance between AI-friendliness and 3D functionality. By combining JSON's readability with three.js compatibility and AI-specific metadata, we create a format that enables seamless collaboration between humans, AI assistants, and 3D engines.

This format will empower developers to:
- **Code in 3D**: Edit scenes programmatically with full AI assistance
- **Collaborate Naturally**: Use version control and merge changes easily
- **Build Faster**: Leverage AI for rapid prototyping and optimization
- **Share Universally**: Export to any standard 3D format when needed

The future of 3D development is collaborative, and `.vs3d` makes that future possible today.
