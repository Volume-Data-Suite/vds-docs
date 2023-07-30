# Supported Plattforms

## Cross-Platform Support

Currently the following graphics APIs are supported:

- Windows (Vulkan and DirectX)
- macOS (Metal)
- Linux (Vulkan)
- Web (WebGPU)

## Web Plattform restrictions:

**_WebGL_** is currrently not supported due to significant performance reductions. Make sure that you use the latest version of your web browser and check its **_WebGPU_** support, if you want to use the web based version of VDS. The latest versions of Chromium based browsers like Google Chrome, Microsoft Edge and Chromium itself should work.

**_Direct file system access_** is currently [not possible](https://stackoverflow.com/questions/71017592/can-i-read-files-from-the-disk-by-using-webassembly-re-evaluated) with WebAssembly (WASM). Therefore files can only be opened via drag and drop or by downloading them from a URL and not all file types are supported. Files can only be exported as a download.

**_Gerneral Perfomance_** is better on native versions of VDS that run directly on the operating system and not inside a web browser. Main reason are missing [SIMD](https://en.wikipedia.org/wiki/Single_instruction,_multiple_data) like _f16c_ on x86 or _fp16_ on arm64 which are not available in WASM.
