workspace(name = "distroless")

load("@bazel_tools//tools/build_defs/repo:http.bzl", "http_archive")
load("@bazel_tools//tools/build_defs/repo:http.bzl", "http_file")

http_archive(
    name = "io_bazel_rules_go",
    sha256 = "7be7dc01f1e0afdba6c8eb2b43d2fa01c743be1b9273ab1eaf6c233df078d705",
    urls = ["https://github.com/bazelbuild/rules_go/releases/download/0.16.5/rules_go-0.16.5.tar.gz"],
)

load("@io_bazel_rules_go//go:def.bzl", "go_register_toolchains", "go_rules_dependencies")

go_rules_dependencies()

go_register_toolchains()

load(
    "//package_manager:package_manager.bzl",
    "dpkg_list",
    "dpkg_src",
    "package_manager_repositories",
)

package_manager_repositories()

dpkg_src(
    name = "debian_stretch",
    arch = "amd64",
    distro = "stretch",
    sha256 = "79a66cd92ba9096fce679e15d0b5feb9effcf618b0a6d065eb32684dbffd0311",
    snapshot = "20190322T151132Z",
    url = "https://snapshot.debian.org/archive",
)

dpkg_src(
    name = "debian_stretch_backports",
    arch = "amd64",
    distro = "stretch-backports",
    sha256 = "36b3c35b2c01d22476736b0c26e6037dddeccf1d7a775b3fbd5dd991b58cceab",
    snapshot = "20190322T155353Z",
    url = "https://snapshot.debian.org/archive",
)

dpkg_src(
    name = "debian_stretch_security",
    package_prefix = "https://snapshot.debian.org/archive/debian-security/20190322T155353Z/",
    packages_gz_url = "https://snapshot.debian.org/archive/debian-security/20190322T155353Z/dists/stretch/updates/main/binary-amd64/Packages.gz",
    sha256 = "5dc5641648e7773dcd14e5ac2afd1803e8639b8b793ac5975793f8e98908d8ff",
)

dpkg_list(
    name = "package_bundle",
    packages = [
        # Version required to skip a security fix to the pre-release library
        # TODO: Remove when there is a security fix or dpkg_list finds the recent version
        "libc6=2.24-11+deb9u4",
        "base-files",
        "ca-certificates",
        "openssl",
        "libssl1.0.2",
        "libssl1.1",
        "libbz2-1.0",
        "libdb5.3",
        "libffi6",
        "libncursesw5",
        "liblzma5",
        "libexpat1",
        "libreadline7",
        "libtinfo5",
        "libsqlite3-0",
        "mime-support",
        "netbase",
        "readline-common",
        "tzdata",

        #c++
        "libgcc1",
        "libgomp1",
        "libstdc++6",

        #java
        "zlib1g",
        "libjpeg62-turbo",
        "libpng16-16",
        "liblcms2-2",
        "libfreetype6",
        "fonts-dejavu-core",
        "fontconfig-config",
        "libfontconfig1",
        "openjdk-8-jre-headless",
        "openjdk-11-jre-headless",

        #python
        "libpython2.7-minimal",
        "python2.7-minimal",
        "libpython2.7-stdlib",
        "dash",
        # Version required to skip a security fix to the pre-release library
        # TODO: Remove when there is a security fix or dpkg_list finds the recent version
        "libc-bin=2.24-11+deb9u4",

        #python3
        "libpython3.5-minimal",
        "python3.5-minimal",
        "libpython3.5-stdlib",

        #dotnet
        "libcurl3",
        "libkrb5-3",
        "libicu57",
        "liblttng-ust0",
        "libssl1.0.2",
        "zlib1g",
    ],
    # Takes the first package found: security updates should go first
    # If there was a security fix to a package before the stable release, this will find
    # the older security release. This happened for stretch libc6.
    sources = [
        "@debian_stretch_security//file:Packages.json",
        "@debian_stretch_backports//file:Packages.json",
        "@debian_stretch//file:Packages.json",
    ],
)

# For Jetty
http_archive(
    name = "jetty",
    build_file = "//:BUILD.jetty",
    sha256 = "c66abd7323f6df5b28690e145d2ae829dbd12b8a2923266fa23ab5139a9303c5",
    strip_prefix = "jetty-distribution-9.4.14.v20181114/",
    type = "tar.gz",
    urls = ["https://repo1.maven.org/maven2/org/eclipse/jetty/jetty-distribution/9.4.14.v20181114/jetty-distribution-9.4.14.v20181114.tar.gz"],
)

# Node
http_archive(
    name = "nodejs",
    build_file = "//experimental/nodejs:BUILD.nodejs",
    sha256 = "dc004e5c0f39c6534232a73100c194bc1446f25e3a6a39b29e2000bb3d139d52",
    strip_prefix = "node-v8.15.0-linux-x64/",
    type = "tar.gz",
    urls = ["https://nodejs.org/dist/v8.15.0/node-v8.15.0-linux-x64.tar.gz"],
)

# dotnet
http_archive(
    name = "dotnet",
    build_file = "//experimental/dotnet:BUILD.dotnet",
    sha256 = "fee8973feb7f964a20be8ed7ff8e277d343b7a9ee032af2f4deb90913e58f638",
    type = "tar.gz",
    urls = ["https://download.microsoft.com/download/9/1/7/917308D9-6C92-4DA5-B4B1-B4A19451E2D2/dotnet-runtime-2.1.0-linux-x64.tar.gz"],
)

# For the debug image
http_file(
    name = "busybox",
    executable = True,
    sha256 = "b51b9328eb4e60748912e1c1867954a5cf7e9d5294781cae59ce225ed110523c",
    urls = ["https://busybox.net/downloads/binaries/1.27.1-i686/busybox"],
)

# Docker rules.
# TODO: substitute below for "git_repository()" once rules_docker makes a new release > 0.7.0.
#http_archive(
#    name = "io_bazel_rules_docker",
#    sha256 = "...",
#    strip_prefix = "rules_docker-0.x.x",
#    urls = ["https://github.com/bazelbuild/rules_docker/archive/v0.x.x.tar.gz"],
#)
load("@bazel_tools//tools/build_defs/repo:git.bzl", "git_repository")

git_repository(
    name = "io_bazel_rules_docker",
    commit = "5edd38041535c2c07ca982218d184a5769e329c6",
    remote = "https://github.com/bazelbuild/rules_docker.git",
    shallow_since = "1553713324 -0400",
)

load(
    "@io_bazel_rules_docker//repositories:repositories.bzl",
    container_repositories = "repositories",
)

container_repositories()

load("@io_bazel_rules_docker//container:container.bzl", "container_pull")

# Used to generate java ca certs.
container_pull(
    name = "debian9",
    # From tag: 2019-02-27-130449
    digest = "sha256:fd26dfa474b76ef931e439537daba90bbd90d6c5bbdd0252616e6d87251cd9cd",
    registry = "gcr.io",
    repository = "google-appengine/debian9",
)

# Have the py_image dependencies for testing.
load(
    "@io_bazel_rules_docker//python:image.bzl",
    _py_image_repos = "repositories",
)

_py_image_repos()

# Have the java_image dependencies for testing.
load(
    "@io_bazel_rules_docker//java:image.bzl",
    _java_image_repos = "repositories",
)

_java_image_repos()

# Have the go_image dependencies for testing.
load(
    "@io_bazel_rules_docker//go:image.bzl",
    _go_image_repos = "repositories",
)

_go_image_repos()
