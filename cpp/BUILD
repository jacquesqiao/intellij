licenses(["notice"])  # Apache 2.0

load(
    "//build_defs:build_defs.bzl",
    "intellij_plugin",
    "merged_plugin_xml",
    "stamped_plugin_xml",
)
load(
    "//testing:test_defs.bzl",
    "intellij_integration_test_suite",
    "intellij_unit_test_suite",
)

java_library(
    name = "cpp",
    srcs = glob(
        ["src/**/*.java"],
    ),
    visibility = ["//visibility:public"],
    deps = [
        "//base",
        "//common/experiments",
        "//intellij_platform_sdk:plugin_api",
        "//sdkcompat",
        "//third_party/auto_value",
        "@jsr305_annotations//jar",
    ],
)

filegroup(
    name = "plugin_xml",
    srcs = ["src/META-INF/blaze-cpp.xml"],
    visibility = ["//visibility:public"],
)

intellij_unit_test_suite(
    name = "unit_tests",
    srcs = glob(["tests/unittests/**/*.java"]),
    test_package_root = "com.google.idea.blaze.cpp",
    deps = [
        ":cpp",
        "//base",
        "//base:unit_test_utils",
        "//intellij_platform_sdk:plugin_api_for_tests",
        "//sdkcompat",
        "@jsr305_annotations//jar",
        "@junit//jar",
    ],
)

merged_plugin_xml(
    name = "merged_plugin_xml",
    srcs = [
        "//base:plugin_xml",
    ] + [
        ":plugin_xml",
    ],
)

stamped_plugin_xml(
    name = "cpp_plugin_xml",
    plugin_id = "com.google.idea.bazel.cpp",
    plugin_name = "com.google.idea.bazel.cpp",
    plugin_xml = ":merged_plugin_xml",
)

intellij_plugin(
    name = "cpp_integration_test_plugin",
    testonly = 1,
    plugin_xml = ":cpp_plugin_xml",
    deps = [
        ":cpp",
    ],
)

intellij_integration_test_suite(
    name = "integration_tests",
    srcs = glob(["tests/integrationtests/**/*.java"]),
    required_plugins = "com.google.idea.bazel.cpp",
    test_package_root = "com.google.idea.blaze.cpp",
    runtime_deps = [
        ":cpp_integration_test_plugin",
    ],
    deps = [
        ":cpp",
        "//base",
        "//base:integration_test_utils",
        "//base:unit_test_utils",
        "//intellij_platform_sdk:plugin_api_for_tests",
        "//sdkcompat",
        "@jsr305_annotations//jar",
        "@junit//jar",
    ],
)
