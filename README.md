# S3D

The S3D file format, is a simple alternative to FBX, Collada (DAE), USD, and glTF formats for storing and transmitting 3D scene graphs and dependent assets. The S3D data layout is the [BFAST](https://github.com/ara3d/bfast) data container format. 

Unlike other formats S3D is agnostic about how tools choose to encode specific assets like the geometry, skin weights, or materials. It is a container for the scene graph that make it easy to efficiently get and set transform data and parent-child relationships. By not specifying these things S3D is less prone to change, easier to implement, and far more flexible. 

To get the best performance and flexibility from your 3D scene data  we recommend using the [G3D format](https://github.com/ara3d/g3d) for encoding individual geometry assets. 

## Specification 

The S3D file format is a [BFAST](https://github.com/ara3d/bfast) file consisting of N data-buffers. The first 3 data-buffers have specific interpretation:

1. JSON manifest - Contains meta-information about the file and the assets encoded using UTF-8.
2. Binary scene graph - A BFAST container of binary data describing the scene graph. 
    1. Transforms - An array of matrices representing global transforms encoded as 4x4 single or double precision floating point vaues   
    2. Geometry indexes - An array of indices indicating which geometric asset is associated 
    3. Material indexes - An index indicating which material is associated with this node
    4. Parent indices - An array of indices indicating the parent node
3. Object Metadata - UTF-8 encoded JSON properties 
4. Material assets - A BFAST container of material descriptors. Encoding of the material descriptors is not defined by S3D.
5. Geometric assets - A BFAST container of geometric descriptors. 
6. Texture assets - A BFAST container containing texture assets. Encoding of the texture assets is not defined by S3D. 
7. Additional assets - A BFAST container of additional data files associated with the scene. 

## Why not OBJ?

OBJ is inefficient to read and write, is opinionated, and does not enable encoding of instancing of geometry. 

## Why not Collada (DAE)?

Collada is a very complex and bloated file format that is inefficient to read and write.  

## Why not glTF?

The [glTF is a complex and opinionated format](https://raw.githubusercontent.com/KhronosGroup/glTF/master/specification/2.0/figures/gltfOverview-2.0.0a.png) that dictates how different assets (e.g. meshes, morph targets, materials, etc.) are stored. 

Writing a compliant importer, exporter, or validator for glTF is a massive undertaking. For example, see  
* JavaScript source code for the [THREE.JS importer and exporter](https://github.com/mrdoob/three.js/blob/master/examples/js/loaders/GLTFLoader.js) 
* [Official glTF Validator tool](https://github.com/KhronosGroup/glTF-Validator/tree/master/lib/src)

## Why not FBX?

Like glTF FBX is also a complex and opinionated format, but additionally is closed making it unusable for web applications. 

## Why not USD?

The USD file format does not have a specification. You have to use the USD library or recreate its reading and writing. There      
