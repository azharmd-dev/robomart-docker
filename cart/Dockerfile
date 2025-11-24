FROM node:20.19.5-alpine3.22 AS build    
WORKDIR /opt/server                       
COPY package.json .                        
COPY server.js .                           
RUN npm install    

FROM node:20.19.5-alpine3.22
WORKDIR /opt/server
COPY --from=build /opt/server /opt/server
RUN addgroup -S roboshop && adduser -S roboshop -G roboshop
ENV REDIS_HOST="redis" \
    CATALOGUE_HOST="catalogue" \
    CATALOGUE_PORT="8080"
RUN chown -R roboshop:roboshop /opt/server
USER roboshop
CMD [ "node", "server.js" ]