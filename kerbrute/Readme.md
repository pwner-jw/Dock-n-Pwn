to build `docker build -t kerbrute .`
alias `alias kerbrute='docker run --rm -it --network=host -v "$(pwd)":"$(pwd)" -w "$(pwd)" kerbrute'`
