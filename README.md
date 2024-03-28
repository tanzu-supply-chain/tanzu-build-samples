# Tanzu Build Samples


## Installation

1. Clone the plugin git repo `git clone https://gitlab.eng.vmware.com/build-service/tanzu-build-plugin.git`

2. Ensure the `tanzu builder` plugin is up to date
    - `tanzu plugin list` - View the plugins you have installed and version
    - `tanzu plugin install builder` - Install the builder plugin if you don't have it
    - `tanzu plugin upgrade builder` - Upgrade the builder plugin if you have < v1.3.0

3. From the root of the `tanzu-build-plugin` repo, run `make plugin-build-install-local`

4. Verify the build plugin is installed (and display helptext) `tanzu build --help`


## Usage

1. Ensure you have a platform config file. Example (make sure you change the registry to something you have push access to):

    ```yaml
    apiVersion: build.tanzu.vmware.com/v1
    kind: ContainerAppBuildConfig
    metadata:
      name: platform-config
    spec:
      registry: ""
      nonSecretEnv:
      - name: FOO
        value: bar
      buildpacks:
        builder: "paketobuildpacks/builder-jammy-base"
    ```

2. Go to your app's source code and create a ContainerApp config file. Example (the file name doesn't matter):
   
    ```yaml
    apiVersion: apps.tanzu.vmware.com/v1
    kind: ContainerApp
    metadata:
      name: my-app
    spec:
      build:
        path: "."
        nonSecretEnv:
        - name: BP_JVM_VERSION
          value: "17"
        buildpacks: {}
    ```

3. Run `tanzu build --platform-config <PATH_TO_PLATFORM_CONFIG_FILE>` to build all apps, or `tanzu build --platform-config <PATH_TO_PLATFORM_CONFIG_FILE> --app-name my-app` to build `my-app` only.

4. https://gitlab.eng.vmware.com/build-service/tanzu-build-plugin/-/tree/acceptance-tests/test/testdata contains some sample apps ~ripped~ inspired from other projects including where-for-dinner


Expected output

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