FROM debian:latest

# replace shell with bash so we can source files
RUN rm /bin/sh && ln -s /bin/bash /bin/sh

# update the repository sources list
# and install dependencies
RUN apt-get update \
    && apt-get install -y curl \
    && apt-get -y autoclean \
    && apt-get -y install git

# nvm environment variables
ENV NVM_DIR /usr/local/nvm
ENV NODE_VERSION 6.0.0

# install nvm
# https://github.com/creationix/nvm#install-script
RUN curl --silent -o- https://raw.githubusercontent.com/creationix/nvm/v0.31.2/install.sh | bash

# install node and npm
RUN source $NVM_DIR/nvm.sh \
    && nvm install $NODE_VERSION \
    && nvm alias default $NODE_VERSION \
    && nvm use default

# add node and npm to path so the commands are available
ENV NODE_PATH $NVM_DIR/v$NODE_VERSION/lib/node_modules
ENV PATH $NVM_DIR/versions/node/v$NODE_VERSION/bin:$PATH

# confirm installation
RUN node -v
RUN npm -v

RUN mkdir -p /usr/src/app
RUN git clone https://github.com/steveunixson/bonobo-calling-express.git /usr/src/bonobo

WORKDIR /usr/src/bonobo/
RUN npm install
EXPOSE 8001
CMD [ “node”, “/usr/src/app/dist/server.js” ]