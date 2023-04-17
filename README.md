# GitHub Action - Execute a script on isolated ubuntu environment

This Github action executes a shell script inside a separate docker container, which is cleaner than the default runner environment.

## Usage

### Inputs

- `script`: Script path to execute. Relative to `$GITHUB_WORKSPACE`.
- `on`: Codename of ubuntu to execute build script. `focal`(20.04) or `jammy`(22.04).

### Outputs

No outputs.

### Example workflow - build a project!

```yaml
on: push
name: Build binaries
jobs:
  build:
    runs-on: ubuntu-latest
      matrix:
        codename: [focal, jammy]
    name: Create Release
    runs-on: ubuntu-latest
    steps:
      - name: Checkout code
        uses: actions/checkout@v2
        with:
          submodules: 'recursive'
          fetch-depth: 0
      - name: Build the binary
        uses: avif-community/execute-on-separated-ubuntu@v1
        with:
          on: ${{ matrix.codename }}
          script: scripts/build.sh
```

This will execute `(your-repository)/scripts/build.sh` in both ubuntu-20.04 and 22.04.
Created artifacts are stored in `$GITHUB_WORKSPACE`, so you can upload [as an artifact](https://github.com/actions/upload-artifact), [as an release asset](https://github.com/actions/upload-release-asset) and so on.
