# streamvbyte 1.0.0 on loongarch64

## 结论：零补丁，走标量回退

上游含 x86 SSSE3 路径，但在 loongarch64 上 CMake 探测到 `CMAKE_SYSTEM_PROCESSOR: loongarch64` 后
自动走通用标量实现，不需要写 SIMD 移植。

## 构建

    cmake -B build -DCMAKE_INSTALL_PREFIX=$P -DCMAKE_BUILD_TYPE=Release .
    cmake --build build -j4 && cmake --install build

会有一条 CMake 4.4 的 `cmake_minimum_required` 弃用警告，靠环境变量
`CMAKE_POLICY_VERSION_MINIMUM=3.5`（`~/opt/tools/env.sh` 已设）全局化解，不影响构建。
产物：`installed/lib/libstreamvbyte.a`；链接期软链到 `installed/lib64/` 以匹配
`be/cmake/thirdparty.cmake` 写死的路径。

## 验证

已链接进 `doris_be`，0 undefined symbol。
