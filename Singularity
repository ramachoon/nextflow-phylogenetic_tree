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
VERSION_PRANK=$(prank -version | grep PRANK | cut -d' ' -f4)
export VERSION_PRANK
VERSION_EPA_NG=$(epa-ng --version | cut -d' ' -f 2)
export VERSION_EPA_NG
VERSION_PAPARA=$(papara 2>&1 >/dev/null | grep 'papara_core' | cut -d' ' -f 5)
export VERSION_PAPARA
VERSION_RAXML=$(raxml -version | grep 'RAxML version' | cut -d' ' -f 5)
export VERSION_RAXML
VERSION_GAPPA='v0.0.0'
export VERSION_GAPPA

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
                  autotools-dev \
                  libtool \
                  flex \
                  bison \
                  cmake \
                  automake \
                  autoconf \
                  build-essential \
                  ca-certificates
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

######## PRANK #########
cd /usr/local/
wget http://wasabiapp.org/download/prank/development/prank.source.170703.tgz
tar -xvf prank.source.170703.tgz
cd /usr/local/development/src
make
ln -s /usr/local/development/src/prank /usr/local/bin/prank

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

####### EPA-ng #########
cd /usr/local
git clone https://github.com/Pbdas/epa-ng.git
cd /usr/local/epa-ng
make
ln -s /usr/local/epa-ng/bin/epa-ng /usr/local/bin/epa-ng

######## standard-raxml ########
cd /usr/local/
git clone https://github.com/stamatak/standard-RAxML.git standard-raxml
cd /usr/local/standard-raxml
make -f Makefile.PTHREADS.gcc
rm *.o
ln -s /usr/local/standard-raxml/raxmlHPC-PTHREADS /usr/local/bin/raxml

####### parapa #########
cd /usr/local/bin
wget https://sco.h-its.org/exelixis/resource/download/software/papara_nt-2.5-static_x86_64.tar.gz
tar -xvf papara_nt-2.5-static_x86_64.tar.gz
rm papara_nt-2.5-static_x86_64.tar.gz
mv papara_static_x86_64 papara

####### gappa #########
cd /usr/local/
git clone --recursive https://github.com/lczech/gappa.git
cd /usr/local/gappa
make
ln -s /usr/local/gappa/bin/gappa /usr/local/bin/gappa

%test
/usr/local/megan/tools/daa-meganizer -h
/usr/local/megan/tools/gc-assembler -h
trimal --version
diamond version
fastq-dump --version
which FastTree
iqtree -h
python3 --version
epa-ng --version
raxml -h
papara -h
gappa --help
prank -version

%runscript
echo "MEGAN6: "$VERSION_MEGAN
echo "trimal: "$VERSION_TRIMAL
echo "diamond: "$VERSION_DIAMOND
echo "fastq-dump: "$VERSION_FASTQ_DUMP
echo "FastTree: "$VERSION_FASTTREE
echo "IQ-TREE: "$VERSION_IQTREE
echo "Python3: "$VERSION_PYTHON3
echo "Prank: "$VERSION_PRANK
echo "RAxML: "$VERSION_RAXML
echo "epa-ng: "$VERSION_EPA_NG
echo "PaPaRa: "$VERSION_PAPARA
echo "gappa: "$VERSION_GAPPA
