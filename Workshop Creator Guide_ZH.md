# 创意工坊建筑创作指南

欢迎你创作者！感谢你对创建自定义内容的兴趣!你的助理将为社区的繁荣贡献力量！本指南将帮助你创建符合规范的内容并上传到Steam创意工坊。

## 技术要求
上传内容依赖BuildingData、WorkshopUploadTool两个脚本，BuildingData用于填写建筑数据，WorkshopUploadTool用于上传。还需要拥有Steamworks.NET，如果需要更新Steamworks.NET可在Github页面：https://github.com/rlabrecque/Steamworks.NET/releases下载，将其导入Unity，在空对象上挂载Steam Manager脚本。
我已将上传所需的脚本与Steamworks.NET打包在XXUnity包文件里了，你可以使用这个包文件，也可单独下载：链接XX

### 建筑模型规范

> ⚠️ **必需组件提醒**：建筑预制体必须包含以下三个核心组件，缺少任何一个都会导致功能异常：
> - **Mesh Filter**：显示模型网格
> - **Mesh Renderer**：渲染模型（需要赋值材质）
> - **Collider**（Box Collider）：用于鼠标悬停高亮、点击检测和放置碰撞判定

### 支持的资源格式

- **模型**: FBX
- **纹理**: PNG
- **材质**: Unity URP Lit Shader

### 文件大小限制

- 单个建筑AssetBundle: ≤ 50MB
- 预览图: ≤ 1MB
- 总下载大小: ≤ 100MB

## 建筑设计指南

### 1. 尺寸规范

根据建筑占用的网格大小设计，建筑底部（地基）大小最小为长宽1米:
为了建筑的网格占用能够正确计算，建筑底部大小建议使用奇数正方形，如：1*1、3*3、5*5，不建议使用偶数正方形与非正方形，如：1*2、2*2、2*3、4*4。
高度: 无严格限制,但建议最矮不低于0.1米

### 2. 视觉风格

为保持游戏一致性,建议:
- 使用LowPoly风格
- 确保建筑的整体比例和比例关系（游戏默认比例参照为：人高0.1米）
- 使用等距视角友好的设计
- 避免过于细小的细节(在远距离看不清)
- 使用清晰的轮廓和颜色对比
- 照顾不同角度的细节

### 3. 性能优化建议

- 合理控制面数
- 透明材质(使用Alpha Test而非Alpha Blend)

## 创建流程

### 步骤1: 在3D软件中建模

1. 使用Blender, Maya, 3ds Max等创建模型
2. 确保模型原点在底部中心
以Blender为例：
✅ 正确: 建筑中心点在世界原点
（（图片1））
❌ 错误: 建筑偏移或旋转不对齐
（（图片2））
3. 应用所有变换(Scale, Rotation)
4. 导出为FBX格式

### 步骤2: 导入Unity

1. 将FBX拖入Unity项目
2. 将建筑预制体的Transform Position设为 (0, 0, 0)
2. 确保建筑底部在Y=0平面上
3. 建筑应朝向Z轴正方向

### 步骤3: 设置材质和纹理

1. 创建材质(使用URP Lit Shader)
2. 调整材质参数

### 步骤4: 添加必需组件

确保建筑预制体包含以下三个**必需组件**：

#### 1. Mesh Filter 和 Mesh Renderer

1. 选择建筑根对象
2. 确认已添加 **Mesh Filter** 组件（如果没有，点击 Add Component → Mesh Filter）
3. 确认已添加 **Mesh Renderer** 组件
4. 在 Mesh Renderer 中分配材质（使用 Standard 或 URP Lit Shader）

> ⚠️ **重要**：没有 Mesh Renderer 的建筑将无法显示，也无法触发鼠标悬停高亮效果！

#### 2. Collider

1. 选择建筑根对象
2. 添加 **Box Collider** 组件（推荐）或 **Mesh Collider**
3. 调整 Collider 大小覆盖整个建筑
4. 确保 Collider 不要超出建筑范围，超出后会导致与其他建筑产生碰撞而无法放置
（（图片3））

> ⚠️ **重要**：Collider 是鼠标悬停高亮和点击检测的必要条件！没有 Collider 的建筑无法被玩家选中或交互。

### 步骤5: 创建预制体

1. 将建筑拖入Project窗口创建Prefab
2. 命名规范: `{BuildingName}_Prefab`
3. 测试预制体可以正常实例化

### 步骤6: 创建 BuildingData

1. 右键 → **Create → City Builder → Building Data
2. 填写以下字段：

| **Building Name** | 建筑显示名称，可以使用名称或编号 | 如：Ferriswheel、SSmbl2 |
| **Building ID** | 唯一标识符，建议使用风格名称加编号，也可以使用纯编号 | 如：SunsetCity_01、SSmbl2 |
| **Style**（风格）⭐ | **决定在游戏建筑菜单哪个风格标签下显示**，建议使用你的开发者名称、包名或主题名称，同一系列建筑应填写相同值 | 如：Lyon'sPack、SunsetCity |
| **Category**（分类）⭐ | **决定在该风格下哪个分类栏显示**，可以使用游戏已有分类保证兼容（见下表），也可根据自己建筑分类填写 | 如：Large、Residential |
| **Grid Size** | 建筑占用网格大小 | 如：3x3 |
| **Building Prefab** | 拖入你的建筑预制体 | — |

**游戏内置分类参考**：

| `Large` | 大型 |
| `Medium` | 中型 |
| `Small` | 小型 |

> ⚠️ **Style 和 Category 字段非常重要**：它们会被打包进 `metadata.json`（由上传工具自动写入），游戏加载你的模组时根据这两个字段决定建筑在菜单中的位置。请务必填写，否则建筑可能无法正确分类显示。

