# bazel-auto-maven-libs
Automatically and dynamically resolve maven dependencies for your bazel project

# What is this?
This tool uses @Johnynet's `[bazel-deps](https://github.com/johnynek/bazel-deps)` in order to resolve maven dependencies at build time. 

# But why?
So you won't have to figure out build is failing because you forgot to regenerate the dependecies. 

# But, but how the hell I gonna use it?
That's pretty straight-forward, here is an example:

`WORKSPACE`:
```python
load(
    "@bazel_tools//tools/build_defs/repo:http.bzl",
    "http_archive",
)

bazel_auto_maven_libs_version = 'master'
http_archive(
    name = "bazel_auto_maven_libs",
    strip_prefix = "bazel_auto_maven_libs-%s" % bazel_auto_maven_libs_version,
    urls = ["https://github.com/Reflexe/bazel-auto-maven-libs/archive/%s.zip" % bazel_auto_maven_libs_version],
)

load("@bazel_auto_maven_libs//:maven_libs.bzl", "maven_libs")

maven_libs(
  name = "third_party",
  artifacts = ["org.slf4j:slf4j-simple:1.8.0-beta4"]
)

load(
    "@third_party//:workspace.bzl",
    "maven_dependencies",
)

maven_dependencies()

```

`BUILD`:
```
java_library(
...
  deps = [
    "@third_party//org/slf4j:slf4j-simple"
  ]
)
```
