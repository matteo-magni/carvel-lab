# Authoring packages with kctrl

The official reference is available [here][kctrl-authoring-docs].

The `kctrl package release` command is used to build and release packages in the form of OCIs to a registry (i.e. docker.io, ghcr.io, ...).
Two specific files hold the required information to do so:

- `package-build.yml`: must define a [`PackageBuild`][packagebuild-resource-docs] resource
- `package-release.yml`: must define [`Package`][package-resource-docs], [`PackageMetadata`][packagemetadata-resource-docs] and [`PackageInstall`][packageinstall-resource-docs] resources

[kctrl-authoring-docs]: https://carvel.dev/kapp-controller/docs/v0.43.2/kctrl-package-authoring/.
[package-resource-docs]: https://carvel.dev/kapp-controller/docs/v0.43.2/packaging/#package
[packagemetadata-resource-docs]: https://carvel.dev/kapp-controller/docs/v0.43.2/packaging/#package-metadata
[packageinstall-resource-docs]: https://carvel.dev/kapp-controller/docs/v0.43.2/packaging/#package-install
[packagebuild-resource-docs]: https://carvel.dev/kapp-controller/docs/v0.43.2/packaging/#package-build
