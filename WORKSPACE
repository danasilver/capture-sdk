load("@bazel_tools//tools/build_defs/repo:http.bzl", "http_archive", "http_jar")

http_archive(
    name = "build_bazel_apple_support",
    sha256 = "cf4d63f39c7ba9059f70e995bf5fe1019267d3f77379c2028561a5d7645ef67c",
    url = "https://github.com/bazelbuild/apple_support/releases/download/1.11.1/apple_support.1.11.1.tar.gz",
)

load(
    "@build_bazel_apple_support//lib:repositories.bzl",
    "apple_support_dependencies",
)

apple_support_dependencies()

http_archive(
    name = "build_bazel_rules_apple",
    sha256 = "20da675977cb8249919df14d0ce6165d7b00325fb067f0b06696b893b90a55e8",
    url = "https://github.com/bazelbuild/rules_apple/releases/download/3.0.0/rules_apple.3.0.0.tar.gz",
)

http_archive(
    name = "rules_java",
    sha256 = "27abf8d2b26f4572ba4112ae8eb4439513615018e03a299f85a8460f6992f6a3",
    urls = [
        "https://github.com/bazelbuild/rules_java/releases/download/6.4.0/rules_java-6.4.0.tar.gz",
    ],
)

http_archive(
    name = "build_bazel_rules_swift",
    sha256 = "28a66ff5d97500f0304f4e8945d936fe0584e0d5b7a6f83258298007a93190ba",
    url = "https://github.com/bazelbuild/rules_swift/releases/download/1.13.0/rules_swift.1.13.0.tar.gz",
)

load(
    "@build_bazel_rules_swift//swift:repositories.bzl",
    "swift_rules_dependencies",
)

swift_rules_dependencies()

load(
    "@build_bazel_rules_swift//swift:extras.bzl",
    "swift_rules_extra_dependencies",
)

swift_rules_extra_dependencies()

RULES_RUST_VERSION = "0.49.1"

http_archive(
    name = "rules_rust",
    sha256 = "bcf710126f6974dc2db56d9ec5ec4c7c6c4e2c53b0728b44c35b770a48024bc9",
    urls = ["https://github.com/bazelbuild/rules_rust/releases/download/%s/rules_rust-v%s.tar.gz" % (RULES_RUST_VERSION, RULES_RUST_VERSION)],
)

load("@rules_rust//rust:repositories.bzl", "rules_rust_dependencies", "rust_register_toolchains", "rust_repository_set")

rules_rust_dependencies()

RUST_VERSION = "1.81.0"

