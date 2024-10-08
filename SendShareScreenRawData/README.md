
# Meeting SDK for Windows - Send ScreenShare Raw Data

A Windows C++ Application demonstrate Zoom Meeting SDK sending ScrnenShare raw data to a Zoom Meeting.

## Additional dependency libs.

This sample requires CURL and Json Parser
This sample also requires opencv

We are using vcpkg to install these dependencies on Windows

### Installing vcpkg
You might need to use Powershell (as administrator) or Windows Terminal to execute the sh script files
```
git clone https://github.com/Microsoft/vcpkg.git
cd vcpkg
./bootstrap-vcpkg.sh 
./vcpkg integrate install --vcpkg-root c:\vcpkg
```

### Installing cURL and JSON Parser using vckpg

`./vcpkg install curl`

`./vcpkg install jsoncpp`

### Installing opencv 

`./vcpkg install opencv`


## Add a configuration file named `config.json`

This is file which is used to pass in your sdk auth signature (aka jwt token, aka auth signature)

```
{
  "sdk_jwt": "<your_sdk_jwt>",
  "meeting_number": <meeting_number_to_join>,
  "passcode": "<passcode>",
  "video_source": "Big_Buck_Bunny_1080_10s_1MB.mp4",
  "zak":""
}
```

The app will try to join the meeting follow the Meeting Number you specified in the config.json. 

## Add the sdk files into a folder name `SDK`

The folder should look something like this

- SDK
	- x64
	- x86
	- CHANGELOG.MD
	- OSS-LICENSE.txt
	- README.md
	- version.txt



## Open and Run Project

Right click on "SendShareScreenRawData" and "Startup as Project" in Visual Studio


Hit F5 or click from menu "Debug" -> "Start Debugging" in x86 or x64 to launch the application.


## Error

if you are getting an error about not being able to open source json/json.h , include this in your

Visual Studio Project -> Properties. Under C/C++ ->General ->Additional Include Directories,


## Getting Started

The main method, or main entry point of this application is at `MSDK_xxxxxxxDemo.cpp`

From a high level point of view it will do the below.
- Join a meeting
- Wait for callback or status update. There are some prerequistes before you can get video raw data. The `prereqCheck()` method helps to check if you have fulfilled these requirements
  - `CanStartShare()` check if no one else is sharing screen, and there are no permissions blocking youf from doing so.
  - You need to be in-meeting. This is the status when you have fully joined a meeting.
- Create virtual_share_source, which is an implementation of `IZoomSDKShareSource`
	- Call `GetRawdataShareSourceHelper` to get `IZoomSDKShareSourceHelper` interface. This `IZoomSDKShareSourceHelper` is needed to assign the share source.
	- Call `setExternalShareSource(virtual_share_source)` to set the virtual share source.
- At this stage, the sharing should start to trigger the methods below.
  - In `VirtualShareSource.cpp`, `onStartSend()`, you can implement your own logic to send video raw data to the meeting. I'm using an additional method `PlayVideoFileToShare` which reads a video file, converts it to YUV420 buffer, and sends it via `sendShareFrame` method.
  

# Upgrading Guide

You will need to download the latest Meeting SDK Windows for c++ from marketplace.zoom.us

Replace the files in the folder `SDK` with those found in the downloaded files from marketplace.zoom.us

You will need to ensure any missing abstract classes are implemented etc... before you can compile and upgrade to a newer SDK version.

# Troubleshooting Guide

## Error

if you are getting an error about not being able to open source json/json.h , include this in your

Visual Studio Project -> Properties. Under C/C++ ->General ->Additional Include Directories,

 ### x64
 C:\yourpath\whereyouinstalled\vcpkg\packages\jsoncpp_x64-windows\include
 
 or

 ### x86
 C:\yourpath\whereyouinstalled\vcpkg\packages\jsoncpp_x86-windows\include
 
## openCV errors

  There are some errors about opencv being unable to find certain libraries?

  They should not affect the basic functionality of this sample, but you can do a list of additional opencv libraries to install
  ```
  ./cvpkg list opencv
  ```

  to install addition libraries, do something like this. [Warning]This might take around 1 hour, depending on the number of libraries you install
  ```
  ./vcpkg install opencv[contrib,ffmpeg,nonfree,opengl,openmp,world]
  ```
