# libtorrent-binary

Prebuilt `xdmtorrent` libraries for [XDM](https://github.com/subhra74/xdm): a small native library with a
flat C API and a JNI binding over [libtorrent](https://libtorrent.org) (rasterbar). XDM loads it to download
and seed torrents and magnet links.

This repository holds only the binaries, published as GitHub release assets. It has no source code, and
the XDM build never compiles C++.

## Releases

Each release is tagged `xdmtorrent-native-<abi>.<n>`:

- `<abi>` is the library's C API version (`XT_ABI_VERSION`). XDM refuses to load a library with a
  different C API version.
- `<n>` counts the rebuilds of the same API, for example a new libtorrent or OpenSSL version.

Assets, one per platform:

| Platform        | File                              |
|-----------------|-----------------------------------|
| Windows x64     | `windows-x64-xdmtorrent.dll`      |
| Windows arm64   | `windows-arm64-xdmtorrent.dll`    |
| macOS arm64     | `macos-arm64-libxdmtorrent.dylib` |
| macOS x64       | `macos-x64-libxdmtorrent.dylib`   |
| Linux x64       | `linux-x64-libxdmtorrent.so`      |
| Linux arm64     | `linux-arm64-libxdmtorrent.so`    |

Each release also lists the SHA-256 of every asset. XDM pins the release it bundles in
`packaging/torrent/binaries.lock` (platform, file, URL, SHA-256). `packaging/torrent/fetch-binaries.sh` and
`fetch-binaries.ps1` download the files and refuse any file whose hash differs.

How the libraries are built:

- Windows: MSVC with the static CRT, so no Visual C++ runtime is needed.
- macOS: separate arm64 and x64 builds.
- Linux: built on manylinux2014 (glibc 2.17 or newer), with libstdc++ and libgcc linked statically.

The libraries export only the `xt_*` C API and the JNI entry points. They are not code-signed here: XDM's
installers sign them together with the app.

## What is inside

Every library is statically linked against:

| Component  | Version | License                                                     |
|------------|---------|-------------------------------------------------------------|
| libtorrent | 2.1.2   | BSD 3-Clause, plus the notices in its license file          |
| OpenSSL    | 3.5.9   | Apache License 2.0                                          |
| Boost      | 1.92.0  | Boost Software License 1.0 (header-only parts)              |

The full license texts are in [`third-party/`](third-party). On Linux, the statically linked GCC runtime
(libstdc++, libgcc) is covered by the GCC Runtime Library Exception.

## License

The `xdmtorrent` wrapper is covered by [LICENSE](LICENSE): the binaries may be used and redistributed
unmodified, with XDM or on their own. Its source is not published. The third-party components keep their
own licenses, listed above.
