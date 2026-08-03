# IGAnets

This repository provides Docker build scripts for creating customizable IGANets images. It supports both CPU-only and GPU-enabled configurations, with two build variants:

- **Production:** Provides a clean installation of IGANets and the configured modules.
- **Development:** Stops at the end of the build stage, preserving the complete source and build directories for further development and debugging.

## Prebuilt Docker images

Pre-built Docker images are available through GitHub Container Registry (GHCR)

| Image name                       | Description|
| --------------------------------- | ---------------------------- |
| `ghcr.io/iganets/iganet:cpu`      | CPU-only build               |
| `ghcr.io/iganets/iganet:cpu-dev`  | CPU-only developer build     |
| `ghcr.io/iganets/iganet:cuda`     | CUDA-enabled build           |
| `ghcr.io/iganets/iganet:cuda-dev` | CUDA-enabled developer build |

## Building the Docker image

Build the CPU-only Docker image from the repository root:

```
docker build \
  --file Dockerfile.cpu \
  --tag iganet:cpu \
  .
```

On Linux distributions that use Podman instead of Docker, run the equivalent command:

```
podman build \
  --file Dockerfile.cpu \
  --tag iganet:cpu \
  .
```

Here, `--file` selects the Dockerfile, `--tag` assigns a name and tag to the resulting image, and `.` sets the current directory as the build context.

The build process can be customized by passing one or more of the build arguments listed below using the `--build-arg` option:


| Argument               | Description                                                 | Default       |
| ---------------------- | ----------------------------------------------------------- | ------------- |
| `CMAKE_VERSION`        | CMake version                                               | `4.4.0`       |
| `LIBTORCH_VERSION`     | LibTorch version                                            | `2.13.0`      |
| `NJOBS`                | Number of parallel build jobs                               | `1`           |
| `IGANET_BUILD_CPUONLY` | Build CPU-only binaries, even when a GPU driver is detected | `OFF`         |
| `IGANET_BUILD_DOCS`    | Build IGANet documentation                                  | `OFF`         |
| `IGANET_BUILD_PCH`     | Use precompiled headers                                     | `ON`          |
| `IGANET_BUILD_TYPE`    | CMake build type                                            | `Release`     |
| `IGANET_OPTIONAL`      | Optional IGANet modules to build                            | None          |
| `IGANET_ROOT`          | Installation prefix                                         | `/opt/iganet` |
| `IGANET_WITH_GISMO`    | Enable G+Smo support                                        | `OFF`         |
| `IGANET_WITH_MATPLOT`  | Enable Matplotlib support                                   | `OFF`         |
| `IGANET_WITH_MPI`      | Enable MPI support                                          | `OFF`         |
| `IGANET_WITH_OPENMP`   | Enable OpenMP support                                       | `ON`          |

### Examples

To build IGANet with the optional `examples` module enabled, run:

```
docker build \
  --file Dockerfile.cpu \
  --tag iganet:cpu \
  --build-arg IGANET_OPTIONAL=examples \
  .
```

To build the optional `examples` module from a specific `branch`, run:

```
docker build \
  --file Dockerfile.cpu \
  --tag iganet:cpu \
  --build-arg IGANET_OPTIONAL=examples[branch] \
  .
```

To build multiple optional modules, provide their names as a semicolon-separated list:

```
docker build \
  --file Dockerfile.cpu \
  --tag iganet:cpu \
  --build-arg IGANET_OPTIONAL=module1;module2 \
  .
```

### Optional modules

- [Examples](https://github.com/iganets/iganet-examples) `examples[main]`
- [Unit tests](https://github.com/iganets/iganet-unittests) `unittests[main]`
- [Performance tests](https://github.com/iganets/iganet-perftests) `perftests[main]`
- [Python bindings](https://github.com/iganets/iganet-python) `python[main]`
- [MATLAB bindings](https://github.com/iganets/iganet-matlab) `matlab[main]`

### Development builds

By default, the Docker images contain a clean installation of IGANets and the configured modules. If you want to have a development build, run:
```
docker build \
  --file Dockerfile.cpu \
  --tag iganet:cpu-dev \
  --target dev \
  .
```

## Running the Docker image

Once the build process completed you can run the image:

```
docker run -ti --rm --name iganet iganet:cpu
```

This also works with podman under Redhat Linux:

```
podman run -ti --rm --name iganet iganet:cpu
```

You can also run executables directly provided that you enabled the optional module `examples`:

```
docker run --rm --name iganet iganet:cpu iganet_poisson
```

or

```
podman run --rm --name iganet iganet:cpu iganet_poisson
```

