FROM microsoft/dotnet:1.1.0-sdk-projectjson-nanoserver

ENV SCENARIOS plaintext
ENV THREADCOUNT -1
ENV ASPNETCORE_URLS http://+:8080

EXPOSE 8080

WORKDIR /app
COPY . .

COPY project.json project.json.net451
RUN findstr /v net451 project.json.net451 > project.json

RUN dotnet restore
RUN dotnet publish -o /pub -c Release

WORKDIR /pub
COPY benchmarks-docker/1.1/nanoserver/run.cmd .

CMD ["run.cmd"]
