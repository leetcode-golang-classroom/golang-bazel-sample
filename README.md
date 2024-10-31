# golang_bazel_sample

This repository is for demo how to use bazel to run golang test

[rule_go](https://github.com/bazel-contrib/rules_go.git)

## Setup steps

### 1. Requirements
1. 安裝 bazelisk 來執行 bazel
```shell
go install github.com/bazelbuild/bazelisk@latest
```
2. C/C++ toolchain(if using cgo)
3. Bash

### 2. Setup Workspace
1. 建立 "WORKSPACE" 放在 root 資料夾下 
```toml
load("@bazel_tools//tools/build_defs/repo:http.bzl", "http_archive")

http_archive(
    name = "io_bazel_rules_go",
    integrity = "sha256-M6zErg9wUC20uJPJ/B3Xqb+ZjCPn/yxFF3QdQEmpdvg=",
    urls = [
        "https://mirror.bazel.build/github.com/bazelbuild/rules_go/releases/download/v0.48.0/rules_go-v0.48.0.zip",
        "https://github.com/bazelbuild/rules_go/releases/download/v0.48.0/rules_go-v0.48.0.zip",
    ],
)

load("@io_bazel_rules_go//go:deps.bzl", "go_register_toolchains", "go_rules_dependencies")

go_rules_dependencies()

go_register_toolchains(version = "1.23.1")
```
2. 建立 BUILD.bazel 來設定 bazel 建制的相關檔案
```toml
load("@io_bazel_rules_go//go:def.bzl", "go_binary", "go_library", "go_test")

go_binary(
    name = "main",
    srcs = ["cmd/main.go"],
    deps = ["greeter"],
)

go_library(
    name = "greeter",
    importpath = "github.com/leetcode-golang-classroom/golang-bazel-sample/internal/greeter",
    srcs = ["internal/greeter/greeter.go"],
)

go_test(
    name = "greeter_test",
    srcs = ["internal/greeter/greeter_test.go"],
    embed = ["greeter"],
)
```
我在這個檔案定義了 go_binary, go_library, go_test 三個 build task
可以依照以下指令
分別執行 build, test, run
1. 執行 main
```shell
bazelisk build :main
```
2. 測試 test
```shell
bazelisk test :greeter_test
```

