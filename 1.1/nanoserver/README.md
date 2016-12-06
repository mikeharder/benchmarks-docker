# Build
## Common
```
netsh advfirewall firewall add rule name="TCP 8080" dir=in action=allow protocol=TCP localport=8080

git clone https://github.com/aspnet/Benchmarks
cd Benchmarks\src\Benchmarks
copy ..\..\NuGet.config
```


## Host
```
cd Benchmarks\src\Benchmarks
copy project.json project.json.net451
findstr /v net451 project.json.net451 > project.json
dotnet restore
dotnet publish -c Release
```

## Container
```
cd Benchmarks\src\Benchmarks
git submodule add https://github.com/mikeharder/benchmarks-docker
docker build -t benchmarks -f benchmarks-docker/1.1/nanoserver/Dockerfile .
```

# Plaintext
## Server
### Host
```
dotnet bin\Release\netcoreapp1.1\Benchmarks.dll scenarios=plaintext server.urls=http://+:8080 threadcount=[nproc / 4] noninteractive=true
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
dotnet bin\Release\netcoreapp1.1\Benchmarks.dll scenarios=json server.urls=http://+:8080 noninteractive=true
```

### Container
```
docker run -it --rm -p 8080:8080 -e SCENARIOS=json benchmarks
```

## Client
```
wrk -c 256 -t 32 -d 10 http://server-ip-or-name:8080/json
```

