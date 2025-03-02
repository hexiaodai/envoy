load("@envoy//bazel:envoy_build_system.bzl", "envoy_package")
load("//bazel:kotlin_lib.bzl", "envoy_mobile_so_to_jni_lib")
load("//bazel:envoy_mobile_test_extensions.bzl", "TEST_EXTENSIONS")

licenses(["notice"])  # Apache 2

envoy_package()

# Library which contains all the JNI related targets and test extensions
cc_library(
    name = "envoy_jni_with_test_extensions_lib",
    testonly = True,
    deps = [
        ":server_envoy_jni_lib",
        "//library/common/jni:envoy_jni_lib",
    ] + TEST_EXTENSIONS,
)

# Library which contains all the JNI related targets.
cc_library(
    name = "server_envoy_jni_lib",
    testonly = True,
    srcs = [
        "test_jni_interface.cc",
    ],
    linkopts = select({
        "@envoy//bazel:dbg_build": ["-Wl,--build-id=sha1"],
        "//conditions:default": [],
    }),
    deps = [
        "//library/common/jni:envoy_jni_lib",
        "//test/common/integration:test_server_interface_lib",
    ],
    # We need this to ensure that we link this into the .so even though there are no code references.
    alwayslink = True,
)

# Base binary (.so) for testing
cc_binary(
    name = "libenvoy_jni_with_test_extensions.so",
    testonly = True,
    linkshared = True,
    deps = [
        ":envoy_jni_with_test_extensions_lib",
        "@envoy_mobile_extra_jni_deps//:extra_jni_dep",
    ],
)

## Targets for local execution
# macOS binary (.jnilib) for testing
envoy_mobile_so_to_jni_lib(
    name = "libenvoy_jni_with_test_extensions_jnilib",
    testonly = True,
    native_dep = "libenvoy_jni_with_test_extensions.so",
)
