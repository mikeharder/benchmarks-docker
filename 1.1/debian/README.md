# Build
## Common
```
git clone https://github.com/aspnet/Benchmarks
cd Benchmarks/src/Benchmarks
cp ../../NuGet.config .
curl -O https://raw.githubusercontent.com/TechEmpower/FrameworkBenchmarks/master/frameworks/CSharp/aspnetcore/Benchmarks/appsettings.postgresql.json
```


## Host
```
cd Benchmarks/src/Benchmarks
sed -i.net451 '/net451/d' ./project.json
sed 's|{db_server_placeholder}|db-name-or-ip|g' appsettings.postgresql.json > appsettings.json
dotnet restore
dotnet publish -c Release
```

## Container
```
cd Benchmarks/src/Benchmarks
git submodule add https://github.com/mikeharder/benchmarks-docker
docker build -t benchmarks -f benchmarks-docker/1.1/debian/Dockerfile .
```

# Plaintext

## Server

### Host
```
dotnet bin/Release/netcoreapp1.1/Benchmarks.dll scenarios=plaintext server.urls=http://+:8080 threadcount=[nproc / 4] noninteractive=true
```

### Container
```
docker run -it --rm -p 8080:8080 -e SCENARIOS=plaintext -e THREADCOUNT=[nproc / 4] benchmarks
```

## Client
```
wget https://raw.githubusercontent.com/mikeharder/pipeline-lua/master/pipeline.lua
wrk -c 256 -t 32 -d 10 -s pipeline.lua http://server-ip-or-name:8080/plaintext -- 16
```

# JSON

## Server

### Host
```
dotnet bin/Release/netcoreapp1.1/Benchmarks.dll scenarios=json server.urls=http://+:8080 noninteractive=true
```

### Container

#### Bridge Networking
```
docker run -it --rm -p 8080:8080 -e SCENARIOS=json benchmarks
```

#### Host Networking
```
docker run -it --rm --network host -e SCENARIOS=json benchmarks
```

## Client
```
wrk -c 256 -t 32 -d 10 http://server-ip-or-name:8080/json
```

# Fortunes

## Server

### Host
```
dotnet bin/Release/netcoreapp1.1/Benchmarks.dll scenarios=fortunes server.urls=http://+:8080 noninteractive=true
```

### Container

#### Bridge Networking
```
docker run -it --rm -p 8080:8080 -e SCENARIOS=fortunes benchmarks
```

#### Host Networking
```
docker run -it --rm --network host -e SCENARIOS=fortunes benchmarks
```

## Client
```
wrk -c 256 -t 32 -d 10 http://server-ip-or-name:8080/json
```

