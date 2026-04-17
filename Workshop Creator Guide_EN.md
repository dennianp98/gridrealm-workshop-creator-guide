# Workshop Building Creation Guide

Welcome, Creator! Thank you for your interest in creating custom content! Your contributions will help the community thrive! This guide will help you create content that meets specifications and upload it to the Steam Workshop.

## Technical Requirements
Uploading content requires two scripts: BuildingData and WorkshopUploadTool. BuildingData is used to fill in building data, and WorkshopUploadTool is used for uploading. You also need Steamworks.NET. If you need to update Steamworks.NET, you can download it from the GitHub page: https://github.com/rlabrecque/Steamworks.NET/releases, import it into Unity, and attach the Steam Manager script to an empty GameObject.
I have packaged the required scripts and Steamworks.NET into a Unity package file. You can use this package file or download them separately: Link XX

### Building Model Specifications

> ⚠️ **Required Components Reminder**: Building prefabs must contain the following three core components. Missing any one of them will cause functionality issues:
> - **Mesh Filter**: Displays the model mesh
> - **Mesh Renderer**: Renders the model (requires material assignment)
> - **Collider** (Box Collider): Used for mouse hover highlighting, click detection, and placement collision detection

### Supported Resource Formats

- **Models**: FBX
- **Textures**: PNG
- **Materials**: Unity URP Lit Shader

### File Size Limits

- Single building AssetBundle: ≤ 50MB
- Preview image: ≤ 1MB
- Total download size: ≤ 100MB

## Building Design Guidelines

### 1. Size Specifications

Design according to the grid size occupied by the building. The building base (foundation) size should be at least 1 meter in length and width:
To ensure correct calculation of building grid occupation, it is recommended to use odd-numbered squares for the building base size, such as: 1×1, 3×3, 5×5. Even-numbered squares and non-square shapes are not recommended, such as: 1×2, 2×2, 2×3, 4×4.
Height: No strict limit, but it is recommended to be no less than 0.1 meters

### 2. Visual Style

To maintain game consistency, it is recommended to:
- Use LowPoly style
- Ensure overall proportions and scale relationships of the building (game default scale reference: character height 0.1 meters)
- Use isometric-friendly designs
- Avoid overly small details (hard to see from a distance)
- Use clear outlines and color contrast
- Consider details from different angles

### 3. Performance Optimization Recommendations

- Reasonably control polygon count
- Transparent materials (use Alpha Test instead of Alpha Blend)

## Creation Process

### Step 1: Modeling in 3D Software

1. Create models using Blender, Maya, 3ds Max, etc.
2. Ensure the model origin is at the bottom center
Using Blender as an example:
✅ Correct: Building center point at world origin
((Image 1))
❌ Wrong: Building offset or rotation misaligned
((Image 2))
3. Apply all transformations (Scale, Rotation)
4. Export as FBX format

### Step 2: Import into Unity

1. Drag the FBX into the Unity project
2. Set the building prefab's Transform Position to (0, 0, 0)
3. Ensure the building base is on the Y=0 plane
4. The building should face the positive Z-axis direction

### Step 3: Set Materials and Textures

1. Create materials (using URP Lit Shader)
2. Adjust material parameters

### Step 4: Add Required Components

Ensure the building prefab contains the following three **required components**:

#### 1. Mesh Filter and Mesh Renderer

1. Select the building root object
2. Confirm that the **Mesh Filter** component has been added (if not, click Add Component → Mesh Filter)
3. Confirm that the **Mesh Renderer** component has been added
4. Assign materials in the Mesh Renderer (use Standard or URP Lit Shader)

> ⚠️ **Important**: Buildings without a Mesh Renderer will not display and will not trigger mouse hover highlighting!

#### 2. Collider

1. Select the building root object
2. Add a **Box Collider** component (recommended) or **Mesh Collider**
3. Adjust the Collider size to cover the entire building
4. Ensure the Collider does not extend beyond the building range, as this will cause collision with other buildings and prevent placement
((Image 3))

> ⚠️ **Important**: Collider is a necessary condition for mouse hover highlighting and click detection! Buildings without a Collider cannot be selected or interacted with by players.

### Step 5: Create Prefab

1. Drag the building into the Project window to create a Prefab
2. Naming convention: `{BuildingName}_Prefab`
3. Test that the prefab can be instantiated normally

### Step 6: Create BuildingData

1. Right-click → **Create → City Builder → Building Data**
2. Fill in the following fields:

| **Building Name** | Building display name, can use name or number | e.g.: Ferriswheel, SSmbl2 |
| **Building ID** | Unique identifier, recommend using style name plus number, or pure number | e.g.: SunsetCity_01, SSmbl2 |
| **Style** ⭐ | **Determines which style tab the building appears under in the game building menu**, recommend using your developer name, package name, or theme name. Buildings in the same series should have the same value | e.g.: Lyon'sPack, SunsetCity |
| **Category** ⭐ | **Determines which category column it appears under within that style**, can use existing game categories for compatibility (see table below), or fill in according to your own building classification | e.g.: Large, Residential |
| **Grid Size** | Building grid occupation size | e.g.: 3x3 |
| **Building Prefab** | Drag in your building prefab | — |

**Built-in Game Categories Reference**:

| `Large` | Large |
| `Medium` | Medium |
| `Small` | Small |

> ⚠️ **Style and Category fields are very important**: They will be packaged into `metadata.json` (automatically written by the upload tool). When the game loads your mod, it uses these two fields to determine the building's position in the menu. Please make sure to fill them in, otherwise the building may not be correctly categorized and displayed.

