# `@pixiv/three-vrm`

[
![@pixiv/three-vrm on npm](https://img.shields.io/npm/v/@pixiv/three-vrm)
](https://www.npmjs.com/package/@pixiv/three-vrm)

[VRM](https://vrm.dev/) 3Dアバターフォーマット用の [three.js](https://threejs.org/) ローダーです。

![A 3D anime-style character model rendered in a web browser, demonstrating the three-vrm library.](https://github.com/pixiv/three-vrm/raw/dev/three-vrm.png)

-   **[サンプル](https://pixiv.github.io/three-vrm/packages/three-vrm/examples)**
-   **[ドキュメント](https://github.com/pixiv/three-vrm/tree/dev/docs/README.md)**
-   **[API リファレンス](https://pixiv.github.io/three-vrm/packages/three-vrm/docs)**

## v1.0 リリースのお知らせ

**`@pixiv/three-vrm` バージョン 1.0 がリリースされました！**

このバージョンでは、VRM 0.0 との下位互換性を維持しつつ、新しい [VRM 1.0](https://vrm.dev/vrm1/) 仕様のサポートを追加しています。また、モダンな `GLTFLoader` プラグインシステムを採用しています。

これは多くの破壊的変更を含むメジャーアップデートです。コードの移行に関する詳細は、**[v1.0 移行ガイド](https://github.com/pixiv/three-vrm/blob/dev/docs/migration-guide-1.0.md)** を参照してください。

## 機能

-   **VRM 1.0 および 0.0 サポート:** 1つのライブラリで両方の仕様のモデルを読み込むことができます。
-   **GLTFLoader プラグイン:** 標準的な three.js のモデル読み込みワークフローとシームレスに統合できます。
-   **Humanoid ボーンマッピング:** モデルの標準化された Humanoid リグにアクセスし、操作することができます。
-   **Expression（ブレンドシェイプ）:** 表情やその他のモーフターゲットを管理します。
-   **First-Person View（一人称視点）:** 一人称視点用にカメラとヘッドカリングを自動的に設定します。
-   **Look-At 制御:** モデルの視線方向を手続き的に制御します。
-   **物理演算（Spring Bone）:** 組み込みの二次アニメーションシステムにより、髪、アクセサリー、衣服などをアニメーションさせます。
-   **MToon マテリアル:** VRM 標準のアニメ調シェーダーを完全サポート（VRM 0.0 モデル用の互換性レイヤーを含みます）。
-   **Node Constraints（ノードコンストレイント）:** 高度なリギングのための Aim、Roll、Position コンストレイントをサポートします。

## 使い方

### npm を使用する場合

まず、必要なパッケージをインストールします。

```sh
npm install three @pixiv/three-vrm
```

次に、`VRMLoaderPlugin` を `GLTFLoader` のプラグインとして使用します。

```javascript
import * as THREE from 'three';
import { GLTFLoader } from 'three/examples/jsm/loaders/GLTFLoader.js';
import { VRMLoaderPlugin } from '@pixiv/three-vrm';

const scene = new THREE.Scene();

const loader = new GLTFLoader();

// GLTFLoader プラグインをインストール
loader.register((parser) => {
  return new VRMLoaderPlugin(parser);
});

loader.load(
  // VRM モデルの URL
  './model.vrm',

  // onLoad コールバック
  (gltf) => {
    // gltf から VRM インスタンスを取得
    const vrm = gltf.userData.vrm;

    // 読み込んだ vrm をシーンに追加
    scene.add(vrm.scene);

    // vrm の機能を処理
    console.log(vrm);
  },

  // onProgress コールバック
  (progress) => console.log('Loading model...', 100.0 * (progress.loaded / progress.total), '%'),

  // onError コールバック
  (error) => console.error(error),
);
```

### HTML で使用する場合

CDN を経由して、HTML ファイル内で直接ライブラリを使用することもできます。この方法では、モジュールの管理に [import maps](https://threejs.org/docs/#manual/en/introduction/Installation) を使用します。

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

  // GLTFLoader プラグインをインストール
  loader.register((parser) => {
    return new VRMLoaderPlugin(parser);
  });

  loader.load(
    // VRM モデルの URL
    './model.vrm',

    // onLoad コールバック
    (gltf) => {
      const vrm = gltf.userData.vrm;
      scene.add(vrm.scene);
      console.log(vrm);
    },

    // onProgress コールバック
    (progress) => console.log('Loading model...', 100.0 * (progress.loaded / progress.total), '%'),

    // onError コールバック
    (error) => console.error(error),
  );
</script>
```

## パッケージ

このプロジェクトは、以下によって管理されるモノレポです
