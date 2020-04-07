# caddy_docker

这是Caddy的docker映像。谢谢[abiosoft](https://github.com/abiosoft/caddy-docker)

## 为什么建造它

来自abiosoft的docker映像有问题。
我认为可能要获取一些错误的链接/程序包，并且我尝试使用一种不友好但有效的方式对其进行修复。

## 版本

caddy v1.0.5

## 默认插件

```dnspod```, ```filter```, ```cache```, ```minify```, ```expires```, ```realip```, ```cors```

## 加入更多插件

### 常规

1. 前往 [官方插件介绍](https://caddyserver.com/v1/docs/http.filter).(filter作为例子)

2. 点击网站上的“完整文档”。

3. 复制包/存储库链接，就像 ```github.com/echocat/caddy-filter/```.

4. 将其添加到“ builder.sh”中，之后您可以在其中看到类似 ```"import/path/here"```
(注意：不要忘记前面的“ _”)

5. 完毕。

### 特殊

我所知，您需要做更多的事情去加入一些插件。（例​​如cors）。

1. 执行“常规”中与1＆2相同的操作

2. 您可以在存储库中看到```caddy```文件夹，并且其中有一个```corsPlugin.go```文件。

3. 打开它可以将内容复制到```builder.sh```中。（您可以看到我所做的，然后按照它添加其他插件（例如cors））

## build

我使用docker-compose构建映像。

```docker

 caddy:
    build:
      context: ./caddy
    container_name: caddy
    environment:
      - TZ=
      - DNSPOD_API_KEY=
      - DNSPOD_HTTP_TIMEOUT=10
    volumes:
      - ./caddy/data/Caddyfile:/caddy/Caddyfile
      - ./caddy/data/certs:/caddy/certs
      - ./caddy/data/.caddy:/root/.caddy
      - ./www:/www/:rw
    ports:
      - 2015:2015
      - 80:80
      - 443:443
    restart: always
    networks:
      - net-default

```

您需要将```context```更改为您的Dockerfile所在的位置。

### 改变 environment.

```TZ``` 是时区。

如果您使用dnspod，则需要使用```DNSPOD_API_KEY```，例如```id，api_tokens```。

如果您使用cloudflare，则需要```CLOUDFLARE_EMAIL```和```CLOUDFLARE_API_KEY```
你可以从中得到它 [cloudflare]( https://dash.cloudflare.com/profile/api-tokens)

## 更重要的是

您想从中获取“ cloudflare”插件 [点击](https://github.com/caddyserver/dnsproviders/blob/master/cloudflare/cloudflare.go)
那么您可以像```特殊```一样添加它或查看我如何添加```dnspod```插件。

## 致谢

再次感谢 [abiosoft](https://github.com/abiosoft/caddy-docker) 
您可以看到有关构建caddy v1的更多详细信息 [caddy v1.0.5](https://github.com/caddyserver/caddy/tree/v1.0.5)
