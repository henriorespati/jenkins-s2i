FROM ocp-tools-4/jenkins-agent-base-rhel8:v4.13.0

# Labels consumed by Red Hat build service
LABEL com.redhat.component="rh-dotnet60-jenkins-agent-container" \
      name="dotnet/dotnet-60-jenkins-agent-rhel8" \
      version="6.0" \
      architecture="x86_64" \
      release="1" \
      io.k8s.display-name="Jenkins Agent .NET Core 6.0" \
      io.k8s.description="The jenkins agent dotnet image has the dotnet tools on top of the jenkins agent base image." \
      io.openshift.tags="openshift,jenkins,agent,dotnet,dotnet31"

# Don't download/extract docs for nuget packages
# Enable dotnet scl
ENV DOTNET_CORE_VERSION=6.0 \
    BASH_ENV=/usr/local/bin/scl_enable \
    ENV=/usr/local/bin/scl_enable \
    PROMPT_COMMAND=". /usr/local/bin/scl_enable" \
    ENABLED_COLLECTIONS="rh-dotnet60" \
    NUGET_XMLDOC_MODE=skip

#COPY contrib/bin/scl_enable /usr/local/bin/scl_enable

# Install
RUN INSTALL_PKGS="libicu dotnet-sdk-6.0" && \
    yum install -y --setopt=tsflags=nodocs --disablerepo=\* \
      --enablerepo=rhel-8-for-x86_64-baseos-rpms,rhel-8-for-x86_64-appstream-rpms \
      $INSTALL_PKGS && \
    rpm -V $INSTALL_PKGS && \
    yum clean all -y && \
# yum cache files may still exist (and quite large in size)
    rm -rf /var/cache/yum/*

# Trigger first time actions.
RUN dotnet --info

RUN chown -R 1001:0 $HOME && \
    chmod -R g+rw $HOME

USER 1001