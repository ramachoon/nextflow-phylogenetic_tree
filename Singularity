Bootstrap: docker
From: debian:stretch

%files
lib/*.py /usr/local/
MEGAN.vmoptions /usr/local/

%labels
Maintainer	max-emil.schon@icm.uu.se

%environment
PYTHONPATH='/usr/local/custom_python3_lib/'
export PYTHONPATH
VERSION_MEGAN="6.11.1"
export VERSION_MEGAN
VERSION_PYTHON3=$(python3 --version | cut -d' ' -f2)
export VERSION_PYTHON3
VERSION_MAFFT="7.312"
export VERSION_MAFFT
VERSION_TRIMAL=$(trimal --version | cut -d ' ' -f2)
export VERSION_TRIMAL
VERSION_DIAMOND="0.9.21"
export VERSION_DIAMOND
VERSION_FASTQ_DUMP=$(fastq-dump --version | cut -d':' -f2)
export VERSION_FASTQ_DUMP
VERSION_FASTTREE=$(FastTree 2>&1 >/dev/null | grep Usage | cut -d' ' -f 5)
export VERSION_FASTTREE
VERSION_IQTREE="1.6.3"
export VERSION_IQTREE
VERSION_ETE3=$(ete3 version | cut -d' ' -f1)
export VERSION_ETE3
VERSION_BIOPYTHON=$(python3 -c 'import Bio; print(Bio.__version__)')
export VERSION_BIOPYTHON
VERSION_PANDAS=$(python3 -c 'import pandas; print(pandas.__version__)')
export VERSION_PANDAS
VERSION_NUMPY=$(python3 -c 'import numpy; print(numpy.__version__)')
export VERSION_NUMPY

%post
######## base system ########
apt-get update
apt-get clean
apt-get install --no-install-recommends -qy \
                  default-jdk \
                  git \
                  wget \
                  vim \
                  tk \
                  libxml-simple-perl \
                  libtime-piece-perl \
                  libdigest-md5-file-perl \
                  python3 \
                  python3-pip \
                  python3-setuptools \
                  python3-dev \
                  python3-tk \
                  qt5-default \
                  xvfb \
                  build-essential
                  # cpanminus \

######## MEGAN6 ########
cd /usr/local/
wget http://ab.inf.uni-tuebingen.de/data/software/megan6/download/MEGAN_Community_unix_6_11_1.sh
chmod +x MEGAN_Community_unix_6_11_1.sh
./MEGAN_Community_unix_6_11_1.sh -q
mv /usr/local/MEGAN.vmoptions /usr/local/megan/MEGAN.vmoptions

######## python ########
pip3 install wheel biopython ete3 scipy pandas seaborn xvfbwrapper pyqt5
mkdir -p /usr/local/custom_python3_lib/
mv /usr/local/*.py /usr/local/custom_python3_lib/

######## MAFFT #########
cd /usr/local/
wget https://mafft.cbrc.jp/alignment/software/mafft-7.312-without-extensions-src.tgz
tar xfvz mafft-7.312-without-extensions-src.tgz
cd  /usr/local/mafft-7.312-without-extensions/core
make clean
make
make install

######## trimal #########
cd /usr/local
git clone https://github.com/scapella/trimal.git
cd trimal/source
make
ln -s /usr/local/trimal/source/trimal /usr/local/bin/

######## diamond #########
cd /usr/local
mkdir diamond
cd diamond
wget https://github.com/bbuchfink/diamond/releases/download/v0.9.21/diamond-linux64.tar.gz
tar xzf diamond-linux64.tar.gz
ln -s /usr/local/diamond/diamond /usr/local/bin/

######## SRA toolkit #########
apt-get install --no-install-recommends -qy sra-toolkit

######## fasttree #########
cd /usr/local/bin
wget http://www.microbesonline.org/fasttree/FastTreeMP
chmod +x FastTreeMP
cp FastTreeMP FastTree

######## IQtree #########
cd /usr/local
wget https://github.com/Cibiv/IQ-TREE/releases/download/v1.6.3/iqtree-1.6.3-Linux.tar.gz
tar -xvf iqtree-1.6.3-Linux.tar.gz
ln -s /usr/local/iqtree-1.6.3-Linux/bin/iqtree /usr/local/bin/iqtree

%test
/usr/local/megan/tools/daa-meganizer -h
/usr/local/megan/tools/gc-assembler -h
trimal --version
mafft --version
diamond version
fastq-dump --version
which FastTree
iqtree -h
python3 --version
ete3 version

%runscript
echo "MEGAN6: "$VERSION_MEGAN
echo "trimal: "$VERSION_TRIMAL
echo "MAFFT: "$VERSION_MAFFT
echo "diamond: "$VERSION_DIAMOND
echo "fastq-dump: "$VERSION_FASTQ_DUMP
echo "FastTree: "$VERSION_FASTTREE
echo "IQ-TREE: "$VERSION_IQTREE
echo "Python3: "$VERSION_PYTHON3
echo "Biopython: "$VERSION_BIOPYTHON
echo "Pandas: "$VERSION_PANDAS
echo "Numpy: "$VERSION_NUMPY
echo "ETE3: "$VERSION_ETE3
