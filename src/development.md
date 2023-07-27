# Development
VDS is written in [Rust](https://www.rust-lang.org/) and shaders are written in [WGSL](https://www.w3.org/TR/WGSL/).

Make sure you are using the latest version of stable rust by running `rustup update`. VDS requires a proper graphics driver that provides at least one of the [supported graphics APIs](#cross-platform-support).

## Building from Source for Native

Run it locally with `cargo run --release`.

On Linux you need to first run:

`sudo apt-get install libxcb-render0-dev libxcb-shape0-dev libxcb-xfixes0-dev libxkbcommon-dev libssl-dev`

On Fedora Rawhide you need to run:

`dnf install clang clang-devel clang-tools-extra libxkbcommon-devel pkg-config openssl-devel libxcb-devel gtk3-devel atk fontconfig-devel`

## Building from Source for Web

[Trunk](https://trunkrs.dev/) can be used to compile `vds` to WASM and spin up a web server with hot reloading.

1. Install Trunk with `cargo install --locked trunk`.
2. Run `trunk serve` to build and serve on `http://127.0.0.1:8080`. Trunk will rebuild automatically if you edit the project.
3. Open `http://127.0.0.1:8080/index.html#dev` in a browser. See the warning below.

> `assets/sw.js` script will try to cache our VDS app, and loads the cached version when it cannot connect to server allowing VDS to work offline (like PWA).
> appending `#dev` to `index.html` will skip this caching, allowing to load the latest builds during development.

## Deploy for Web
1. Just run `trunk build --release`.
2. It will generate a `dist` directory as a "static html" website
3. Upload the `dist` directory to any of the numerous free hosting websites including [GitHub Pages](https://docs.github.com/en/free-pro-team@latest/github/working-with-github-pages/configuring-a-publishing-source-for-your-github-pages-site).
4. This repo already provides a [workflow](.github/workflows/deploy_github_pages.yml) that auto-deploys VDS to GitHub pages.
