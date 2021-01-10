Make a folder named something for SDK like `AndroidSDK`. Create a folder called cmdline-tools in it. Download and decompress SDK commandline-tools from https://dl.google.com/android/repository/commandlinetools-linux-6858069_latest.zip -o tools.zip in it. We will get another cmdline-tools folder.  
Rename it to respective version here `3.0`.

Then run below

```bash
cd AndroidSDK
cmdlime-tools/3.0/bin/sdkmanager --list --include_obsolete --verbose
cmdlime-tools/3.0/bin/sdkmanager --verbose "emulator" "system-images;android-25;default;arm64-v8a" "platforms;android-25" "platform-tools"
cmdlime-tools/3.0/bin/sdkmanager --licenses
cmdlime-tools/3.0/bin/avdmanager -v create avd -n Nougat -k "system-images;android-25;arm64-v8a"
emulator/emulator/emulator -avd Nougat
```

If we want to let sdkmanger create `.android` directory in custom directory instead of `$HOME`, then use `ANDROID_SDK_HOME`.  
Here is the [tree](https://tree.nathanfriend.io/?s=(%27options!(%27fancy!true~fullPath!false~trailingSlash!true)~L(%27L%27ANDROID_SDK_ROOTJ*cmdline-F7%20F%2F%7C%7BM%20like%203.0%7D7WbinCsdkGavdGB*Qr7QEHXrU*XE-F7adb7B*system-imageHdefaultCarm64-v8aC*VCx86_64C*VUB%27)~M!%271%27)W%207J**9androidBVJC7**Er7B*platformFtoolsGmanagerCHs79-257*J%5CnLsource!MversionQemulatoU79-307V...W*%20X9.ja%01XWVUQMLJHGFECB97*)
```
.
└── ANDROID_SDK_ROOT/
    ├── cmdline-tools/
    │   └── tools/|(version like 3.0)/
    │       └── bin/
    │           ├── sdkmanager
    │           ├── avdmanager
    │           └── ...
    ├── emulator/
    │   ├── emulator
    │   └── ...
    ├── platforms/
    │   ├── android-25/
    │   │   └── android.jar
    │   ├── android-30/
    │   │   └── android.jar
    │   └── ...
    ├── platform-tools/
    │   ├── adb
    │   └── ...
    └── system-images/
        ├── android-25/
        │   └── default/
        │       ├── arm64-v8a/
        │       │   └── ...
        │       └── x86_64/
        │           └── ...
        ├── android-30/
        └── ...
```
Finally folder structure should look like this `cmdline-tools/3.0/bin/sdkmanager`. If we use like this, we don't need to set ANDROID_SDK_ROOT([about](https://developer.android.com/studio/command-line/variables)). If we want that then it should be `$(dirname $(realpath cmdline-tools))`.
Finally my script should be look like below.
```bash
mkdir -p ~/AndroidSDK/cmdline-tools
cd ~/AndroidSDK/cmdline-tools
wget https://dl.google.com/android/repository/commandlinetools-linux-6858069_latest.zip -o tools.zip
unzip -q tools.zip
mv cmdline-tools 3.0
cd 3.0/bin
./sdkmanager --list --include_obsolete --verbose
./sdkmanager --verbose "emulator" "system-images;android-25;default;arm64-v8a" "platforms;android-25" "platform-tools"
./sdkmanager --licenses
./avdmanager -v create avd -n Nougat -k "system-images;android-25;default;arm64-v8a"
../../../emulator/emulator -avd Nougat
```
