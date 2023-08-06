# **My Vulkan FAQ**


## **What is abstract level of Vulkan?**
 - Global-level functions. Allow us to create a Vulkan instance.
 - Instance-level functions. Check what Vulkan-capable hardware is available and what Vulkan features are exposed.
 - Device-level functions. Responsible for performing jobs typically done in a 3D application (like drawing).

## What is Vulkan Layer？

a layer is an **optional** library that can **intercept Vulkan functions** on their way from the Vulkan application down to the Vulkan drivers. Multiple layers can be chained together to use multiple layer functionalities simultaneously.

For example, enabling validation layer can detect lots of useful errors and warnings during development.

## Diff between vulkan explicit/implicit/meta/override layer?

- **explicit layer** : be explicitly activated by the user from source in `vkCreateInstance`, using Vulkan Configurator or `VK_INSTANCE_LAYERS` environment variable. mainly used for debugging or profiling
- **implicit layer**: enabled by their existence on the system by default. (explicit or implicit  can be configured by the manifest json file. )
- **meta layer**: a special kind of layer which is only available through the desktop [Vulkan Loader](https://github.com/KhronosGroup/Vulkan-Loader). While usual layers are associated with one particular library, a meta-layer is actually a collection layer which contains an ordered list of other layers called *component layers*.
- **override layer**:  an implicit meta-layer found on the system with the name `VK_LAYER_LUNARG_override`. It is the mechanism used by *Vulkan Configurator* to override Vulkan applications layers. This layer contains:
  - The ordered list of layers to activate
  - The list of layers to exclude from execution
  - The list of paths to executables that the layers override applies to. If this list is empty, the override is applied to every application upon startup.


**Vulkan Layer settings**: Per-layer settings loaded by each layer library and stored in the `vk_layer_settings.txt` file. This file is located either next to the Vulkan application executable or set globally and applied to all Vulkan applications thanks to *Vulkan Configurator*. These settings are described [here for VK_LAYER_KHRONOS_validation](https://github.com/KhronosGroup/Vulkan-ValidationLayers/blob/main/layers/vk_layer_settings.txt) and [here for other layers created by LunarG](https://github.com/LunarG/VulkanTools/blob/main/layersvt/vk_layer_settings.txt).

- see https://renderdoc.org/vulkan-layer-guide.html for a friendly introduction about mechanism of vulkan layer
- see https://vulkan.lunarg.com/doc/view/1.3.250.1/linux/vkconfig.html for the official doc



## How to import Vulkan library?

Three way to choose:

1. Dynamically load the driver’s library that provides Vulkan API implementation and acquire function pointers by ourselves from it. (NOT recommended, due to hardware vendors can modify their drivers in any way, and it may affect compatibility with a given application.)
2. Use the Vulkan SDK and link with the provided Vulkan Runtime (Vulkan Loader) static library.
3. Use the Vulkan SDK, dynamically load Vulkan Loader library at runtime, and load function pointers from it. (adopted by volk, a meta loader for Vulkan API, see https://github.com/zeux/volk)

This choice whether static or runtime link is just a matter of personal preference.

---

Here are some widely used open source libraries to help you do the above work, such as volk. you can find it in `Include` folder of VulkanSDK, or git clone from [github](https://github.com/zeux/volk).

---

To import Vulkan at runtime:

- Step 1: import `vkGetInstanceProcAddr()` by OS-specific interfaces
- Step 2: import all global-level functions by `vkGetInstanceProcAddr(nullptr, func_name)`
- Step 3: import all instance-level functions by `vkGetInstanceProcAddr(vkInstance, func_name)`

## What is Extension?
**additional functionality that is not required by core specifications, and not all hardware vendors may implement them, like layers**. Extensions are also divided into instance and device levels, and extensions from different levels must be enabled separately.

## What is Feature?

**additional hardware capabilities that are similar to extensions.** Features  may not necessarily be supported by the driver and by default are not enabled. If a given physical device supports any feature we can enable it during logical device creation. e.g., geometry shaders.

A logical device represents a physical device and all the features and extensions we enabled for it.

## What does the suffix of extension mean?  
summary from [here](https://www.reddit.com/r/vulkan/comments/9twibs/what_are_these_2khr_things/):

- *KHR: every Vulkan extension entity has a suffix (e.g., EXT, NV, AMD). KHR means Khronos extension, and many KHR were promoted to newer Vulkan version, which means there may as well be the same entities without the suffix.

- *2: (and possibly 3, 4, ...) is used because the original name is already taken. Typically done for improvements of the original function as newer version, or fixing defects. E.g. some *2 structures add pNext (which the original was missing) in order for them to be extensible.

Thus, For the functions and types of the same name in the latest version of Vulkan, it is preferred to choose the one without the extension suffix and the one with a larger suffix number, which can obtain the best functionality.

## What is layout?

**layout, is an internal memory arrangement of an image.** Image data may be organized in such a way that neighboring “image pixels” are also neighbors in memory, which can increase cache hits (faster memory reading) when image is used as a source of data (that is, during texture sampling). But caching is not necessary when the image is used as a target for drawing operations, and the memory for that image may be organized in a totally different way. Image may have linear layout (which gives the CPU ability to read or populate image’s memory contents) or optimal layout (which is optimized for performance but is also hardware/vendor dependent). So some hardware may have special memory organization for some types of operations; other hardware may be operations-agnostic. Some of the memory layouts may be better suited for some intended image “usages.” Or from the other side, some usages may require specific memory layouts. There is also a general layout that is compatible with all types of operations. But from the performance point of view, it is always best to set the layout appropriate for an intended image usage and it is application’s responsibility to inform the driver about transitions. Usually, we may change  image layouts  by using image memory barriers manually or by  the hardware inside a render pass automatically.

## What is framebuffer?

Framebuffer describes specific images that the render pass operates on. In Vulkan, it describes all the textures (attachments) used during the render pass, not only the images we are rendering into (color and depth/stencil attachments) but also images used as a source of data (input attachments).

In Vulkan, the framebuffer specifies what images(i.e., `vkImageView`) are used as attachments on which the render pass operates. We can say that it translates `vkImage` (or `vkImage`) into a given attachment.

 The number of images specified for a framebuffer must be the same as the number of attachments in a render pass for which we are creating a framebuffer.



In Vulkan, RenderPass and FrameBuffer is closely comnnected and corresponded one-to-one. A specified RenderPass -> a specified  FrameBuffer

### Why do we separate framebuffer from RenderPass in Vulkan? 

For more flexibility.  We can use the given render pass with different framebuffers and a given framebuffer with different render passes, if they are compatible, meaning that they operate in a similar fashion on images of similar types and usages.

### What is image and image view?

we don’t operate on images themselves. Images are not accessed directly. For this purpose, image views are used. Image views represent images, they “wrap” images and provide additional (meta)data for them.

## Why can't we change state of pipeline at any time?

Because of the performance implications of such state changes. Changing just one single state of the whole pipeline may cause graphics hardware to perform many background operations like state and error checking. Different hardware vendors may implement (and usually are implementing) such functionality differently. This may cause applications to perform differently (meaning unpredictably, performance-wise) when executed on different graphics hardware. So the ability to change anything at any time is convenient for developers. But, unfortunately, it is not so convenient for the hardware.

## What does layout (set = X, binding = Y) mean ?

A **pipeline layout** describes all the resources that can be accessed by the pipeline. We must specify how many textures can be used by shaders and which shader stages will have access to them. There are of course other resources involved. Apart from shader stages, we must also describe the types of resources (textures, buffers), their total numbers, and layout. This layout can be compared to OpenGL’s active textures and shader uniforms. In OpenGL we bind textures to the desired texture image units and for shader uniforms we don’t provide texture handles but IDs of the texture image units to which actual textures are bound (we provide the number of the unit which the given texture was associated with).

With Vulkan, the situation is similar. We create some form of a memory layout: first there are two buffers, next we have three textures and an image. This memory “structure” is called a set and a collection of these sets is provided for the pipeline. In shaders, we access specified resources using specific memory “locations” from within these sets (layouts). This is done through a layout (set = X, binding = Y) specifier, which can be translated to: take the resource from the Y memory location from the X set.



Pipeline layout can be thought of as an **interface between shader stages and shader resources** as it takes these groups of resources, describes how they are gathered, and provides them to the pipeline.

## Fence vs. Semaphore

Semaphores are used for internal queue synchronization. Fences, on the other hand, allow the application to check if some specific situation occurred, e.g. if command buffer’s execution after it was submitted to queue, has finished. If necessary, application can wait on a fence, until it is signaled. In general, semaphores are used to synchronize queues (GPU) and fences are used to synchronize application (CPU).

## What is *virtual frame*?

To render a single frame of animation we need (at least) one command buffer, two semaphores—one for a swapchain image acquisition (image available semaphore) and the other to signal that presentation may occur (rendering a finished semaphore)—a fence, and a framebuffer. The fence is used later to check whether we can rerecord a given command buffer. We will keep several numbers of such rendering resources, which we can call a *virtual frame*. The number of these *virtual frames* (consisting of a command buffer, two semaphores, a fence, and a framebuffer) should be independent of a number of swapchain images.

## How many *virtual frames* should we have?

One is not enough. When we record and submit a single command buffer, we immediately wait until we can rerecord it. It is a waste of time of both the CPU and the GPU. The GPU is usually faster, so waiting on a CPU causes more waiting on a GPU. We should keep the GPU as busy as possible. That is why thin APIs like Vulkan were created. Using two *virtual frames* gives huge performance gain, as there is much less waiting both on the CPU and the GPU. Adding a third *virtual frame* gives additional performance gain, but the increase isn’t as big. Using four or more groups of rendering resource doesn’t make sense, as the performance gain is negligible (of course this may depend on the complexity of the rendered scene and calculations performed by the CPU-like physics or AI). When we increase the number of *virtual frames* we also increase the input lag, as we present a frame that’s one to three frames behind the CPU. **So two or three *virtual frames* seems to be the most reasonable compromise between performance, memory usage, and input lag.**

## Why the number of *virtual frames* shouldn’t be connected with the number of swapchain images?

This approach may influence the behavior of our application. When we create a swapchain, we ask for the minimal required number of images, but the driver is allowed to create more. So different hardware vendors may implement drivers that offer different numbers of swapchain images, even for the same requirements (present mode and minimal number of images). When we connect the number of *virtual frames* with a number of swapchain images, our application will use only two *virtual frames* on one graphics card, but four *virtual frames* on another graphics card. This may influence both performance and mentioned input lag. It’s not a desired behavior. By keeping the number of *virtual frames* fixed, we can control our rendering algorithm and fine-tune it to our needs, that is, balance the time spent on rendering and AI or physics calculations.



---

## What is mesh shader? Why we need it?

![image-20230511114536127](https://raw.githubusercontent.com/NoSW/CloudImg/PicGo/img/202305111145253.png)

Mesh shading – new programming model for geometry:

- Combines flexibility of compute and efficiency of pipeline scheduling
- Streamlined pipeline through elimination of serial bottlenecks
- Enables greater efficiency and control in geometry processing

Why use it? -> LOD management, data structure traversal, geometry synthesis, procedural-ism

---

## Some Cmd Tips

Cmd(Buf)Pool: Typically, a free list of fixed-size pages. Each command pool can only be used from one thread concurrently.

CmdBuf would contain a list of pages with actual commands, with special jump commands that transfer control from each page to the next one so that GPU can execute all of them in sequence. 

Whenever a command needs to be allocated from a command buffer, it will be encoded into the current page; if the current page doesn’t have space, the driver would allocate the next page using a free list from the associated pool, encode a jump to that page into the current page and switch to the next page for subsequent command recording.

Multi-threading record:

- \# of pool: **F**(usually 2, one for being recorded by CPU, on for being executed by GPU, or 3)**\* T**(can be as high as core count)
-  pool associated with the current frame & thread



!NOTE: command buffers allocated from a given pool can only be submitted to a queue from a queue family specified during pool creation.

!NOTE: Before cmd record, we must set up a barrier that tells the driver that previously queues from one family referenced a given image but now queues from a different family will be referencing it if  graphics queue is different than the present queue

## Some Barrier Tips

- barriers need to be batched as aggressively as possible.
- only relevant stages need to be included by using `dstStageMask`
- specify the dependency via what’s called a split barrier: instead of using `vkCmdPipelineBarrier`, use `vkCmdSetEvent` after the write operation completes, and `vkCmdWaitEvents` before the read operations starts. (make sure there’s enough work submitted between Set and Wait,)

## Type of descriptor

experience based(a free list of  fixed DescriptorPool) -> slot based (divided slot by descriptor type, e.g., buffer, image, sampler,...)  -> freq based (perFrame, perDraw, perMaterial,...) -> bindless (always constant bindings calls)

- for texture requiring filtering,  3 types typically:
  - combined image/sampler
  - separate image and sampler
  - image + immutable sampler(must be specified when pipeline object is created)

push constant with a guaranteed limit of 128 bytes per draw call

uniform buffer with a guaranteed limit of 16 KB

### **What is RenderPass in Vulkan? and subpasses?** 

In Vulkan, a render pass represents (or describes) a set of framebuffer attachments (images) required for drawing operations and a collection of subpasses that drawing operations will be ordered into.

- a RenderPass contains some subpasses, attachments and dependencies between its subpasses.
- each subpass may use multiple attachments in a different way (or different layout)

Driver will help us optimize automatic layout transitions and, more generally, optimize barriers between subpasses.

### **How to pass data CPU->GPU?**

- split memory type to: GPU_ONLY, CPU_TO_GPU
- split data type to: uniform buffer, push constants, texture, buffer, specialization constants

For GPU_ONLY: texture, buffer

1) copy your data into a CPU writable buffer; 
2)  encode a copy cmd in a `vkCommandBuffer` ;
3) submit this cmd to a `vkQueue`, which transfer memory from CPU-allocated-buffer into a GPU-allocated-buffer

---

## Philosophy of Vulkan in my eyes

- layers system (validation, ...), allow api-users to validate/debug/profile code from time to time and guide api-users to do a best  practice.
- elegant and extensible C API, `vkSomethingCreateInfo` -> `vkCreateSomething`-> `vkDestroySomething`, and extensible `pNext`
- lots of underlying opt mind: pooling, type, type of type, type of "type of type", opt by leveraging pre-knowledge

## Learning path

1. [homepage of vulkan](https://www.vulkan.org/), that's the coolest official website I have ever seen! If someone asks me: where to start learning vulkan, I will definitely say, read the official website first!
2. [Understanding Vulkan Objects](https://gpuopen.com/learn/understanding-vulkan-objects/)，Introduce the concepts in the Vulkan API in plain words, with a diagram.
3. prepare some on the desktop:
   - [vulkan-diagrams](https://github.com/David-DiGioia/vulkan-diagrams)，further and more detailed diagram that should to be used with a coding tutorial below. (Diagrams are always easier to read than words :)
   - [Specification](https://registry.khronos.org/vulkan/specs/1.3/html/)， not read it right away, but put it on your hand. Don't forget to come back frequently and read it as needed.

4. choose one or more tutorial **with coding**: (It will be very easy to finish the other after you finish first one)
   - https://vulkan-tutorial.com/，most people's choices
   -  [API without Secrets: Introduction to Vulkan](https://www.intel.com/content/www/us/en/developer/articles/training/api-without-secrets-introduction-to-vulkan-preface.html)，from Intel, a very patient teacher who explained in great detail, even the parameters of each API.
   3. https://vkguide.dev/，from the perspective of building a game engine, and attached some great resource Links.

5. read or write source code of RHI(Rendering Hardware Interface) libraries 
   - [The-Forge](https://github.com/ConfettiFX/The-Forge), cross almost all platforms, lots of good examples about graphics feature, developed by a game engine middleware provider, update is  infrequent since it developed a private repo on Gitlab.
   - [bgfx](https://github.com/bkaradzic/bgfx), (probably) the most popular one, also cross almost all platforms.
   3. [igl](https://github.com/facebook/igl)，common libraries used internally by meta (formerly facebook, all in metaverse?), open source since 2023.07, not support console platforms(Play Station, Switch, XBox)

6. read or write source code in a game engine
   - [Unreal](https://github.com/EpicGames/UnrealEngine), the most advanced and possibly the most complex one
   - [godot](https://github.com/godotengine/godot), completely open source, may be relatively easy-to-read
   - any one you're interested in, or write one yourself

