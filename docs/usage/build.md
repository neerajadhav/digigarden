---
icon: lucide/hammer
tags:
  - Usage
---

# Build

To build your documentation site, run `zensical build`.

[site_dir]: ../setup/basics.md#site_dir

## Usage

```sh
zensical build [OPTIONS] 
```

This will generate the static site in the configured [`site_dir`][site_dir],
with the default being `site`.

## Options

You can run `zensical build --help` to get command-line help for the `build`
command. It supports the following options:

| Option        | Short | Description                     |
| ------------- | ----- | ------------------------------- |
| --config-file | -f    | Path to the config file to use. |
| --clean       | -c    | Clean cache.                    |
| --help        |       | Show a help message and exit.   |
