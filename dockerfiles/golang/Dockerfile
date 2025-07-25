FROM jenkins/ssh-agent:6.23.0-jdk21 as ssh-agent

# ca-certificates because curl uses certificates from ca-certificates
RUN apt-get update && apt-get install -y --no-install-recommends ca-certificates curl && apt-get clean && \
  rm -rf /var/lib/apt/lists/*

# Now time to install golang
ARG GOLANG_VERSION=1.24.5
ARG TARGETARCH
ENV ARCHITECTURE=$TARGETARCH

# Set SHELL flags for RUN commands to allow -e and pipefail
# Rationale:https://github.com/hadolint/hadolint/wiki/DL4006
SHELL ["/bin/bash", "-eo", "pipefail", "-c"]

RUN curl -sS -L -O --output-dir /tmp/ --create-dirs "https://go.dev/dl/go${GOLANG_VERSION}.linux-${ARCHITECTURE}.tar.gz" && \
    rm -rf /usr/local/go && tar -C /usr/local -xzf /tmp/go"$GOLANG_VERSION".linux-${ARCHITECTURE}.tar.gz && \
    rm -rf /tmp/go"$GOLANG_VERSION".linux-${ARCHITECTURE}.tar.gz

ENV PATH=$PATH:/usr/local/go/bin

RUN echo "PATH=${PATH}" >> /etc/environment && chown -R jenkins:jenkins "${JENKINS_AGENT_HOME}" && \
    go version