rust_register_toolchains(
    extra_target_triples = [
        "aarch64-apple-ios-sim",
        "aarch64-apple-ios",
        "aarch64-linux-android",
        "armv7-linux-androideabi",
        "i686-linux-android",
        "x86_64-apple-ios",
        "x86_64-linux-android",
    ],
    rustfmt_version = "nightly/2024-09-09",
    # We need this shas, since these archives are generated by us and we want to make sure we always use these as opposed
    # to the official ones. There is a `rust_std_checksum.sh` script that generates these shas in the `tools` directory.
    # For security reasons, we include all shas here to make sure that a malicious actor with access to rust-std-mobile
    # can't include a compromised tool.
    #
    # tl;dr; run e.g. $ ./tools/rust_std_checksum.sh 1.80.0
    sha256s = {
        "rust-std-" + RUST_VERSION + "-aarch64-apple-ios-sim.tar.gz": "a4bdf0c2ecd0d899629db9ae0e940863da4047f379bec4a969df3f33dce911db",
        "rust-std-" + RUST_VERSION + "-aarch64-apple-ios.tar.gz": "09bd46b2b9297b61dd172da42c9455aaa1c85676715f6b8473b55612c1de10ed",
        "rust-std-" + RUST_VERSION + "-x86_64-apple-ios.tar.gz": "989852a7e82e2a3ad01473a9157ad92f8ff2ebce5300ecb4a716b5987be1bf6b",
        "rust-std-" + RUST_VERSION + "-aarch64-linux-android.tar.gz": "e8bc1c411e60cd8b290afe8bcc903e18ab904efe9782e491ce781f38a19647f0",
        "rust-std-" + RUST_VERSION + "-armv7-linux-androideabi.tar.gz": "84c15635750c1228366189aab6f95d26dcaed6069cac3c1fb3dad2f985952d8a",
        "rust-std-" + RUST_VERSION + "-i686-linux-android.tar.gz": "d205cddae43645e259eb4a1dbbbf6757b08e7408f48d339b5737d7773d749e0e",
        "rustc-" + RUST_VERSION + "-aarch64-apple-darwin.tar.gz": "9c3d36b8860011bddec580b12e4b6937aa0657b393e7aeb20f1dc90e743ffa7e",
        "cargo-" + RUST_VERSION + "-aarch64-apple-darwin.tar.gz": "bca2ed0f3b5dec19bd53b0a7d999eb1b495cd5ace69aafa088b44d83f44e3a8d",
        "llvm-tools-" + RUST_VERSION + "-aarch64-apple-darwin.tar.gz": "7b39fb1d477271d1232e704ef47ea5ec7d323c0cbe358c9a3986bdf6ce27ca55",
        "rust-std-" + RUST_VERSION + "-aarch64-apple-darwin.tar.gz": "44809c3b92c7500c64517151f1e3389b32913a35414553395104bc4a0ee35f69",
        "clippy-" + RUST_VERSION + "-aarch64-apple-darwin.tar.gz": "8666f5241c437f7074488c67819d80af8514ac280cc22973c1b15de3086d8dcc",
        "rustfmt-" + RUST_VERSION + "-aarch64-apple-darwin.tar.gz": "e17c1adda089e922376b2cf0ad3844c132cc1fb2225e1e59fe60af164974883a",
    },
    urls = [
        # NOTE: `urls` are technically mirrors so we want to make sure we always try our own first then the official ones.
        # We'll ensure that the ones we want to serve always come from us by the previous sha256s dictionary. Please ensure
        # that the extensions on both of these are the same.
        "https://github.com/bitdriftlabs/rust-std-mobile/releases/download/" + RUST_VERSION + "/{}.tar.gz",

        # We need this because we only serve std for mobile archs but rustc, clippy, cargo, llvm-tools and even std for
        # apple-darwin/linux are served from the official mirror.
        "https://static.rust-lang.org/dist/{}.tar.gz",
    ],
    versions = [
        RUST_VERSION,
    ],
)

# This is necessary in order to cross compile for darwin x86_64 from aarch64, done in order to provide x86_64
# dylibs in CI.
rust_repository_set(
    name = "rust_macos_x86_64_aarch64_tuple",
    edition = "2021",
    exec_triple = "aarch64-apple-darwin",
    extra_target_triples = ["x86_64-apple-darwin"],
    versions = [RUST_VERSION],
)

# This is necessary in order to cross compile for darwin aarch64 from x86_64, done in order to provide aarch64
# dylibs in CI.
rust_repository_set(
    name = "rust_macos_aarch64_x86_64_tuple",
    edition = "2021",
    exec_triple = "x86_64-apple-darwin",
    extra_target_triples = ["aarch64-apple-darwin"],
    versions = [RUST_VERSION],
)

http_archive(
    name = "rules_xcodeproj",
    integrity = "sha256-b+AKGo9kJFkcN52bTraVuIu6hKlTEe/Y+LAHkhXs29o=",
    url = "https://github.com/MobileNativeFoundation/rules_xcodeproj/releases/download/2.7.0/release.tar.gz",
)

load(
    "@rules_xcodeproj//xcodeproj:repositories.bzl",
    "xcodeproj_rules_dependencies",
)

xcodeproj_rules_dependencies()

load("@bazel_features//:deps.bzl", "bazel_features_deps")

bazel_features_deps()

