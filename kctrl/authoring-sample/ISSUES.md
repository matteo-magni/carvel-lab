# v0.44.1

## Package and PackageInstall resources

The `package-resources.yml` file must define the `PackageMetadata` resource describing the package,
as well as a `Package` and a `PackageInstall`.
Apparently, looking at the produced artefacts, the latter two resources are not taken into account when building the package, as

- the `Package` is built off the `template` section in `PackageBuild` from the `package-build.yml` file;
- the `PackageInstall` is not even present at all.

Therefore, no change is observed even removing the whole `.spec` object from their definitions.

## Input validation

The `kctrl package release` command performs the actions defined in the `PackageBuild` resource,
and one of them is applying `ytt` to a list of paths defined.
Any input validation that requires some variables to be set (i.e. `#@schema/validation min_len=1`, see [schema file](./config/schema.yml)) will just cause the release to fail.

The `config` folder in this repo contains two ytt-compliant files that define a simple package to create a `ConfigMap`:
a schema file and the actual resources definition.

Make sure you have identified the OCI registry you want to push the package to and you are authenticated to it,
either via `docker login` or via [environment variables][imgpkg-auth-env].

The package build and release is run via the following command:

```sh
kctrl package release --repo-output ./repository --tag 0.0.1 --version 0.0.1
```

you are asked to provide the registry URL and repository name.

[imgpkg-auth-env]: https://carvel.dev/imgpkg/docs/v0.34.0/auth/#via-environment-variables

The build is going to fail with the following error:

```text
kctrl: Error: Reconciling: ytt:
  Error:
    Validating final data values:
  name
    from: schema.yml:4
    - must be: length >= 1 (by: schema.yml:3)
      found: length = 0
```

That's because there's a validation rule on the `name` variable and you don't provide a proper value at build time.

If such rule is removed or commented out (i.e. `#!@schema/validation min_len=1`) the build goes through and the package is available on the registry.
You can check by pulling it:

```sh
REGISTRY_URL="ghcr.io"
REPOSITORY_NAME="my-github-username/carvel-sample-package"

imgpkg pull -b ${REGISTRY_URL}/${REPOSITORY_NAME}:0.0.1 -o /tmp/sample
```
