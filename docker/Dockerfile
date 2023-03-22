FROM cicorias/mosquitto-plugin-topic-jail:latest

USER root
RUN apt update && apt install -y curl net-tools inetutils-ping

USER mosquitto
COPY mosquitto.conf /mosquitto/config/mosquitto.conf

CMD ["/usr/bin/mosquitto", "-c", "/mosquitto/config/mosquitto.conf", "-v"]
