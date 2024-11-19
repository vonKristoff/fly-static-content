Scenes are stored as `.gltf` files. You can easily export this file format with Blender. Check the [GLTFLoader documentation](https://threejs.org/docs/#examples/en/loaders/GLTFLoader) of Threejs for more information.

The `.gltf` files are stored in the folder `/static/`, but also need to be referenced in the json file `/src/lib/three_scene_definition.json`, so that the frontend can access them. This file is structured as an array of objects that contains...
- the `filename` which should equal the path within the `/static/` directory. 
- the `position` is where the scene should be placed. This may be important for large scenes if the origin is different then 0, 0, 0 or just for fine tuning the cameras' position
- the `key` phrase, which is simply the string that triggers the transition to this scene. As of right now, the response of the server is used as a key. So e.g. "You are standing on a windswept hill...". In the Nakama implementation, this can be changed in the file `nakama.ts` in the `socket.onnotification` event handler.
