# Tanzu Build Samples

## Usage
1. Follow instructions [below](#plugin-installation) to install the latest plugin
2. update the `.spec.registry` field in `platform-config.yml` to contain a writable registry (make sure to docker login)
3. run `tanzu build set-config --config-type file --config-location platform-config.yml`
4. cd into any one of the sample app in this repo
5. Run `tanzu build --output-dir /tmp/tanzu` to build all apps, or `tanzu build --platform-config ../platform-config.yml --app-name my-app  --output-dir /tmp/tanzu` to build `my-app` only.
   (Note the platform config flag is temporary)


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

1. Clone the plugin git repo `git clone https://gitlab.eng.vmware.com/build-service/tanzu-build-plugin.git`

2. Ensure the `tanzu builder` plugin is up to date
    - `tanzu plugin list` - View the plugins you have installed and version
    - `tanzu plugin install builder` - Install the builder plugin if you don't have it
    - `tanzu plugin upgrade builder` - Upgrade the builder plugin if you have < v1.2.0

3. From the root of the `tanzu-build-plugin` repo, run `make plugin-build-install-local`

4. Verify the build plugin is installed (and display helptext) `tanzu build --help`
