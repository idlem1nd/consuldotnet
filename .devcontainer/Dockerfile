# [Choice] .NET Core version: 3.1, 2.1
ARG VARIANT=3.1
FROM mcr.microsoft.com/vscode/devcontainers/dotnetcore:${VARIANT}

# [Option] Install Node.js
ARG INSTALL_NODE="true"
ARG NODE_VERSION="lts/*"
RUN if [ "${INSTALL_NODE}" = "true" ]; then su vscode -c "source /usr/local/share/nvm/nvm.sh && nvm install ${NODE_VERSION} 2>&1"; fi

# [Option] Install Azure CLI
ARG INSTALL_AZURE_CLI="false"
COPY library-scripts/azcli-debian.sh /tmp/library-scripts/
RUN if [ "$INSTALL_AZURE_CLI" = "true" ]; then bash /tmp/library-scripts/azcli-debian.sh; fi \
    && apt-get clean -y && rm -rf /var/lib/apt/lists/* /tmp/library-scripts

# Install the consul server for unit tests to /bin/consul
# TODO we could parameterise this on version/arch, and checksum the download 
# inspired by https://github.com/hashicorp/docker-consul/blob/master/0.X/Dockerfile
RUN mkdir -p /tmp/consul-build && \
    cd /tmp/consul-build && \
    wget https://releases.hashicorp.com/consul/1.8.5/consul_1.8.5_linux_amd64.zip && \
    unzip -d /bin consul_1.8.5_linux_amd64.zip && \
    # cleanup
    cd /tmp && \
    rm -rf /tmp/consul-build && \
    # smoke test our install
    consul version

# [Optional] Uncomment this section to install additional OS packages.
# RUN apt-get update && export DEBIAN_FRONTEND=noninteractive \
#     && apt-get -y install --no-install-recommends <your-package-list-here>

# [Optional] Uncomment this line to install global node packages.
# RUN su vscode -c "source /usr/local/share/nvm/nvm.sh && npm install -g <your-package-here>" 2>&1