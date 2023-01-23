FROM debian:latest

# URL to the BLAST source code
ARG BLAST_URL=https://ftp.ncbi.nlm.nih.gov/blast/executables/blast+

RUN apt-get update && \
  apt-get install -y wget

# Download the latest version of BLAST and install in /opt/blast.  Update
# /etc/profile to modify the PATH environment variable.
RUN \
  VERSION=$(wget --quiet --output-document - ${BLAST_URL}/VERSION) && \
  BLAST="ncbi-blast-$VERSION+-x64-linux.tar.gz" && \
  BLAST_ROOT="/opt/blast/$VERSION" && \
  BLAST_BIN="$BLAST_ROOT/bin" && \
  wget --output-document /tmp/blast.tar.gz "${BLAST_URL}/LATEST/${BLAST}" && \
  mkdir -p $BLAST_ROOT && \
  mkdir -p /tmp/blast && \
  tar -C /tmp/blast -xzvf /tmp/blast.tar.gz && \
  cp -R /tmp/blast/ncbi-blast-$VERSION+/* $BLAST_ROOT && \
  rm -rf /tmp/* && \
  rm /root/.wget-hsts && \
  sed -i '/PATH\=/ s@\"$@:'"$BLAST_BIN"'\"@' /etc/profile && \
  . /etc/profile

# Clean up
RUN apt-get remove -y wget && \
  apt-get autoremove -y

