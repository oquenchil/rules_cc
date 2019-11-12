# TODO: The rule does not yet report proper errors
# for the different test cases

load("//cc:defs.bzl", "cc_library")
load("//examples:experimental_cc_shared_library.bzl", "cc_shared_library")

# Not yet implemented.
# 1. Visibility with export_define
# 2. Free for all linked_statically_by with *.
#

# No headers here. Inspect symbols of the .so with `nm`.

cc_shared_library(
    name = "foo_so",
    dynamic_deps = ["bar_so"],
    visibility_file = "foo.lds",
    exports = [
        "foo",
        # Not a problem to export "baz" which is depended on by foo
        "baz",
        # Case3
        # This is fine. bar3 can be exported by "foo_so"
        # even though bar3 is linked statically by bar_so, since bar_so
        # doesn't export bar3 this is fine.
        "bar3",
        # Case4
        # This is fine. foo depends on bar4. Even though bar is
        # linked dynamically, when bar is pruned, we still have a dependency
        # edge to bar4 from foo. Also bar_so doesn't export bar4.
        #
        # Note that even though the target claims to export bar4. Unless the version script is
        # changed, the symbols from bar4 won't be exported.
        #
        # (should also uncomment the dependency from foo to bar4)
        "bar4",
    ],
)

cc_library(
    name = "foo",
    srcs = ["foo.cc"],
    linked_statically_by = ["//examples/test_cc_shared_library:foo_so"],
    deps = [
        # Add headers so that we can use from foo.
        "bar",
        "baz",
        # Not exported.
        "qux",
        # Case2
        # "bar2"
        # Case4
        # "bar4"
        # Case5: error, quux does not have statically linked by foo_so and is not exported
        # by bar_so.
        # "quux",
    ],
)

cc_library(
    name = "baz",
    srcs = ["baz.cc"],
    linked_statically_by = ["//examples/test_cc_shared_library:foo_so"],
)

cc_library(
    name = "qux",
    srcs = ["qux.cc"],
    linked_statically_by = ["//examples/test_cc_shared_library:foo_so"],
)

cc_library(
    name = "quux",
    srcs = ["quux.cc"],
)

cc_shared_library(
    name = "bar_so",
    visibility_file = "bar.lds",
    exports = [
        "bar",
        "bar2",
    ],
)

cc_library(
    name = "bar",
    srcs = ["bar.cc"],
    hdrs = ["bar.h"],
    linked_statically_by = [
        "//examples/test_cc_shared_library:bar_so",
    ],
)

cc_library(
    name = "bar2",
    srcs = ["bar2.cc"],
    linked_statically_by = [
        "//examples/test_cc_shared_library:bar_so",
        "//examples/test_cc_shared_library:foo_so",
    ],
)

cc_library(
    name = "bar3",
    srcs = ["bar3.cc"],
    linked_statically_by = [
        "//examples/test_cc_shared_library:bar_so",
        "//examples/test_cc_shared_library:foo_so",
    ],
)

cc_library(
    name = "bar4",
    srcs = ["bar4.cc"],
    linked_statically_by = [
        "//examples/test_cc_shared_library:bar_so",
        "//examples/test_cc_shared_library:foo_so",
    ],
)
