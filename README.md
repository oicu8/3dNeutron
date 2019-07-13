## Neutron 3D

Live demo: [Neutron 3D](https://oicu8.github.io/3dneutron/index.html)

Neutron 3D Blockchain Visualizer using Three.js, Cannon.js and Insight API.

### Build

Clone the repo:

```
git clone https://github.com/oicu8/3dneutron
```

Install npm packages:
```
cd 3dneutron && npm install
```

Build:
```
npm run build
```

To run the visualizer after build, copy the whole folder to a local or remote webserver and open index.html from a browser.

Keys: you can press 'q' to manually cycle through quality settings.

### Goal

Due to the lack of open-source 3D blockchain visualizers I decided to roll one for Dash.. inspired by BitBonkers & BitListen
And I (oicu8) Decided to Fork it, see if we can make it better.
### Overview

When the page loads, the 3d Neutron object in  [/src/3dneutron.js] is created which runs the visualization in an HTML5 canvas passed into the constructor.

A benchmark is run for 3 seconds behind a loading screen at start to determine the device FPS (and later tune the graphics and physics settings appropriately), preload all the assets and get the best block and its tx from Insight.

After preload the best block and tx are added to the scene as a starting point and then we listen for new unconfirmed tx and confirmed blocks from a websocket hooked into Insight using socket.io, defaulted to https://insight.is/.

When a new block hash is seen we re-query Insight to get the block height and a cube is added with the height written on each face and to the html HUD including a timer that just increments since the last block was received.

When new unconfirmed tx are seen we add a sphere for each output, with the size roughly calculated using the log of the spherical volume with r = txo-amount, and different textures for amounts falling between the limits <1, <10, <100, <1000.  

During visualization if no network activity is detected within any (default) 30 second period, Insight is pinged for the best block hash to check if it's still alive - if not, the loader screen is shown again until a connection is re-established.
# 3dNeutron
