# Tanzu Build Samples

## Usage
1. Follow instructions [below](#plugin-installation) to install the latest plugin
2. run `tanzu build config --build-plan-source-type=file --build-plan-source platform-config.yaml --containerapp-registry <some-writable-registry>/{name}`
3. cd into any one of the sample app in this repo
4. Run `tanzu build --output-dir /tmp/tanzu` to build all apps, or `tanzu build my-app  --output-dir /tmp/tanzu` to build `my-app` only.
 *NOTE*: If you are using a localhost registry, you will need to add `--network=host` when running tanzu build to allow the build container to access localhost
 
Expected output:

- Standard err should print:

    - List of ContainerApps found
    - Build log for each ContainerApp
    - Location of additional output files
    - Digest of built digest for each ContainerApp

- A temporary directory should be created, but can be customized via `tanzu build --output-dir ./some/dir/`. This dir will be printed to stdout at the end of the build.

- There will be a sub-directory for each built ContainerApp. The contents will vary depending on the runtime

##  Plugin Installation

1. Configure the tanzu CLI to use the staging inventory `tanzu config set env.TANZU_CLI_ADDITIONAL_PLUGIN_DISCOVERY_IMAGES_TEST_ONLY harbor-repo.vmware.com/tanzu_cli_stage/plugins/plugin-inventory:latest`

1. Install the build plugin `tanzu plugin install build`

1. Verify the build plugin is installed (and display helptext) `tanzu build --help`

1. Once available, the plugin can be upgrade with `tanzu plugin upgrade build`

