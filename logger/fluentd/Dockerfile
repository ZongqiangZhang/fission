# Fluentd docker file

FROM centos:centos7

COPY td.repo /etc/yum.repos.d/
RUN ulimit -n 65535 ** \
    yum check-update && \
    yum install -y td-agent && \
    sed -i -e "s/USER=td-agent/USER=root/" -e "s/GROUP=td-agent/GROUP=root/" /etc/init.d/td-agent && \
    td-agent-gem install --no-document fluent-plugin-record-reformer && \
    td-agent-gem install --no-document fluent-plugin-influxdb && \
    # Remove unnecessary files
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* \
        /opt/td-agent/embedded/share/doc \
        /opt/td-agent/embedded/share/gtk-doc \
        /opt/td-agent/embedded/lib/postgresql \
        /opt/td-agent/embedded/bin/postgres \
        /opt/td-agent/embedded/share/postgresql \
        /etc/td-agent/td-agent.conf

# Copy the Fluentd configuration file for logging Docker container logs.
COPY fluent.conf /etc/td-agent/td-agent.conf

# Copy the entrypoint for the container
COPY run.sh /run.sh
RUN ["chmod", "+x", "/run.sh"]

volume /var/log/lamda

# Start Fluentd to pick up our config that watches Docker container logs.
CMD /run.sh $FLUENTD_ARGS