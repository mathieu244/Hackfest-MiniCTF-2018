FROM ubuntu:16.04

# Base Docker for unpriviledge SSH
RUN apt-get update && apt-get install -y openssh-server
RUN mkdir /var/run/sshd
RUN mkdir /home/challenge/
RUN useradd -d /home/challenge/ -s /bin/bash challenge
RUN echo 'challenge:password' | chpasswd
RUN sed 's@session\s*required\s*pam_loginuid.so@session optional pam_loginuid.so@g' -i /etc/pam.d/sshd
ENV NOTVISIBLE "in users profile"
RUN echo "export VISIBLE=now" >> /etc/profile
RUN echo "AllowUsers challenge" >> /etc/ssh/sshd_config

# Target victim of the challenge
RUN useradd admin
RUN apt-get install -y python sudo vim gdb strace binutils ltrace git

RUN git clone https://github.com/longld/peda.git /home/challenge/peda
RUN echo "source ~/peda/peda.py" >> /home/challenge/.gdbinit

# Faulty permission
COPY main /home/challenge/main



EXPOSE 22
CMD ["/usr/sbin/sshd", "-D"]
                             
