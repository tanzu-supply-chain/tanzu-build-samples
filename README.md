# Tanzu Build Samples

## Usage
1. Follow instructions [below](#plugin-installation) to install the latest plugin
2. run `tanzu build config --build-plan-source-type=file --build-plan-source platform-config.yaml --containerapp-registry <some-writable-registry>`
3. cd into any one of the sample app in this repo
4. Run `tanzu build --output-dir /tmp/tanzu` to build all apps, or `tanzu build --app-name my-app  --output-dir /tmp/tanzu` to build `my-app` only.


Expected output:

- Standard out should print:

    - List of ContainerApps found
    - Build log for each ContainerApp
    - Location of additional output files
    - Digest of built digest for each ContainerApp

- A directory should be created in the directory you ran the command from (default is `tanzu/`, but can be customized via `tanzu build --output-dir ./some/dir/`)

- There will be a sub-directory for each built ContainerApp 
    
    [feedback wanted: It doesn't clean the directory each build. So if you've built an app with a under a different name, the old results gets left here. Is this desirable behaviour?]

- In each sub-directory, there will be:
    - `containerapp.yaml` which is essentially the same as the ContainerApp config, but with the `.spec.build` removed and `.spec.image` filled in.
    - `image.txt` which contains the full image digest for the built app.

##  Plugin Installation

1. Configure the tanzu CLI to use the staging inventory `tanzu config set env.TANZU_CLI_ADDITIONAL_PLUGIN_DISCOVERY_IMAGES_TEST_ONLY harbor-repo.vmware.com/tanzu_cli_stage/plugins/plugin-inventory:latest`

1. Install the build plugin `tanzu plugin install build`

1. Verify the build plugin is installed (and display helptext) `tanzu build --help`

1. Once available, the plugin can be upgrade with `tanzu plugin upgrade build`
