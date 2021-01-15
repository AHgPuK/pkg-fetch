# pkg-fetch

## Adding patches

1. Open `alexk111/node` repo
2. Create a `vXX.XX.XX-pkg` branch from a newer version commit (tagged with `vXX.XX.XX`)
3. Merge a previous patched `vXX.XX.XX-pkg` branch into the new one and resolve conflicts
4. Commit
5. Execute `git diff --output=node.patch --unified=5 --src-prefix=node/ --dst-prefix=node/ HEAD^ HEAD`
6. Copy the `node.patch` to `patches/node.vXX.XX.XX.cpp.patch` in this repo
7. Update `patches/patches.json` with the new version

## Building & Releasing Patched Node.js

On each OS:

1. `yarn start`
2. Upload the compiled binary file to Releases

## About

Github Releases page of this project contains base binaries,
used by `pkg` to create executables. `pkg-fetch` npm package
downloads base binaries or compiles them from source.

## Manual Building

Here is a guide to manually compile a nodejs binary from source. Usually pkg automatically compiles this binary if it doesn't find them in his resources but this process may fail and this is how to do it by your self. In this example I'm compiling nodejs 8 LTS using a device with Linux with architecture arm64 (to check the information about your device run the comand uname -a).

Install required build tools:

`sudo apt-get install build-essential`

Than clone node:

`git clone https://github.com/nodejs/node.git`

Checkout to the desired version:

`cd node git checkout v14.15.4`

Create the patch file inside the node dir and paste the content from the patch file you find on pkg-fetch github inside patch directory (https://raw.githubusercontent.com/zeit/pkg-fetch/master/patches/node.v8.11.3.cpp.patch)

`sudo nano node.v14.15.4.cpp.patch` (Ctrl+Maiusc+V - Ctrl+X - Y)

`git apply node.v14.15.4.cpp.patch`

`./configure`

`make -j4` (this takes many minutes, even hours in some devices)

Finally copy the binary:

`cp node ~/.pkg-cache/v2.6/fetched-v14.15.4-linux-arm64`
