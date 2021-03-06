转自适应码流是指将视频转码并打包生成自适应码流输出文件的过程。它的特点是包含多个码率的音视频文件和一个描述性文件（manifest），播放器能够根据当前带宽，动态选择最合适的码率播放。目前应用最广泛的自适应码流格式，有 [HLS](https://developer.apple.com/streaming/) 和 [Dash](https://mpeg.chiariglione.org/standards/mpeg-dash)。

云点播的转自适应码流，同时支持 HLS 和 Dash 两种格式。使用该功能可以实现：
* 播放器根据当前带宽动态选择合适的码率播放，为观看者带来良好的体验。
* 主流播放器原生支持 HLS 和 Dash，无需定制播放器。

## <span id = "zsy"></span>转自适应码流模板

通过转自适应码流参数，可以控制自适应码流中各个子流的“视频转码参数”、“音频转码参数”等参数。云点播使用转自适应码流模板表示参数集合，通过转自适应码流模板，可以指定以下相关参数。

| 参数 | 说明 |
| -- | -- |
| 打包类型 | 自适应码流的格式，目前仅支持 HLS |
| 子流规格 | 控制输出多少个子流，以及各个子流的视频转码参数和音频转码参数：<li>视频转码参数：分辨率、码率、帧率、编码格式等</li><li>音频转码参数：采样频率、声道数、编码格式等</li> |
| 是否过滤“低分辨率转高分辨率” | 通常来说，低分辨率的原始视频转码高分辨率无法获得画质和音质的提升。开启过滤“低分辨率转高分辨率”，可以避免不必要的转码|

针对常见的参数组合，云点播提供了 [预置转自适应码流模板](https://intl.cloud.tencent.com/document/product/266/33932#preset-adaptive-bitrate-streaming-templates)，目前暂不支持自定义转自适应码流模板。

## 任务发起

发起转自适应码流任务，有“通过服务端 API 直接发起”，“通过控制台直接发起”和“上传时指定要执行的任务”三种方式。具体请参照视频处理的 [任务发起](https://intl.cloud.tencent.com/document/product/266/33931#OriginatingTask)。

以下是各种方式发起转自适应码流任务的说明：

* 调用服务端 API [ProcessMedia](#APIhttps://intL.cloud.tencent.com/document/product/266/33427) 发起任务：在请求中的`MediaProcessTask.AdaptiveDynamicStreamingTaskSet`参数指定 [转自适应码流模板](#zsy) 的模板 ID。
* 通过控制台对视频发起任务：调用 [服务端 API](#APIhttps://intl.cloud.tencent.com/document/product/266/33897) 创建任务流，任务流中配置转自适应码流任务（`MediaProcessTask.AdaptiveDynamicStreamingTaskSet`中指定）；在控制台使用该任务流 [发起视频处理](https://intl.cloud.tencent.com/document/product/266/33890)。
* 服务端上传时指定任务：调用 [服务端 API](#APIhttps://intl.cloud.tencent.com/document/product/266/33897) 创建任务流，任务流中配置转自适应码流任务（`MediaProcessTask.AdaptiveDynamicStreamingTaskSet`中指定）；[申请上传](#APIhttps://intl.cloud.tencent.com/document/api/266/31767#2.-.E8.BE.93.E5.85.A5.E5.8F.82.E6.95.B0) 中的`procedure`参数指定为该任务流。
* 客户端上传时指定任务：调用 [服务端 API](#APIhttps://intl.cloud.tencent.com/document/product/266/33897) 创建任务流，任务流中配置转自适应码流任务（`MediaProcessTask.AdaptiveDynamicStreamingTaskSet`中指定）；在 [客户端上传签名](https://intl.cloud.tencent.com/document/product/266/33922#.E7.AD.BE.E5.90.8D.E5.8F.82.E6.95.B0) 中的`procedure`指定该任务流。
* 控制台上传：调用 [服务端 API](#APIhttps://intl.cloud.tencent.com/document/product/266/33897) 创建任务流，任务流中配置转自适应码流任务（`MediaProcessTask.AdaptiveDynamicStreamingTaskSet`中指定）；通过控制台上传视频，选择[【上传的同时对视频进行处理操作】](https://intl.cloud.tencent.com/document/product/266/33890)并指定视频上传后执行该任务流。

## 结果获取

发起转自适应码流任务后，您可以通过异步等待 [结果通知](https://intl.cloud.tencent.com/document/product/266/33931#ResultNotification) 和同步进行 [任务查询](https://intl.cloud.tencent.com/document/product/266/33931#TaskQuery) 两种方式获取转自适应码流任务的执行结果。下面是发起转自适应码流任务后，普通回调方式下结果通知的示例（省略了值为 null 的字段）：

```json
{
    "EventType":"ProcedureStateChanged",
    "ProcedureStateChangeEvent":{
        "TaskId":"1256768367-Procedure-2e1af2456351812be963e309cc133403t0",
        "Status":"FINISH",
        "FileId":"5285890784246869930",
        "FileName":"动物世界",
        "FileUrl":"http://1256768367.vod2.myqcloud.com/xxx/xxx/AtUCmy6gmIYA.mp4",
        "MetaData":{
            "AudioDuration":60,
            "AudioStreamSet":[
                {
                    "Bitrate":383854,
                    "Codec":"aac",
                    "SamplingRate":48000
                }
            ],
            "Bitrate":1021028,
            "Container":"mov,mp4,m4a,3gp,3g2,mj2",
            "Duration":60,
            "Height":480,
            "Rotate":0,
            "Size":7700180,
            "VideoDuration":60,
            "VideoStreamSet":[
                {
                    "Bitrate":637174,
                    "Codec":"h264",
                    "Fps":23,
                    "Height":480,
                    "Width":640
                }
            ],
            "Width":640
        },
        "MediaProcessResultSet":[
            {
                "Type":"AdaptiveDynamicStreaming",
                "AdaptiveDynamicStreamingTask":{
                    "Status":"SUCCESS",
                    "ErrCode":0,
                    "Message":"",
                    "Input":{
                        "Definition":10
                    },
                    "Output":{
                        "Definition":10,
                        "Package":"hls",
                        "DrmType":"",
                        "Url":"http://1256768367.vod2.myqcloud.com/xxx/xxx/adp.10.m3u8"
                    }
                }
            },
            {
                "Type":"AdaptiveDynamicStreaming",
                "AdaptiveDynamicStreamingTask":{
                    "Status":"SUCCESS",
                    "ErrCode":0,
                    "Message":"",
                    "Input":{
                        "Definition":20
                    },
                    "Output":{
                        "Definition":20,
                        "Package":"dash",
                        "DrmType":"",
                        "Url":"http://1256768367.vod2.myqcloud.com/xxx/xxx/adp.20.mpd"
                    }
                }
            }
        ],
        "TasksPriority":0,
        "TasksNotifyMode":""
    }
}
```

回调结果中，`ProcedureStateChangeEvent.MediaProcessResultSet`有两个`Type`为`AdaptiveDynamicStreaming`类型的转自适应码流结果，`Definition`分别为10和20。
