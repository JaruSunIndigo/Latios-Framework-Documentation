# Getting Started with Kinemation – Part 2

In this part, we’ll bake a Skinned Mesh Renderer into an entity and use an
exposed skeleton. No code will be required.

If you haven’t done so already, create a new DOTS project and add the Latios
Framework as a package. Then, create one of the Standard Bootstraps.

**Do not forget to create a bootstrap!**

## Importing a Character

When importing a character, in the rig tab, the most important settings are
highlighted in this image:

![](media/a16538b197e863a720ff35b6d76010dd.png)

The *Animation Type* can be set to whatever type your animation clips work with.
The *Avatar Definition* must be set to *Create From This Model* (for this
example only). *Skin Weights* is dependent on which skinning algorithm you plan
to use. If you use *Vertex Skinning* (Vertex Shader), you must set it to
*Standard (4 Bones)* as shown above. For *Deform Skinning* (Compute Shader), you
can change it to custom with a *max weights* set up to a maximum of 256. Leave
*Optimize Game Objects* unchecked (for this example only).

Lastly, turn **off** compression of animation clips.

![](media/3d23064968acfbda1080d8eb22f5ce24.png)

*Q: Why did we turn animation compression off?*

*A: Kinemation uses ACL’s state-of-the-art compression algorithm during baking.
If it reads from Unity’s lossy-compressed clips, quality will be significantly
reduced without any benefit.*

## Making the Shader Graph and Materials

Kinemation has the same requirements as Entities Graphics in that skinned meshes
must use a shader that supports one of the two skinning algorithms. You can
easily create such a shader using *Shader Graph* and the provided deform nodes.
Only the *Vertex* block matters for skinning. Here’s what such a graph looks
like using *Latios Vertex Skinning*:

![](media/eedd8e6559dc35bc7dedda9dc55c9317.png)

**Important: Note the use of Object Space!** By default, those nodes will be set
to World Space.

And here’s what it looks like using *Latios Deform*:

![](media/1ccddd38ca9b0f7bc01002d59f428565.png)

*Q: What about Unity’s Linear Blend Skinning and Compute Deform nodes?*

*A: Those are supported too, but may have slightly worse performance. In
addition, Linear Blend Skinning produces artifacts when every vertex in the mesh
has two or less bone influences each.*

*Q: When would I use Latios Vertex Skinning, and when would I use Latios
Deform?*

*A: Latios Deform works in all scenarios, but consumes more VRAM and is slower
on some platforms. Latios Vertex Skinning only supports skeletal mesh
deformations (no dynamic meshes or blend shapes) and only supports up to 4 bone
influences per vertex, but is faster on some platforms and consumes less VRAM.*

Once you’ve done that, you will need to create materials using your new shaders.

![](media/299fe25fca0ebd0661503dc965a4a4d9.png)

*Q: I get an error: Material {material_name} used by non-deforming
MeshRendererBakeSettings index {number} for object {game_object_name} uses
shader {shader_name} which uses deformations. This may result in incorrect
rendering.*

*A: Your character probably has a rigid (non-skinned) mesh attached to a bone,
and you assigned a skinning shader to it. This doesn’t work. Use a normal
unskinned material for this mesh.*

### Dual Quaternion Skinning

If your skinned mesh should use dual quaternion skinning, how you set it up
differs based on which graph node you chose. If you chose *Latios Vertex
Skinning*, you can simply change the Algorithm dropdown in the node. However, if
you chose *Latios Deform*, you must instead add a *Skinned Mesh Settings*
component to your *Skinned Mesh Renderer* Game Object and check the *Use Dual
Quaternion Skinning* checkbox:

![](media/9f4934c5bae8734acd11b9e871c2a420.png)

Dual quaternion skinning is more expensive than matrix skinning. Use it only if
deformations look better.

## Baking the Entity

Now, drag your character into a subscene. Assign it the new materials, and make
sure it has an *Animator* component attached. The *Animator* doesn’t need to be
configured with anything. It just serves as a marker so Kinemation bakes it to a
skeleton.

![](media/9be6de904e3dc9e73410fc9762c0a304.png)

Now press Play. If everything went right, the character should still be posed,
but now using Kinemation. I included some of the components on the skeleton in
the screenshot on the right so that you can really see this is being driven by
Kinemation. But I encourage you to explore the components yourself!

![](media/181bc6e24e10dd3e36841ace4cb2985b.png)

You can even mess with the transform components!

![](media/2c8bdb8a31c10c3d5f2e6a600a47870e.gif)

## On to Part 3

And there ends our code-free part of our tour. Next up, we will need to write
some code to play some animation. If you aren’t familiar with writing DOTS code,
you may want to stop here.

[Continue to Part 3](Getting%20Started%20-%20Part%203.md)
