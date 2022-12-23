# Dependencies management

## Theory
*1, 2. What is Docker, and how it differs from dependencies management systems? From virtual machines? What are the advantages and disadvantages of using containers over other approaches?*

Docker is a software that divides kernel into multiple isolated user space instances, called containers. These containers have their own software, libraries and configuration files, they can even been build on different operating systems. They are more than dependency management systems, because those only have a set of packages for different softwares while containers also can have OS configuration and a set of commands that should be run together with the container building. It makes extremely easy to reproduce the exact conditions for the service you are going to run inside the container. They are also different from virtual machines which must have its own operating system while docker containers share the host operating system. It makes docker containers ligther and easily portable, but more prone to security risks and vulnerabilities. Apart from that, VMs can emulate more than one OS at the moment, while docker containers are restricted to single one, which makes them easier to maintain and scale, but not applicable for problems where several OS are needed. Finally, once a VM is assigned to a resource, it takes up the whole space, even when it needs less, while containers resources can be allocated dynamically according to its needs.


*3. Explain how Docker works: what are Dockerfiles, how are containers created, and how are they run and destroyed?*

Docker uses a client-server architecture. The Docker client communicates using a REST API, over UNIX sockets or a network interface with Docker daemon. The Docker daemon manages Docker objects such as images, containers, networks, and volumes. An image is a read-only template with instructions for creating a Docker container. Often, an image is based on another image, with some additional customization. Docker can build images automatically by reading the instructions from a Dockerfile. A Dockerfile is a text document that contains all the commands a user could call on the command line to assemble an image. A container is a runnable instance of an image. Containers can be created, started, run, stoped, moved, or deleted -- all via commands to Docker client. Container is defined both by its image as well as any configuration options user provides to it when he/she creates or starts it. When a container is removed, any changes to its state that are not stored in persistent storage disappear. It is also possible create new image from the current state of container.

*5. Name and describe at least one Docker competitor (i.e., a tool based on the same containerization technology).*

First, let's define what exactly is the technology that Docker is build on. It is called `namespaces`. It takes advantage of several features of the Linux kernel to deliver its functionality i.e. provide the isolated workspace called the container. When the container is being created, Docker creates a set of `namespaces` for it, which provide a layer of isolation. Each aspect of a container runs in a separate `namespace` and its access is limited to that `namespace`.

The same technology is used in Podman, a container engine developed by RedHat. Podman maintains compatibility with the OCI container image spec just like Docker, meaning Podman can run container images produced by Docker and vice versa. Podman’s command line interface is identical to Docker’s, including the arguments. So, they are almost identical, except for the fact that Podman does not have daemon. Instead, the containers are started as child processes of the Podman process, heavily using user namespaces and network namespaces. The plus side of such change is that there’s no persistent connection to some long-living process, which failures automatically fail everything else. Podman also differentiates from Docker by using rootless containers by default.

*6. What is conda? How it differs from apt, yarn, and others?*

Conda is environment and package managing system. The ability to create virtual environments and install same packages, but with different versions to different environment, if needed, is what differs conda from apt, yarn and other tools for managing packages.

## Anaconda

Install conda, create virtual environment and install packages in it (we will need to specify channels for it).
```bash
#!/bin/bash

# getting conda
wget https://repo.continuum.io/archive/Anaconda3-2022.10-MacOSX-x86_64.sh
bash Anaconda3-2022.10-MacOSX-x86_64.sh

# as we get specific version, it is better to put update here for the future
conda update -n base -c defaults conda 

# creating environment
conda create --prefix ./envs/cool_bioinf_env 

# installing packages to environment
conda activate ./envs/cool_bioinf_env
conda config --add channels bioconda
conda config --add channels conda-forge  
conda install fastqc=0.11.9 
conda install STAR=2.7.10b 
conda install samtools=1.16.1 
conda install picard=2.27.5  # bioconda has only previous version, so we will need to push newest version in its repo
conda install bedtools=2.30.0 
conda install multiqc=1.13 
conda install salmon=1.9.0
```