load(
    "@build_bazel_rules_apple//apple:apple.bzl",
    "provisioning_profile_repository",
)

provisioning_profile_repository(
    name = "local_provisioning_profiles",
)

load("@rules_java//java:repositories.bzl", "rules_java_dependencies", "rules_java_toolchains")

rules_java_dependencies()

rules_java_toolchains()

apple_support_dependencies()

load(
    "@build_bazel_rules_apple//apple:repositories.bzl",
    "apple_rules_dependencies",
)

apple_rules_dependencies()

RULES_ANDROID_VERSION = "0.1.2"

RULES_ANDROID_NDK_SHA = "65aedff0cd728bee394f6fb8e65ba39c4c5efb11b29b766356922d4a74c623f5"

http_archive(
    name = "rules_android_ndk",
    sha256 = RULES_ANDROID_NDK_SHA,
    strip_prefix = "rules_android_ndk-%s" % RULES_ANDROID_VERSION,
    url = "https://github.com/bazelbuild/rules_android_ndk/releases/download/v%s/rules_android_ndk-v%s.tar.gz" % (RULES_ANDROID_VERSION, RULES_ANDROID_VERSION),
)

load("//bazel/android:configure.bzl", "android_configure")

android_configure(
    name = "local_config_android",
    build_tools_version = "34.0.0",
    # This value is the minimum supported Android sdk version.
    ndk_api_level = 21,
    # This is the target SDK version.
    sdk_api_level = 34,
)

load("@local_config_android//:android_configure.bzl", "android_workspace")

android_workspace()

load(
    "//bazel:capture_repositories.bzl",
    "capture_repositories",
)

capture_repositories()

load(
    "//bazel:capture_dependencies.bzl",
    "jvm_dependencies",
)

jvm_dependencies()

load(
    "//bazel:capture_tool_dependencies.bzl",
    "tool_dependencies",
)

tool_dependencies()

load("@SwiftLint//bazel:repos.bzl", "swiftlint_repos")

swiftlint_repos()

load("@SwiftLint//bazel:deps.bzl", "swiftlint_deps")

swiftlint_deps()

### Kotlin
load("@io_bazel_rules_kotlin//kotlin:repositories.bzl", "kotlin_repositories", "kotlinc_version")

_KOTLIN_COMPILER_VERSION = "1.9.24"

_KOTLIN_COMPILER_SHA = "eb7b68e01029fa67bc8d060ee54c12018f2c60ddc438cf21db14517229aa693b"

kotlin_repositories(
    compiler_release = kotlinc_version(
        release = _KOTLIN_COMPILER_VERSION,
        sha256 = _KOTLIN_COMPILER_SHA,
    ),
)

register_toolchains("//:kotlin_toolchain")

load("@rules_detekt//detekt:dependencies.bzl", "rules_detekt_dependencies")

rules_detekt_dependencies()

load("@rules_detekt//detekt:toolchains.bzl", "rules_detekt_toolchains")

rules_detekt_toolchains()

load("@rules_rust//tools/rust_analyzer:deps.bzl", "rust_analyzer_deps")

rust_analyzer_deps()

load("@bazel_tools//tools/build_defs/repo:git.bzl", "new_git_repository")

new_git_repository(
    name = "bitdrift_api",
    branch = "main",
    build_file = "//bazel:BUILD.bitdriftlabs_api",
    remote = "https://github.com/bitdriftlabs/api.git",
)

_RULES_ANDROID_VERSION = "0.1.1"

_RULES_ANDROID_SHA = "cd06d15dd8bb59926e4d65f9003bfc20f9da4b2519985c27e190cddc8b7a7806"

http_archive(
    name = "build_bazel_rules_android",
    sha256 = _RULES_ANDROID_SHA,
    strip_prefix = "rules_android-{}".format(_RULES_ANDROID_VERSION),
    urls = [
        "https://github.com/bazelbuild/rules_android/archive/v{}.zip".format(_RULES_ANDROID_VERSION),
    ],
)

