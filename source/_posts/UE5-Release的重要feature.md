---
title: UE5 Release的重要feature
date: 2022-11-06 18:56:28
tags: 
 - Unreal Engine 5
 - UE5
categories: UE5
---

[UE5 release note](https://docs.unrealengine.com/5.0/zh-CN/unreal-engine-5-0-release-notes/)
从UE5的release note里面摘取一些重要的feature和bug修复及优化，当作是一个release note阅读笔记

<!--more-->

#### Feature
- luman nanite不用多说
- VSM，移动端有无可能？
- Temporal Super Resolution代替taa，1080->4k，降低taa重影
- **局部曝光处理Local Exposure，精细调节局部曝光**
- **world partition**重点关注，数据层概念，actor文件
	-  自动网格化的hlod，自动创建数据驱动的自定义hlod层，优化加载区外的大量静态actor
	- 大世界坐标
- chaos布料+物理场
- Unreal Insights更新
	- **Memory Insights**
	- **Task Graph Insights**可以看到task的关系，牛逼
	- 上下文切换

移动平台
- android文件服务器
- **android二进制文件改进**
- gles3.2 + vulkan dxc
- sss
- **dfshadow**
- csm cache
- skinCache
- **GPU Scene Instancing**，动态实例化物体

#### Bug Fix 
重点关注移动端和rendering



#### Optimization

#### **Mobile Rendering**

New:

-   The mobile deferred and forward lighting calculations are now unified.
    
-   Added an option to use high quality BRDF on mobile, same as with PC.
    
-   If a light shading model is not supported on mobile, it will now fall back to the default light shading model.
    
-   Use EnvBrdf for mobile deferring lighting pass.
    
-   Mobile now supports the Subsurface and PreIntegrated Skin shading models. Subsurface Profile shading on mobile uses preintegral burley diffusion on ring, just like the PreIntegrated Skin but could be generated at runtime based on Burley inputs.
    
-   Vulkan now uses G8 format for mobile Ground Truth Ambient Occlusion (GTAO). Enabled mobile AO and PPR on high-end mobile devices.
    
-   Added support for Mobile Deferred Rendering mode on Android OpenGL ES.
    
-   Integrated Signed Distance Fields to Mobile Deferred Renderer.
    
-   Added GL_MAX_TEXTURE_BUFFER_SIZE to OpenGLES.h to not depend on the extension GL_MAX_TEXTURE_BUFFER_SIZE_EXT.
    
-   The variable r.MobileNumDynamicPointLights is now read from the platform .ini file instead of from the project console variable. The console variable r.MobileNumDynamicPointLights was added to the shader keystring.
    
-   Disabled Parallel RDG for Mobile Platforms from the RenderGraph.
    
-   Hair Materials now directly use the Hair Shading Model instead of doing so with a FeatureLevelSwitchExpression, improving hair Shading Model support on mobile.
    
-   Added support for a Skin Cache on mobile platforms.
    
-   Set Fast Approximate Anti-Aliasing (FXAA) quality to 0 on mobile platforms by default.
    
-   Added an option for projects to disable support for per-pixel Material shading models on mobile platforms. Use the console variable r.Mobile.AllowPerPixelShadingModels=0.
    
-   Account for Reflection Capture influence radius when finding closest capture for an object. Fallback to skylight reflection when there are no influencing Reflection Captures on mobile platforms.
    
-   Removed perspective correct shadow depth shader permutation when its not required on mobile platforms
    

Improvement:

-   Removed NoLightMap shader permutation for Materials that are not using it on mobile platforms.
    

Crash Fix:

-   Fixed a crash on mobile while rendering custom depth primitives that require scene texture access.
    

Bug Fix:

-   Fixed an issue where the brightness of the Reflection Captures couldn't be changed at runtime on mobile.
    
-   Fixed the specular artifacts that would occur on Snapdragon 845 devices.
    
-   Disabled mobile MSAA if DepthFullPrepass is enabled.
    
-   Fixed an issue where depth was not stored with fullprepass enabled.
    
-   Fixed an issue with incorrect depth stencil access flag in mobile multi-view rendering.
    
-   Integrated mobile Static Lighting when Distance Fields are enabled.
    
-   Enqueue FrameSyncEvent ReleaseResource on the Render Thread when in the RHI thread in OpenGLViewport.
    
-   Resolved a Depth issue with Editor Primitives in Mobile Preview.
    
-   Implemented a Binary Cache fix for the Compute shader and fixed calculation of ShaderLib Processing time.
    
-   Fixed a bug that caused Editor to stall in the Material Editor while in Mobile Preview.
    
-   Fixed Custom Stencil and Depth on mobile by making them not always memoryless. This changes the logic back to the way it was implemented in SceneRenderTargets.
    
-   Resolve issues with texture mip generation at runtime on Android devices running Vulkan.
    
-   Resolved an issue preventing PerlinNoise3D texture from correctly initializing on mobile platforms.
    
-   Allow access to CustomDepth and CustomStencil textures in Decal Materials for mobile platforms.
    

Removed:

-   Removed IsMobileDistanceFieldShadowingEnabled.
    
-   Removed the shader define MOBILE_PROPAGATE_ALPHA, and temporarily removed premultiply alpha. FXAA luminance calculation was moved from the tonemapper to FXAA when MobilePropagateAlpha is enabled.
    
-   Removed the OpacitySceneCapturePass from MobileSceneCapture since Depth is now stored in a different Render target. Moved everything to the shared SceneCaptureRendering file and removed MobileSceneCapture.usf.
