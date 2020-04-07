# caddy_docker

[中文](Readme_zh.md)

this is a docker image for Caddy. Thanks to [abiosoft](https://github.com/abiosoft/caddy-docker)

## why to build it

the docker image from abiosoft has something wrong. I think it may have some wrong links/packages to get, and I try to use an unfriendly way to fix it.

## version

caddy v1.0.5

## default plugins

```dnspod```, ```filter```, ```cache```, ```minify```, ```expires```, ```realip```, ```cors```

## how to add more plugins

### nomally

1. go to the [official website for plugins](https://caddyserver.com/v1/docs/http.filter).(for filter as example)

2. click the 'Full documentation' in the website.

3. copy the packages/repository link as like ```github.com/echocat/caddy-filter/```.

4. add it into 'builder.sh' following where you can see the word like that ```"import/path/here"```
(note: don't foget the ```_``` in front of it)

5. it's ok.

### specially

as I know. some plugins you need do more things.(as cors for example).

1. do the same things like 1&2 in 'nomally'

2. you can see ```caddy``` folder in the repository, and in it there is a  ```corsPlugin.go``` file.

3. open it can copy the content into ```builder.sh```.(you could see what I did, and just follow it to add other plugins like cors)

## build

I use docker-compose to build the image.

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

you need change ```context``` to where your Dockerfile is.

### change environment.

```TZ``` is time zone.

if you use dnspod, you need ```DNSPOD_API_KEY``` like ```id,api_tokens```.

if you use cloudflare, you need ```CLOUDFLARE_EMAIL``` and ```CLOUDFLARE_API_KEY```
you can get it from [cloudflare]( https://dash.cloudflare.com/profile/api-tokens)

## what's more

you want to get 'cloudflare' plugins from [it](https://github.com/caddyserver/dnsproviders/blob/master/cloudflare/cloudflare.go)
then you can do like ```specially``` to add it or to see how I add the ```dnspod``` plugins.

## Thanks

Thanks to [abiosoft](https://github.com/abiosoft/caddy-docker) again.
What's more detail about build caddy v1 you can see [caddy v1.0.5](https://github.com/caddyserver/caddy/tree/v1.0.5)
