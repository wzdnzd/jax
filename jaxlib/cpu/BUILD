# Copyright 2018 The JAX Authors.
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#     https://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.

# JAX is Autograd and XLA

load(
    "//jaxlib:jax.bzl",
    "nanobind_extension",
)

licenses(["notice"])

package(
    default_applicable_licenses = [],
    default_visibility = ["//jax:internal"],
)

# LAPACK

cc_library(
    name = "lapack_kernels",
    srcs = ["lapack_kernels.cc"],
    hdrs = ["lapack_kernels.h"],
    copts = ["-fexceptions"],
    features = ["-use_header_modules"],
    deps = [
        "//jaxlib:ffi_helpers",
        "@com_google_absl//absl/algorithm:container",
        "@com_google_absl//absl/base:dynamic_annotations",
        "@com_google_absl//absl/status:statusor",
        "@com_google_absl//absl/types:span",
        "@xla//xla/ffi/api:c_api",
        "@xla//xla/ffi/api:ffi",
    ],
)

cc_library(
    name = "lapack_kernels_using_lapack",
    srcs = ["lapack_kernels_using_lapack.cc"],
    deps = [":lapack_kernels"],
    alwayslink = 1,
)

nanobind_extension(
    name = "_lapack",
    srcs = ["lapack.cc"],
    copts = [
        "-fexceptions",
        "-fno-strict-aliasing",
    ],
    enable_stub_generation = False,  # generated stubs are incorrect
    features = ["-use_header_modules"],
    module_name = "_lapack",
    pytype_srcs = [
        "_lapack/__init__.pyi",
        "_lapack/schur.pyi",
        "_lapack/svd.pyi",
        "_lapack/eig.pyi",
    ],
    deps = [
        ":lapack_kernels",
        "//jaxlib:kernel_nanobind_helpers",
        "@com_google_absl//absl/base",
        "@nanobind",
        "@xla//xla/ffi/api:ffi",
    ],
)

cc_library(
    name = "cpu_kernels",
    srcs = ["cpu_kernels.cc"],
    visibility = ["//visibility:public"],
    deps = [
        ":lapack_kernels",
        ":lapack_kernels_using_lapack",
        ":sparse_kernels",
        "@xla//xla/ffi/api:c_api",
        "@xla//xla/ffi/api:ffi",
    ],
    alwayslink = 1,
)

cc_library(
    name = "sparse_kernels",
    srcs = ["sparse_kernels.cc"],
    hdrs = ["sparse_kernels.h"],
    deps = [
        "@eigen_archive//:eigen3",
        "@xla//xla/ffi/api:ffi",
    ],
)

nanobind_extension(
    name = "_sparse",
    srcs = ["sparse.cc"],
    copts = [
        "-fexceptions",
        "-fno-strict-aliasing",
    ],
    enable_stub_generation = False,
    features = ["-use_header_modules"],
    module_name = "_sparse",
    pytype_srcs = [
        "_sparse/__init__.pyi",
    ],
    deps = [
        ":sparse_kernels",
        "//jaxlib:kernel_nanobind_helpers",
        "@com_google_absl//absl/base",
        "@nanobind",
        "@xla//xla/ffi/api:ffi",
    ],
)
