# use an existing image as a base
# each steps produce intermediate container until 
# producing a final image
# steps are cached for faster future builds.
FROM alpine

# run and install a dependency
RUN apk add --update redis
RUN apk add --update gcc
# tell the image what to do when it starts 
# as a container
CMD ["redis-server"]