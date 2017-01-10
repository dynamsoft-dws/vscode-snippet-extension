# Dynamsoft Camera SDK for Windows: JavaScript SDK for Any Web Browsers
[Dynamsoft Camera SDK][1] provides JavaScript APIs that enable you to easily capture images and video streams from USB Video Class (UVC) compatible webcams from within a browser. With the browser-based webcam library, you can capture a live video stream into a container and grab an image with a couple lines of code in your web application.

## Screenshot

![embed webcam in HTML page](http://www.codepool.biz/wp-content/uploads/2016/12/dws-online-demo.PNG)

## Features
* Developers can have complete control over a webcam, e.g., exposure, iris, auto focus, etc.
* A Dynamsoft Webcam object consists of one video viewer and multiple image viewers. Image viewers can be dynamically created and destroyed.
* Supports embedding video stream in a browser.
* Supports image editing.
* Supports importing from DIB and exporting to base64 and DIB.
* Uploads images to an HTTP server. Both sync and async transfer mode are supported.

[More][2]

## Resources
* [Sample Code][3]
* [Release Notes][4]
* [Developer Center][5]

## How to Use the Extension
Use the prefix **dcs** to list the code snippets in **HTML** files.

![dcs vscode extension](http://www.codepool.biz/wp-content/uploads/2017/01/vscode-dcs.PNG)

## Sample Code: Hello World

```HTML
<!DOCTYPE html>
<html>

<head>
    <title>DCS Demo</title>
</head>

<body>
    <h1>Dynamsoft Camera SDK: Hello World</h1>
    <select size="1" id="source" width="220px" onchange="onSourceChanged()"></select>
    <input type="button" value="Capture" onclick="onCapture();" />

    <div id="video-container"> </div>
    <div id="image-container"> </div>

    <script type="text/javascript" src="http://www.dynamsoft.com/library/dcs/dynamsoft.camera.min.js">
    </script>
    <script>
        var dcsObject, imageViewer;
        var width = 480,
            height = 320;

        function onInitSuccess(videoViewerId, imageViewerId) {
            dcsObject = dynamsoft.dcsEnv.getObject(videoViewerId); // Get the Dynamsoft Webcam object
            dcsObject.videoviewer.setWidth(width);
            dcsObject.videoviewer.setHeight(height);
            imageViewer = dcsObject.getImageViewer(imageViewerId); // Get a specific image viewer
            imageViewer.ui.setWidth(width);
            imageViewer.ui.setHeight(height);
            var cameraList = dcsObject.camera.getCameraList(); // Get a list of available cameras
            for (var i = 0; i < cameraList.length; i++) {
                document.getElementById("source").options.add(new Option(cameraList[i], i));
            }

            if (cameraList.length > 0) {
                // Call this method to take the ownership, 
                // in case the listed camera is occupied by another Dynamsoft Webcam process.
                dcsObject.camera.takeCameraOwnership(cameraList[0]);
                dcsObject.camera.playVideo();
            } else {
                alert('No camera is connected.');
            }
        }

        function onInitFailure(errorCode, errorString) {
            alert('Init failed: ' + errorString);
        };

        function onSourceChanged() {
            if (!dcsObject) return;
            var source = document.getElementById("source");
            var camera = source.options[source.selectedIndex].text;
            dcsObject.camera.takeCameraOwnership(camera);
            dcsObject.camera.playVideo();
        };

        function onCapture() {
            if (!dcsObject) return;
            dcsObject.camera.captureImage('image-container');
            if (dcsObject.getErrorCode() !== EnumDWS_ErrorCode.OK) {
                alert('Capture error: ' + dcsObject.getErrorString());
            }
        };

        // Initialize Dynamsoft Webcam object
        dynamsoft.dcsEnv.init('video-container', 'image-container', onInitSuccess, onInitFailure);
        // Destroy Dynamsoft Webcam object when the page is closed
        window.onbeforeunload = function() {
            if (dcsObject) dcsObject.destroy();
        };
    </script>
</body>

</html>
```

[1]:http://www.dynamsoft.com/Products/dynamsoft-webcam-sdk.aspx
[2]:http://www.dynamsoft.com/Products/webcam-sdk-features.aspx
[3]:http://www.dynamsoft.com/Downloads/dynamsoft-webcam-sdk-sample-download.aspx
[4]:http://www.dynamsoft.com/Products/webcam-sdk-news.aspx
[5]:http://developer.dynamsoft.com/dws