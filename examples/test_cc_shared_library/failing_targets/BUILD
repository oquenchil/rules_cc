load("//cc:defs.bzl", "cc_binary")
load("//examples:experimental_cc_shared_library.bzl", "cc_shared_library", "cc_shared_library_permissions")

package(
    default_visibility = ["//examples/test_cc_shared_library:__pkg__"],
)

cc_binary(
    name = "should_fail_binary",
    dynamic_deps = ["//examples/test_cc_shared_library:foo_so"],
    tags = [
        "manual",
        "nobuilder",
    ],
    deps = [
        "//examples/test_cc_shared_library:foo",
        "//examples/test_cc_shared_library:qux",
    ],
)

cc_shared_library(
    name = "permissions_fail_so",
    roots = [
        "//examples/test_cc_shared_library3:bar",
    ],
)

cc_shared_library_permissions(
    name = "permissions_fail",
    targets = [
        "//examples/test_cc_shared_library:foo",
    ],
)

cc_shared_library(
    name = "export_matched_by_filter_repeated_fail_so",
    dynamic_deps = ["export_matched_by_filter_repeated_fail_so_dep"],
    exports_filter = [
        ":bar",
    ],
    roots = [
        ":foo",
    ],
)

cc_library(
    name = "foo",
    srcs = ["foo.cc"],
    deps = ["bar"],
)

cc_shared_library(
    name = "export_matched_by_filter_repeated_fail_so_dep",
    roots = [
        ":bar",
    ],
)

cc_library(
    name = "bar",
    srcs = ["bar.cc"],
)
