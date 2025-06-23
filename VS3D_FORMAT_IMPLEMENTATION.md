# VS3D Format Implementation Plan 🚀

## Phase 1: Core Format Definition

### JSON Schema for .vs3d
```json
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "title": "VS3D Scene Format",
  "type": "object",
  "properties": {
    "metadata": {
      "type": "object",
      "properties": {
        "version": { "type": "string", "pattern": "^\\d+\\.\\d+$" },
        "generator": { "type": "string", "default": "VS Code 3D" },
        "created": { "type": "string", "format": "date-time" },
        "modified": { "type": "string", "format": "date-time" },
        "description": { "type": "string" }
      },
      "required": ["version", "generator"]
    },
    "scene": {
      "type": "object",
      "properties": {
        "name": { "type": "string" },
        "background": { "type": "string" },
        "fog": { "$ref": "#/definitions/fog" },
        "nodes": {
          "type": "array",
          "items": { "$ref": "#/definitions/node" }
        }
      }
    },
    "assets": {
      "type": "object",
      "properties": {
        "geometries": { "type": "object" },
        "materials": { "type": "object" },
        "textures": { "type": "object" },
        "animations": { "type": "object" }
      }
    },
    "aiMetadata": {
      "type": "object",
      "properties": {
        "intent": { "type": "string" },
        "style": { "type": "string" },
        "complexity": { "enum": ["low", "medium", "high"] },
        "tags": { "type": "array", "items": { "type": "string" } }
      }
    }
  },
  "required": ["metadata", "scene", "assets"]
}
```

## Phase 2: Three.js Loader Implementation

### VS3DLoader.js
```javascript
import { BufferGeometry, BufferAttribute, Mesh, Group, Scene } from 'three';
import { FileLoader } from 'three';

class VS3DLoader {
  constructor(manager) {
    this.manager = manager !== undefined ? manager : THREE.DefaultLoadingManager;
  }

  load(url, onLoad, onProgress, onError) {
    const scope = this;
    const loader = new FileLoader(scope.manager);
    loader.setPath(scope.path);
    loader.setResponseType('json');

    loader.load(url, function(json) {
      try {
        const scene = scope.parse(json);
        onLoad(scene);
      } catch (e) {
        if (onError) onError(e);
        else console.error(e);
      }
    }, onProgress, onError);
  }

  parse(json) {
    const vs3d = new VS3DDocument(json);
    return vs3d.buildThreeJSScene();
  }
}

class VS3DDocument {
  constructor(json) {
    this.json = json;
    this.geometries = new Map();
    this.materials = new Map();
    this.textures = new Map();
  }

  buildThreeJSScene() {
    const scene = new Scene();
    scene.name = this.json.scene.name || 'VS3D Scene';

    // Parse assets first
    this.parseGeometries();
    this.parseMaterials();
    this.parseTextures();

    // Build scene graph
    this.json.scene.nodes.forEach(nodeData => {
      const node = this.parseNode(nodeData);
      scene.add(node);
    });

    // Apply scene settings
    if (this.json.scene.background) {
      scene.background = new THREE.Color(this.json.scene.background);
    }

    return scene;
  }

  parseNode(nodeData) {
    let object;

    switch (nodeData.type) {
      case 'mesh':
        object = this.parseMesh(nodeData);
        break;
      case 'group':
        object = new Group();
        break;
      case 'light':
        object = this.parseLight(nodeData);
        break;
      default:
        object = new Group();
    }

    // Set common properties
    object.name = nodeData.name || '';
    object.userData.vs3d = {
      id: nodeData.id,
      description: nodeData.description,
      aiContext: nodeData.aiContext || {}
    };

    // Apply transform
    if (nodeData.transform) {
      const t = nodeData.transform;
      if (t.position) object.position.fromArray(t.position);
      if (t.rotation) object.rotation.fromArray(t.rotation);
      if (t.scale) object.scale.fromArray(t.scale);
    }

    // Add children
    if (nodeData.children) {
      nodeData.children.forEach(childData => {
        const child = this.parseNode(childData);
        object.add(child);
      });
    }

    return object;
  }

  parseMesh(nodeData) {
    const geometry = this.geometries.get(nodeData.geometry);
    const material = this.materials.get(nodeData.material);

    if (!geometry || !material) {
      console.warn('Missing geometry or material for mesh:', nodeData.name);
      return new Group();
    }

    const mesh = new Mesh(geometry, material);
    mesh.castShadow = nodeData.castShadow !== false;
    mesh.receiveShadow = nodeData.receiveShadow !== false;

    return mesh;
  }

  parseGeometries() {
    Object.entries(this.json.assets.geometries || {}).forEach(([id, geoData]) => {
      const geometry = new BufferGeometry();

      // Parse attributes
      Object.entries(geoData.attributes || {}).forEach(([name, attrData]) => {
        const array = new (this.getTypedArrayConstructor(attrData.type))(attrData.data);
        const attribute = new BufferAttribute(array, attrData.itemSize);
        geometry.setAttribute(name, attribute);
      });

      // Parse index
      if (geoData.index) {
        const indexArray = new Uint16Array(geoData.index);
        geometry.setIndex(new BufferAttribute(indexArray, 1));
      }

      // Set bounding box/sphere
      if (geoData.boundingBox) {
        geometry.boundingBox = new THREE.Box3(
          new THREE.Vector3().fromArray(geoData.boundingBox.min),
          new THREE.Vector3().fromArray(geoData.boundingBox.max)
        );
      }

      this.geometries.set(id, geometry);
    });
  }

  getTypedArrayConstructor(type) {
    switch (type) {
      case 'Float32Array': return Float32Array;
      case 'Uint16Array': return Uint16Array;
      case 'Uint32Array': return Uint32Array;
      default: return Float32Array;
    }
  }
}

export { VS3DLoader };
```

