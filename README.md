# rapidsnark

rapid snark is a zkSnark proof generation written in C++. That generates proofs created in [circom](https://github.com/iden3/snarkjs) and [snarkjs](https://github.com/iden3/circom) very fast.

## dependencies

You should have installed gcc and cmake

In ubuntu:

````
sudo apt install build-essential
sudo apt install cmake
````

## compile prover for x86_64 host machine

````sh
git submodule init
git submodule update
./build_gmp.sh host
mkdir build_prover && cd build_prover
cmake .. -DCMAKE_BUILD_TYPE=Release -DCMAKE_INSTALL_PREFIX=../package
make -j4 && make install
````

## compile prover for macOS arm64 host machine

````sh
git submodule init
git submodule update
./build_gmp.sh host_noasm
mkdir build_prover && cd build_prover
cmake .. -DTARGET_PLATFORM=arm64_host -DCMAKE_BUILD_TYPE=Release -DCMAKE_INSTALL_PREFIX=../package
make -j4 && make install
````

## compile prover for linux arm64 host machine

````sh
git submodule init
git submodule update
./build_gmp.sh host
mkdir build_prover && cd build_prover
cmake .. -DTARGET_PLATFORM=arm64_host -DCMAKE_BUILD_TYPE=Release -DCMAKE_INSTALL_PREFIX=../package
make -j4 && make install
````

## compile prover for linux arm64 machine

````sh
git submodule init
git submodule update
./build_gmp.sh host
mkdir build_prover && cd build_prover
cmake .. -DTARGET_PLATFORM=aarch64 -DCMAKE_BUILD_TYPE=Release -DCMAKE_INSTALL_PREFIX=../package_aarch64
make -j4 && make install
````

## compile prover for Android

Install Android NDK from https://developer.android.com/ndk or with help of "SDK Manager" in Android Studio.

Set the value of ANDROID_NDK environment variable to the absolute path of Android NDK root directory.

Examples:

````sh
export ANDROID_NDK=/home/test/Android/Sdk/ndk/23.1.7779620  # NDK is installed by "SDK Manager" in Android Studio.
export ANDROID_NDK=/home/test/android-ndk-r23b              # NDK is installed as a stand-alone package.
````

Compilation:

````sh
git submodule init
git submodule update
./build_gmp.sh android
mkdir build_prover_android && cd build_prover_android
cmake .. -DTARGET_PLATFORM=ANDROID -DCMAKE_BUILD_TYPE=Release -DCMAKE_INSTALL_PREFIX=../package_android
make -j4 && make install
````

## compile prover for iOS

Install Xcode.

````sh
git submodule init
git submodule update
./build_gmp.sh ios
mkdir build_prover_ios && cd build_prover_ios
cmake .. -GXcode -DTARGET_PLATFORM=IOS -DCMAKE_BUILD_TYPE=Release -DCMAKE_INSTALL_PREFIX=../package_ios
````
Open generated Xcode project and compile prover.


## Using Docker

You might use the Dockerfile avialable on this repository for setting up the build environment and perform the compilation.

To create the docker image, execute:

`docker image build -t rapidsnark .`

The image compiles rapidsnark and libgpm for host x86_64.

You can create a new container and access to bash (source code available in /src directory).

`docker run -it rapidsnark`

Then you can extract the compiled files by executing the following docker command:

`docker cp <container_id>:/src/package rapidsnark_package`

## Building proof

You have a full prover compiled in the build directory.

So you can replace snarkjs command:

````sh
snarkjs groth16 prove <circuit.zkey> <witness.wtns> <proof.json> <public.json>
````

by this one
````sh
./package/bin/prover <circuit.zkey> <witness.wtns> <proof.json> <public.json>
````

## Benchmark

This prover parallelizes as much as it can the proof generation.

The prover is much faster that snarkjs and faster than bellman.

[TODO] Some comparative tests should be done.


## License

rapidsnark is part of the iden3 project copyright 2021 0KIMS association and published with GPL-3 license. Please check the COPYING file for more details.
