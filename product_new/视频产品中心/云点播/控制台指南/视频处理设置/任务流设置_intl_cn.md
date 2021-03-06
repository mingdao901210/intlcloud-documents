## 操作场景
任务流设置可以通过创建的模版，流程化地对视频进行转码、添加水印、截图及视频审核等操作。

## 操作步骤
1. 进入 [云点播控制台](https://console.cloud.tencent.com/vod/overview)，选择左侧导航栏的【视频处理设置】>【任务流设置】，进入“任务流设置”页面：
	- 任务流列表展示了任务流名称、创建时间及最后修改时间。
	- 任务流列表支持按照创建时间及最后修改时间排序。
	- 单击目标任务流操作栏的具体操作按钮，可对该任务流进行查看、编辑和删除操作。
2. 单击列表上方的【创建任务流】，进入“创建任务流”页面，进行**任务流模板**的配置：
	- 任务流名称：您可自定义任务流名称，仅支持中文、英文、数字、短横线加下划线（-_），长度不能超过20字符。
	- 任务类型配置：包括普通转码、极速高清转码、自适应码流、截图、截取封面、转动图及视频审核，至少需要选择一项才可以进行任务流模板的配置，详细请参见 [任务配置说明](#p1)。
3. 任务配置项完成后，单击【提交】，则创建任务流成功。

<span id = "p1"></span>**任务配置说明**

任务类型|是否支持预置或自定义模板|支持配置的模板
-|-|-
 普通转码|转码模板：<li>支持预置模板<br><li>支持自定义模板|<li>转码模板：在已创建好的模板列表中进行选择，每个任务配置支持添加一到多个转码模板。如果已有模板不符合使用要求，则可以在 [模板设置 - 转码模板](https://intl.cloud.tencent.com/document/product/266/14059#.E8.A7.86.E9.A2.91.E8.BD.AC.E7.A0.81.E6.A8.A1.E6.9D.BF) 中重新创建新的模板。<br><li>水印模板：每个转码模板支持添加水印。如果已有水印不符合使用要求，则可以在 [模板设置 - 水印模板](https://intl.cloud.tencent.com/document/product/266/14059#.E6.B0.B4.E5.8D.B0.E6.A8.A1.E6.9D.BF) 中重新创建新的模板。
  极速高清转码|转码模板：<li>支持预置模板<br><li>支持自定义模板|<li>转码模板：在已创建好的模板列表中进行选择，每个任务配置支持添加一到多个转码模板。如果已有模板不符合使用要求，则可以在 [模板设置 - 转码模板](https://intl.cloud.tencent.com/document/product/266/14059#.E6.9E.81.E9.80.9F.E9.AB.98.E6.B8.85.E6.A8.A1.E6.9D.BF) 中重新创建新的模板。<br><li>水印模板：每个转码模板支持添加水印。如果已有水印不符合使用要求，则可以在 [模板设置 - 水印模板](https://intl.cloud.tencent.com/document/product/266/14059#.E6.B0.B4.E5.8D.B0.E6.A8.A1.E6.9D.BF) 中重新创建新的模板。
  自适应码流|仅支持预置模板|<li>自适应码流模板：在已创建好的模板列表中进行选择，每个任务配置支持添加一到多个自适应码流模板。<br><li>水印模板：每个自适应码流模板支持添加水印。如果已有水印不符合使用要求，则可以在 [模板设置 - 水印模板](https://intl.cloud.tencent.com/document/product/266/14059#.E6.B0.B4.E5.8D.B0.E6.A8.A1.E6.9D.BF) 中重新创建新的模板。	
 截图|截图模板：<li>支持预置模板<br><li>支持自定义模板|<li>截图模板：包含时间点截图、采样截图和雪碧图截图的截图方式，每种截图方式只能选择对应方式下已配置好的模板，**时间点截图需要进行时间点的配置**。如果已有模板不符合使用要求，则可以在 [模板设置 - 截图模板](https://intl.cloud.tencent.com/document/product/266/14059#.E6.88.AA.E5.9B.BE.E6.A8.A1.E6.9D.BF) 中重新创建新的模板。<br><li>水印模板：每个转码模板最多可以支持添加四个水印。如果已有水印不符合使用要求，则可以在 [模板设置 - 水印模板](https://intl.cloud.tencent.com/document/product/266/14059#.E6.B0.B4.E5.8D.B0.E6.A8.A1.E6.9D.BF) 中重新创建新的模板。
 截取封面|截图模板：<li>支持预置模板<br><li>支持自定义模板|<li>截图模板：**仅支持时间点截图方式的截图模板**，采样时间点可选择时间偏移量或百分比设置。如果已有模板不符合使用要求，则可以在 [模板设置 - 截图模板](https://intl.cloud.tencent.com/document/product/266/14059#.E6.97.B6.E9.97.B4.E7.82.B9.E6.88.AA.E5.9B.BE) 中重新创建新的模板。<br><li>水印模板：每个转码模板最多可以支持添加四个水印。如果已有水印不符合使用要求，则可以在 [模板设置 - 水印模板](https://intl.cloud.tencent.com/document/product/266/14059#.E6.B0.B4.E5.8D.B0.E6.A8.A1.E6.9D.BF) 中重新创建新的模板。
转动图|转动图模板：<li>支持预置模板<br><li>支持自定义模板|转动图模板：支持添加多个转动图模板及**转动图时间段的配置**。如果已有模板不符合使用要求，则可以在 [模板设置 - 转动图模板](https://intl.cloud.tencent.com/document/product/266/14059#.E8.BD.AC.E5.8A.A8.E5.9B.BE.E6.A8.A1.E6.9D.BF) 中重新创建新的模板。
视频审核|审核模板：<li>支持预置模板<br><li>支持自定义模板|审核模板：仅支持添加1个审核模板。如果已有模板不符合使用要求，则可以在 [模板设置 - 审核模板](https://intl.cloud.tencent.com/document/product/266/14059#.E5.AE.A1.E6.A0.B8.E6.A8.A1.E6.9D.BF) 中重新创建新的模板。

>任务流仅支持选择配置好的模板。
