## Tutorials and Overviews
100s Svelte overview:
- [Svelte](https://youtu.be/rv3Yq-B8qp4?feature=shared)
- [SvelteKit](https://youtu.be/H1eEFfAkIik?feature=shared)
Svelte interactive tutorial:
- https://learn.svelte.dev/tutorial/welcome-to-svelte

## Technology Overview 
JS Framework:
- [Svelte Kit](https://kit.svelte.dev/)
CSS Framework:
- [TailwindCSS](https://tailwindcss.com/)
Other Libraries:
- [Three.js](https://threejs.org/)
- [Nakama JS Client](https://heroiclabs.com/docs/nakama/client-libraries/javascript/)
Bundler:
- [Vite](https://vitejs.dev/)

## Recommended VS Code Extensions
- [Prettier](https://marketplace.visualstudio.com/items?itemName=esbenp.prettier-vscode)
- [Tailwind CSS IntelliSense](https://marketplace.visualstudio.com/items?itemName=bradlc.vscode-tailwindcss)
- [Svelte for VS Code](https://marketplace.visualstudio.com/items?itemName=svelte.svelte-vscode)
- [Tailwind Fold](https://marketplace.visualstudio.com/items?itemName=stivo.tailwind-fold) (Optional if long class names are annoying)
- [Pretty TypeScript Errors](https://marketplace.visualstudio.com/items?itemName=yoavbls.pretty-ts-errors) (Optional, but makes reading error messages easier)
- [vscode-icons](https://marketplace.visualstudio.com/items?itemName=vscode-icons-team.vscode-icons) (Optional, adds more expressive icons to files and folders)

## Developing
Once you've created a project and installed dependencies with `npm install` (or `pnpm install` or `yarn`), start a development server:
```bash
npm run dev
# or start the server and open the app in a new browser tab
npm run dev -- --open
```

## Project Structure
### Main Page
The definition of the main site is in `/src/routes/+page.svelte`. This file is named based on the [routing system of Svelte](https://kit.svelte.dev/docs/routing). In the file, you'll find a `<script>` area that includes some imports as well as important setup functions that should run as soon as Svelte is mounted on the client: `setupThree()` which sets the Three.js system up and renders to the div with the `id=viewport`, as well as `authenticateUser()` which authenticates the user with Nakama. These two are handled within the files [three.ts](###ThreeJS) and [nakama.ts](###Nakama) respectively. Furthermore, the file also imports two components [Wallet](###Wallet) and [Terminal](###Terminal), which are in a container that controls their size and position to centre them in the middle of the screen.
### Wallet
Just a box with a yellow border and a string for now, will be used for more stuff in the future.
### Terminal
This is the terminal that handles user input and displays the terminal output. The terminal consists of a `<form>` with three event handlers:
- On `click` or on `keydown` within the form the terminal input will be focused.
- On `submit` the terminal input gets disabled and the `sendCommand` function in `terminal.ts` is called. This function is designed as a [Promise](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise) to hook different backends at this point.
In the `<input>` tag, there is one input handler for the `keydown` event, which handles command history.
There are further functionalities like smooth scrolling to the most recent message or typing "clear" in the input field to clear the terminal.
### ThreeJS
This file handles the three.js rendering of the project. It uses [OrbitControls](https://threejs.org/docs/index.html?q=orbi#examples/en/controls/OrbitControls) to allow panning the camera and the [ASCII effect](https://threejs.org/examples/?q=asci#webgl_effects_ascii) to render the 3D scene using ASCII symbols. The symbols can be modified in the `AsciiEffect` constructor and the `color` and `backgroundColor` can be set later via `effect.domElement.style.`.
The scene is illuminated by a soft ambient light and a directional sun light.
All scenes are asynchronously preloaded on mount to reduce loading times later. This works when there are only a few scenes to load, but needs to be refined later to only load relevant scenes to avoid lag and RAM waste.
### Nakama
Can be replaced by another library by hooking another function into `terminal.ts` and calling `updateScene()` somewhere else, see [Command Processing](####Command%20Processing).
#### Authentication
Nakama's users are currently authenticated with their device key. This key is automatically randomly generated using the UUID math helper of three.js and is then stored in localStorage for future reuse. After the user is authenticated, the client sends a transaction to claim the persona, then using a socket rpc transaction, the player is created.
#### Command Processing
Once authenticated, commands can be processed. `processCommand` is called from `terminal.ts` see [Terminal](###Terminal). It takes an input command and the resolve function of a [Promise](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise). The function then sends a socket rpc transaction to the server and awaits the response, which returns an  object that contains a hash string. This hash is saved into a object/map/record (whatever name makes sense for you, it's a key-value object) consisting of a string key (the hash) and a value (the resolve method that was passed to `processCommand` as a parameter).

Using `socket.onnotification`, we now await a response from the server that matches any hash we saved into the object/map/record. If we find a match, we update the scene, resolve the promise using the response string we got from the server and delete the entry from the object/map/record.