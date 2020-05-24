### Pull an image
```
docker pull mcr.microsoft.com/dotnet/core/samples:aspnetapp
```

### Image list
```
docker image list
```

### Run
```
docker run -p 8080:80 -d mcr.microsoft.com/dotnet/core/samples:aspnetapp
```

### Delete container
```
docker rm <name>
```

### Delete image
```
docker image rm mcr.microsoft.com/dotnet/core/samples:aspnetapp
```

### Build
```
docker build <name> -t <tag> <docker file path>
```



