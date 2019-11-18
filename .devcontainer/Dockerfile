FROM rubensa/ubuntu-tini-dev

# $(id -un)
ARG NEW_USER_NAME=rubensa
# $(id -gn)
ARG NEW_GROUP_NAME=rubensa

# Tell docker that all future commands should be run as root
USER root

# Set root home directory
ENV HOME=/root

RUN curl -s https://archive.apache.org/dist/spark/spark-2.3.2/spark-2.3.2-bin-hadoop2.7.tgz | tar -xz -C /opt/ \
    #
    # Assign group folder ownership
    && chgrp -R ${GROUP_NAME} /opt/spark-2.3.2-bin-hadoop2.7 \
    #
    # Set the segid bit to the folder
    && chmod -R g+s /opt/spark-2.3.2-bin-hadoop2.7 \
    #
    # Give write and exec acces so anyobody can use it
    && chmod -R ga+wX /opt/spark-2.3.2-bin-hadoop2.7 \
    #
    # Configure spark for the non-root user
    && printf "\nexport SPARK2_HOME=/opt/spark-2.3.2-bin-hadoop2.7\nexport PATH=\$PATH:\$SPARK2_HOME/bin\n" >> /home/${USER_NAME}/.bashrc \
    #
    # Custom username
    && usermod -l ${NEW_USER_NAME} -d /home/${NEW_USER_NAME} -m ${USER_NAME} \
    #
    # Custom groupname
    && groupmod -n ${NEW_GROUP_NAME} ${GROUP_NAME} \
    #
    # Update fixuid config
    && printf "user: ${NEW_USER_NAME}\ngroup: ${NEW_GROUP_NAME}\npaths:\n  - /home/${NEW_USER_NAME}" > /etc/fixuid/config.yml

ENV USER_NAME=${NEW_USER_NAME}
ENV GROUP_NAME=${NEW_GROUP_NAME}

# Tell docker that all future commands should be run as the non-root user
USER ${USER_NAME}

# Set user home directory (see: https://github.com/microsoft/vscode-remote-release/issues/852)
ENV HOME /home/${USER_NAME}

# Set default working directory to user home directory
WORKDIR ${HOME}

# Create java environment
RUN /bin/bash -l -c "export SDKMAN_DIR=/opt/sdkman;. /opt/sdkman/bin/sdkman-init.sh; sdk install java 8.0.232.hs-adpt"

# Create python environment and install packages
RUN /bin/bash -l -c ". /opt/conda/etc/profile.d/conda.sh; conda create -y -n pyspark python=3.7; conda install -y -n pyspark conda-build pylint" \
    # Activate project environment
    && printf "\nconda activate pyspark\n" >> /home/${USER_NAME}/.bashrc

# Keep container running (for use in VSCode)
CMD [ "tail", "-f", "/dev/null" ]