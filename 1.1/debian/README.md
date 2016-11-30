# Build
```
git clone https://github.com/aspnet/Benchmarks
cd Benchmarks/src/Benchmarks
cp ../../NuGet.config .

git submodule add https://github.com/mikeharder/benchmarks-docker
docker build -t benchmarks -f benchmarks-docker/1.1/debian/Dockerfile .
```
