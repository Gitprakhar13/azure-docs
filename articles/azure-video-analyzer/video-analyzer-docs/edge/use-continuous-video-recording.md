---
title: Continuous video recording and playback tutorial - Azure Video Analyzer
description: In this tutorial, you'll learn how to use Azure Video Analyzer to continuously record video to the cloud and play back that recording.
ms.topic: tutorial
ms.date: 11/04/2021
ms.custom: ignite-fall-2021
---
# Tutorial: Continuous video recording and playback

[!INCLUDE [header](includes/edge-env.md)]

In this tutorial, you'll learn how to use Azure Video Analyzer to perform [continuous video recording](../continuous-video-recording.md) (CVR) to the cloud and play back that recording. This capability is useful for scenarios such as safety and compliance where there is a need to maintain an archive of the footage from a camera for days, weeks, months or even years or alternatively you could specify the retention period for the video being recorded. Retention policy defines how many days of video should be stored (for example, the last 7 days), you can learn more about it in the [Manage retention policy](../manage-retention-policy.md) article.

In this tutorial you will:

> [!div class="checklist"]
> * Set up the relevant resources.
> * Examine the code that performs CVR.
> * Run the sample code.
> * Examine the results, and view the video.

[!INCLUDE [quickstarts-free-trial-note](../../../../includes/quickstarts-free-trial-note.md)]

## Suggested pre-reading  

Read these articles before you begin:

* [Video Analyzer overview](../overview.md)
* [Video Analyzer terminology](../terminology.md)
* [Video Analyzer pipeline concepts](../pipeline.md) 
* [Continuous video recording scenarios](../continuous-video-recording.md)

## Prerequisites

Prerequisites for this tutorial are:
[!INCLUDE [prerequisites](./includes/common-includes/csharp-prerequisites.md)]

## Set up Azure resources

