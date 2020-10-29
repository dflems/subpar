# Utility for creating self-contained python executables.

load("@rules_python//python:defs.bzl", "py_library", "py_binary", "py_test")
load("//:subpar.bzl", "parfile")

package(default_visibility = ["//visibility:public"])

py_library(
    name = "compiler_lib",
    srcs = [
        "__init__.py",
        "cli.py",
        "error.py",
        "manifest_parser.py",
        "python_archive.py",
        "stored_resource.py",
        "//:__init__.py",
    ],
    data = [
        "//runtime:support",
    ],
    srcs_version = "PY2AND3",
)

py_library(
    name = "test_utils",
    srcs = ["test_utils.py"],
    srcs_version = "PY2AND3",
)

[py_binary(
    name = "compiler_" + version,
    srcs = ["compiler.py"],
    python_version = version,
    main = "compiler.py",
    srcs_version = "PY2AND3",
    deps = [":compiler_lib"],
) for version in ["PY2", "PY3"]]

alias(
    name = "compiler",
    actual = ":compiler_PY2",
)

# Compile the compiler while carefully avoiding a circular dependency
[parfile(
    name = "compiler_{}.par".format(version),
    src = ":compiler_" + version,
    compiler = ":compiler_" + version,
    default_python_version = version,
    main = "compiler.py",
) for version in ["PY2", "PY3"]]

alias(
    name = "compiler.par",
    actual = ":compiler_PY2.par",
)

[py_test(
    name = "%s_test" % (src_name,),
    size = "small",
    srcs = [src_name + "_test.py"],
    main = src_name + "_test.py",
    srcs_version = "PY2AND3",
    deps = [
        ":compiler_lib",
        ":test_utils",
    ],
) for src_name in [
    "cli",
    "manifest_parser",
    "python_archive",
    "stored_resource",
]]
