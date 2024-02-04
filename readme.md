# Debug UI

## Debug Libraries

- dat.GUI
- lil-gui
- control-panel
- ControlKit
- Uil
- Tweakpane
- Guify
- Oui

## Setup: lil-gui

1. Install lil-gui

    ```bash
    npm install lil-gui
    ```

2. Import and Declare in Javascript

    ```javascript

    import GUI from 'lil-gui'

        /**
     * Debug: Top of the file
    */
    const gui = new GUI()

    ```

## Tweaks

### Ranges on Mesh

```javascript

//Most of the tweaks can be done using 

gui.add(object, object property, min value, max value, steps) 

//or 
gui
    .add(object, property)
    .min(min value)
    .max(max value)
    .step(steps)
    .name("tag name")

//What if you want to modify a variable? Well you can create a variables object and pass it into gui.add

const myGuiVariables = {
    variable1: 200;
}

gui.add(myGuiVariables, variable1)
```

### Checkboxes

```javascript

//Add Checkbox

gui.add(mesh, "visible")

gui.add(mesh.material, "wireframe")

```

### Color

```javascript

// The hex string in the debugger is not in hex format so you can get the color from the console like this

//Approach 1:

gui
    .addColor(material, "color")
    .onChange((value) => {
        console.log(value.getHexString())
    })

//Approach 2: Create Variable Object and use that color and pass it to three.js

const debugObject = {}

debugObject.color = '#3a6ea6'

const material = new THREE.MeshBasicMaterial({ color: debugObject.color })

gui
    .addColor(debugObject, "color")
    .onChange(() => {
        material.color.set(debugObject.color)
    })
//
```

### Function (Button)

Declare function inside debug object. Add it to gui.

```javascript
debugObject.spin = () => {
   gsap.to(mesh.rotation, { y: mesh.rotation.y + Math.PI * 2})
}

gui.add(debugObject, 'spin')
```

## Tweaking the Geometry

When the tweak value changes, we need to destroy previous geometry and generate new one.

```javascript

//  Add a value in debugObject and add an onFinishChange event handler that removes previous mesh and creates a new one

debugObject.subdivision = 2

gui
    .add(debugObject, 'subdivision')
    .min(1)
    .max(20)
    .step(1)
    .onFinishChange(() => {
        //Must destroy previous mesh to prevent memory leak
        mesh.geometry.dispose()
        //Create replacement
        mesh.geometry = new THREE.BoxGeometry(
            1, 1, 1, 
            debugObject.subdivision, debugObject.subdivision, debugObject.subdivision
            )
    })

```

## Create Folder and More

Step 1: Declare Folder

```javascript

const cubeTweaks = gui.addFolder("cool cube")

```

Step 2: Add to folder not gui directly!

Step 3 (optional): Use .close() method to have it closed default

Step 4 (optional): You can nest folders so you can even add a folder to cubeTweaks like we did to gui in step 1

Step 5 (optional): Pass Params to GUI

```javascript
const gui = new GUI({
    width: 300,
    title: 'Debug UI',
    closeFolders: false,

})
```

Step 6 (optional): Toggle debugger

```javascript
window.addEventListener('keydown', (e) => {
    if (event.key == 'h'){
        gui.show(gui._hidden)
    }
})
```