[![Deploy to Azure](https://aka.ms/deploytoazurebutton)](https://aka.ms/ava-click-to-deploy)
[!INCLUDE [resources](./includes/common-includes/azure-resources.md)]

## Concepts

As explained in [this](../pipeline.md) article, a video analyzer pipeline lets you define:

- Where media should be captured from.
- How it should be processed.
- Where the results should be delivered. 
 
 To accomplish CVR, you need to capture the video from an RTSP-capable camera and continuously record it to a [video resource](../terminology.md#video). This diagram shows a graphical representation of that pipeline. 

> [!div class="mx-imgBorder"]
> :::image type="content" source="../media/continuous-video-recording/continuous-video-recording-overview.svg" alt-text="Video Analyzer pipeline for CVR":::
<!-- ./media/continuous-video-recording-tutorial/continuous-video-recording-overview.svg -->

In this tutorial, you'll use an edge module built using the [Live555 Media Server](https://github.com/Azure/video-analyzer/tree/main/edge-modules/sources/rtspsim-live555) to simulate an RTSP camera. Inside the pipeline, you'll use an [RTSP source](../pipeline.md#rtsp-source) node to get the live feed and send that video to the [video sink node](../pipeline.md#video-sink), which records the video to your Video Analyzer account. The video that will be used in this tutorial is [a highway intersection sample video](https://lvamedia.blob.core.windows.net/public/camera-300s.mkv).

> [!VIDEO https://www.microsoft.com/en-us/videoplayer/embed/RE4LTY4]

In this tutorial, you will:

1. Setup your development environment.
1. Deploy the required edge modules.
1. Create and deploy the live pipeline.
1. Interpret the results.
1. Clean up resources.

## Set up your development environment


### Get the sample code

1. Clone the [AVA C# samples repository](https://github.com/Azure-Samples/video-analyzer-iot-edge-csharp).
1. Start Visual Studio Code, and open the folder where the repo has been downloaded.
1. In Visual Studio Code, browse to the src/cloud-to-device-console-app folder and create a file named **appsettings.json**. This file contains the settings needed to run the program.
1. Browse to the file share in the storage account created in the setup step above, and locate the **appsettings.json** file under the "deployment-output" file share. Click on the file, and then hit the "Download" button. The contents should open in a new browser tab, which should look like:

   ```
   {
       "IoThubConnectionString" : "HostName=xxx.azure-devices.net;SharedAccessKeyName=iothubowner;SharedAccessKey=XXX",
       "deviceId" : "avasample-iot-edge-device",
       "moduleId" : "avaedge"
   }
   ```

   The IoT Hub connection string lets you use Visual Studio Code to send commands to the edge modules via Azure IoT Hub. Copy the above JSON into the **src/cloud-to-device-console-app/appsettings.json** file.

### Connect to the IoT Hub

1. In Visual Studio Code, set the IoT Hub connection string by selecting the **More actions** icon next to the **AZURE IOT HUB** pane in the lower-left corner. Copy the string from the src/cloud-to-device-console-app/appsettings.json file.

    <!-- commenting out the image for now ![Set IoT Hub connection string]()./media/quickstarts/set-iotconnection-string.png-->
    [!INCLUDE [provide-builtin-endpoint](./includes/common-includes/provide-builtin-endpoint.md)]
1. In about 30 seconds, refresh Azure IoT Hub in the lower-left section. You should see the edge device `avasample-iot-edge-device`, which should have the following modules deployed:
    - Edge Hub (module name **edgeHub**)
    - Edge Agent (module name **edgeAgent**)
    - Video Analyzer (module name **avaedge**)
    - RTSP simulator (module name **rtspsim**)

### Prepare to monitor the modules

When you use run this quickstart or tutorial, events will be sent to the IoT Hub. To see these events, follow these steps:

1. Open the Explorer pane in Visual Studio Code, and look for **Azure IoT Hub** in the lower-left corner.
1. Expand the **Devices** node.
1. Right-click on `avasample-iot-edge-device`, and select **Start Monitoring Built-in Event Endpoint**.

    [!INCLUDE [provide-builtin-endpoint](./includes/common-includes/provide-builtin-endpoint.md)]

## Examine the sample files

In Visual Studio Code, browse to the src/cloud-to-device-console-app folder. Here you'll see the appsettings.json file that you created along with a few other files:

* **c2d-console-app.csproj**: The project file for Visual Studio Code.
* **operations.json**: This file lists the different operations that you would run.
* **Program.cs**: The sample program code, which:
    * Loads the app settings.
    * Invokes direct methods exposed by Video Analyzer edge module. You can use the module to analyze live video streams by invoking its [direct methods](direct-methods.md).
    * Pauses for you to examine the output from the program in the **TERMINAL** window and the events generated by the module in the **OUTPUT** window.
    * Invokes direct methods to clean up resources.

## Run the program 

1. In Visual Studio Code, go to src/cloud-to-device-console-app/operations.json.
1. Under the **pipelineTopologySet** node, edit the following:

    `"pipelineTopologyUrl" : "https://raw.githubusercontent.com/Azure/video-analyzer/main/pipelines/live/topologies/cvr-video-sink/topology.json" `
1. Next, under the **livePipelineSet** and **pipelineTopologyDelete** nodes, ensure that the value of **topologyName** matches the value of the **name** property in the above pipeline topology:

    `"topologyName" : "CVRToVideoSink"`  
1. Open the [pipeline topology](https://raw.githubusercontent.com/Azure/video-analyzer/main/pipelines/live/topologies/cvr-video-sink/topology.json) in a browser, and look at videoName - it is hard-coded to `sample-cvr-video`. This is acceptable for a tutorial. In production, you would take care to ensure that each unique RTSP camera is recorded to a video resource with a unique name.
1. Start a debugging session by selecting F5. You'll see some messages printed in the **TERMINAL** window.
1. The operations.json file starts off with calls to `pipelineTopologyList` and `livePipelineList`. If you've cleaned up resources after previous quickstarts or tutorials, this action returns empty lists as shown:

    ```
    --------------------------------------------------------------------------
    Executing operation pipelineTopologyList
    -----------------------  Request: pipelineTopologyList  --------------------------------------------------
    {
      "@apiVersion": "1.1"
    }
    ---------------  Response: pipelineTopologyList - Status: 200  ---------------
    {
      "value": []
    }
    --------------------------------------------------------------------------

    ```

1. After that the next set of direct method calls is made:
   * A call to `pipelineTopologySet` by using the previous `topologyUrl`
   * A call to `livePipelineSet` by using the following body
     
     ```
     {
       "@apiVersion": "1.1",
       "name": "Sample-Pipeline-1",
       "properties": {
         "topologyName": "CVRToVideoSink",
         "description": "Sample pipeline description",
         "parameters": [
           {
             "name": "rtspUrl",
             "value": "rtsp://rtspsim:554/media/camera-300s.mkv"
           },
           {
             "name": "rtspUserName",
             "value": "testuser"
           },
           {
             "name": "rtspPassword",
             "value": "testpassword"
           }
         ]
       }
     }
     ```
   * A call to `livePipelineActivate` to start the live pipeline and to start the flow of video and then pauses for you to select **Enter** in the **Terminal** window
1. The output in the **TERMINAL** window pauses now at a **Press Enter to continue** prompt. Do not select **Enter** at this time. Scroll up to see the JSON response payloads for the direct methods you invoked.
1. If you now switch over to the **OUTPUT** window in Visual Studio Code, you'll see messages being sent to IoT Hub by the Video Analyzer edge module. These messages are discussed in the following section.
1. The live pipeline continues to run and record the video. The RTSP simulator keeps looping the source video. To stop recording, go back to the **TERMINAL** window and select **Enter**. The next series of calls are made to clean up resources by using:

   * A call to `livePipelineDeactivate` to deactivate the live pipeline.
   * A call to `livePipelineDelete` to delete the live pipeline.
   * A second call to `livePipelineList` to show that the live pipeline is in the running state.
   * A call to `pipelineTopologyDelete` to delete the topology.
   * A final call to `pipelineTopologyList` to show that the list is now empty.

## Interpret the results

When you run the live pipeline, the Video Analyzer edge module sends certain diagnostic and operational events to the IoT Edge hub. These events are the messages you see in the **OUTPUT** window of Visual Studio Code. They contain a `body` section and an `applicationProperties` section. To understand what these sections represent, see [Create and read IoT Hub messages](../../../iot-hub/iot-hub-devguide-messages-construct.md).

In the following messages, the application properties and the content of the body are defined by the Video Analyzer edge module.


## Diagnostics events 

### MediaSession Established event

When the live pipeline is activated, the RTSP source node attempts to connect to the RTSP server running in the rtspsim module. If successful, it prints this event:

```
[IoTHubMonitor] [9:42:18 AM] Message received from [avasample-iot-edge-device/avaedge]:
{
  "body": {
    "sdp": "SDP:\nv=0\r\no=- 1586450538111534 1 IN IP4 XXX.XX.XX.XX\r\ns=Matroska video+audio+(optional)subtitles, streamed by the LIVE555 Media Server\r\ni=media/camera-300s.mkv\r\nt=0 0\r\na=tool:LIVE555 Streaming Media v2020.03.06\r\na=type:broadcast\r\na=control:*\r\na=range:npt=0-300.000\r\na=x-qt-text-nam:Matroska video+audio+(optional)subtitles, streamed by the LIVE555 Media Server\r\na=x-qt-text-inf:media/camera-300s.mkv\r\nm=video 0 RTP/AVP 96\r\nc=IN IP4 0.0.0.0\r\nb=AS:500\r\na=rtpmap:96 H264/90000\r\na=fmtp:96 packetization-mode=1;profile-level-id=4D0029;sprop-parameter-sets=XXXXXXXXXXXXXXXXXXXXXX\r\na=control:track1\r\n"
  },
  "applicationProperties": {
    "dataVersion": "1.0",
    "topic": "/subscriptions/{subscriptionID}/resourceGroups/{name}/providers/microsoft.media/videoanalyzers/{ava-account-name}",
    "subject": "/edgeModules/avaedge/livePipelines/Sample-Pipeline-1/sources/rtspSource",
    "eventType": "Microsoft.VideoAnalyzers.Diagnostics.MediaSessionEstablished",
    "eventTime": "2021-04-09T09:42:18.1280000Z"
  }
}
```

* The message is a Diagnostics event (**Microsoft.VideoAnalyzers.Diagnostics.MediaSessionEstablished**). It indicates that the RTSP source node (the subject) established a connection with the RTSP simulator and began to receive a (simulated) live feed.
* The subject section in applicationProperties references the node in the pipeline topology from which the message was generated. In this case, the message originates from the RTSP source node.
* The eventType section in applicationProperties indicates that this is a Diagnostics event.
* The eventTime section indicates the time when the event occurred.
* The body section contains data about the Diagnostics event, which in this case is the [SDP](https://en.wikipedia.org/wiki/Session_Description_Protocol) details.

## Operational events 

### RecordingStarted event

When the video sink node starts to record media, it emits this event of type **Microsoft.VideoAnalyzers.Pipeline.OperationalRecordingStarted**:

```
[IoTHubMonitor] [9:42:38 AM] Message received from [avasample-iot-edge-device/avaedge]:
{
  "body": {
    "outputType": "videoName",
    "outputLocation": "sample-cvr-video"
  },
  "applicationProperties": {
    "topic": "/subscriptions/{subscriptionID}/resourceGroups/{resource-group-name}/providers/microsoft.media/videoAnalyzers/{ava-account-name}",
    "subject": "/edgeModules/avaedge/livePipelines/Sample-Pipeline-1/sinks/videoSink",
    "eventType": "Microsoft.VideoAnalyzers.Pipeline.Operational.RecordingStarted",
    "eventTime": "2021-04-09T09:42:38.1280000Z",
    "dataVersion": "1.0"
  }
}
```

The subject section in applicationProperties references the video sink node in the live pipeline, which generated this message.

The body section contains information about the output location. In this case, it's the name of the Video Analyzer resource into which video is recorded.

### RecordingAvailable event

As the name suggests, the **RecordingStarted** event is sent when recording has started, but media data might not have been uploaded to the video resource yet. When the video sink node has uploaded media, it emits an event of type **Microsoft.VideoAnalyzers.Pipeline.Operational.RecordingAvailable**:

```
[IoTHubMonitor] [[9:43:38 AM] Message received from [ava-sample-device/avaedge]:
{
  "body": {
    "outputType": "videoName",
    "outputLocation": "sample-cvr-video"
  },
  "applicationProperties": {
    "topic": "/subscriptions/{subscriptionID}/resourceGroups/{resource-group-name}/providers/microsoft.media/videoAnalyzers/{ava-account-name}",
    "subject": "/edgeModules/avaedge/livePipelines/Sample-Pipeline-1/sinks/videoSink",
    "eventType": "Microsoft.VideoAnalyzers.Pipeline.Operational.RecordingAvailable",
    "eventTime": "2021-04-09T09:43:38.1280000Z",
    "dataVersion": "1.0"
  }
}
```

This event indicates that enough data was written to the video resource for players or clients to start playback of the video.

The subject section in applicationProperties references the video sink node in the live pipeline, which generated this message.

The body section contains information about the output location. In this case, it's the name of the Video Analyzer resource into which video is recorded.

### RecordingStopped event

When you deactivate the live pipeline, the video sink node stops recording media. It emits this event of type **Microsoft.VideoAnalyzers.Pipeline.Operational.RecordingStopped**:

```
[IoTHubMonitor] [11:33:31 PM] Message received from [avasample-iot-edge-device/avaedge]:
{
  "body": {
    "outputType": "videoName",
    "outputLocation": "sample-cvr-video"
  },
  "applicationProperties": {
    "topic": "/subscriptions/{subscriptionID}/resourceGroups/{resource-group-name}/providers/microsoft.media/videoAnalyzers/{ava-account-name}",
    "subject": "/edgeModules/avaedge/livePipelines/Sample-Pipeline-1/sinks/videoSink",
    "eventType": "Microsoft.VideoAnalyzers.Pipeline.Operational.RecordingStopped",
    "eventTime": "2021-04-10T11:33:31.051Z",
    "dataVersion": "1.0"
  }
}
```

This event indicates that recording has stopped.

The subject section in applicationProperties references the video sink node in the live pipeline, which generated this message.

The body section contains information about the output location, which in this case is the name of the Video Analyzer resource into which video is recorded.

## Playing back the recording

You can examine the Video Analyzer video resource that was created by the live pipeline by logging in to the Azure portal and viewing the video.

1. Open your web browser, and go to the [Azure portal](https://portal.azure.com/). Enter your credentials to sign in to the portal. The default view is your service dashboard.
1. Locate your Video Analyzers account among the resources you have in your subscription, and open the account pane.
1. Select **Videos** in the **Video Analyzer** section.
1. You'll find a video listed with the name `sample-cvr-video`. This is the name chosen in your pipeline topology file.
1. Select the video.
1. The video details page will open and the playback should start automatically.

[!INCLUDE [activate-deactivate-pipeline](./includes/common-includes/activate-deactivate-pipeline.md)]

## Clean up resources

[!INCLUDE [prerequisites](./includes/common-includes/clean-up-resources.md)]

## Next steps

* Use an [IP camera](https://en.wikipedia.org/wiki/IP_camera) with support for RTSP instead of using the RTSP simulator. You can search for IP cameras with RTSP support on the [ONVIF conformant products page](https://www.onvif.org/conformant-products/) by looking for devices that conform with profiles G, S, or T.
* Use an AMD64 or X64 Linux device (vs. using an Azure Linux VM). This device must be in the same network as the IP camera. Follow the instructions in [Install Azure IoT Edge runtime on Linux](../../../iot-edge/how-to-install-iot-edge.md). Then follow the instructions in the [Deploy your first IoT Edge module to a virtual Linux device](../../../iot-edge/quickstart-linux.md) quickstart to register the device with Azure IoT Hub.
