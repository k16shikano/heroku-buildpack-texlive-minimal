# Use Heroku-24 as the base image
FROM heroku/heroku:24

# Ensure we're running as root
USER root

# Set the TeX Live year as a build argument so we can pass the latest year dynamically
ARG TEXLIVE_YEAR=2024

# Set non-interactive environment for apt-get and texlive installation
ENV DEBIAN_FRONTEND=noninteractive

# Install required dependencies
RUN apt-get update && apt-get install -y wget tar perl xz-utils

# Copy the minimal profile file into the container
COPY minimal.profile /tmp/minimal.profile

# Download the latest TeX Live installer
RUN wget https://texlive.texjp.org/$TEXLIVE_YEAR/tlnet/install-tl-unx.tar.gz -O /tmp/install-tl-unx.tar.gz \
    && tar -xzf /tmp/install-tl-unx.tar.gz -C /tmp \
    && rm /tmp/install-tl-unx.tar.gz

# Run the TeX Live installer with the minimal profile and repository settings
RUN cd /tmp/install-tl-* \
    && ./install-tl --profile /tmp/minimal.profile --repository http://texlive.texjp.org/$TEXLIVE_YEAR/tlnet \
    && rm -rf /tmp/install-tl-*

# Create a tarball of the installed TeX Live directory
RUN tar -czf /tmp/texlive.tar.gz -C /opt texlive

# Command to copy the resulting tarball outside of the container (manual step needed after build)
CMD ["cp", "/tmp/texlive.tar.gz", "/output"]
