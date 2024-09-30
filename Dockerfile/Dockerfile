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
      yum update -y && \
      yum install -y wget gnupg2 && \
      amazon-linux-extras install epel -y && \
      yum install -y ocl-icd ocl-icd-devel && \
      # yum install -y opencl-headers && \  
      # yum clean all && \  
      # yum install -y https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm && \
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
    elif echo "${BASE_IMAGE}" | grep -q "ubuntu"; then \   
      bash -c 'apt-get update && \    
      apt-get install -y wget && \   
      apt-get install -y wget systemd linux-headers-$(uname -r) || apt-get install -y linux-headers-generic && \  
      apt-get install -y ocl-icd-libopencl1 lsb-release dkms udev python3 ocl-icd-opencl-dev uuid-dev libboost-filesystem1.65.1 libboost-program-options1.65.1 libboost-system1.65.1 libprotobuf10 libyaml-0-2 && \  
      dpkg -i /tmp/${PACKAGE_FILE} || apt-get install -f -y && \    
      rm -f /tmp/${PACKAGE_FILE} && \    
      dpkg -l | grep xrt'; \   
    else \  
      echo "Unsupported base image: ${BASE_IMAGE}"; \  
      exit 1; \  
    fi
