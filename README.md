# tauri-smol

> How to make them apps smol.

This repo aims to be the smallest possible Tauri app, it doesn't do anything and doesn't have any proper UI, but it's very small. It also serves as the baseline for future size optimization work.

## The magic tricks

A great magician never reveals their tricks, but I'm a software engineer, no magician so here goes:

1. Disable all the features.

This is easy, just set `"allowlist": false`.

2. Use small icons.

This repo uses an all black icon as it can be compressed the best.

4. Enable rust optimization features.

If you look at the `src-tauri/Cargo.toml` file you will notice the following section at the bottom. These settings enable all code size reducing optimizations that Rust has.

```toml
[profile.release]
panic = "abort" # Strip expensive panic clean-up logic
codegen-units = 1 # Compile crates one after another so the compiler can optimize better
lto = true # Enables link to optimizations
opt-level = "s" # Optimize for binary size
strip = true # Strip debug symbols
```

5. Rebuild the standard library.

The Rust standard library comes precompiled by default, but to allow the compiler to really got to town with the optimizations, you should rebuild the standard library too.

Follow [this section](https://tauri.studio/docs/building/app-size/#unstable-rust-compression-features) from the Tauri docs for more info. 

6. UPX

UPX is literal magic, use this command to effortlessly make your app 29%ish smaller. The tradeoff? Your app will look like malware ¯\\_(ツ)_/¯.

> NOTE: UPX currently doesn't work with aarch64 binaries, so you have to compile for x86 instead.

```console
upx --ultra-brute src-tauri/target/x86_64-apple-darwin/release/bundle/macos/tauri-app.app/Contents/MacOS/tauri-app
```