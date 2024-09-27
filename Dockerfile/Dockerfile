# FROM ubuntu:22.04  
  
# ARG BASE_IMAGE  
# ARG PACKAGE_FILE

# RUN echo "package file variable: ${PACKAGE_FILE}" > /message.txt  
# RUN echo "Base image variable: ${BASE_IMAGE}" > /message.txt  
  
# CMD ["cat", "/message.txt"]  





#ARG BASE_IMAGE 

ARG BASE_IMAGE  
FROM ${BASE_IMAGE} 
RUN echo "Using base image: ${BASE_IMAGE}"  # Debug statement 


ARG PACKAGE_FILE
RUN echo "Using package file: ${PACKAGE_FILE}"  # Debug statement
       
  
# Copy the package into the image      
COPY ${PACKAGE_FILE} /tmp/      
  
# Install the package based on the package type  
ARG BASE_IMAGE
# RUN if [ "${BASE_IMAGE#*:}" = "centos" ] || [ "${BASE_IMAGE#*:}" = "amazonlinux" ]; then \    
RUN if [[ "${BASE_IMAGE}" == amazonlinux* ]]; then \  
      # yum install -y epel-release && \  
      # yum install -y dnf-plugins-core wget && \  
      # yum clean all && \  
      yum update; yum install -y wget && \ 
      yum install -y https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm && \
      yum install -y tar && \
      yum install -y /tmp/${PACKAGE_FILE} && \  
      rm -f /tmp/${PACKAGE_FILE}; \  
    elif [[ "${BASE_IMAGE}" == centos* ]]; then \  
      sed -i 's|^mirrorlist=|#mirrorlist=|g' /etc/yum.repos.d/CentOS-* && \  
      sed -i 's|^#baseurl=http://mirror.centos.org|baseurl=http://vault.centos.org|g' /etc/yum.repos.d/CentOS-* && \  
      yum install -y epel-release && \  
      yum install -y dnf-plugins-core wget && \  
      yum config-manager --set-enabled powertools && \  
      yum clean all && \  
      yum install -y /tmp/${PACKAGE_FILE} && \  
      rm -f /tmp/${PACKAGE_FILE}; \  
    elif [[ "${BASE_IMAGE}" == ubuntu* ]]; then \  
      apt-get update && \  
      apt-get install -y wget && \  
      dpkg -i /tmp/${PACKAGE_FILE} || apt-get install -f -y && \  
      rm -f /tmp/${PACKAGE_FILE} && \  
      dpkg -l | grep xrt; \  
    else \  
      echo "Unsupported base image: ${BASE_IMAGE}"; \  
      exit 1; \  
    fi
