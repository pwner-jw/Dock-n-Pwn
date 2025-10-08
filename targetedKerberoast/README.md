To build the image - `docker build -t targetedkerberoast .`
Alias to run the commands - `alias targetedkerberoast='docker run --rm -it --network=host -v "$(pwd)":/data -w /data targetedkerberoast'`
