FROM microsoft/dotnet:2.1-sdk-alpine AS build
WORKDIR /build

# Copy the global and csproj files
COPY ./src/services/idp/VND.CoolStore.Services.Idp.csproj ./src/services/idp/VND.CoolStore.Services.Idp.csproj
COPY ./global.json ./global.json

# Restore all packages
RUN dotnet restore ./src/services/idp/VND.CoolStore.Services.Idp.csproj --force --no-cache

# Copy the remaining source
COPY ./src/services/idp/ ./src/services/idp/

# Build the source code
RUN dotnet build ./src/services/idp/VND.CoolStore.Services.Idp.csproj --configuration Release --no-restore

# Publish application
RUN dotnet publish ./src/services/idp/VND.CoolStore.Services.Idp.csproj --configuration Release --no-restore --no-build --output "../../../dist"

FROM microsoft/dotnet:2.1-aspnetcore-runtime-alpine AS app
WORKDIR /app
COPY --from=build /build/dist .

ENV ASPNETCORE_URLS http://+:5000
# ENV ASPNETCORE_HTTPS_PORT=5001

# ENV ASPNETCORE_Kestrel__Certificates__Default__Path ./Certificate/coolstore.pfx
# ENV ASPNETCORE_Kestrel__Certificates__Default__Password "vietnam"

ENTRYPOINT ["dotnet", "VND.CoolStore.Services.Idp.dll"]
