FROM debian:latest

WORKDIR /home/steam/linuxgsm

# Stop apt-get asking to get Dialog frontend
ENV DEBIAN_FRONTEND noninteractive
ENV TERM xterm

RUN dpkg --add-architecture i386; apt update; apt install -y mailutils postfix curl wget file bzip2 gzip unzip bsdmainutils python util-linux ca-certificates binutils bc jq tmux lib32gcc1 libstdc++6 libstdc++6:i386 locales procps

RUN echo 'en_US.UTF-8 UTF-8' >> /etc/locale.gen
RUN cat /etc/locale.gen

RUN locale-gen

# Debug tools
RUN apt-get install -y netcat iputils-ping dnsutils traceroute iptables nano git cpufrequtils htop cron 
# Cleanup 
RUN apt-get -y autoremove && \
    apt-get -y clean && \
    rm -rf /var/lib/apt/lists/* && \
    rm -rf /tmp/* && \
    rm -rf /var/tmp/*

RUN  mkdir ~/bin \
  && curl -sSLf -z /usr/bin/gomplate -o /usr/bin/gomplate https://github.com/hairyhenderson/gomplate/releases/download/v2.2.0/gomplate_linux-amd64-slim \
  && chmod 755 /usr/bin/gomplate

# Add the steam user
#RUN adduser \
   # --disabled-login \
   # --disabled-password \
  #  --shell /bin/bash \
 #   --gecos "" \
#    steam

#RUN usermod -G tty steam

#RUN chown -R steam:steam /home/steam


RUN groupadd -g 750 -o steam && \
	adduser --uid 750 --disabled-password --gecos "" --ingroup steam steam && \	
	usermod -G tty steam && \
	chown -R steam:steam /home/steam/ && \
	chmod 755 /home/steam


# Switch to the user steam
USER steam

# Install LinuxGSM
RUN git clone "https://github.com/GameServerManagers/LinuxGSM.git" /home/steam/linuxgsm \
 && git checkout tags/181027.1

# Install GameConfigs
RUN git clone "https://github.com/GameServerManagers/Game-Server-Configs.git" /home/steam/linuxgsm-configs

# RUN git fetch --all \
#  && git reset --hard origin/master

# RUN git checkout tags/181027.1

USER root 
 
RUN find /home/steam/linuxgsm -type f -name "*.sh" -exec chmod u+x {} \; \
 && find /home/steam/linuxgsm -type f -name "*.py" -exec chmod u+x {} \; \
 && chmod u+x /home/steam/linuxgsm/lgsm/functions/README.md

ADD common.cfg.tmpl ./lgsm/config-default/config-lgsm/

RUN chown -R steam:steam /home/steam/linuxgsm \
 && chmod -R 775 /home/steam/linuxgsm \
 && ls -ltr

ADD docker-runner.sh docker-health.sh docker-ready.sh ./

RUN chown steam:steam docker-runner.sh docker-health.sh docker-ready.sh \
 && chmod +x docker-runner.sh docker-health.sh docker-ready.sh

ADD functions/* /home/steam/linuxgsm/lgsm/functions/

RUN chown steam:steam /home/steam/linuxgsm/lgsm/functions/* \
 && chmod +x /home/steam/linuxgsm/lgsm/functions/* 


ADD cronjob /etc/cron.d/lgsmupdate
 
# Give execution rights on the cron job
RUN chmod 0644 /etc/cron.d/lgsmupdate
 
# Create the log file to be able to run tail
RUN touch /var/log/cron.log
 
# Run the command on container startup
CMD cron


USER steam
RUN mkdir logs serverfiles 

