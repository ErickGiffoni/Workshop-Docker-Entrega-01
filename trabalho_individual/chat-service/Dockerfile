FROM node:16

WORKDIR /chat-service

COPY . /chat-service/

RUN yarn

COPY . /chat-service/

EXPOSE 7100

CMD yarn watch