## Phase 3: VS3D Writer/Exporter

### VS3DExporter.js
```javascript
class VS3DExporter {
  constructor() {
    this.options = {};
  }

  parse(scene, options = {}) {
    this.options = options;

    const vs3dData = {
      metadata: {
        version: "1.0",
        generator: "VS Code 3D Exporter",
        created: new Date().toISOString(),
        description: options.description || ""
      },
      scene: {
        name: scene.name || "Untitled Scene",
        background: this.parseBackground(scene.background),
        nodes: []
      },
      assets: {
        geometries: {},
        materials: {},
        textures: {},
        animations: {}
      },
      aiMetadata: options.aiMetadata || {}
    };

    // Process scene graph
    scene.children.forEach(child => {
      vs3dData.scene.nodes.push(this.parseObject(child, vs3dData.assets));
    });

    return JSON.stringify(vs3dData, null, 2);
  }

  parseObject(object, assets) {
    const nodeData = {
      id: this.generateId(),
      name: object.name || object.type,
      type: this.getObjectType(object),
      transform: {
        position: object.position.toArray(),
        rotation: object.rotation.toArray(),
        scale: object.scale.toArray()
      },
      visible: object.visible
    };

    // Add AI context from userData
    if (object.userData.vs3d) {
      nodeData.description = object.userData.vs3d.description;
      nodeData.aiContext = object.userData.vs3d.aiContext;
    }

    // Handle specific object types
    if (object.isMesh) {
      nodeData.geometry = this.parseGeometry(object.geometry, assets.geometries);
      nodeData.material = this.parseMaterial(object.material, assets.materials);
      nodeData.castShadow = object.castShadow;
      nodeData.receiveShadow = object.receiveShadow;
    }

    // Handle children
    if (object.children.length > 0) {
      nodeData.children = object.children.map(child =>
        this.parseObject(child, assets)
      );
    }

    return nodeData;
  }

  parseGeometry(geometry, geometries) {
    const id = this.generateId();

    const geoData = {
      type: "BufferGeometry",
      description: `Geometry with ${geometry.attributes.position.count} vertices`,
      attributes: {}
    };

    // Export attributes
    Object.entries(geometry.attributes).forEach(([name, attribute]) => {
      geoData.attributes[name] = {
        type: attribute.array.constructor.name,
        itemSize: attribute.itemSize,
        data: Array.from(attribute.array),
        count: attribute.count
      };
    });

    // Export index
    if (geometry.index) {
      geoData.index = Array.from(geometry.index.array);
    }

    // Export bounding box
    if (geometry.boundingBox) {
      geoData.boundingBox = {
        min: geometry.boundingBox.min.toArray(),
        max: geometry.boundingBox.max.toArray()
      };
    }

    geometries[id] = geoData;
    return id;
  }

  generateId() {
    return 'vs3d_' + Math.random().toString(36).substr(2, 9);
  }
}

export { VS3DExporter };
```

