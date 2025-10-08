

`docker build -t bloodyad .`

`alias bloodyad='docker run --rm -it --network=host -v "$(pwd)":/data -w /data bloodyad:latest'`
