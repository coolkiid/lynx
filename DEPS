import os
import platform

system = platform.system().lower()
machine = platform.machine().lower()
machine = "x86_64" if machine == "amd64" else machine


deps = {
    'platform/android/gradle/wrapper/gradle-6.7.1-all.zip': {
        "type": "http",
        "url": "https://services.gradle.org/distributions/gradle-6.7.1-all.zip",
        "decompress": False,
    },
    'explorer/android/gradle/wrapper/gradle-6.7.1-all.zip': {
        "type": "http",
        "url": "https://services.gradle.org/distributions/gradle-6.7.1-all.zip",
        "decompress": False,
    },
    '../buildtools/ninja': {
        "type": "http",
        "url": {
            "linux": "https://github.com/ninja-build/ninja/releases/download/v1.11.1/ninja-linux.zip",
            "darwin": "https://github.com/ninja-build/ninja/releases/download/v1.11.1/ninja-mac.zip",
            "windows": "https://github.com/ninja-build/ninja/releases/download/v1.11.1/ninja-win.zip"
        }.get(system, None),
        "sha256": {
            "linux": "b901ba96e486dce377f9a070ed4ef3f79deb45f4ffe2938f8e7ddc69cfb3df77",
            "darwin": "482ecb23c59ae3d4f158029112de172dd96bb0e97549c4b1ca32d8fad11f873e",
            "windows": "524b344a1a9a55005eaf868d991e090ab8ce07fa109f1820d40e74642e289abc"
        }.get(system, None),
        "ignore_in_git": True,
        "condition": system in ['linux', 'darwin', 'windows']
    },
    '../buildtools/emsdk': {
        "type": "git",
        "url": "https://github.com/emscripten-core/emsdk.git",
        "commit": "2346baa7bb44a4a0571cc75f1986ab9aaa35aa03",
        "ignore_in_git": True,
        "condition": target == "tasm"
    },
    'config_emsdk': {
        "type": "action",
        "cwd": os.path.join(root_dir, "../buildtools/emsdk/"),
        "commands": [
            "./emsdk install latest",
            "./emsdk activate latest",
            ". ./emsdk_env.sh",
        ],
        "require": ['../buildtools/emsdk']
    },
    "../third_party/xhook": {
        'type': 'git',
        'url': 'https://github.com/iqiyi/xHook.git',
        'commit': 'e59285034feadfdd4ba9b65e1eea1d381da83ed3',
        "patches": os.path.join(root_dir, 'patches', 'xhook', '0001-Infra-Add-BUILD.gn-file-of-xhook.patch'),        
        "ignore_in_git": True,
    },    
    'copy_root_gn_config': {
        "type": "action",
        "cwd": "../",
        "commands": [
            "cp lynx/root.gn .gn",
        ],
    },
    'change_executable_permission': {
        "type": "action",
        "cwd": root_dir,
        "commands": [
            "chmod +x buildtools/ninja/ninja",
        ],
        "require": ["buildtools/ninja"],
        "condition": system in ['linux', 'darwin']
    },
    '../buildtools/gn': {
        "type": "http",
        "url": f"https://github.com/lynx-family/buildtools/releases/download/gn-cc28efe6/buildtools-gn-{system}-{machine}.tar.gz",
        "ignore_in_git": True,
        "condition": system in ['linux', 'darwin', 'windows']
    },
    '../buildtools/llvm': {
        "type": "http",
        'url': f"https://github.com/lynx-family/buildtools/releases/download/llvm-020d2fb7/buildtools-llvm-{system}-{machine}.tar.gz",
        "ignore_in_git": True,
        "decompress": True,
        "condition": system in ['linux', 'darwin'],
    },
    'update_gradle_local_properties': {
        "type": "action",
        "cwd": os.path.join(root_dir),
        "commands": [
            "python3 " + os.path.join(root_dir, "tools", "android_tools", "update_local_properties.py") + ' '
            "-f "
            f'{root_dir}/platform/android/local.properties '
            f'{root_dir}/explorer/android/local.properties '
            "-p "
            f'ndk.dir={os.environ.get("ANDROID_NDK", os.path.join(root_dir, "tools/android_tools/ndk") if target == "dev" else "")} '
            f'sdk.dir={os.environ.get("ANDROID_HOME", os.path.join(root_dir, "tools/android_tools/sdk") if target == "dev" else "")} '
            f'cmake.dir={root_dir}/../buildtools/cmake'
        ],
        "condition": system in ['linux', 'darwin']
    },
    '../buildtools/cmake': {
        "type": "http",
        "url": {
            "linux": f"https://cmake.org/files/v3.18/cmake-3.18.1-Linux-x86_64.tar.gz",
            "darwin": f"https://dl.google.com/android/repository/ba34c321f92f6e6fd696c8354c262c122f56abf8.cmake-3.18.1-darwin.zip"
        }.get(system, None),
        "ignore_in_git": True,
        "condition": system in ['linux', 'darwin']
    },
    '../buildtools/android_sdk_manager': {
        "type": "http",
        "url": {
            "darwin": "https://dl.google.com/android/repository/commandlinetools-mac-8512546_latest.zip",
            "linux": "https://dl.google.com/android/repository/commandlinetools-linux-8512546_latest.zip"
        }.get(system, None),
        "ignore_in_git": True,
        "condition": system in ['linux', 'darwin']
    },
    '../third_party/gyp': {
        "type": "git",
        "url": "https://chromium.googlesource.com/external/gyp",
        "commit": "9d09418933ea2f75cc416e5ce38d15f62acd5c9a",
        "ignore_in_git": True,
        "condition": system in ['linux', 'darwin', 'windows'],
    },
    '../build': {
        "type": "git",
        "url": "https://github.com/lynx-family/buildroot.git",
        "commit": "b74a2ad3759ed710e67426eb4ce8e559405ed63f",
        "ignore_in_git": True,
        "condition": system in ['linux', 'darwin', 'windows']
    },
    "../build/linux/debian_sid_amd64-sysroot": {
        "type": "http",
        "url": "https://commondatastorage.googleapis.com/chrome-linux-sysroot/toolchain/79a7783607a69b6f439add567eb6fcb48877085c/debian_sid_amd64_sysroot.tar.xz",
        "ignore_in_git": True,
        "condition": machine == "x86_64" and system == "linux",
        "require": ["../build"]
    },
    '../third_party/libcxx': {
        "type": "git",
        "url": "https://chromium.googlesource.com/external/github.com/llvm/llvm-project/libcxx",
        "commit": "64d36e572d3f9719c5d75011a718f33f11126851",
        "ignore_in_git": True,     
    },
    '../third_party/libcxxabi': {
        "type": "git",
        "url": "https://chromium.googlesource.com/external/github.com/llvm/llvm-project/libcxxabi",
        "commit": "9572e56a12c88c011d504a707ca94952be4664f9",
        "ignore_in_git": True,
    },
    '../third_party/googletest': {
        'type': 'git',
        'url': 'https://github.com/google/googletest.git',
        'commit': '04cf2989168a3f9218d463bea6f15f8ade2032fd',
        'ignore_in_git': True,
    },
    '../third_party/checkstyle/checkstyle.jar':{
        "type": "http",
        "url": "https://github.com/checkstyle/checkstyle/releases/download/checkstyle-9.2.1/checkstyle-9.2.1-all.jar",
        "ignore_in_git": True,
        "decompress": False,
        "condition": system in ['linux', 'darwin'],
    },    
    '../third_party/zlib': {
        'type': 'git',
        'url': 'https://chromium.googlesource.com/chromium/src/third_party/zlib',
        'commit': 'f5fd0ad2663e239a31184ad4c9919991dda16f46',
        "patches": os.path.join(root_dir, 'patches', 'zlib', '0001-Adapt-zlib-to-the-Lynx-project.patch'),         
        "ignore_in_git": True,
    },
    'third_party/NativeScript/include': {
        "type": "http",
        "url": "https://github.com/lynx-family/v8-build/releases/download/11.1.277.1/v8_include_headers.zip",
        "ignore_in_git": True,
        "condition": system in ['darwin']
    },
    'third_party/NativeScript/lib/macOS': {
        "type": "http",
        "url": "https://github.com/lynx-family/v8-build/releases/download/11.1.277.1/v8_lib_macOS.zip",
        "ignore_in_git": True,
        "condition": system in ['darwin']
    },
    'third_party/v8/11.1.277/include': {
        "type": "http",
        "url": "https://github.com/lynx-family/v8-build/releases/download/11.1.277.1/v8_include_headers.zip",
        "ignore_in_git": True,
    },
    '../third_party/jacoco':{
        "type":"http",
        "url":f"https://repo1.maven.org/maven2/org/jacoco/jacoco/0.8.12/jacoco-0.8.12.zip",
        "sha256": "35cf2a2b8937659ecbc8d0d3902dfa7f55b7ed2250e82424036a3e1d12462cd7",
        "ignore_in_git":True,
        "decompress":True,
        "condition": system in ['linux', 'darwin'],
    },
    "../third_party/benchmark": {
        'type': 'git',
        'url': 'https://github.com/google/benchmark.git',
        'commit': 'f91b6b42b1b9854772a90ae9501464a161707d1e',      
        "ignore_in_git": True,
    }, 
    "third_party/quickjs/src": {
        "type": "git",
        "url": "https://github.com/lynx-family/primjs.git",
        "commit": "ea4d2d5d6e5fa4d36d31af560f00fb1880f52fe7",
        "ignore_in_git": True,
    },
    ### AUTO GENERATED SCRIPT START
    'gen_feature_count': {
        "type": "action",
        "cwd": root_dir,
        "commands": [
            "python3 tools/feature_count/generate_feature_count.py",
        ],
    },
    'gen_error_code': {
        "type": "action",
        "cwd": root_dir,
        "commands": [
            "python3 tools/error_code/gen_error_code.py",
        ],
    },
    'gen_lynx_perfromance_entry': {
        "type": "action",
        "cwd": root_dir,
        "commands": [
            "python3 tools/performance/performance_observer/generate_performance_entry.py",
        ]
    },
    ### AUTO GENERATED SCRIPT END
    'explorer/darwin/ios/lynx_explorer/xctestrunner': {
        "type": "http",
        "url": "https://github.com/google/xctestrunner/releases/download/0.2.15/ios_test_runner.par",
        "decompress": False,
        "ignore_in_git": True,
        "condition": system in ['darwin'],
    },
    "./tools_shared": {
        "type": "solution",
        "url": "https://github.com/lynx-family/tools-shared.git",
        "commit": "f8df9cbb723a2ac40ae092b0de26415eb9154026",
        'deps_file': 'dependencies/DEPS',
        "ignore_in_git": True,
    },
}