## Phase 4: AI Integration Layer

### AI Scene Manipulator
```javascript
class AISceneManipulator {
  constructor(scene) {
    this.scene = scene;
    this.commandHistory = [];
  }

  async processCommand(naturalLanguage) {
    const intent = await this.parseIntent(naturalLanguage);
    const result = await this.executeIntent(intent);

    this.commandHistory.push({
      command: naturalLanguage,
      intent: intent,
      timestamp: new Date().toISOString(),
      result: result
    });

    return result;
  }

  async parseIntent(command) {
    // This would integrate with AI service
    // For now, simple pattern matching
    const patterns = {
      add: /add|create|place/i,
      move: /move|position|relocate/i,
      delete: /remove|delete|eliminate/i,
      modify: /change|modify|update|make/i,
      color: /color|paint|tint/i,
      scale: /scale|resize|size/i
    };

    const intent = { action: 'unknown', params: {} };

    for (const [action, pattern] of Object.entries(patterns)) {
      if (pattern.test(command)) {
        intent.action = action;
        break;
      }
    }

    // Extract object references
    intent.params.objects = this.extractObjectReferences(command);
    intent.params.values = this.extractValues(command);

    return intent;
  }

  async executeIntent(intent) {
    switch (intent.action) {
      case 'add':
        return this.addObject(intent.params);
      case 'move':
        return this.moveObjects(intent.params);
      case 'color':
        return this.changeColor(intent.params);
      case 'scale':
        return this.scaleObjects(intent.params);
      default:
        throw new Error(`Unknown action: ${intent.action}`);
    }
  }

  addObject(params) {
    // Create object based on description
    const geometry = new THREE.BoxGeometry(1, 1, 1);
    const material = new THREE.MeshStandardMaterial({ color: 0x00ff00 });
    const mesh = new THREE.Mesh(geometry, material);

    mesh.userData.vs3d = {
      id: this.generateId(),
      description: params.description || 'AI-generated object',
      aiContext: {
        createdBy: 'AI',
        intent: params.intent || 'user request'
      }
    };

    this.scene.add(mesh);
    return mesh;
  }

  findObjectsByDescription(description) {
    const results = [];
    this.scene.traverse(object => {
      if (object.userData.vs3d) {
        const context = object.userData.vs3d;
        if (context.description && context.description.toLowerCase().includes(description.toLowerCase())) {
          results.push(object);
        }
      }
    });
    return results;
  }
}
```

## Phase 5: VS Code Integration

### Extension Entry Point
```typescript
// extension.ts
import * as vscode from 'vscode';
import { VS3DViewProvider } from './VS3DViewProvider';
import { AICommandProcessor } from './AICommandProcessor';

export function activate(context: vscode.ExtensionContext) {
  // Register 3D view provider
  const provider = new VS3DViewProvider(context.extensionUri);
  context.subscriptions.push(
    vscode.window.registerWebviewViewProvider(VS3DViewProvider.viewType, provider)
  );

  // Register commands
  context.subscriptions.push(
    vscode.commands.registerCommand('vs3d.newScene', () => {
      provider.createNewScene();
    })
  );

  context.subscriptions.push(
    vscode.commands.registerCommand('vs3d.aiCommand', async () => {
      const command = await vscode.window.showInputBox({
        prompt: 'What would you like to do in 3D?',
        placeHolder: 'e.g., "add a red cube in the center"'
      });

      if (command) {
        provider.processAICommand(command);
      }
    })
  );

  // Register file association
  context.subscriptions.push(
    vscode.workspace.onDidOpenTextDocument(doc => {
      if (doc.fileName.endsWith('.vs3d')) {
        provider.openFile(doc.fileName);
      }
    })
  );
}
```

## Implementation Timeline

| Week | Focus | Deliverables |
|------|-------|-------------|
| 1 | Core Format | JSON schema, basic loader |
| 2 | Three.js Integration | Complete loader/exporter |
| 3 | AI Layer | Natural language processing |
| 4 | VS Code Extension | Working extension |
| 5 | Polish & Testing | Bug fixes, documentation |
| 6 | Release | VS Code Marketplace |

## Let's Start Building! 🔨

Ready to create the future of 3D development? Which part should we tackle first - the core format specification or jump straight into the three.js loader?
