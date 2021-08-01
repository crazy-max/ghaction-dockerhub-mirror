[![GitHub release](https://img.shields.io/github/release/crazy-max/ghaction-dockerhub-mirror.svg?style=flat-square)](https://github.com/crazy-max/ghaction-dockerhub-mirror/releases/latest)
[![CI workflow](https://img.shields.io/github/workflow/status/crazy-max/ghaction-dockerhub-mirror/ci?label=ci&logo=github&style=flat-square)](https://github.com/crazy-max/ghaction-dockerhub-mirror/actions?workflow=test)
[![Become a sponsor](https://img.shields.io/badge/sponsor-crazy--max-181717.svg?logo=github&style=flat-square)](https://github.com/sponsors/crazy-max)
[![Paypal Donate](https://img.shields.io/badge/donate-paypal-00457c.svg?logo=paypal&style=flat-square)](https://www.paypal.me/crazyws)

## About

GitHub Action [composite](https://docs.github.com/en/actions/creating-actions/creating-a-composite-run-steps-action)
to mirror a DockerHub repo to another registry.

If you are interested, [check out](https://git.io/Je09Y) my other :octocat: GitHub Actions!

![](.github/workflow_dispatch.png) ![Screenshot](.github/screenshot.png)

___

* [Usage](#usage)
* [Contributing](#contributing)
* [License](#license)

## Usage

```yaml
name: dockerhub-mirror

on:
  workflow_dispatch:
    inputs:
      dockerhub-repo:
        description: 'DockerHub repository'
        required: true
      dest-registry:
        description: 'Destination registry (eg. ghcr.io)'
        required: true
      dest-repo:
        description: 'Destination repository (eg. username/repo)'
        required: true
      dry-run:
        description: 'Dry run'
        required: false
        default: 'false'

jobs:
  dockerhub-mirror:
    runs-on: ubuntu-latest
    steps:
      -
        name: Set up QEMU
        uses: docker/setup-qemu-action@v1
      -
        name: Set up Docker Buildx
        uses: docker/setup-buildx-action@v1
      -
        name: Login to DockerHub
        uses: docker/login-action@v1
        with:
          username: ${{ secrets.DOCKERHUB_USERNAME }}
          password: ${{ secrets.DOCKERHUB_TOKEN }}
      -
        name: Login to ${{ github.event.inputs.dest-registry }}
        uses: docker/login-action@v1
        with:
          registry: ${{ github.event.inputs.dest-registry }}
          username: ${{ secrets.DEST_USERNAME }}
          password: ${{ secrets.DEST_PASSWORD }}
      -
        name: Mirror ${{ github.event.inputs.dockerhub-repo }} to ${{ github.event.inputs.dest-registry }}/${{ github.event.inputs.dest-repo }}
        uses: crazy-max/ghaction-dockerhub-mirror@v1
        with:
          dockerhub-username: ${{ secrets.DOCKERHUB_USERNAME }}
          dockerhub-password: ${{ secrets.DOCKERHUB_TOKEN }}
          dockerhub-repo: ${{ github.event.inputs.dockerhub-repo }}
          dest-registry: ${{ github.event.inputs.dest-registry }}
          dest-repo: ${{ github.event.inputs.dest-repo }}
          dry-run: ${{ github.event.inputs.dry-run }}
```

## Customizing

### inputs

See [action.yml](action.yml)

## Contributing

Want to contribute? Awesome! The most basic way to show your support is to star the project, or to raise issues. If
you want to open a pull request, please read the [contributing guidelines](.github/CONTRIBUTING.md).

You can also support this project by [**becoming a sponsor on GitHub**](https://github.com/sponsors/crazy-max) or by
making a [Paypal donation](https://www.paypal.me/crazyws) to ensure this journey continues indefinitely!

Thanks again for your support, it is much appreciated! :pray:

## License

MIT. See `LICENSE` for more details.
