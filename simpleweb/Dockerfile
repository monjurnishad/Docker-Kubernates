# Specifiy a base image
FROM node:alpine

# Install some dependencies
WORKDIR /usr/app

# so that we don't reinstall all dependencies
# when we make changes to other files.
COPY ./package.json ./
RUN npm install

COPY ./ ./

# default command
CMD ["npm", "start"]
  