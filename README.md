# Rive runtime for Pixi.js
This runtime allows you to use [Rive](https://rive.app/) animation inside [Pixi.js](https://pixijs.com/).

This is a unofficial fork of the npm package `pixi-rive`. You can access the original package at [npm](https://www.npmjs.com/package/pixi-rive).

## Installing

```
npm install pixi.js
npm install rive-pixi
```

## Usage Example

```tsx
import * as PIXI from 'pixi.js';
import { RiveSprite } from 'rive-pixi';

// Create Pixi.js Application
const app = new PIXI.Application({ resizeTo: window });

// Add Pixi.js canvas to the page
document.body.appendChild(app.view);

// Add demo Rive animation resource to Pixi.js Assets Loader
PIXI.Assets.add({ alias: 'vehicles', src: 'https://cdn.rive.app/animations/vehicles.riv' });

// Create PixiRive component (extended from Pixi.js Sprite)
const vehicles = new RiveSprite({
  asset: 'vehicles',
  interactive: true,
  autoPlay: true
});

// Add our animation to pixi scene
app.stage.addChild(vehicles);
```

## Animation Fit and Alignment

```tsx
import { Fit, Alignment } from 'rive-pixi';

// Set fit rule for Rive animation inside container
// Also available: Cover, Fill, FitWidth, FitHeight, ScaleDown
vehicles.fit = Fit.Contain;

// Set Rive animation alignment inside container
// Also available: TopLeft, TopCenter, TopRight, CenterLeft,
// CenterRight, BottomLeft, BottomCenter, BottomRight
vehicles.align = Alignment.Center;

// Set maximum width and height of animation container
vehicles.maxWidth = 500;
vehicles.maxHeight = 500;
```

## Extended Initialization

```tsx
const vehicles = new RiveSprite({

  // Instead of asset name you can pass already loaded *.riv file in Uint8Array type
  asset: new Uint8Array(),

  // You can disable interaction with animation
  // You can't turn on or turn off interaction after initialization
  interactive: false,

  // You can turn-off autoPlay and enable animation whenever you want
  // using vehicles.enable() and vehicles.disable() methods
  autoPlay: false,

  // You can set name of the loaded Artboard from Rive file
  // or you can use loadArtboard(name: string) method
  artboard: 'nameOfTheRiveArtboardToDisplay',

  // You can pass name of the loaded state machine or first state machine will be used
  // Also you can call loadStateMachine(name: string) later and unloadStateMachine(name: string)
  stateMachine: 'nameOfTheLoadedStateMachine',

  // You can set single or multiple animations
  // also you can use methods playAnimation(name: string) for play animation
  // and stopAnimation(name: string) for stop animation
  animation: ['animation1','animation2'],

  // onStateChange will be called every time when something were changed on scene
  // usually when new animation is runned
  // You can manually check if input fields was changed using parameter:
  // vehicles.inputFields: Map<string, SMIInput>
  // Each input field has such properties:
  // { name, type, value: (string | boolean) }
  onStateChange: (states: string[]) => {
    // states will contain list of new runned animations
  },

  // onReady method will be called only once when Rive animation was loaded and ready
  onReady: () => {

    // Manually enable playing of the loaded animations or state machines
    vehicles.enable();

    // Get array of artboard names available in rive file
    const artboards: string[] = vehicles.getAvailableArtboards();

    // Get array of state machine names available in rive file
    const stateMachines: string[] = vehicles.getAvailableStateMachines();

    // Get array of animation names available in rive file
    const animations: string[] = vehicles.getAvailableAnimations();

    // Get current statemachine input field value by name (string)
    const value: (string | boolean) = vehicles.getInputValue('name');

    // Set current state machine input value by name
    // Value can be string or boolean, dependent on it's basic type
    vehicles.setInputValue('name', 'value');

    // This will fire trigger field for current state machine
    vehicles.fireTrigger('someTriggerName');

    // And this will destroy all inner Rive components and resources,
    // stop all state machines and animations and remove events
    vehicles.destroy();

  }

});
```