FROM node:16

WORKDIR /users-service

COPY . /users-service/

RUN yarn

EXPOSE 7101

CMD yarn db:migrate; yarn watch