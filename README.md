# IGAnets

## Local docker/podman deployment

Build the docker image:

```
docker build -f ./ops/Dockerfile.cpu -t iganet .
```

Some Linux distributions like Redhat replace `docker` by `podman`. In that case use:

```
podman build -f ./ops/Dockerfile.cpu -t iganet .
```

The build process can be customized by passing one or more of the following build arguments

| Argument | Description | Default value |
|----------|-------------|---------------|
| `CMAKE_VERSION` | CMake version | 4.2.0 |
| `CMAKE_BUILD_TYPE` | CMake build type | `Release` |
| `CMAKE_INSTALL_PREFIX` | Installation prefix | `/opt/iganet` |
| `LIBTORCH_VERSION` | LibTorch version | 2.9.1 |
| `NJOBS` | Number of build jobs | 1 |
| `IGANET_BUILD_CPUONLY` | Build CPU-only binaries even of GPU driver is found | `OFF` |
| `IGANET_BUILD_DOCS` | Build IGAnet documentation | `OFF` |
| `IGANET_BUILD_PCH` | Build IGAnet with precompiled headers | `ON` |
| `IGANET_OPTIONAL` | Build IGAnet with optional modules | `None` |
| `IGANET_WITH_GISMO` | Build IGAnet with G+Smo support | `OFF` |
| `IGANET_WITH_MATPLOT` | Build IGAnet with Matplotlib support | `OFF` |
| `IGANET_WITH_MPI` | Build IGAnet with MPI support | `OFF` |
| `IGANET_WITH_OPENMP` | Build IGAnet with OpenMP support | `ON` |

Optional modules are:

- [Examples](https://github.com/iganets/iganet-examples) `examples[main]`
- [Unit tests](https://github.com/iganets/iganet-unittests) `unittests[main]`
- [Performance tests](https://github.com/iganets/iganet-perftests) `perftests[main]`
- [Python bindings](https://github.com/iganets/iganet-python) `python[main]`
- [MATLAB bindings](https://github.com/iganets/iganet-matlab) `matlab[main]`

Then you can run the image:

```
docker run -ti --rm --name iganet iganet
```

This also works with podman under Redhat Linux:

```
podman run -ti --rm --name iganet iganet
```

You can also run executables directly:

```
docker run --rm --name iganet iganet iganet_poisson
```

or

```
podman run --rm --name iganet iganet iganet_poisson
```
