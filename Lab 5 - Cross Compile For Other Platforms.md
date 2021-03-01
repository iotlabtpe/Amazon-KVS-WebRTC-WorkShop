---
layout: default
categories: [Lab]
tags: [Lab5]
excerpt_separator: <!--more-->
permalink: /lab/lab-5
name: /lab/lab-5.html
---

# Cross Compile For Other Platforms

Here we take an arm chip as a reference:

| Main Chip        | Ambarella S2L33M                |
| ---------------- | ------------------------------- |
| Toolchain        | linaro-armv7ahf-2015.11-gcc5.2  |
| External Library | Customer's middleware:libfbm.so |

In this case, we will use the AWS KVS WebRTC sample code (kvsWebRTCClientMaster.c) to integrate with middleware into this specified platform.

1. Download the latest AWS KVS WebRTC C SDK.
    ```bash
    cd ~
    git clone --recursive https://github.com/awslabs/amazon-kinesis-video-streams-webrtc-sdk-c.git
    cd amazon-kinesis-video-streams-webrtc-sdk-c
    ```

2. Use the following command to generate and build the target executable binary and libraries.
    ```bash
    mkdir build && cd build
    #Export the cross compiler 
    export CC=/path/to/toolchain/bin/arm-linux-gnueabihf-gcc
    export CXX=/path/to/toolchain/bin/arm-linux-gnueabihf-g++
    #Generate Makefile
    cmake .. -DBUILD_OPENSSL=TRUE -DBUILD_OPENSSL_PLATFORM=linux-generic32 -DBUILD_LIBSRTP_HOST_PLATFORM=x86_64-unknown-linux-gnu -DBUILD_LIBSRTP_DESTINATION_PLATFORM=arm-linux-gnueabihf
    #Build code
    make -j16
    ```
3. Run the sample code and verify in the KVS WebRTC console or [KVS WebRTC test page](https://awslabs.github.io/amazon-kinesis-video-streams-webrtc-sdk-js/examples/index.html).

    ```bash
    #Use the access key ID and secret access key you created above.
    #If you don't setup the region, the defualt region will be us-west-2.
    export AWS_DEFAULT_REGION=your_desired_region
    export AWS_SECRET_ACCESS_KEY=your_secret_access_key
    export AWS_ACCESS_KEY_ID=your_access_key_id
    #Optional, to configure debug level. The level is 1 (VERBOSE) t0 7 (SLIENT).
    export AWS_KVS_LOG_LEVEL=1

    #Make sure your system time is up-to-date. Or you should sync it manually.
    date

    cd ~/amazon-kinesis-video-streams-webrtc-sdk-c/build
    #To run Amazon KVS WebRTC stream local file to the viewer side
    ./kvsWebrtcClientMaster your_desired_channel_name
    ```

## Done

You are done with this workshop. Congrats!