3. 创建建筑图标（建议 128×128 PNG），拖入 **Icon** 字段

### 步骤7: 测试建筑

1. 在游戏中测试建筑:
   - 能否正确放置
   - 旋转是否正常
   - 碰撞检测是否正确
   - 视觉效果是否满意
2. 调整直到满意

### 步骤8: 准备上传

- 创建预览图（推荐600x600）
- 确认 BuildingData 中 **Style** 和 **Category** 已正确填写（上传工具会从中自动读取）
上传前检查:
   - [ ] 模型三角面数在限制内
   - [ ] 纹理分辨率合理
   - [ ] 包含 **Mesh Filter** 组件
   - [ ] 包含 **Mesh Renderer** 组件（已分配材质）
   - [ ] 包含 **Collider** 组件
   - [ ] 预制体可以正常实例化
   - [ ] 建筑对齐到网格
   - [ ] 没有缺失的材质或纹理
   - [ ] BuildingData所有字段已填写
   - [ ] 预览图清晰
   - [ ] 在游戏中测试通过

## 步骤9: 上传到工坊

> ⚠️ **上传分为两步完成**，因为 AssetBundle 打包与 Steam 上传对编辑器状态要求不同，必须分开操作。

### 第一步：打包资源（非 Play 模式）

1. 确保当前处于**非 Play 模式**（编辑器未运行）
2. 打开 **City Builder → Workshop Upload Tool**
3. 选择模组类型：**Building**
4. 拖入 **BuildingData**（工具会自动从中读取 Style 和 Category，可手动修改）
5. 拖入**建筑预制体**
6. 填写工坊信息（主要填写标题，描述可在创意工坊中编辑）
7. 点击 **"第一步：打包资源为 AssetBundle"**
8. 看到提示 **"打包完成！请进入 Play 模式后点击第二步上传"** 即为成功

> 💡 打包路径会自动保存，进入 Play 模式后不会丢失，无需重新填写信息。

### 第二步：上传到 Steam（Play 模式）

1. 点击 Unity 编辑器的 **运行（Play）** 按钮，等待 SteamManager 初始化
2. 工具窗口中确认 Steam 已初始化（无警告提示）
3. 点击 **"第二步：上传到 Steam 创意工坊"**
4. 等待上传完成，记录显示的**工坊条目 ID**（用于后续更新）

> 💡 上传工具会自动生成 `metadata.json` 并打包进 AssetBundle 内容目录，其中包含 Style 和 Category 信息，玩家订阅后游戏会自动正确分类显示。

### 步骤10: 发布和维护

1. 在Steam工坊页面完善信息
2. 添加更多截图

## 天空与地面材质注意事项
- 天空与地面shader需使用URP管线
- 地面shader尽量用程序化法线以应对网格区域动态大小调整

## 常见错误和解决方案

### 错误0: 打包时报错 "Building AssetBundles while in play mode is not allowed"

**原因**: 在 Play 模式下点击了打包按钮，而 AssetBundle 打包只能在非 Play 模式下执行

**解决方案**:
1. 退出 Play 模式（点击停止运行按钮）
2. 重新点击工具中的『第一步：打包资源为 AssetBundle』
3. 打包完成后再进入 Play 模式上传

### 错误1: 建筑显示为粉红色

**原因**: 材质或Shader缺失

**解决方案**:
1 确保所有纹理已正确分配
2. 检查材质是否包含在AssetBundle中

### 错误2: 建筑无法放置

**原因**: 缺少必需组件（Mesh Filter/Mesh Renderer/Collider）或网格大小设置错误

**解决方案**:
1. 确认建筑预制体包含 **Mesh Filter**、**Mesh Renderer** 和 **Collider** 三个组件
2. 检查 Mesh Renderer 是否已分配材质
3. 添加 Box Collider 到建筑根对象
4. 检查 BuildingData 的 Grid Size 设置
5. 确认建筑尺寸

### 错误3: 建筑位置偏移

**原因**: 预制体Transform设置不正确

**解决方案**:
1. 将预制体Position设为 (0, 0, 0)
2. 确保建筑底部在Y=0平面
3. 检查模型的Pivot Point位置

### 错误4: 旋转后建筑错位

**原因**: 建筑中心点不在网格中心

**解决方案**:
1. 在3D软件中调整模型原点
2. 或在Unity中调整子对象的位置
3. 确保建筑围绕中心点旋转

### 错误5: 文件过大无法上传

**原因**: 纹理分辨率过高或模型过于复杂

**解决方案**:
1. 降低纹理分辨率(如从4K降到2K)
2. 优化模型减少多边形数
3. 压缩纹理(使用DXT5或BC7格式)
4. 移除不必要的细节

## 进阶技巧

### 添加动画

如果你的建筑包含动画(如旋转的风车):
1. 创建动画控制器，将动画拖入控制器
2. 添加Animator组件到预制体
3. 将动画控制器拖入Animator组件

### 使用LOD系统

为大型建筑挂载细节剔除脚本:
1. 添加BuildingDetailCulling脚本
2. 为模型分配不同LOD级别
3. 设置不同级别剔除距离

### 自定义材质效果

使用Shader Graph创建特殊效果:
1. 创建Shader Graph资产
2. 设计自定义效果(如发光窗户)
3. 创建材质使用该Shader
4. 确保Shader兼容目标平台

## 结语

感谢你为社区内容的创造与付出，希望游戏能通过你的内容迸发出长久的生命力！

我期待看到你的创作为游戏增加更多丰富多彩的可能性!

---

**版本**: 1.0  
**最后更新**: 2026  
**适用于**: Unity 2022.3 LTS