From the required list we could not install picard with the version we need as bioconda has only previous version. So, we would need to:
- clone bioconda repo to our computer
- create new branch from master
- choose recipes dir by `cd recipes`
- add picard of the right version with `conda skeleton pypi picard=2.27.5`
- commit these changes, push then to repo and ask for pull request to master
- once it would be reviewed and merged by bioconda core team, we will need to delete our branch
...and then repeat picard installation.

Now we can export our environment to file and check if we can recreate it.

```bash
# exporting environment
conda env export cool_bioinf_env > cool_bioinf_env.yml
conda deactivate # to check if we can rebuild it from file

# checking if we can rebuild it
conda env create -f cool_bioinf_env.yml
conda activate cool_bioinf_env.yml
conda env list
```

The last command gives us info that we have 2 nvironments now: base and cool_bioinf_env, proving that we were able to recreate it.
How does our conda yml look like:
```bash
name: cool_bioinf_env
channels:
  - conda-forge
  - bioconda
  - defaults
dependencies:
  - bedtools=2.30.0=haa7f73a_2
  - boost-cpp=1.74.0=hdbf7018_7
  - brotlipy=0.7.0=py38hef030d1_1005
  - bzip2=1.0.8=h0d85af4_4
  - c-ares=1.18.1=h0d85af4_0
  - ca-certificates=2022.12.7=h033912b_0
  - certifi=2022.12.7=pyhd8ed1ab_0
  - cffi=1.15.1=py38hb368cf1_2
  - charset-normalizer=2.1.1=pyhd8ed1ab_0
  - click=8.1.3=unix_pyhd8ed1ab_2
  - coloredlogs=15.0.1=pyhd8ed1ab_3
  - colormath=3.0.0=py_2
  - commonmark=0.9.1=py_0
  - cryptography=38.0.4=py38ha6c3189_0
  - cycler=0.11.0=pyhd8ed1ab_0
  - dataclasses=0.8=pyhc8e2a94_3
  - expat=2.5.0=hf0c8a7f_0
  - fastqc=0.11.9=hdfd78af_1
  - font-ttf-dejavu-sans-mono=2.37=hd3eb1b0_0
  - fontconfig=2.14.1=h5bb23bf_0
  - freetype=2.12.1=hd8bbffd_0
  - future=0.18.2=pyhd8ed1ab_6
  - gdbm=1.18=hdccc71a_4
  - htslib=1.16=h567f53e_0
  - humanfriendly=10.0=py38h50d1736_4
  - icu=69.1=he49afe7_0
  - idna=3.4=pyhd8ed1ab_0
  - importlib-metadata=5.1.0=pyha770c72_0
  - jinja2=3.1.2=pyhd8ed1ab_1
  - kiwisolver=1.4.4=py38h98b9b1b_1
  - krb5=1.19.3=hb49756b_0
  - libblas=3.9.0=16_osx64_openblas
  - libcblas=3.9.0=16_osx64_openblas
  - libcurl=7.86.0=h57eb407_1
  - libcxx=14.0.6=h9765a3e_0
  - libdeflate=1.13=h775f41a_0
  - libedit=3.1.20191231=h0678c8f_2
  - libev=4.33=haf1e3a3_1
  - libffi=3.4.2=h0d85af4_5
  - libgfortran=5.0.0=9_5_0_h97931a8_26
  - libgfortran5=11.3.0=h082f757_26
  - libiconv=1.16=hca72f7f_2
  - libjemalloc=5.2.1=he49afe7_6
  - liblapack=3.9.0=16_osx64_openblas
  - libnghttp2=1.47.0=h7cbc4dc_1
  - libopenblas=0.3.21=openmp_h429af6e_3
  - libpng=1.6.37=ha441bb4_0
  - libssh2=1.10.0=h7535e13_3
  - libzlib=1.2.13=hfd90126_4
  - llvm-openmp=15.0.6=h61d9ccf_0
  - lzstring=1.0.4=py_1001
  - markdown=3.4.1=pyhd8ed1ab_0
  - markupsafe=2.1.1=py38hef030d1_2
  - matplotlib-base=3.2.2=py38h1300a51_1
  - multiqc=1.13=pyhdfd78af_0
  - ncurses=6.3=hca72f7f_3
  - networkx=2.8.8=pyhd8ed1ab_0
  - numpy=1.23.5=py38hc2f29e8_0
  - openjdk=11.0.13=h8346a28_0
  - openssl=1.1.1s=hfd90126_1
  - perl=5.34.0=h435f0c2_2
  - pip=22.3.1=pyhd8ed1ab_0
  - pycparser=2.21=pyhd8ed1ab_0
  - pygments=2.13.0=pyhd8ed1ab_0
  - pyopenssl=22.1.0=pyhd8ed1ab_0
  - pyparsing=3.0.9=pyhd8ed1ab_0
  - pysocks=1.7.1=pyha2e5f31_6
  - python=3.8.15=h218abb5_2
  - python-dateutil=2.8.2=pyhd8ed1ab_0
  - python_abi=3.8=2_cp38
  - pyyaml=6.0=py38hef030d1_5
  - readline=8.2=hca72f7f_0
  - requests=2.28.1=pyhd8ed1ab_1
  - rich=12.6.0=pyhd8ed1ab_0
  - rich-click=1.6.0=pyhd8ed1ab_0
  - salmon=1.9.0=ha8e4af9_1
  - samtools=1.16.1=h7e39424_1
  - setuptools=65.5.1=pyhd8ed1ab_0
  - simplejson=3.18.0=py38hef030d1_0
  - six=1.16.0=pyh6c4a22f_0
  - spectra=0.0.11=py_1
  - sqlite=3.40.0=h880c91c_0
  - star=2.7.10b=h527b516_0
  - tbb=2021.7.0=hb8565cd_1
  - tk=8.6.12=h5d9f67b_0
  - tornado=6.2=py38hef030d1_1
  - typing_extensions=4.4.0=pyha770c72_0
  - urllib3=1.26.13=pyhd8ed1ab_0
  - wheel=0.38.4=pyhd8ed1ab_0
  - xz=5.2.8=h6c40b1e_0
  - yaml=0.2.5=h0d85af4_2
  - zipp=3.11.0=pyhd8ed1ab_0
  - zlib=1.2.13=hfd90126_4
  - zstd=1.5.2=hfa58983_4
prefix: /Users/vvzimina/envs/cool_bioinf_env
```
## Docker

