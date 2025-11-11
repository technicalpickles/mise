# Ruby

Like `rvm`, `rbenv`, or `asdf`, `mise` can manage multiple versions of [Ruby](https://www.ruby-lang.org/) on the same system.

> The following are instructions for using the ruby mise core plugin. This is used when there isn't a
> git plugin installed named "ruby". If you want to use [asdf-ruby](https://github.com/asdf-vm/asdf-ruby)
> then use `mise plugins install ruby GIT_URL`.

The code for this is inside the mise repository at
[`./src/plugins/core/ruby.rs`](https://github.com/jdx/mise/blob/main/src/plugins/core/ruby.rs).

## Usage

The following installs the latest version of ruby-3.2.x (if some version of 3.2.x is not already
installed) and makes it the global default:

```sh
mise use -g ruby@3.2
```

Behind the scenes, mise can use prebuilt Ruby binaries from [rv-ruby](https://github.com/spinel-coop/rv-ruby)
for significantly faster installations, or compile ruby from source using [`ruby-build`](https://github.com/rbenv/ruby-build).
See [Prebuilt Binaries](#prebuilt-binaries) below for more details.

You can also install a specific ruby flavour. To get the latest version from a flavour, just use the
flavour prefix.

```sh
mise use -g ruby@truffleruby            # latest version of truffleruby
```

## Prebuilt Binaries

mise can optionally use prebuilt Ruby binaries from [rv-ruby](https://github.com/spinel-coop/rv-ruby)
for significantly faster installations (~1 second vs several minutes). Enable with:

```sh
mise settings set ruby.rv_prebuilt_binaries true
```

When a prebuilt binary isn't available for your platform or the requested Ruby version, mise
automatically falls back to compiling from source using `ruby-build`. Ensure that you have the necessary
[dependencies](https://github.com/rbenv/ruby-build/wiki#suggested-build-environment) installed
for source compilation. You can check the ruby-build [README](https://github.com/rbenv/ruby-build/blob/master/README.md)
for additional settings and troubleshooting.

See the [Settings](#settings) section below for more configuration options including checksum verification
and fallback behavior.

## Default gems

mise can automatically install a default set of gems right after installing a new ruby version.
To enable this feature, provide a `$HOME/.default-gems` file that lists one gem per line, for
example:

```text
# supports comments
pry
bcat ~> 0.6.0 # supports version constraints
rubocop --pre # install prerelease version
```

## `.ruby-version` and `Gemfile` support

mise uses a `mise.toml` or `.tool-versions` file for auto-switching between software versions.
However, it can also read ruby-specific version files `.ruby-version` or `Gemfile`
(if it specifies a ruby version).

Create a `.ruby-version` file for the current version of ruby:

```sh
ruby -v > .ruby-version
```

Enable idiomatic version file reading for ruby:

```sh
mise settings add idiomatic_version_file_enable_tools ruby
```

See [idiomatic version files](/configuration.html#idiomatic-version-files) for more information.

## Manually updating ruby-build

ruby-build should update daily, however if you find versions do not yet exist you can force an
update:

```bash
mise cache clean
mise ls-remote ruby
```

## Settings

`ruby-build` already has a
[handful of settings](https://github.com/rbenv/ruby-build?tab=readme-ov-file#custom-build-configuration),
in additional to that mise has a few extra settings:

<script setup>
import Settings from '/components/settings.vue';
</script>
<Settings child="ruby" :level="3" />
