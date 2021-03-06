# Generated by: Neurodocker version 0.7.0+0.gdc97516.dirty
# Latest release: Neurodocker version 0.7.0
# Timestamp: 2021/01/22 13:09:08 UTC
# 
# Thank you for using Neurodocker. If you discover any issues
# or ways to improve this software, please submit an issue or
# pull request on our GitHub repository:
# 
#     https://github.com/ReproNim/neurodocker

FROM neurodebian:stretch

USER root

ARG DEBIAN_FRONTEND="noninteractive"

RUN apt-get update -qq \
    && apt-get install -y -q --no-install-recommends neurodebian-freeze \
    && nd_freeze  20210121 \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

ENV LANG="en_US.UTF-8" \
    LC_ALL="en_US.UTF-8" \
    ND_ENTRYPOINT="/neurodocker/startup.sh"
RUN export ND_ENTRYPOINT="/neurodocker/startup.sh" \
    && apt-get update -qq \
    && apt-get install -y -q --no-install-recommends \
           apt-utils \
           bzip2 \
           ca-certificates \
           curl \
           locales \
           unzip \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/* \
    && sed -i -e 's/# en_US.UTF-8 UTF-8/en_US.UTF-8 UTF-8/' /etc/locale.gen \
    && dpkg-reconfigure --frontend=noninteractive locales \
    && update-locale LANG="en_US.UTF-8" \
    && chmod 777 /opt && chmod a+s /opt \
    && mkdir -p /neurodocker \
    && if [ ! -f "$ND_ENTRYPOINT" ]; then \
         echo '#!/usr/bin/env bash' >> "$ND_ENTRYPOINT" \
    &&   echo 'set -e' >> "$ND_ENTRYPOINT" \
    &&   echo 'export USER="${USER:=`whoami`}"' >> "$ND_ENTRYPOINT" \
    &&   echo 'if [ -n "$1" ]; then "$@"; else /usr/bin/env bash; fi' >> "$ND_ENTRYPOINT"; \
    fi \
    && chmod -R 777 /neurodocker && chmod a+s /neurodocker

ENTRYPOINT ["/neurodocker/startup.sh"]

RUN apt-get update -qq \
    && apt-get install -y -q --no-install-recommends \
           feh \
           gv \
           tree \
           libdbus-glib-1-2 \
           libjpeg62 \
           libgtk2.0-0 \
           libdbus-glib-1-2 \
           libjpeg62 \
           libgtk2.0-0 \
           libgl1-mesa-dev \
           libgl1-mesa-dri \
           libglu1-mesa-dev \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

ENV PATH="/opt/afni-latest:$PATH" \
    AFNI_PLUGINPATH="/opt/afni-latest"
RUN apt-get update -qq \
    && apt-get install -y -q --no-install-recommends \
           ed \
           gsl-bin \
           libglib2.0-0 \
           libglu1-mesa-dev \
           libglw1-mesa \
           libgomp1 \
           libjpeg62 \
           libxm4 \
           netpbm \
           tcsh \
           xfonts-base \
           xvfb \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/* \
    && curl -sSL --retry 5 -o /tmp/toinstall.deb http://mirrors.kernel.org/debian/pool/main/libx/libxp/libxp6_1.0.2-2_amd64.deb \
    && dpkg -i /tmp/toinstall.deb \
    && rm /tmp/toinstall.deb \
    && curl -sSL --retry 5 -o /tmp/toinstall.deb http://snapshot.debian.org/archive/debian-security/20160113T213056Z/pool/updates/main/libp/libpng/libpng12-0_1.2.49-1%2Bdeb7u2_amd64.deb \
    && dpkg -i /tmp/toinstall.deb \
    && rm /tmp/toinstall.deb \
    && apt-get install -f \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/* \
    && gsl2_path="$(find / -name 'libgsl.so.19' || printf '')" \
    && if [ -n "$gsl2_path" ]; then \
         ln -sfv "$gsl2_path" "$(dirname $gsl2_path)/libgsl.so.0"; \
    fi \
    && ldconfig \
    && echo "Downloading AFNI ..." \
    && mkdir -p /opt/afni-latest \
    && curl -fsSL --retry 5 https://afni.nimh.nih.gov/pub/dist/tgz/linux_openmp_64.tgz \
    | tar -xz -C /opt/afni-latest --strip-components 1

ENV ANTSPATH="/opt/ants-2.3.1" \
    PATH="/opt/ants-2.3.1:$PATH"
RUN echo "Downloading ANTs ..." \
    && mkdir -p /opt/ants-2.3.1 \
    && curl -fsSL --retry 5 https://dl.dropbox.com/s/1xfhydsf4t4qoxg/ants-Linux-centos6_x86_64-v2.3.1.tar.gz \
    | tar -xz -C /opt/ants-2.3.1 --strip-components 1

ENV FREESURFER_HOME="/opt/freesurfer-6.0.0-min" \
    PATH="/opt/freesurfer-6.0.0-min/bin:$PATH"
RUN apt-get update -qq \
    && apt-get install -y -q --no-install-recommends \
           bc \
           libgomp1 \
           libxmu6 \
           libxt6 \
           perl \
           tcsh \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/* \
    && echo "Downloading FreeSurfer ..." \
    && mkdir -p /opt/freesurfer-6.0.0-min \
    && curl -fsSL --retry 5 https://dl.dropbox.com/s/nnzcfttc41qvt31/recon-all-freesurfer6-3.min.tgz \
    | tar -xz -C /opt/freesurfer-6.0.0-min --strip-components 1 \
    && sed -i '$isource "/opt/freesurfer-6.0.0-min/SetUpFreeSurfer.sh"' "$ND_ENTRYPOINT"

COPY ["license.txt", "/opt/freesurfer-6.0.0-min/"]

COPY ["neuroconda.yml", "/"]

ENV FSLDIR="/opt/fsl-6.0.3" \
    PATH="/opt/fsl-6.0.3/bin:$PATH" \
    FSLOUTPUTTYPE="NIFTI_GZ" \
    FSLMULTIFILEQUIT="TRUE" \
    FSLTCLSH="/opt/fsl-6.0.3/bin/fsltclsh" \
    FSLWISH="/opt/fsl-6.0.3/bin/fslwish" \
    FSLLOCKDIR="" \
    FSLMACHINELIST="" \
    FSLREMOTECALL="" \
    FSLGECUDAQ="cuda.q"
RUN apt-get update -qq \
    && apt-get install -y -q --no-install-recommends \
           bc \
           dc \
           file \
           libfontconfig1 \
           libfreetype6 \
           libgl1-mesa-dev \
           libgl1-mesa-dri \
           libglu1-mesa-dev \
           libgomp1 \
           libice6 \
           libxcursor1 \
           libxft2 \
           libxinerama1 \
           libxrandr2 \
           libxrender1 \
           libxt6 \
           sudo \
           wget \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/* \
    && echo "Downloading FSL ..." \
    && mkdir -p /opt/fsl-6.0.3 \
    && curl -fsSL --retry 5 https://fsl.fmrib.ox.ac.uk/fsldownloads/fsl-6.0.3-centos6_64.tar.gz \
    | tar -xz -C /opt/fsl-6.0.3 --strip-components 1 \
    && sed -i '$iecho Some packages in this Docker container are non-free' $ND_ENTRYPOINT \
    && sed -i '$iecho If you are considering commercial use of this container, please consult the relevant license:' $ND_ENTRYPOINT \
    && sed -i '$iecho https://fsl.fmrib.ox.ac.uk/fsl/fslwiki/Licence' $ND_ENTRYPOINT \
    && sed -i '$isource $FSLDIR/etc/fslconf/fsl.sh' $ND_ENTRYPOINT \
    && echo "Installing FSL conda environment ..." \
    && bash /opt/fsl-6.0.3/etc/fslconf/fslpython_install.sh -f /opt/fsl-6.0.3

ENV FORCE_SPMMCR="1" \
    SPM_HTML_BROWSER="0" \
    LD_LIBRARY_PATH="$LD_LIBRARY_PATH:/usr/lib/x86_64-linux-gnu:/opt/matlabmcr-2010a/v713/runtime/glnxa64:/opt/matlabmcr-2010a/v713/bin/glnxa64:/opt/matlabmcr-2010a/v713/sys/os/glnxa64:/opt/matlabmcr-2010a/v713/extern/bin/glnxa64" \
    MATLABCMD="/opt/matlabmcr-2010a/v713/toolbox/matlab"
RUN export TMPDIR="$(mktemp -d)" \
    && apt-get update -qq \
    && apt-get install -y -q --no-install-recommends \
           bc \
           libncurses5 \
           libxext6 \
           libxmu6 \
           libxpm-dev \
           libxt6 \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/* \
    && echo "Downloading MATLAB Compiler Runtime ..." \
    && curl -sSL --retry 5 -o /tmp/toinstall.deb http://mirrors.kernel.org/debian/pool/main/libx/libxp/libxp6_1.0.2-2_amd64.deb \
    && dpkg -i /tmp/toinstall.deb \
    && rm /tmp/toinstall.deb \
    && apt-get install -f \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/* \
    && curl -fsSL --retry 5 -o "$TMPDIR/MCRInstaller.bin" https://dl.dropbox.com/s/zz6me0c3v4yq5fd/MCR_R2010a_glnxa64_installer.bin \
    && chmod +x "$TMPDIR/MCRInstaller.bin" \
    && "$TMPDIR/MCRInstaller.bin" -silent -P installLocation="/opt/matlabmcr-2010a" \
    && rm -rf "$TMPDIR" \
    && unset TMPDIR \
    && echo "Downloading standalone SPM ..." \
    && curl -fsSL --retry 5 -o /tmp/spm12.zip https://www.fil.ion.ucl.ac.uk/spm/download/restricted/utopia/previous/spm12_r7771_R2010a.zip \
    && unzip -q /tmp/spm12.zip -d /tmp \
    && mkdir -p /opt/spm12-r7771 \
    && mv /tmp/spm12/* /opt/spm12-r7771/ \
    && chmod -R 777 /opt/spm12-r7771 \
    && rm -rf /tmp/spm* \
    && /opt/spm12-r7771/run_spm12.sh /opt/matlabmcr-2010a/v713 quit \
    && sed -i '$iexport SPMMCRCMD=\"/opt/spm12-r7771/run_spm12.sh /opt/matlabmcr-2010a/v713 script\"' $ND_ENTRYPOINT

RUN apt-get update -qq \
    && apt-get install -y -q --no-install-recommends \
           openbox \
           tightvncserver \
           xfonts-base \
           xterm \
           xvfb \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/* \
    && echo 'Configuring VNC password ...' \
    && mkdir -p ~/.vnc \
    && echo neuroconda | vncpasswd -f > ~/.vnc/passwd \
    && chmod 0600 ~/.vnc/passwd \
    && sed -i '$iexport USER=$(whoami)' $ND_ENTRYPOINT \
    && sed -i '$iexport DISPLAY=$HOSTNAME:1' $ND_ENTRYPOINT \
    && sed -i '$ivncserver -geometry 1920x1080' $ND_ENTRYPOINT

ENV CONDA_DIR="/opt/miniconda-py38_4.9.2" \
    PATH="/opt/miniconda-py38_4.9.2/bin:$PATH"
RUN export PATH="/opt/miniconda-py38_4.9.2/bin:$PATH" \
    && echo "Downloading Miniconda installer ..." \
    && conda_installer="/tmp/miniconda.sh" \
    && curl -fsSL --retry 5 -o "$conda_installer" https://repo.continuum.io/miniconda/Miniconda3-py38_4.9.2-Linux-x86_64.sh \
    && bash "$conda_installer" -b -p /opt/miniconda-py38_4.9.2 \
    && rm -f "$conda_installer" \
    && conda config --system --prepend channels conda-forge \
    && conda config --system --set auto_update_conda false \
    && conda config --system --set show_channel_urls true \
    && sync && conda clean -y --all && sync \
    && conda env create -q --name neuroconda --file neuroconda.yml \
    && rm -rf ~/.cache/pip/*

RUN sed -i '$i. /opt/freesurfer-6.0.0-min/SetUpFreeSurfer.sh && . /opt/miniconda-py38_4.9.2/etc/profile.d/conda.sh && conda activate neuroconda' $ND_ENTRYPOINT

RUN echo '{ \
    \n  "pkg_manager": "apt", \
    \n  "instructions": [ \
    \n    [ \
    \n      "base", \
    \n      "neurodebian:stretch" \
    \n    ], \
    \n    [ \
    \n      "ndfreeze", \
    \n      { \
    \n        "date": "20210121" \
    \n      } \
    \n    ], \
    \n    [ \
    \n      "install", \
    \n      [ \
    \n        "feh", \
    \n        "gv", \
    \n        "tree", \
    \n        "libdbus-glib-1-2", \
    \n        "libjpeg62", \
    \n        "libgtk2.0-0", \
    \n        "libdbus-glib-1-2", \
    \n        "libjpeg62", \
    \n        "libgtk2.0-0", \
    \n        "libgl1-mesa-dev", \
    \n        "libgl1-mesa-dri", \
    \n        "libglu1-mesa-dev" \
    \n      ] \
    \n    ], \
    \n    [ \
    \n      "afni", \
    \n      { \
    \n        "version": "latest", \
    \n        "method": "binaries" \
    \n      } \
    \n    ], \
    \n    [ \
    \n      "ants", \
    \n      { \
    \n        "version": "2.3.1", \
    \n        "method": "binaries" \
    \n      } \
    \n    ], \
    \n    [ \
    \n      "freesurfer", \
    \n      { \
    \n        "version": "6.0.0-min", \
    \n        "method": "binaries" \
    \n      } \
    \n    ], \
    \n    [ \
    \n      "copy", \
    \n      [ \
    \n        "license.txt", \
    \n        "/opt/freesurfer-6.0.0-min/" \
    \n      ] \
    \n    ], \
    \n    [ \
    \n      "copy", \
    \n      [ \
    \n        "neuroconda.yml", \
    \n        "/" \
    \n      ] \
    \n    ], \
    \n    [ \
    \n      "fsl", \
    \n      { \
    \n        "version": "6.0.3", \
    \n        "method": "binaries" \
    \n      } \
    \n    ], \
    \n    [ \
    \n      "spm12", \
    \n      { \
    \n        "version": "r7771", \
    \n        "method": "binaries" \
    \n      } \
    \n    ], \
    \n    [ \
    \n      "vnc", \
    \n      { \
    \n        "passwd": "neuroconda", \
    \n        "start_at_runtime": true, \
    \n        "geometry": "1920x1080" \
    \n      } \
    \n    ], \
    \n    [ \
    \n      "miniconda", \
    \n      { \
    \n        "version": "py38_4.9.2", \
    \n        "create_env": "neuroconda", \
    \n        "yaml_file": "neuroconda.yml" \
    \n      } \
    \n    ], \
    \n    [ \
    \n      "add_to_entrypoint", \
    \n      ". /opt/freesurfer-6.0.0-min/SetUpFreeSurfer.sh && . /opt/miniconda-py38_4.9.2/etc/profile.d/conda.sh && conda activate neuroconda" \
    \n    ] \
    \n  ] \
    \n}' > /neurodocker/neurodocker_specs.json
