# Setup: Linux or Mac OS host, Android device, arm64 kernel

Prerequisites:
 - go1.8+ toolchain (can be downloaded from [here](https://golang.org/dl/))
 - Android NDK (tested with r15 on API24) (can be downloaded from [here](https://developer.android.com/ndk/downloads/index.html))
     + Set the `$NDK` environment variable to point at it
 - Android Serial Cable or [Suzy-Q](https://chromium.googlesource.com/chromiumos/platform/ec/+/master/docs/case_closed_debugging.md) device to capture console output is preferable but optional. syzkaller can work with normal USB cable as well, but that can be somewhat unreliable and turn lots of crashes into "lost connection to test machine" crashes with no additional info.

 - Build syzkaller

```sh
$ NDK=/path/to/android/ndk make TARGETOS=android TARGETARCH=arm64
```

 - Create config with `"type": "adb"` and specify adb devices to use. For example:
```
{
	"target": "linux/arm64",
	"http": "localhost:50000",
	"workdir": "/gopath/src/github.com/google/syzkaller/workdir",
	"syzkaller": "/gopath/src/github.com/google/syzkaller",
	"sandbox": "none",
	"procs": 8,
	"type": "adb",
	"vm": {
		"devices": ["ABCD000010"]
	}
}
```

 - Start `syz-manager -config adb.cfg` as usual.

If you get issues after `syz-manager` starts, consider running it with the `-debug` flag.
Also see [this page](troubleshooting.md) for troubleshooting tips and [Building a Pixel kernel with KASAN+KCOV](https://source.android.com/devices/tech/debug/kasan-kcov) for kernel build/boot instructions.
