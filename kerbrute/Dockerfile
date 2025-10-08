# Dockerfile for Ropnop's Kerbrute
# This file uses a multi-stage build pattern. This is the best practice for
# compiled languages like Go because it creates a final image that is
# extremely small, containing only the tool itself and nothing else.

#--------------------------------------------------------------------------------
# STAGE 1: The Build Environment
# This first stage acts as a temporary "workshop". It contains all the tools
# needed to compile the program from its source code (like the Go compiler).
# Nothing from this stage will be in our final, lean image except the compiled tool.
#--------------------------------------------------------------------------------
FROM golang:1.21-alpine AS builder

# Set the working directory inside the container.
WORKDIR /src

# Install necessary build-time dependencies. We need 'git' to clone the source code.
# The --no-cache flag helps keep this temporary stage a bit smaller.
RUN apk --no-cache add git

# Download the source code and compile the binary using 'go install'.
# The '@latest' tag ensures we always get the newest version of the tool.
# The compiled binary will be placed in /go/bin/kerbrute inside this stage.
RUN go install github.com/ropnop/kerbrute@latest


#--------------------------------------------------------------------------------
# STAGE 2: The Final Production Image
# This second stage creates the actual image we will use. It starts from a
# completely empty base and only adds the single, compiled file from the
# "workshop" stage above.
#--------------------------------------------------------------------------------
FROM scratch

# Copy the compiled 'kerbrute' binary from the 'builder' stage into the root
# directory of our new, empty image. The binary is statically linked, meaning
# it has no external dependencies, which is why this works.
COPY --from=builder /go/bin/kerbrute /kerbrute

# Set the entrypoint for the container. This makes the container act like an
# executable. When we run the container, it will execute '/kerbrute'.
# Any arguments provided to 'docker run' will be passed on to the tool.
ENTRYPOINT ["/kerbrute"]
