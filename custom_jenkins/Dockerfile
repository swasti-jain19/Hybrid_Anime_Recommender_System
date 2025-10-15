# Start from the base Jenkins LTS image
FROM jenkins/jenkins:lts

# Install Docker-in-Docker (DinD) prerequisites
USER root

# Corrected steps to install Docker, using modern best practices and avoiding the problematic package.
RUN apt-get update -y && \
    # Install required tools
    apt-get install -y apt-transport-https ca-certificates curl gnupg lsb-release && \
    
    # Add Docker's official GPG key
    install -m 0755 -d /etc/apt/keyrings && \
    curl -fsSL https://download.docker.com/linux/debian/gpg | gpg --dearmor -o /etc/apt/keyrings/docker.gpg && \
    chmod a+r /etc/apt/keyrings/docker.gpg && \
    
    # Add the Docker repository (Using a variable for Debian codename is safer)
    echo \
      "deb [arch=\"$(dpkg --print-architecture)\" signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/debian \
      \"$(. /etc/os-release && echo "$VERSION_CODENAME")\" stable" | tee /etc/apt/sources.list.d/docker.list > /dev/null && \
    
    # Install Docker components
    apt-get update -y && \
    apt-get install -y docker-ce docker-ce-cli containerd.io && \
    
    # Cleanup
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

# Add jenkins user to the docker group (assuming the jenkins user exists, which it does in the base image)
RUN usermod -aG docker jenkins

# Switch back to the jenkins user
USER jenkins


# checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[credentialsId: 'github-token', url: 'https://github.com/swasti-jain19/Hybrid_Anime_Recommender_System.git']])