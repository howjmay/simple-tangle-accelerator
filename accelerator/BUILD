package(default_visibility = ["//visibility:public"])

load("@io_bazel_rules_docker//cc:image.bzl", "cc_image")

cc_binary(
    name = "accelerator",
    srcs = ["server.cc"],
    copts = ["-DLOGGER_ENABLE"],
    deps = [
        ":apis",
        ":ta_config",
        ":ta_errors",
        "@served",
    ],
)

cc_binary(
    name = "accelerator_microhttpd",
    srcs = ["main.c"],
    copts = ["-DLOGGER_ENABLE"],
    deps = [
        ":http",
        ":ta_config",
        ":ta_errors",
        "@entangled//utils/handles:signal",
    ],
)

cc_binary(
    name = "accelerator_mqtt",
    srcs = ["mqtt_interface.c"],
    copts = [
        "-DLOGGER_ENABLE",
        "-DENABLE_MQTT",
    ],
    deps = [
        ":ta_config",
        ":ta_errors",
        "//connectivity/mqtt:mqtt_utils",
        "@entangled//utils/handles:signal",
    ],
)

cc_image(
    name = "ta_image",
    binary = ":accelerator",
)

cc_library(
    name = "apis",
    srcs = ["apis.c"],
    hdrs = ["apis.h"],
    visibility = ["//visibility:public"],
    deps = [
        ":common_core",
        ":ta_errors",
        "//map:mode",
        "//serializer",
        "@entangled//common/model:bundle",
        "@entangled//common/trinary:trit_tryte",
        "@entangled//mam/api",
    ],
)

cc_library(
    name = "http",
    srcs = ["http.c"],
    hdrs = ["http.h"],
    visibility = ["//visibility:public"],
    deps = [
        ":apis",
        ":ta_config",
        ":ta_errors",
        "@libmicrohttpd",
    ],
)

cc_library(
    name = "common_core",
    srcs = ["common_core.c"],
    hdrs = ["common_core.h"],
    visibility = ["//visibility:public"],
    deps = [
        ":ta_config",
        ":ta_errors",
        "//request",
        "//response",
        "@com_github_uthash//:uthash",
        "@entangled//cclient/api",
        "@entangled//cclient/serialization:serializer",
        "@entangled//cclient/serialization:serializer_json",
        "@entangled//common/model:bundle",
        "@entangled//utils:time",
    ],
)

cc_library(
    name = "ta_config",
    srcs = ["config.c"],
    hdrs = ["config.h"],
    visibility = ["//visibility:public"],
    deps = [
        ":message",
        ":ta_errors",
        "//utils:cache",
        "//utils:pow",
        "@entangled//cclient/api",
    ],
)

cc_library(
    name = "ta_errors",
    hdrs = ["errors.h"],
    visibility = ["//visibility:public"],
)

cc_library(
    name = "message",
    srcs = ["message.c"],
    hdrs = ["message.h"],
)

cc_binary(
    name = "transfer",
    srcs = ["transfer.c"],
    copts = ["-DLOGGER_ENABLE"],
    deps = [
        ":apis",
        ":ta_errors",
    ],    
)

cc_binary(
    name = "recv_txn",
    srcs = ["recv_txn.c"],
    copts = ["-DLOGGER_ENABLE"],
    deps = [
        ":apis",
        ":ta_errors",
    ],    
)
