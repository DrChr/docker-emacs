FROM alpine:3.9 as dev

RUN apk add --no-cache \
        alsa-lib-dev \
        aspell \
        aspell-en \
        autoconf \
        automake \
        build-base \
        curl \
        dpkg \
        dpkg-dev \
        fontconfig-dev \
        gawk \
        gconf-dev \
        giflib-dev \
        git \
        glib-dev \
        gnupg \
        gnutls-dev \
        gtk+3.0-dev \
        lcms2-dev \
        libcanberra-gtk3 \
        libjpeg-turbo-dev \
        libpng-dev \
        librsvg-dev \
        libxaw-dev \
        libxml2-dev \
        jansson-dev \
        ncurses-dev \
        openssh-client \
        pango-dev \
        paxmark \
        python \
        texinfo \
        tiff-dev \
    && /bin/true

ENV EMACS_BRANCH="{{BRANCH}}"
ENV EMACS_VERSION="{{VERSION}}"

COPY source /opt/emacs/

RUN cd /opt/emacs && \
    if [ -d patches ]; then git apply patches/*; fi && \
    ./autogen.sh && \
    ./configure{{CONFIGURE}} && \
    make -j 8 && \
    make install

RUN mkdir -p /root/.emacs.d/elpa/gnupg && \
    chmod 700 /root/.emacs.d/elpa/gnupg && \
    gpg --homedir /root/.emacs.d/elpa/gnupg --keyserver hkp://ipv4.pool.sks-keyservers.net --recv-keys 066DAFCB81E42C40

RUN curl -fsSL https://raw.githubusercontent.com/cask/cask/master/go | python
ENV PATH="/root/.cask/bin:$PATH"

CMD ["emacs"]

# ------------------------------------------------------------

FROM alpine:3.9

RUN apk add --no-cache \
        alsa-lib \
        aspell \
        aspell-en \
        curl \
        desktop-file-utils \
        gconf \
        giflib \
        gnupg \
        gtk+3.0 \
        lcms2 \
        libcanberra-gtk3 \
        librsvg \
        libxpm \
        jansson \
        openssh-client \
        tiff \
    && /bin/true

ENV EMACS_BRANCH="{{BRANCH}}"
ENV EMACS_VERSION="{{VERSION}}"

COPY --from=0 /root/.emacs.d /root/.emacs.d
COPY --from=0 /usr/local /usr/local

CMD ["emacs"]
