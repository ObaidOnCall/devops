# Use Ubuntu Jammy as the base image
FROM ubuntu:jammy

# Set working directory
WORKDIR /home


# Set environment variables to avoid timezone prompts
ENV DEBIAN_FRONTEND=noninteractive
ENV TZ=Europe/London

# Update apt and install necessary packages in a single layer to keep the image small
RUN apt-get update && \
    apt-get install -y nano tzdata && \
    rm -rf /var/lib/apt/lists/*

ADD https://install.citusdata.com/community/deb.sh  ./add-citus-repo.sh
# Combine all setup steps into a single RUN command to minimize layers
RUN apt-get update && \
    apt-get install -y nano curl && \
    bash add-citus-repo.sh && \
    apt-get update && \
    apt-get install -y postgresql-15-citus-12.0 && \
    pg_conftool 15 main set shared_preload_libraries citus && \
    rm -rf /var/lib/apt/lists/*


# Start PostgreSQL as the entrypoint
ENTRYPOINT ["service", "postgresql", "start"]
