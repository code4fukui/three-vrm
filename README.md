# `@pixiv/three-vrm`

> 日本語のREADMEはこちらです: [README.ja.md](README.ja.md)

[
![@pixiv/three-vrm on npm](https://img.shields.io/npm/v/@pixiv/three-vrm)
](https://www.npmjs.com/package/@pixiv/three-vrm)

A [three.js](https://threejs.org/) loader for the [VRM](https://vrm.dev/) 3D avatar format.


![A 3D anime-style character model rendered in a web browser, demonstrating the three-vrm library.](https://github.com/pixiv/three-vrm/raw/dev/three-vrm.png)


-   **[Examples](https://pixiv.github.io/three-vrm/packages/three-vrm/examples)**
-   **[Documentation](https://github.com/pixiv/three-vrm/tree/dev/docs/README.md)**
-   **[API Reference](https://pixiv.github.io/three-vrm/packages/three-vrm/docs)**

## Announcing v1.0

**`@pixiv/three-vrm` version 1.0 has been released!**

This version adds support for the new [VRM 1.0](https://vrm.dev/vrm1/) specification while maintaining backward compatibility with VRM 0.0. It also adopts the modern `GLTFLoader` plugin system.

This is a major update with many breaking changes. Please see the **[v1.0 Migration Guide](https://github.com/pixiv/three-vrm/blob/dev/docs/migration-guide-1.0.md)** for details on updating your code.

## Features

-   **VRM 1.0 and 0.0 Support:** Load models of both specifications with a single library.
-   **GLTFLoader Plugin:** Integrates seamlessly with the standard three.js model loading workflow.
-   **Humanoid Bone Mapping:** Access and manipulate the model's standardized humanoid rig.
-   **Expression (Blend Shapes):** Manage facial expressions and other morph targets.
-   **First-Person View:** Automatically configure the camera and head-culling for a first-person perspective.
-   **Look-At Control:** Procedurally control the model's eye direction.
-   **Physics (Spring Bone):** Animate hair, accessories, and clothing with a built-in secondary animation system.
-   **MToon Material:** Full support for the VRM standard anime-style shader, including a compatibility layer for VRM 0.0 models.
-   **Node Constraints:** Supports Aim, Roll, and Position constraints for advanced rigging.

## Usage

### via npm

First, install the necessary packages:

```sh
npm install three @pixiv/three-vrm
```

Then, use `VRMLoaderPlugin` as a plugin for `GLTFLoader`:

```javascript
import * as THREE from 'three';
import { GLTFLoader } from 'three/examples/jsm/loaders/GLTFLoader.js';
import { VRMLoaderPlugin } from '@pixiv/three-vrm';

const scene = new THREE.Scene();

const loader = new GLTFLoader();

// Install GLTFLoader plugin
loader.register((parser) => {
  return new VRMLoaderPlugin(parser);
});

loader.load(
  // URL of the VRM model
  './model.vrm',

  // onLoad callback
  (gltf) => {
    // retrieve a VRM instance from gltf
    const vrm = gltf.userData.vrm;

    // add the loaded vrm to the scene
    scene.add(vrm.scene);

    // deal with vrm features
    console.log(vrm);
  },

  // onProgress callback
  (progress) => console.log('Loading model...', 100.0 * (progress.loaded / progress.total), '%'),

  // onError callback
  (error) => console.error(error),
);
```

### via HTML

You can also use the library directly in an HTML file via a CDN. This approach uses [import maps](https://threejs.org/docs/#manual/en/introduction/Installation) to manage modules.

```html
<!-- Import maps polyfill -->
<script async src="https://unpkg.com/es-module-shims@1.3.6/dist/es-module-shims.js"></script>

<script type="importmap">
  {
    "imports": {
      "three": "https://unpkg.com/three@0.148.0/build/three.module.js",
      "three/addons/": "https://unpkg.com/three@0.148.0/examples/jsm/",
      "@pixiv/three-vrm": "https://unpkg.com/@pixiv/three-vrm@1.0.8/lib/three-vrm.module.js"
    }
  }
</script>

<script type="module">
  import * as THREE from 'three';
  import { GLTFLoader } from 'three/addons/loaders/GLTFLoader.js';
  import { VRMLoaderPlugin } from '@pixiv/three-vrm';

  const scene = new THREE.Scene();

  const loader = new GLTFLoader();

  // Install GLTFLoader plugin
  loader.register((parser) => {
    return new VRMLoaderPlugin(parser);
  });

  loader.load(
    // URL of the VRM model
    './model.vrm',

    // onLoad callback
    (gltf) => {
      const vrm = gltf.userData.vrm;
      scene.add(vrm.scene);
      console.log(vrm);
    },

    // onProgress callback
    (progress) => console.log('Loading model...', 100.0 * (progress.loaded / progress.total), '%'),

    // onError callback
    (error) => console.error(error),
  );
</script>
```

## Packages

This project is a monorepo managed by