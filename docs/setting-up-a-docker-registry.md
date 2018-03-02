# Setting Up A Docker Registry

This document explains how to use different Docker registries with Elafros. It
assumes you have gone through the steps listed in
[DEVELOPMENT.md](/DEVELOPMENT.md) to set up your development environment (or
that you at least have installed `go`, set `GOPATH`, and put `$GOPATH/bin` on
your `PATH`).

It currently only contains instructions for [Google Container Registry
(GCR)](https://cloud.google.com/container-registry/), but you should be able to
use any Docker registry.

## Google Container Registry (GCR)

### Required Tools

Install the following tools:

1.  [`gcloud`](https://cloud.google.com/sdk/downloads)
1.  [`docker-credential-gcr`](https://github.com/GoogleCloudPlatform/docker-credential-gcr)

    If you installed `gcloud` using the archive or installer, you can install
    `docker-credential-gcr` like this:

    ```shell
    gcloud components install docker-credential-gcr
    ```

    If you installed `gcloud` using a package manager, you may need to install
    it with `go get`:

    ```shell
    go get github.com/GoogleCloudPlatform/docker-credential-gcr
    ```

    If you used `go get` to install and `$GOPATH/bin` isn't already in `PATH`,
    add it:

    ```shell
    export PATH=$PATH:$GOPATH/bin
    ```

### Setup

1.  If you haven't already set up a GCP project, create one and export its name
    for use in later commands.

    ```shell
    export PROJECT_ID=my-project-name
    gcloud projects create "${PROJECT_ID}"
    ```

1.  Enable the GCR API.

    ```shell
    gcloud --project="${PROJECT_ID}" services enable \
    containerregistry.googleapis.com
    ```

1.  Hook up your GCR credentials. Note that this may complain if you don't have
    the docker CLI installed, but it is not necessary and should still work.

    ```shell
    docker-credential-gcr configure-docker
    ```

1.  If you need to, update your `DOCKER_REPO_OVERRIDE` in your `.bashrc`. It
    should now be

    ```shell
    export DOCKER_REPO_OVERRIDE='us.gcr.io/<your-project-id>'
    ```

    (You may need to use a different region than `us` if you didn`t pick a`us`
    Google Cloud region.)

1.  You may need to run `bazel clean` after updating your `DOCKER_REPO_OVERRIDE`
    environment variable for `bazel` to pick up the change.

That's it, you're done!