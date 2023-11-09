> **⚠️ Note:**
> **This sample cannot be converted to MonoGame because it focuses specifically on the Xbox 360 avatar integration provided through `Microsoft.Framework.Xna.GameServices` which is not part of MonoGame.**
> If you have an idea for how to take this sample and make it relevant for something in MonoGame, please submit discussion on the [Port Issue](https://github.com/xna-to-monogame/AvatarAnimationBlendingSample/issues/1)

 
# Avatar Animation Blending Sample


This sample shows how to blend two avatar animations together so that they smoothly transition from one animation to the next.

## Sample Overview

The **AvatarAnimation** object gives developers the ability to play one of the built-in animations on an **AvatarRenderer**. When you switch animations, the positions of the avatars arms and legs can quickly snap to the new animation positions. Blending between the two animations can smooth out this transition.

This sample demonstrates how to blend between two animations over a fixed period of time. Once a new animation is selected by the user, the new target animation is blended into the current animation over the period of a quarter of a second.

## Sample Controls

This sample uses the following gamepad controls.

| Action                     | Gamepad control        |
|----------------------------|------------------------|
| Play the cheer animation.  | **A**                  |
| Play the clap animation.   | **B**                  |
| Play the stand animation.  | **X**                  |
| Play the wave animation.   | **Y**                  |
| Switch blending on or off. | Left shoulder button   |
| New random avatar..        | Right shoulder button  |
| Rotate the camera.         | Right thumbstick       |
| Zoom in.                   | Right trigger          |
| Zoom out.                  | Left trigger           |
| Reset the camera.          | Right thumbstick press |
| Exit the sample.           | **BACK**               |

## How the Sample Works

This sample implements a new type called ``AvatarBlendedAnimation``. The new type has similar properties and methods to the **AvatarAnimation** type. One difference is the **Play** method, which sets the next animation to play and represents the target animation that will be blended to.

The **Update** method is where the blending calculations take place. The method first checks to see if there is a target animation to blend to. If not, that means the current animation is still playing and the bone transforms for the current animation should be used by themselves.

If there is a target animation, then a blend factor from 0 to 1 is calculated. 0 means use 100 percent of the current animation's bone transforms, 1 means use 100 percent of the target animation's bone transforms, and 0.5 means use 50 percent from each. The blend factor changes from 0 to 1 over the course of the total blend time. In this sample, the total blend time is hard-coded to a quarter of a second.

Once you know the blend factor, you check to see if it is greater than 1. If it is, it means you no longer need to blend and instead need to switch the target animation to be the current animation.

The last part of the **Update** method loops each bone and creates a blended bone transform. Creating the new blended transform takes three steps:

- First, calculate the blended transform's rotation. The current and target rotations are used to find the final rotation by using spherical linear interpolation, or slerp. The **Quaternion.Slerp** method takes the current and target rotations along with the blend factor.
- Second, calculate the blended transform's translation. The current and target translations are used to find the final translation by using linear interpolation, or lerp. The **Vector3.Lerp** method takes the current and target translations along with the blend factor.
- Third, the final blended transform is created by multiplying the matrix created by the blended rotation and the matrix created by the blended translation. This final blended transform is then used by the **AvatarRenderer.Draw** method to display the avatar.

## Extending the Sample

Some ways to extend the sample include adding support for animation tracks. These would allow the developer to define any number of animations to blend together by blend weight. Another feature that can be added to the sample is the ability to wait until one animation is complete before completing the blend to another animation.

© 2009 Microsoft Corporation. All rights reserved.
