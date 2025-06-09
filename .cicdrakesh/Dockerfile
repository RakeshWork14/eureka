# Official openjdk18 image as base image
FROM openjdk:18

# Crate a directory /opt/rakesh-eureka, to run the application and store the application require files
RUN mkdir -p /opt/rakesh-eureka

# Making the above created directory as working directory to store application files
WORKDIR /opt/rakesh-eureka/

# Define a build time variable called JAR_Source to specify the location of jar file
# Args menas to pass the values dynamically, during the docker build run time
ARG JAR_SOURCE 

# COPY the jar file
# Syntax copy ["source" , "destination"]
COPY ["${JAR_SOURCE}" "/opt/rakesh-eureka/rakesh-eureka.jar"]


# Gratting the executable permissions(RWX- read,write,execute) to the above created directory
RUN chmod 777 /opt/rakesh-eureka/

# Exposing the port 8761 to run the external access to the application
# port will be under this path src->main->resources->application.yaml
EXPOSE 8761

#  Specify the command to run the spring based application using java jar file
# "rakesh-eureka.jar"  jar file name
CMD [ "java", "-jar", "/opt/rakesh-eureka/rakesh-eureka.jar" ]   