Создаем Dockerfile для создания образа с необходимыми нам пакетами.
```dockerfile
# syntax=docker/dockerfile:1
FROM ubuntu:22.10

# we need a lot of supplementary packages for our bioinformatic ones (especially for samtools), so it is easier to put them in the variable
ENV PACKAGES wget python3-pip bzip2 unzip cmake gcc make libbz2-dev zlib1g-dev libncurses5-dev libncursesw5-dev liblzma-dev gawk perl gnuplot ca-certificates git ant g++

# downloading fastqc archive that we can't get through wget
ADD http://www.bioinformatics.babraham.ac.uk/projects/fastqc/fastqc_v0.11.9.zip /tmp/

# creating args for versions of packages that we get trough wget to make our life easier
ARG SAMTOOLS_VERSION=1.16.1
ARG STAR_VERSION=2.7.10b
ARG SALMON_VERSION=1.9.0
ARG BEDTOOLS_VERSION=2.30.0

# doing all installations with one RUN command to avoid additional layers
RUN apt-get update &&\
    # installing suplementary packages 
    apt-get install --no-install-recommends -qq -y ${PACKAGES} &&\
    apt-get clean &&\ 
    cd /home && \
    # downloading and installing our packages with specific versions where it was possible
    # multiqc is installed easily with pip
    pip install multiqc==1.13 &&\
    # samtools installation from binary
    wget https://github.com/samtools/samtools/releases/download/${SAMTOOLS_VERSION}/samtools-${SAMTOOLS_VERSION}.tar.bz2 && \
    tar -vxjf samtools-${SAMTOOLS_VERSION}.tar.bz2 && \
    rm samtools-${SAMTOOLS_VERSION}.tar.bz2 && \
    cd samtools-${SAMTOOLS_VERSION} && \
    ./configure && \
    make && \
    make install && \
    # creating directory where samtools will store data
    mkdir /data && \
    # STAR installation from binary
    wget https://github.com/alexdobin/STAR/archive/${STAR_VERSION}.tar.gz &&\ 
    tar -xzf ${STAR_VERSION}.tar.gz &&\
    cd STAR-${STAR_VERSION}/source &&\
    make STAR &&\
    # creating directory for exe and copying it there
    mkdir /home/bin && \
    cp STAR /home/bin && \
    cd /home && \
    # cleaning rubbish
    rm -rf STAR-${STAR_VERSION} &&\
    # picard installation, could not find how to install specific version
    git clone https://github.com/broadinstitute/picard.git &&\ 
    cd picard/ &&\
    ./gradlew shadowJar &&\
    # salmon installation
    tar -zxvf /tmp/salmon-1.9.0_linux_x86_64.tar.gz &&\
    # needed to compile salmon
    git clone https://github.com/oneapi-src/oneTBB.git &&\
    cd salmon-1.9.0_linux_x86_64 &&\
    mkdir build &&\
    cd build &&\ 
    cmake -DFETCH_BOOST=TRUE -DTBB_INSTALL_DIR=../../oneTBB  ../../oneTBB &&\
    make &&\
    make install &&\ 
    # bedtools installation from binary
    wget https://github.com/arq5x/bedtools2/releases/download/v${BEDTOOLS_VERSION}/bedtools-${BEDTOOLS_VERSION}.tar.gz &&\
    tar -xzf bedtools-${BEDTOOLS_VERSION}.tar.gz &&\
    rm -rf bedtools-${BEDTOOLS_VERSION}.tar.gz  && \
    cd bedtools2 &&\
    make &&\
    cd bedtools2/bin &&\ 
    # grant rights to execute downloaded/compiled binaries
    chmod a+x $(ls) &&\
    # copying all bedtools binaries to our home dir
    cp $(ls) /home/bin/ &&\
    # installing fastqc from pre-downloaded archive
    unzip /tmp/fastqc_v0.11.9.zip &&\
    # grant rights to execute downloaded/compiled binaries
    chmod a+x FastQC/fastqc && \
    # creating symbolic link so we can open fastqc from our home path
    ln -s /FastQC/fastqc /home/bin/fastqc &&\
    # cleaning rubbish
    rm -rf /tmp/fastqc_v0.11.9.zip
    
# add bin dir to PATH
ENV PATH /home/bin:${PATH}


# add labels
LABEL author="Victoria Zimina" \
      maintainer="vzimina@nes.ru"

ARG BUILD_DATE 
ARG BUILD_VERSION
LABEL   org.label-schema.build-date=$BUILD_DATE \
        org.label-schema.version=$BUILD_VERSION \
        org.label-schema.description="RNA-seq analysis pipeline dependencies: fastqc=0.11.9, STAR=2.7.10b, samtools=1.16.1, picard=2.27.5, salmon=1.9.0, bedtools=2.30.0, multiqc=1.13" \
```

Собираем образ из Dockerfile и запускаем контейнер.

```dockerfile
# building image from Dockerfile
docker build -f hw1_zimina.dockerfile  -t hw1_zimina .

# run container from image
docker run --name hw1_zimina_container --rm -it hw1_zimina
```
