# VS Code 3D - Visual Studio Code Fork With Built-In 3D Modeling Capabilities

[![3D Features](https://img.shields.io/badge/3D%20Modeling-Enabled-brightgreen.svg)](#3d-modeling-features)
[![Three.js](https://img.shields.io/badge/Three.js-Integrated-blue.svg)](https://threejs.org/)
[![Based on VS Code](https://img.shields.io/badge/Based%20on-VS%20Code-007ACC.svg)](https://github.com/microsoft/vscode)
[![MIT License](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE.txt)

## The Repository

This repository contains **VS Code 3D**, a powerful fork of Visual Studio Code that integrates native 3D modeling capabilities directly into the editor. Built on the solid foundation of Microsoft's VS Code, this enhanced version adds comprehensive 3D development tools powered by [three.js](https://threejs.org/), creating the ultimate environment for developers working with 3D graphics, WebGL, game development, and interactive visualizations.

## VS Code 3D - Code Editor Meets 3D Modeling

<p align="center">
  <img alt="VS Code 3D in action with 3D modeling interface" src="https://user-images.githubusercontent.com/35271042/118224532-3842c400-b438-11eb-923d-a5f66fa6785a.png">
  <br>
  <em>Traditional code editing enhanced with integrated 3D modeling capabilities</em>
</p>

**VS Code 3D** combines the familiar, powerful code editing experience of Visual Studio Code with native 3D modeling tools. Whether you're developing WebGL applications, creating interactive 3D websites, building games, or working on architectural visualizations, VS Code 3D provides an integrated workflow that eliminates the need to switch between multiple applications.

### Key Features

- **🎨 Integrated 3D Viewport**: Native 3D scene editor built directly into VS Code
- **⚡ Three.js Integration**: Full three.js library integration with IntelliSense and debugging
- **🔧 3D Modeling Tools**: Basic to advanced 3D modeling capabilities
- **🎯 Live Preview**: Real-time 3D scene updates as you code
- **📐 Geometry Editor**: Visual mesh editing and manipulation tools
- **🎭 Material Editor**: Advanced material and texture editing interface
- **🚀 WebGL Development**: Optimized workflow for WebGL and WebXR projects
- **📱 Cross-Platform**: Available for Windows, macOS, and Linux

## 3D Modeling Features

### Built-in 3D Editor
- **Scene Graph Management**: Visual hierarchy editor for 3D scenes
- **Mesh Editing**: Vertex, edge, and face manipulation tools
- **Primitive Creation**: Quick generation of basic 3D shapes
- **Import/Export**: Support for popular 3D formats (glTF, OBJ, FBX, DAE)
- **Animation Timeline**: Keyframe animation editor and timeline

### Three.js Integration
- **Enhanced IntelliSense**: Full code completion for three.js API
- **Live Debugging**: Debug 3D scenes with breakpoints and inspection
- **Performance Profiling**: Built-in WebGL performance analysis tools
- **Shader Editor**: GLSL syntax highlighting and live shader compilation
- **Asset Management**: Integrated texture and model asset browser

### Development Workflow
- **Hot Reload**: Instant 3D scene updates during development
- **Split View**: Code and 3D viewport side-by-side
- **Version Control**: Full Git integration for 3D assets and scenes
- **Extensions**: Compatible with existing VS Code extensions
- **Templates**: 3D project templates and snippets

## Getting Started

### Installation

Download VS Code 3D for your platform:
- **Windows**: [Download for Windows](releases/latest)
- **macOS**: [Download for macOS](releases/latest)
- **Linux**: [Download for Linux](releases/latest)

### Quick Start

1. **Create a 3D Project**: Use `Ctrl+Shift+P` and run "Create 3D Project"
2. **Open 3D Viewport**: Press `Ctrl+Shift+3` to toggle the 3D editor
3. **Add 3D Objects**: Use the built-in primitive tools or import models
4. **Live Coding**: Edit three.js code and see changes in real-time

### System Requirements

- **Minimum**: 4GB RAM, DirectX 11 / OpenGL 3.3 compatible graphics
- **Recommended**: 8GB RAM, Dedicated GPU with 2GB VRAM
- **For Development**: 16GB RAM, Modern GPU with 4GB+ VRAM

## Contributing

We welcome contributions to VS Code 3D! There are many ways to participate:

### Code Contributions
* **3D Features**: Help improve the 3D modeling tools and viewport
* **Three.js Integration**: Enhance the three.js development experience
* **Performance**: Optimize 3D rendering and editor performance
* **Platform Support**: Improve cross-platform compatibility

### Community Contributions
* **Submit 3D-related bugs and feature requests**
* **Review pull requests for 3D functionality**
* **Create 3D project templates and examples**
* **Contribute to documentation and tutorials**
* **Share 3D models and scenes with the community**

### Development Setup

To contribute to VS Code 3D development:

1. **Prerequisites**: Node.js 16+, Python 3.x, C++ build tools
2. **Clone Repository**: `git clone https://github.com/yourusername/vscode-3d.git`
3. **Install Dependencies**: `npm install`
4. **Build 3D Components**: `npm run build:3d`
5. **Run Development**: `npm start`

See our [Contributing Guide](CONTRIBUTING.md) for detailed information on:
- Setting up the development environment
- Building and testing 3D features
- Code style guidelines for 3D components
- Submitting pull requests for 3D functionality

## Feedback and Support

### Community Support
* **Ask Questions**: [Stack Overflow](https://stackoverflow.com/questions/tagged/vscode-3d)
* **Feature Requests**: [GitHub Issues](issues/new?template=feature_request.md)
* **Bug Reports**: [GitHub Issues](issues/new?template=bug_report.md)
* **3D Showcase**: Share your creations in [Discussions](discussions)
* **Discord Community**: [Join our 3D development Discord](https://discord.gg/vscode3d)

### Learning Resources
* **3D Development Tutorials**: [Getting Started with 3D in VS Code](docs/tutorials/)
* **Three.js Integration Guide**: [Three.js Best Practices](docs/threejs-guide.md)
* **Example Projects**: [3D Project Gallery](examples/)
* **Video Tutorials**: [YouTube Channel](https://youtube.com/c/vscode3d)

## Related Projects and Technologies

### Core Technologies
- **[three.js](https://threejs.org/)**: The foundation of our 3D capabilities
- **[WebGL](https://www.khronos.org/webgl/)**: Hardware-accelerated graphics
- **[WebXR](https://www.w3.org/TR/webxr/)**: Virtual and Augmented Reality support
- **[Babylon.js](https://www.babylonjs.com/)**: Alternative 3D engine support (planned)

### Integration Partners
- **[Blender](https://www.blender.org/)**: Import/export pipeline with Blender
- **[Unity](https://unity.com/)**: Asset sharing between Unity and VS Code 3D
- **[Unreal Engine](https://www.unrealengine.com/)**: Compatible asset formats
- **[Sketchfab](https://sketchfab.com/)**: Direct model import from Sketchfab

### Community Extensions
- **3D Model Viewers**: Enhanced preview capabilities
- **Shader Editors**: Advanced GLSL development tools
- **Physics Engines**: Integration with physics simulations
- **VR/AR Tools**: Extended reality development support

## Bundled 3D Extensions

VS Code 3D includes specialized extensions for 3D development:

### Core 3D Extensions
- **`3d-viewer`**: Native 3D model viewer and editor
- **`threejs-language-features`**: Enhanced three.js development support
- **`webgl-debugger`**: WebGL debugging and profiling tools
- **`shader-editor`**: GLSL shader development environment
- **`3d-assets-manager`**: Asset organization and management

### Format Support
- **`gltf-tools`**: glTF 2.0 editing and validation
- **`obj-support`**: Wavefront OBJ file handling
- **`fbx-importer`**: Autodesk FBX file support
- **`collada-dae`**: COLLADA DAE file processing

## Development Container

This repository includes enhanced development container support for 3D development:

### Container Features
- **GPU Acceleration**: Hardware-accelerated graphics in containers
- **3D Libraries**: Pre-installed three.js, WebGL tools, and dependencies
- **Development Tools**: Complete 3D development toolchain
- **Performance Optimized**: Configured for 3D development workflows

### Requirements
- **Docker**: Latest version with GPU support
- **Hardware**: Dedicated GPU recommended for 3D development
- **Memory**: 8GB RAM minimum, 16GB recommended
- **Storage**: 50GB available space for 3D assets and builds

For detailed setup instructions, see the [development container guide](.devcontainer/README.md).

## Architecture and Technical Details

### 3D Engine Architecture
VS Code 3D integrates three.js through a custom extension architecture that provides:

- **Native Performance**: Direct GPU access through Electron's renderer process
- **Memory Management**: Optimized memory usage for large 3D scenes
- **Concurrent Processing**: Background processing for 3D operations
- **Plugin System**: Extensible 3D tool and format support

### File Format Support
- **Import**: glTF, OBJ, FBX, DAE, STL, PLY, 3MF
- **Export**: glTF, OBJ, STL (additional formats via plugins)
- **Textures**: PNG, JPG, TIFF, EXR, HDR
- **Materials**: PBR workflows, custom shader materials

## License and Attribution

**VS Code 3D** is based on Visual Studio Code and maintains compatibility with the original MIT license.

- **Original VS Code**: Copyright (c) Microsoft Corporation
- **3D Extensions**: Copyright (c) VS Code 3D Contributors
- **three.js Integration**: Uses three.js under MIT license
- **Additional Libraries**: See [THIRD-PARTY-LICENSES.md](THIRD-PARTY-LICENSES.md)

Licensed under the [MIT License](LICENSE.txt).

---

## Quick Links

- 🚀 [Download Latest Release](releases/latest)
- 📚 [Documentation](docs/)
- 🎮 [3D Examples](examples/)
- 💬 [Community Discord](https://discord.gg/vscode3d)
- 🐛 [Report Issues](issues/)
- 🎯 [Feature Requests](issues/new?template=feature_request.md)

**Transform your coding experience with integrated 3D capabilities. Welcome to the future of 3D development!**
