# setup-zram
A GitHub Action that sets up compressed swap in RAM using zram on Ubuntu runners. It loads the required kernel module and creates and activates a swap device. The device size, compression algorithm, and swap priority are configurable.

## Usage

Example workflow in this repository:

```yaml
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: ergolyam/setup-zram@v1
        with:
          size: 4G
          algorithm: zstd
          priority: 100
```

## Inputs

| Input | Required | Default | Description |
| --- | --- | --- | --- |
| `size` | Yes | — | Device size: a positive integer with an optional `K`, `M`, `G`, `T`, or `P` suffix, e.g. `4G`. Without a suffix, the size is in bytes. |
| `algorithm` | No | `zstd` | Compression algorithm supported by the kernel. Lowercase letters, digits, `_`, and `-` are allowed. |
| `priority` | No | `100` | Swap priority passed to `swapon`. |

## Requirements

Tested on GitHub-hosted runners. Behavior on self-hosted runners is untested and may vary.

- Linux with `sudo` access and zram support.
- If the zram module cannot be loaded, the action attempts to install the `linux-modules-extra` package for the current kernel using APT.

The action does not disable swap or remove the device after execution.
