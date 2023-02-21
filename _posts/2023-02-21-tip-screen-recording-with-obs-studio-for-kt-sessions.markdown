---
layout: post
title:  "Tip: Screen Recording With OBS Studio For KT Sessions"
date:   2023-02-21 09:00:00 +0300
categories: jekyll update
---

# Installation

Download & install OBS Studio from https://obsproject.com/download

# Auto-Configuration Wizard

Select "Optimize just for recording" -> Select resolution, and 30 FPS would be sufficient -> Nest -> Apply Setting

# Main UI

You can find below sections that we'll configure some of them next:

"Scenes" "Sources" "Audio Mixer" "Scene Transitions" "Controls"

# Configure Video Capture

Under "Sources", click the add + button -> Choose Display Capture -> Choose the display you want to record -> OK.  

To verify, the OBS studio main UI should be able to show screen capture preview.

It should look like below:

![image](https://user-images.githubusercontent.com/17779623/220268095-c1dd849d-d617-4367-b806-db2c10a9490b.png)


# Configure Audio Capture

Mic:  
Under "Audio Mixer", select options button next to "Mic/Aux" -> Properties -> Device -> Choose your specific audio device instead of leaving as "Default device". Because "default device" can point to some other device later on.

![image](https://user-images.githubusercontent.com/17779623/220268164-80a9cd5d-a80c-46c3-96eb-9dd1e5b10737.png)

To verify, physically tap to your mic or do some sound checks and see if the volume meter moves up to yellow/red zone.

![image](https://user-images.githubusercontent.com/17779623/220268356-02e37e06-2004-4f60-bfc4-561007f94a3d.png)



Desktop Audio:  
It comes as added and enabled by default. Under the Audio Mixer it should show both "Desktop Audio" and "Mic/Aux" inputs similarly. Volume is also 100% by default. Nothing to do at least with the version as of writing (29).


# Test the setup

Try "Start Recording" Button, speak up for a few seconds and stop the recording. Go to "Videos" in windows explorer, you'll find a MKV file with timestamp as the filename, open it to verify the video and audio. Windows 11 seems to be able to open MKV files but VLC player can also be used to playback.

![image](https://user-images.githubusercontent.com/17779623/220268603-9245bcc2-ff78-498f-bbd6-802c9decb594.png)  ![image](https://user-images.githubusercontent.com/17779623/220268720-113801bf-578c-460e-81d8-42afe710d2a6.png)

