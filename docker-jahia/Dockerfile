ARG IMAGE_REPO_CLASSIFIER
ARG PARENT_TAG

FROM jahia/jahia-ee${IMAGE_REPO_CLASSIFIER}:${PARENT_TAG}

ARG PROJECT_VERSION
ARG C_USER="tomcat"
ARG C_GROUP="tomcat"

LABEL maintainer="Jahia Product team <product@jahia.com>"

RUN mkdir -p /usr/local/tomcat/digital-factory-data/patches/provisioning
COPY headless-industrial.zip /tmp
COPY target/dependency/jahia-additional-modules-*-provisioning.yaml /usr/local/tomcat/digital-factory-data/patches/provisioning/3.1-jahia-additional-modules.yaml
COPY 5.1-mf.yaml /usr/local/tomcat/digital-factory-data/patches/provisioning
COPY 5.2-next.yaml /usr/local/tomcat/digital-factory-data/patches/provisioning
COPY 5.3-config.graphql /usr/local/tomcat/digital-factory-data/patches/provisioning

COPY --chown=${C_USER}:${C_GROUP} target/repository /home/${C_USER}/.m2/repository/