http_archive(
    name = "robolectric",
    sha256 = "a270fd6fd83f9f024623e787696e6b73c44664b7c95f3d937ed35bf0a94a67ae",
    strip_prefix = "robolectric-bazel-4.13",
    urls = ["https://github.com/robolectric/robolectric-bazel/releases/download/4.13/robolectric-bazel-4.13.tar.gz"],
)

load("@robolectric//bazel:robolectric.bzl", "robolectric_repositories")

robolectric_repositories()

load("@rules_rust//crate_universe:repositories.bzl", "crate_universe_dependencies")

crate_universe_dependencies()

load("@rules_rust//crate_universe:defs.bzl", "crate", "crates_repository")

crates_repository(
    name = "crate_index",
    annotations = {
        "bd-grpc": [
            crate.annotation(
                build_script_env = {"SKIP_PROTO_GEN": "1"},
            ),
        ],
        "bd-pgv": [
            crate.annotation(
                build_script_env = {"SKIP_PROTO_GEN": "1"},
            ),
        ],
        "bd-proto": [
            crate.annotation(
                build_script_env = {"SKIP_PROTO_GEN": "1"},
            ),
        ],
        # A recent rustix update seems to have broken something here, so manually add in the crates we need to build under Bazel.
        "linux-raw-sys": [
            crate.annotation(
                crate_features = [
                    "errno",
                    "std",
                    "general",
                    "ioctl",
                ],
            ),
        ],
    },
    cargo_config = "//:Cargo.toml",
    cargo_lockfile = "//:Cargo.lock",
    # This trades the chance that the registry gets corrupted for speed when repinning.
    isolated = False,
    lockfile = "//:Cargo.Bazel.lock",
    manifests = [
        "//:Cargo.toml",
        "//proto:Cargo.toml",
        "//platform/jvm:Cargo.toml",
        "//test/platform/jvm:Cargo.toml",
        "//platform/shared:Cargo.toml",
        "//platform/swift/source:Cargo.toml",
        "//platform/test_helpers:Cargo.toml",
        "//test/platform/pom_checker:Cargo.toml",
        "//test/platform/swift/bridging:Cargo.toml",
        "//test/benchmark:Cargo.toml",
    ],
)

load(
    "@crate_index//:defs.bzl",
    cargo_remote_crate_repositories = "crate_repositories",
)

cargo_remote_crate_repositories()

http_jar(
    name = "bazel_diff",
    sha256 = "9c4546623a8b9444c06370165ea79a897fcb9881573b18fa5c9ee5c8ba0867e2",
    urls = [
        "https://github.com/Tinder/bazel-diff/releases/download/4.3.0/bazel-diff_deploy.jar",
    ],
)

http_archive(
    name = "SwiftBenchmark",
    build_file = "@//bazel/third_party:SwiftBenchmark.BUILD",
    sha256 = "9c5bccfbddaeed7d3aa731118644655c0e550ab2267e1a3238ca0daa06ade0f9",
    strip_prefix = "swift-benchmark-0.1.2",
    urls = ["https://github.com/google/swift-benchmark/archive/0.1.2.tar.gz"],
)

http_archive(
    name = "SwiftArgumentParser",
    build_file = "@//bazel/third_party:SwiftArgumentParser.BUILD",
    sha256 = "44782ba7180f924f72661b8f457c268929ccd20441eac17301f18eff3b91ce0c",
    strip_prefix = "swift-argument-parser-1.2.2",
    urls = ["https://github.com/apple/swift-argument-parser/archive/1.2.2.tar.gz"],
)

http_archive(
    name = "Difference",
    build_file = "@//bazel/third_party:Difference.BUILD",
    sha256 = "3a8f2e1f0f347f512da60968dab6bafdafb0afb46c0d0876f23b3dcb7e0ec199",
    strip_prefix = "Difference-1.0.2",
    urls = ["https://github.com/krzysztofzablocki/Difference/archive/1.0.2.tar.gz"],
)