3. Create a building icon (recommended 128×128 PNG), drag it into the **Icon** field

### Step 7: Test Building

1. Test the building in-game:
   - Can it be placed correctly
   - Does rotation work normally
   - Is collision detection correct
   - Are you satisfied with the visual effect
2. Adjust until satisfied

### Step 8: Prepare for Upload

- Create a preview image (recommended 600×600)
- Confirm that **Style** and **Category** in BuildingData are correctly filled (the upload tool will automatically read them)
Pre-upload checklist:
   - [ ] Model triangle count within limits
   - [ ] Texture resolution is reasonable
   - [ ] Contains **Mesh Filter** component
   - [ ] Contains **Mesh Renderer** component (material assigned)
   - [ ] Contains **Collider** component
   - [ ] Prefab can be instantiated normally
   - [ ] Building aligned to grid
   - [ ] No missing materials or textures
   - [ ] All BuildingData fields filled
   - [ ] Preview image is clear
   - [ ] Passed in-game testing

## Step 9: Upload to Workshop

> ⚠️ **Upload is completed in two steps**, because AssetBundle packaging and Steam upload have different requirements for editor state, they must be done separately.

### Step One: Package Resources (Non-Play Mode)

1. Ensure you are in **Non-Play Mode** (editor is not running)
2. Open **City Builder → Workshop Upload Tool**
3. Select mod type: **Building**
4. Drag in **BuildingData** (the tool will automatically read Style and Category from it, can be modified manually)
5. Drag in **building prefab**
6. Fill in workshop information (mainly fill in the title, description can be edited in the Workshop)
7. Click **"Step One: Package Resources as AssetBundle"**
8. When you see the message **"Packaging complete! Please enter Play mode and click Step Two to upload"**, it was successful

> 💡 The packaging path will be automatically saved and will not be lost after entering Play mode, no need to re-enter information.

### Step Two: Upload to Steam (Play Mode)

1. Click the Unity editor's **Play** button, wait for SteamManager to initialize
2. Confirm in the tool window that Steam is initialized (no warning messages)
3. Click **"Step Two: Upload to Steam Workshop"**
4. Wait for upload to complete, record the displayed **Workshop Item ID** (for future updates)

> 💡 The upload tool will automatically generate `metadata.json` and package it into the AssetBundle content directory, which contains Style and Category information. After players subscribe, the game will automatically display it in the correct category.

### Step 10: Publish and Maintain

1. Complete information on the Steam Workshop page
2. Add more screenshots

## Sky and Ground Material Notes
- Sky and ground shaders need to use URP pipeline
- Ground shaders should preferably use procedural normals to handle dynamic grid area size adjustments

## Common Errors and Solutions

### Error 0: Packaging Error "Building AssetBundles while in play mode is not allowed"

**Cause**: Clicked the package button in Play mode, but AssetBundle packaging can only be executed in Non-Play mode

**Solution**:
1. Exit Play mode (click the stop button)
2. Click "Step One: Package Resources as AssetBundle" in the tool again
3. Enter Play mode after packaging is complete to upload

### Error 1: Building Displays as Pink

**Cause**: Missing material or shader

**Solution**:
1. Ensure all textures are correctly assigned
2. Check if materials are included in the AssetBundle

### Error 2: Building Cannot Be Placed

**Cause**: Missing required components (Mesh Filter/Mesh Renderer/Collider) or incorrect grid size settings

**Solution**:
1. Confirm the building prefab contains **Mesh Filter**, **Mesh Renderer**, and **Collider** components
2. Check if Mesh Renderer has materials assigned
3. Add Box Collider to the building root object
4. Check BuildingData's Grid Size setting
5. Confirm building dimensions

### Error 3: Building Position Offset

**Cause**: Prefab Transform settings are incorrect

**Solution**:
1. Set prefab Position to (0, 0, 0)
2. Ensure building base is on the Y=0 plane
3. Check the model's Pivot Point position

### Error 4: Building Misaligned After Rotation

**Cause**: Building center point is not at the grid center

**Solution**:
1. Adjust model origin in 3D software
2. Or adjust child object positions in Unity
3. Ensure building rotates around the center point

### Error 5: File Too Large to Upload

**Cause**: Texture resolution too high or model too complex

**Solution**:
1. Lower texture resolution (e.g., from 4K to 2K)
2. Optimize model to reduce polygon count
3. Compress textures (use DXT5 or BC7 format)
4. Remove unnecessary details

## Advanced Techniques

### Adding Animations

If your building contains animations (like a rotating windmill):
1. Create an Animator Controller, drag the animation into the controller
2. Add an Animator component to the prefab
3. Drag the Animator Controller into the Animator component

### Using LOD System

Mount detail culling scripts for large buildings:
1. Add BuildingDetailCulling script
2. Assign different LOD levels to the model
3. Set culling distances for different levels

### Custom Material Effects

Use Shader Graph to create special effects:
1. Create a Shader Graph asset
2. Design custom effects (like glowing windows)
3. Create a material using that shader
4. Ensure shader is compatible with target platform

## Conclusion

Thank you for your contributions to community content creation. I hope the game will have lasting vitality through your content!

I look forward to seeing your creations add more colorful possibilities to the game!

---

**Version**: 1.0  
**Last Updated**: 2026  
**Applicable To**: Unity 2022.3 LTS
