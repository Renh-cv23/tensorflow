load("@llvm-project//mlir:tblgen.bzl", "gentbl_cc_library")
load("//tensorflow:tensorflow.bzl", "tf_cc_binary")

package(
    # copybara:uncomment default_applicable_licenses = ["//tensorflow:license"],
    default_visibility = [
        ":friends",
    ],
)

package_group(
    name = "friends",
    packages = [
        "//tensorflow/compiler/mlir/lite/...",
    ],
)

tf_cc_binary(
    name = "odml-converter",
    testonly = True,
    srcs = ["odml_converter_main.cc"],
    compatible_with = ["//buildenv/target:non_prod"],
    visibility = [
        "//tensorflow/compiler/mlir/lite/stablehlo/odml_converter:__subpackages__",
        "//third_party/odml/infra:__subpackages__",
    ],  # Prototype phase.
    deps = [
        ":outline_composites",
        "//tensorflow/compiler/mlir:init_mlir",
        "//tensorflow/compiler/mlir/lite:tensorflow_lite",
        "//tensorflow/compiler/mlir/lite/stablehlo:legalize_stablehlo_to_vhlo_pass",
        "//tensorflow/compiler/mlir/tensorflow:tensorflow_types",
        "@llvm-project//llvm:Support",
        "@llvm-project//mlir:ArithDialect",
        "@llvm-project//mlir:FuncDialect",
        "@llvm-project//mlir:MlirOptLib",
        "@llvm-project//mlir:Support",
        "@stablehlo//:chlo_ops",
        "@stablehlo//:stablehlo_ops",
    ],
)

cc_library(
    name = "outline_composites",
    srcs = [
        "transforms/outline_composites.cc",
    ],
    hdrs = ["passes.h"],
    compatible_with = ["//buildenv/target:non_prod"],
    deps = [
        ":passes_inc_gen",
        "@llvm-project//llvm:Support",
        "@llvm-project//mlir:BufferizationInterfaces",
        "@llvm-project//mlir:FuncDialect",
        "@llvm-project//mlir:IR",
        "@llvm-project//mlir:Pass",
        "@llvm-project//mlir:Support",
        "@llvm-project//mlir:TransformUtils",
        "@stablehlo//:chlo_ops",
        "@stablehlo//:stablehlo_ops",
    ],
    alwayslink = 1,
)

gentbl_cc_library(
    name = "passes_inc_gen",
    compatible_with = ["//buildenv/target:non_prod"],
    tbl_outs = [
        (
            [
                "-gen-pass-decls",
                "-name=ODMLConverter",
            ],
            "passes.h.inc",
        ),
    ],
    tblgen = "@llvm-project//mlir:mlir-tblgen",
    td_file = "passes.td",
    deps = ["@llvm-project//mlir:PassBaseTdFiles"],
)
