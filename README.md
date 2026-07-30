# IGAnets

## Building the docker image

```
docker build -f Dockerfile.cpu -t iganet .
```

Some Linux distributions like Redhat replace `docker` by `podman`. In that case use:

```
podman build -f Dockerfile.cpu -t iganet .
```

The build process can be customized by passing one or more of the following build arguments

| Argument | Description | Default |
|:---|:---|:---:|
| `CMAKE_VERSION` | CMake version | `4.4.0` |
| `LIBTORCH_VERSION` | LibTorch version | `2.13.0` |
| `NJOBS` | Number of parallel build jobs | `1` |
| `IGANET_BUILD_CPUONLY` | Build CPU-only binaries, even when a GPU driver is detected | `OFF` |
| `IGANET_BUILD_DOCS` | Build IGANet documentation | `OFF` |
| `IGANET_BUILD_PCH` | Use precompiled headers | `ON` |
| `IGANET_BUILD_TYPE` | CMake build type | `Release` |
| `IGANET_OPTIONAL` | Optional IGANet modules to build | None |
| `IGANET_ROOT` | Installation prefix | `/opt/iganet` |
| `IGANET_WITH_GISMO` | Enable G+Smo support | `OFF` |
| `IGANET_WITH_MATPLOT` | Enable Matplotlib support | `OFF` |
| `IGANET_WITH_MPI` | Enable MPI support | `OFF` |
| `IGANET_WITH_OPENMP` | Enable OpenMP support | `ON` |

### Example
```
docker build -f Dockerfile.cpu -t iganet --build-arg IGANET_OPTIONAL="examples" .
```

### Optional modules

- [Examples](https://github.com/iganets/iganet-examples) `examples[main]`
- [Unit tests](https://github.com/iganets/iganet-unittests) `unittests[main]`
- [Performance tests](https://github.com/iganets/iganet-perftests) `perftests[main]`
- [Python bindings](https://github.com/iganets/iganet-python) `python[main]`
- [MATLAB bindings](https://github.com/iganets/iganet-matlab) `matlab[main]`

## Running the docker image

Once the build process completed you can run the image:

```
docker run -ti --rm --name iganet iganet
```

This also works with podman under Redhat Linux:

```
podman run -ti --rm --name iganet iganet
```

You can also run executables directly provided that you enabled the optional module `examples`:

```
docker run --rm --name iganet iganet iganet_poisson
```

or

```
podman run --rm --name iganet iganet iganet_poisson
```
