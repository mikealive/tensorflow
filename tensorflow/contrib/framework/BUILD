# Description:
#   contains parts of TensorFlow that are experimental or unstable and which are not supported.

licenses(["notice"])  # Apache 2.0

exports_files(["LICENSE"])

package(default_visibility = [
    "//learning/brain:__subpackages__",
    "//tensorflow:__subpackages__",
])

load("//tensorflow:tensorflow.bzl", "cuda_py_test")
load("//tensorflow:tensorflow.bzl", "tf_custom_op_library")
load("//tensorflow:tensorflow.bzl", "tf_gen_op_wrapper_py")
load("//tensorflow:tensorflow.bzl", "tf_gen_op_libs")

py_library(
    name = "framework_py",
    srcs = [
        "__init__.py",
        "python/framework/__init__.py",
        "python/framework/checkpoint_utils.py",
        "python/framework/experimental.py",
        "python/framework/tensor_util.py",
        "python/ops/__init__.py",
        "python/ops/arg_scope.py",
        "python/ops/ops.py",
        "python/ops/prettyprint_ops.py",
        "python/ops/variables.py",
    ],
    data = [
        ":python/ops/_variable_ops.so",
    ],
    srcs_version = "PY2AND3",
    deps = [
        ":gen_variable_ops",
        "//tensorflow/contrib/util:util_py",
        "//tensorflow/python:array_ops",
        "//tensorflow/python:control_flow_ops",
        "//tensorflow/python:framework",
        "//tensorflow/python:framework_for_generated_wrappers",
        "//tensorflow/python:init_ops",
        "//tensorflow/python:io_ops",
        "//tensorflow/python:logging_ops",
        "//tensorflow/python:math_ops",
        "//tensorflow/python:platform",
        "//tensorflow/python:state_ops",
        "//tensorflow/python:state_ops_gen",
        "//tensorflow/python:tensor_array_ops",
        "//tensorflow/python:training",
        "//tensorflow/python:util",
        "//tensorflow/python:variable_scope",
        "//tensorflow/python:variables",
        "//third_party/py/numpy",
        "@six_archive//:six",
    ],
)

tf_custom_op_library(
    name = "python/ops/_variable_ops.so",
    srcs = [
        "kernels/zero_initializer_op.cc",
        "kernels/zero_initializer_op.h",
        "ops/variable_ops.cc",
    ],
    gpu_srcs = [
        "kernels/zero_initializer_op_gpu.cu.cc",
        "kernels/zero_initializer_op.h",
    ],
)

tf_gen_op_libs(
    op_lib_names = ["variable_ops"],
)

cc_library(
    name = "all_ops",
    deps = [
        ":variable_ops_op_lib",
    ],
)

tf_gen_op_wrapper_py(
    name = "gen_variable_ops",
    out = "python/ops/gen_variable_ops.py",
    deps = [
        ":variable_ops_op_lib",
    ],
)

py_test(
    name = "arg_scope_test",
    size = "small",
    srcs = ["python/ops/arg_scope_test.py"],
    srcs_version = "PY2AND3",
    deps = [
        ":framework_py",
        "//tensorflow/python:client_testlib",
        "//third_party/py/numpy",
    ],
)

py_test(
    name = "checkpoint_utils_test",
    size = "small",
    srcs = ["python/framework/checkpoint_utils_test.py"],
    srcs_version = "PY2AND3",
    tags = ["manual"],  # http://b/30468735
    deps = [
        ":framework_py",
        "//tensorflow/python:client_testlib",
        "//tensorflow/python:errors",
        "//tensorflow/python:framework_for_generated_wrappers",
        "//tensorflow/python:init_ops",
        "//tensorflow/python:partitioned_variables",
        "//tensorflow/python:training",
        "//tensorflow/python:variable_scope",
        "//tensorflow/python:variables",
        "//third_party/py/numpy",
    ],
)

py_test(
    name = "ops_test",
    size = "small",
    srcs = ["python/ops/ops_test.py"],
    srcs_version = "PY2AND3",
    deps = [
        ":framework_py",
        "//tensorflow/python:client_testlib",
        "//tensorflow/python:framework_for_generated_wrappers",
        "//third_party/py/numpy",
    ],
)

py_test(
    name = "prettyprint_ops_test",
    size = "small",
    srcs = ["python/ops/prettyprint_ops_test.py"],
    srcs_version = "PY2AND3",
    deps = [
        ":framework_py",
        "//tensorflow/python:client_testlib",
        "//tensorflow/python:framework",
        "//tensorflow/python:framework_for_generated_wrappers",
        "//tensorflow/python:sparse_ops",
        "//tensorflow/python:tensor_array_ops",
        "//tensorflow/python:variables",
        "//third_party/py/numpy",
    ],
)

py_test(
    name = "experimental_test",
    srcs = ["python/framework/experimental_test.py"],
    srcs_version = "PY2AND3",
    deps = [
        ":framework_py",
        "//tensorflow/python:client_testlib",
        "//tensorflow/python:platform",
        "//third_party/py/numpy",
    ],
)

py_test(
    name = "tensor_util_test",
    srcs = ["python/framework/tensor_util_test.py"],
    srcs_version = "PY2AND3",
    deps = [
        ":framework_py",
        "//tensorflow/python:array_ops",
        "//tensorflow/python:client_testlib",
        "//tensorflow/python:errors",
        "//tensorflow/python:framework",
        "//tensorflow/python:framework_for_generated_wrappers",
        "//tensorflow/python:variables",
        "//third_party/py/numpy",
    ],
)

py_test(
    name = "variables_test",
    size = "small",
    srcs = ["python/ops/variables_test.py"],
    srcs_version = "PY2AND3",
    tags = ["manual"],
    deps = [
        ":framework_py",
        "//tensorflow/python:array_ops",
        "//tensorflow/python:client",
        "//tensorflow/python:client_testlib",
        "//tensorflow/python:errors",
        "//tensorflow/python:framework_for_generated_wrappers",
        "//tensorflow/python:init_ops",
        "//tensorflow/python:nn_ops",
        "//tensorflow/python:platform",
        "//tensorflow/python:training",
        "//tensorflow/python:variable_scope",
        "//tensorflow/python:variables",
        "//third_party/py/numpy",
    ],
)

filegroup(
    name = "all_files",
    srcs = glob(
        ["**/*"],
        exclude = [
            "**/METADATA",
            "**/OWNERS",
        ],
    ),
    visibility = ["//tensorflow:__subpackages__"],
)
