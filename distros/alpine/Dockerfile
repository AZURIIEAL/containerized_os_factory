FROM alpine:latest

LABEL maintainer="AZURIIEAL <abinbinu678@gmail.com>"

ARG USER_NAME=azuriieal
ARG USER_PASSWORD=1234
ARG VNC_PASSWORD=123456

RUN apk add --no-cache \
    sudo \
    git \
    xfce4 \
    faenza-icon-theme \
    bash \
    python3 \
    tigervnc \
    xfce4-terminal \
    && adduser -h /home/${USER_NAME} -s /bin/bash -S -D ${USER_NAME} \
    && echo -e "${USER_PASSWORD}\n${USER_PASSWORD}" | passwd ${USER_NAME} \
    && echo "${USER_NAME} ALL=(ALL) NOPASSWD:ALL" >> /etc/sudoers \
    && git clone https://github.com/novnc/noVNC /opt/noVNC \
    && git clone https://github.com/novnc/websockify /opt/noVNC/utils/websockify

USER ${USER_NAME}
WORKDIR /home/${USER_NAME}

RUN mkdir -p ~/.vnc \
    && echo -e "#!/bin/bash\nstartxfce4 &" > ~/.vnc/xstartup \
    && chmod +x ~/.vnc/xstartup \
    && echo -e "${VNC_PASSWORD}\n${VNC_PASSWORD}\nn\n" | vncpasswd

USER root

RUN printf '#!/bin/bash\n/usr/bin/vncserver :99 &\n/opt/noVNC/utils/novnc_proxy --vnc 127.0.0.1:5999\n' > /entry.sh

RUN chmod +x /entry.sh

USER ${USER_NAME}

CMD ["/bin/bash", "/entry.sh"]
