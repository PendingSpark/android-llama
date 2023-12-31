# android-llama
Instruction to run llama models on Android phone. Based on llama.cpp project

## Requirements
- an Android device with 8GB of RAM or more. The more RAM you have, the bigger models you can run.
- Android 13 or higher
- termux app installed on your device
- models that can run with llama

## Compile llama.cpp from your phone

You can also compile it on your laptop and copy the binary and the model over to your phone and run by following this [instruction](https://github.com/ggerganov/llama.cpp?tab=readme-ov-file#android). But I dont have a rooted phone so I couldnt run the binary and have to compile llama directly on my phone.

The phone has a little ram so I have to find a small models to run on it, the one i found is https://huggingface.co/TheBloke/TinyLlama-1.1B-Chat-v0.3-GGUF. It's not great but at least it can run. 

Its good to play around with more models and see which one would work best.

Below are the steps to compile llama.cpp on your phone and run the model.

1. Install [termux](https://termux.dev/en/)
2. Clone [llama.cpp](https://github.com/ggerganov/llama.cpp)
In termux, run:
```
git clone https://github.com/ggerganov/llama.cpp.git
```
3. Download [Android NDK for termux](https://github.com/lzhiyong/termux-ndk)
```
wget https://github.com/lzhiyong/termux-ndk/releases/download/android-ndk/android-ndk-r26b-aarch64.zip

unzip android-ndk-r26b-aarch64.zip

export NDK=~/android-ndk-r26b
```

4. Compile llama.cpp
```
cd llama.cpp

mkdir build-android

cd build-android

cmake -DCMAKE_TOOLCHAIN_FILE=$NDK/build/cmake/android.toolchain.cmake -DANDROID_ABI=arm64-v8a -DANDROID_PLATFORM=android-23 -DCMAKE_C_FLAGS=-march=armv8.4a+dotprod ..

make
```

5. Go to `bin` folder to download the models and run llama 
```
cd bin

wget https://huggingface.co/TheBloke/TinyLlama-1.1B-Chat-v0.3-GGUF/resolve/main/tinyllama-1.1b-chat-v0.3.Q4_0.gguf

./main -m tinyllama-1.1b-chat-v0.3.Q4_0.gguf -p "hi there"
```

## Notes
1. If your phone doesnt have enough RAM, running ./main would likely crash the termux and the device 

2. Latest llama.cpp is running with ggul format so we would go to https://huggingface.co/TheBloke and find more models in that format that we would use.
