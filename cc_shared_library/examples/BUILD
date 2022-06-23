LINKABLE_MORE_THAN_ONCE = "LINKABLE_MORE_THAN_ONCE"

cc_binary(
    name = "binary",
    srcs = ["main.cc"],
    dynamic_deps = ["foo_shared"],
    deps = ["foo"],
)

cc_shared_library(
    name = "foo_shared",
    additional_linker_inputs = select({
        "@bazel_tools//src/conditions:linux": [
            ":foo.lds",
        ],
        "//conditions:default": [],
    }),
    dynamic_deps = ["bar_shared"],
    roots = [
        "baz",
        "foo",
    ],
    static_deps = [
        ":qux",
        "qux2",
    ],
    user_link_flags = select({
        "@bazel_tools//src/conditions:linux": [
            "-Wl,--version-script=$(location :foo.lds)",
        ],
        "//conditions:default": [],
    }),
)

cc_shared_library(
    name = "bar_shared",
    additional_linker_inputs = select({
        "@bazel_tools//src/conditions:linux": [":bar.lds"],
        "//conditions:default": [],
    }),
    exports_filter = [
        ":bar3",  # Exported transitive dependency
    ],
    roots = [
        "bar",
    ],
    static_deps = [
        "//examples:bar2",
        "//examples:qux2",
    ],
    user_link_flags = select({
        "@bazel_tools//src/conditions:linux": [
            "-Wl,--version-script=$(location :bar.lds)",
        ],
        "//conditions:default": [],
    }),
)

cc_library(
    name = "foo",
    srcs = [
        "foo.cc",
    ],
    hdrs = ["foo.h"],
    deps = [
        "bar",
        "baz",
        "qux",
        "qux2",
    ],
)

cc_library(
    name = "bar",
    srcs = ["bar.cc"],
    hdrs = ["bar.h"],
    deps = [
        "bar2",
        "qux2",
    ],
)

cc_library(
    name = "bar2",
    srcs = ["bar2.cc"],
    hdrs = ["bar2.h"],
    deps = ["bar3"],
)

cc_library(
    name = "baz",
    srcs = ["baz.cc"],
    hdrs = ["baz.h"],
    deps = ["bar3"],
)

cc_library(
    name = "bar3",
    srcs = ["bar3.cc"],
    hdrs = ["bar3.h"],
)

cc_library(
    name = "qux",
    srcs = ["qux.cc"],
    hdrs = ["qux.h"],
    linkstamp = "l.cc",
)

cc_library(
    name = "qux2",
    srcs = ["qux2.cc"],
    hdrs = ["qux2.h"],
    tags = [LINKABLE_MORE_THAN_ONCE],
)
