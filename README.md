# unity-2d-rigging-shader

This is an example of one way to add an outline shader to a 2D animated rigged model.
Created using **Unity version 2021.3.27f1** on 14th June 2023.

**Demo video:**
https://github.com/albertferras/unity-2d-rigging-shader/assets/7689174/0f47da84-2861-42c5-b097-260ce68657f1

**Setup:**
![Picture](https://github.com/albertferras/unity-2d-rigging-shader/assets/7689174/5a9b03d9-5a05-4eb5-877e-4d0d21f2ec2e)

The rigged model is Fei, which is available in the Samples provided by the official 2D animation package.
There are two GameObjects added on screen:
1 - (WHITE FLOOR. name=Fei Without Shader) Gameobject with the rigged model objects (sprite&bones), on layer 'Default'.
2 - (RED FLOOR. name=Fei With Shader) Same as above, but this one is rendered on layer 'RenderLayerForCameraB'
The Fei appearing on the GREEN FLOOR is the 'Fei with shader' but with the outline shader applied.

## Setup:
Globally:
- Create one Layer 'RenderLayerForCameraB'
- Create a Target Sorting Layer (for sprites): SpriteLayerB. You can do this from any Light2D or GameObject with sprite

**Rigged Model** (on the RED floor in the example above):
- Set Layer=RenderLayerForCameraB on (+all its child too)
- Set SortingLayer=SpriteLayerB to all SpriteRenderers of your rigged model.
- (Optional) Add animator to later see the outline match the animation.

**Main Camera:**
- CullingMask: Everything except 'RenderLayerForCameraB'

**Camera B:**
- CullingMask: Only 'RenderLayerForCameraB'
- Output Texture: 'CameraBRenderTexture'
- Place the camera so that it can see your rigged model (RED FLOOR one in our case)

**GlobalLight for Camera B:**
- TargetSortingLayers: SpriteLayerB

**RenderTexture** (Create->Render Texture on the Project panel (assets folder))
- Name: CameraBRenderTexture
- Color format: R32G32B32A32_SFLOAT
- Size: 512x512

With this setup, the CameraB is going to draw our RiggedModel on our RenderTexture. It should have Alpha channel (transparency) already working.
If not, try configuring your CameraB: `Environment>BackgroundType: SolidColor` (any with alpha=0). Note that I also got it working with 'Unitialised' or 'Skybox'.

We will now draw this RenderTexture on our screen and apply a `Material(with a shader)` in it.
Disclaimer: I've only managed to add the rendertexture on a Canvas, so placing it properly on the screen is a bit more tricky since you need to check your camera position/viewport and where you're rendering your texture. This is out of scope for this example project.

Right click on your Scene and create a `UI->RawImage`. Select the RenderTexture we've created and move it to your desired location.
To make it easier to place, modify the `Canvas` and set `Render Mode` to `World Space`.
Now, you just need to apply a `Material` with your favorite shader. In this project we added a Material with the Outline shader.


## Outline Shader used
![Captura de pantalla 2023-06-14 205147](https://github.com/albertferras/unity-2d-rigging-shader/assets/7689174/80e0e9d0-ef13-4b12-980f-b983f957b337)

Zoom Left side:
![Captura de pantalla 2023-06-14 205308](https://github.com/albertferras/unity-2d-rigging-shader/assets/7689174/be9ab4c3-a5f3-4c20-9696-8bff4928979b)

Zoom Right side:
![Captura de pantalla 2023-06-14 205341](https://github.com/albertferras/unity-2d-rigging-shader/assets/7689174/f7b333df-95ba-4f64-b502-df86b481133a)

OutlineSubgraph:
![Captura de pantalla 2023-06-14 205357](https://github.com/albertferras/unity-2d-rigging-shader/assets/7689174/ac0dc6de-ea2a-4cd2-8af9-dc77802051a3)


