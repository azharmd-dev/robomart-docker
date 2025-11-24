FROM maven:3.5-jdk-8-alpine AS build
WORKDIR /opt/server
COPY pom.xml .
COPY src ./src
RUN mvn clean package 
RUN mv target/shipping-1.0.jar shipping.jar

FROM maven:3.5-jdk-8-alpine
WORKDIR /opt/server
COPY --from=build /opt/server /opt/server/
RUN addgroup -S roboshop && adduser -S roboshop -G roboshop
ENV CART_ENDPOINT="cart:8080" \
    DB_HOST=mysql
RUN chown -R roboshop:roboshop /opt/server
USER roboshop  
CMD [ "java", "-jar", "shipping.jar" ]