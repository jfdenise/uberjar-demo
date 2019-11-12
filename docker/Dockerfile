FROM adoptopenjdk/openjdk11
COPY *.jar ./uberjar.jar
RUN chmod g+r uberjar.jar
EXPOSE 8080
CMD ["java", "-Djava.net.preferIPv4Stack=true", "-Djava.net.preferIPv4Addresses=true", "-Djboss.bind.address=0.0.0.0", "-jar", "uberjar.jar"]