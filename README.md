# FFmpeg Transcoding Example

Develop a C command line application that utilizes the `FFmpeg` library, including `Opus Codec` for audio encoding support.
This application should be capable of:

1. Read a media file containing audio, specifically supporting G711 and AAC codecs.
2. Transcode the media file to Opus codec.
3. Write into an MP4 file.


## Knowledge

* There are two Opus encoders for FFmpeg:

    1. `opus`: This is the native Opus encoder that comes bundled with FFmpeg.
    2. `libopus`: This is the Opus encoder that uses the official Opus library.
    
    According to official documents [opus](https://ffmpeg.org/ffmpeg-codecs.html#opus) and [libopus](https://ffmpeg.org/ffmpeg-codecs.html#libopus-1),
    
    > This is a native FFmpeg encoder for the Opus format. Its quality is usually worse and at best is equal to the libopus encoder.
    
    we choose `libopus` in this project.

* Opus supports only 8000, 12000, 16000, 24000, and 48000 Hz sample rates. It will be resampled if the sample rate of the input audio is not in this list.


## Setup

1. Install required toolchains

    ```bash
    sudo apt install cmake yasm
    ```

2. Compile `Opus Codec` library

   1. Download source code from [here](https://archive.mozilla.org/pub/opus/opus-1.3.1.tar.gz) (see [Official Site](https://opus-codec.org/downloads/))

   2. Compile and install

       ```bash
       tar -zxvf opus-1.3.1.tar.gz
       cd opus-1.3.1
       ./configure --prefix=/path/to/ffmpeg-transcoding-example/3rdparty/opus
       make -j4
       make install
       ```

3. Compile `FFmpeg` library

    1. Download source code from [here](https://ffmpeg.org/releases/ffmpeg-snapshot.tar.bz2) (see [Official Site](https://ffmpeg.org/download.html))

    2. Compile and install

        ```bash
        tar -jxvf ffmpeg-snapshot.tar.bz2
        cd ffmpeg
        PKG_CONFIG_PATH=/path/to/ffmpeg-transcoding-example/3rdparty/opus/lib/pkgconfig \
            ./configure --prefix=/path/to/ffmpeg-transcoding-example/3rdparty/ffmpeg --enable-libopus
        make -j4
        make install
        ```

4. Check the codec of media files

    ```bash
    ffprobe ./inputs/sample1.alaw
    # Stream #0:0: Audio: pcm_alaw
   
    ffprobe ./inputs/sample2.aac
    # Stream #0:0: Audio: aac (LC), 48000 Hz
   
    ffprobe ./inputs/sample3.aac
    # Stream #0:0: Audio: aac (LC), 44100 Hz
    ```

5. Now your folder structure should be like

    ```
    - ffmpeg-transcoding-example
        - 3rdparty
            - ffmpeg
                - bin
                - include
                - lib
                - share
            - opus
                - include
                - lib
                - share
        - inputs
            - sample1.alaw
            - sample2.aac
            - sample3.aac
        - outputs
    ```


## Usage

1. Compile

    ```bash
    mkdir build
    cd build
    cmake ..
    make -j4
    ```

2. Run

    ```bash
    ./ffmpeg-transcoding-example ../inputs/sample1.alaw ../outputs/sample1.mp4
    ./ffmpeg-transcoding-example ../inputs/sample2.aac ../outputs/sample2.mp4
    ./ffmpeg-transcoding-example ../inputs/sample3.aac ../outputs/sample3.mp4
    ```

3. Validate

    ```bash
    ffprobe ../outputs/sample1.mp4
    # Stream #0:0(und): Audio: opus
   
    ffprobe ../outputs/sample2.mp4
    # Stream #0:0(und): Audio: opus
   
    ffprobe ../outputs/sample3.mp4
    # Stream #0:0(und): Audio: opus
    ```
