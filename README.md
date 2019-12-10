# threebox

A three.js plugin for Mapbox GL JS, using the custom layer feature. Provides convenient methods to manage objects in lnglat coordinates, and to synchronize the map and scene cameras.


## File

[Click here to view the document](https://github.com/peterqliu/threebox)

## optimization

More strict handwriting mode is adopted to solve the problem of compiling luixus

## It might help you

 ```
import mapboxgl from 'mapbox-gl'
import * as THREE from 'three'
import {GLTFLoader} from 'three/examples/jsm/loaders/GLTFLoader';
import {DRACOLoader} from 'three/examples/jsm/loaders/DRACOLoader';
import {Threebox} from 'threebox-map';

/*Load gltfdraco model*/
let data = {
    id: "",
    url: '',
    origin: []
};
let _this = this;
let gltfLoader = new GLTFLoader();
let dracoLoader = new DRACOLoader();
let tb, cube;
let layer = {
    id: data.id,
    type: 'custom',
    onAdd: function (map, mbxContext) {
    tb = new Threebox(
        map,
        mbxContext,
        {defaultLights: true}
    );
    let pointLight = new THREE.PointLight('red', 1000, 5000);
    tb.add(pointLight);
    dracoLoader.setDecoderPath('static/draco/');
    gltfLoader.setDRACOLoader(dracoLoader);
    gltfLoader.load(data.url, (gltf => {
            cube = tb.Object3D({obj: gltf.scene});
            cube.set({rotation: {x: 90, y: -180, z: 0}});
            cube.setCoords(data.origin);
            tb.add(cube);
            }));
        },
    render: function (gl, matrix) {
            tb.update();
            }
    };
_this.map.on('style.load', function () {
    _this.map.addLayer(layer, 'country-label');
});
```


