FROM public.ecr.aws/lambda/dotnet:5.0 AS base

FROM mcr.microsoft.com/dotnet/sdk:5.0-buster-slim as build
WORKDIR /src
COPY ["ServerlessCICD.csproj", "ServerlessCICD/"]
RUN dotnet restore "ServerlessCICD/ServerlessCICD.csproj"

WORKDIR "/src/ServerlessCICD"
COPY . .
RUN dotnet build "ServerlessCICD.csproj" --configuration Release --output /app/build

FROM build AS publish
RUN dotnet publish "ServerlessCICD.csproj" \
            --configuration Release \ 
            --runtime linux-x64 \
            --self-contained false \ 
            --output /app/publish \
            -p:PublishReadyToRun=true  

FROM base AS final
WORKDIR /var/task
COPY --from=publish /app/publish .