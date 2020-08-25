
#（1）基础镜像信息

　　#（2）维护者信息

　　#（3）镜像操作指令

#　　（4）容器启动时执行指令。

#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

#基础信息
FROM mcr.microsoft.com/dotnet/core/aspnet:3.1-buster-slim AS base   #Linux Debian 发行版上的 .NET Core 3.1 运行时
WORKDIR /app   # --指定工作目录
EXPOSE 80      #指定需要对外提供服务的端口
EXPOSE 443   
#EXPOSE命令只是声明了容器应该打开的端口并没有实际上将它打开!
#也就是说，如果你不用-p或者-P中指定要映射的端口，
#你的容器是不会映射端口出去的，从而我们知道我们是没有办法在Dockerfile里面进行端口映射的，
#我们只能在容器启动的时候或者在docker-compose文件中使用ports来指定将要映射的端口

#编译
FROM mcr.microsoft.com/dotnet/core/sdk:3.1-buster AS build
WORKDIR /src
COPY ["JenkinsDocker_Demo.csproj", ""]
RUN dotnet restore "./JenkinsDocker_Demo.csproj"     #dotnet restore - 恢复项目的依赖项和工具。
COPY . .
WORKDIR "/src/."
RUN dotnet build "JenkinsDocker_Demo.csproj" -c Release -o /app/build

#发布
FROM build AS publish
RUN dotnet publish "JenkinsDocker_Demo.csproj" -c Release -o /app/publish

#运行
FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "JenkinsDocker_Demo.dll"]