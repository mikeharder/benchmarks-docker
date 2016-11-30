# Build
```
git clone https://github.com/aspnet/Benchmarks
cd Benchmarks/src/Benchmarks
cp ../../NuGet.config .

git submodule add https://github.com/mikeharder/benchmarks-docker
docker build -t benchmarks -f benchmarks-docker/1.1/debian/Dockerfile .
```

# Plaintext

## Server
```
docker run -it --rm -p 8080:8080 -e SCENARIOS=plaintext -e THREADCOUNT=[nproc / 4] benchmarks
```

## Client
```
wget https://raw.githubusercontent.com/mikeharder/pipeline-lua/master/pipeline.lua
wrk -c 256 -t 32 -d 10 -s pipeline.lua http://server-ip-or-name:8080/plaintext -- 16
```
