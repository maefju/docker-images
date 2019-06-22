FROM lgsmbase:v1 

WORKDIR /home/steam/linuxgsm


ENV TERM xterm

USER root


ADD docker-runner.sh docker-health.sh docker-ready.sh /home/steam/linuxgsm/

RUN chown steam:steam /home/steam/linuxgsm/docker-runner.sh /home/steam/linuxgsm/docker-health.sh /home/steam/linuxgsm/docker-ready.sh \
 && chmod +x /home/steam/linuxgsm/docker-runner.sh /home/steam/linuxgsm/docker-health.sh /home/steam/linuxgsm/docker-ready.sh

ADD csgo/ /home/steam/linuxgsm/csgo/
ADD orangebot/ /home/steam/linuxgsm/orangebot/
RUN chmod +x /home/steam/linuxgsm/orangebot/orangebot
RUN chmod +x /home/steam/linuxgsm/orangebot/orangebot-runner.sh


RUN chown steam:steam /home/steam/linuxgsm/ -R


USER steam

 # HEALTHCHECK --start-period=30s CMD ./docker-health.sh

CMD ["bash", "./docker-runner.sh"]
