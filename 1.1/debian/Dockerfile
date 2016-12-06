FROM microsoft/dotnet:1.1.0-sdk-projectjson

ENV SCENARIOS plaintext
ENV THREADCOUNT -1
ENV ASPNETCORE_URLS http://+:8080

EXPOSE 8080

WORKDIR /app
COPY . .

RUN sed -i.net451 '/net451/d' ./project.json

RUN dotnet restore
RUN dotnet publish -o /pub -c Release

WORKDIR /pub
COPY benchmarks-docker/1.1/debian/run.sh .
RUN chmod +x run.sh

CMD ["./run.sh"]
