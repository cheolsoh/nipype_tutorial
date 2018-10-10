# Generated by Neurodocker version 0.4.2-dev
# Timestamp: 2018-10-10 04:25:18 UTC
# 
# Thank you for using Neurodocker. If you discover any issues
# or ways to improve this software, please submit an issue or
# pull request on our GitHub repository:
# 
#     https://github.com/kaczmarj/neurodocker

Bootstrap: docker
From: neurodebian:stretch-non-free

%post
export ND_ENTRYPOINT="/neurodocker/startup.sh"
apt-get update -qq
apt-get install -y -q --no-install-recommends \
    apt-utils \
    bzip2 \
    ca-certificates \
    curl \
    locales \
    unzip
apt-get clean
rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*
sed -i -e 's/# en_US.UTF-8 UTF-8/en_US.UTF-8 UTF-8/' /etc/locale.gen
dpkg-reconfigure --frontend=noninteractive locales
update-locale LANG="en_US.UTF-8"
chmod 777 /opt && chmod a+s /opt
mkdir -p /neurodocker
if [ ! -f "$ND_ENTRYPOINT" ]; then
  echo '#!/usr/bin/env bash' >> "$ND_ENTRYPOINT"
  echo 'set -e' >> "$ND_ENTRYPOINT"
  echo 'if [ -n "$1" ]; then "$@"; else /usr/bin/env bash; fi' >> "$ND_ENTRYPOINT";
fi
chmod -R 777 /neurodocker && chmod a+s /neurodocker

apt-get update -qq
apt-get install -y -q --no-install-recommends \
    convert3d \
    ants \
    fsl \
    gcc \
    g++ \
    graphviz \
    tree \
    git-annex-standalone \
    vim \
    emacs-nox \
    nano \
    less \
    ncdu \
    tig \
    git-annex-remote-rclone \
    octave \
    netbase
apt-get clean
rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

sed -i '$isource /etc/fsl/fsl.sh' $ND_ENTRYPOINT

apt-get update -qq
apt-get install -y -q --no-install-recommends \
    bc \
    libncurses5 \
    libxext6 \
    libxmu6 \
    libxpm-dev \
    libxt6
apt-get clean
rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*
echo "Downloading MATLAB Compiler Runtime ..."
curl -sSL --retry 5 -o /tmp/toinstall.deb http://mirrors.kernel.org/debian/pool/main/libx/libxp/libxp6_1.0.2-2_amd64.deb
dpkg -i /tmp/toinstall.deb
rm /tmp/toinstall.deb
apt-get install -f
apt-get clean
rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*
curl -fsSL --retry 5 -o /tmp/MCRInstaller.bin https://dl.dropbox.com/s/zz6me0c3v4yq5fd/MCR_R2010a_glnxa64_installer.bin
chmod +x /tmp/MCRInstaller.bin
/tmp/MCRInstaller.bin -silent -P installLocation="/opt/matlabmcr-2010a"
rm -rf /tmp/*
echo "Downloading standalone SPM ..."
curl -fsSL --retry 5 -o /tmp/spm12.zip http://www.fil.ion.ucl.ac.uk/spm/download/restricted/utopia/previous/spm12_r7219_R2010a.zip
unzip -q /tmp/spm12.zip -d /tmp
mkdir -p /opt/spm12-r7219
mv /tmp/spm12/* /opt/spm12-r7219/
chmod -R 777 /opt/spm12-r7219
rm -rf /tmp/*
/opt/spm12-r7219/run_spm12.sh /opt/matlabmcr-2010a/v713 quit
sed -i '$iexport SPMMCRCMD=\"/opt/spm12-r7219/run_spm12.sh /opt/matlabmcr-2010a/v713 script\"' $ND_ENTRYPOINT

useradd --no-user-group --create-home --shell /bin/bash neuro
su - neuro

export PATH="/opt/miniconda-latest/bin:$PATH"
echo "Downloading Miniconda installer ..."
conda_installer="/tmp/miniconda.sh"
curl -fsSL --retry 5 -o "$conda_installer" https://repo.continuum.io/miniconda/Miniconda3-latest-Linux-x86_64.sh
bash "$conda_installer" -b -p /opt/miniconda-latest
rm -f "$conda_installer"
conda update -yq -nbase conda
conda config --system --prepend channels conda-forge
conda config --system --set auto_update_conda false
conda config --system --set show_channel_urls true
sync && conda clean -tipsy && sync
conda create -y -q --name neuro
conda install -y -q --name neuro \
    python=3.6 \
    pytest \
    jupyter \
    jupyterlab \
    jupyter_contrib_nbextensions \
    traits \
    pandas \
    matplotlib \
    scikit-learn \
    scikit-image \
    seaborn \
    nbformat \
    nb_conda
sync && conda clean -tipsy && sync
bash -c "source activate neuro
  pip install --no-cache-dir  \
      https://github.com/nipy/nipype/tarball/master \
      https://github.com/INCF/pybids/tarball/0.6.5 \
      nilearn \
      datalad[full] \
      nipy \
      duecredit \
      nbval"
rm -rf ~/.cache/pip/*
sync
sed -i '$isource activate neuro' $ND_ENTRYPOINT


bash -c 'source activate neuro && jupyter nbextension enable exercise2/main && jupyter nbextension enable spellchecker/main'

su - root

mkdir /data && chmod 777 /data && chmod a+s /data

mkdir /output && chmod 777 /output && chmod a+s /output

su - neuro

printf "[user]\n\tname = miykael\n\temail = michaelnotter@hotmail.com\n" > ~/.gitconfig

bash -c 'source activate neuro && cd /data && datalad install -r ///workshops/nih-2017/ds000114 && cd ds000114 && datalad update -r && datalad get -r sub-01/ses-test/anat sub-01/ses-test/func/*fingerfootlips*'

curl -L https://files.osf.io/v1/resources/fvuh8/providers/osfstorage/580705089ad5a101f17944a9 -o /data/ds000114/derivatives/fmriprep/mni_icbm152_nlin_asym_09c.tar.gz && tar xf /data/ds000114/derivatives/fmriprep/mni_icbm152_nlin_asym_09c.tar.gz -C /data/ds000114/derivatives/fmriprep/. && rm /data/ds000114/derivatives/fmriprep/mni_icbm152_nlin_asym_09c.tar.gz && find /data/ds000114/derivatives/fmriprep/mni_icbm152_nlin_asym_09c -type f -not -name ?mm_T1.nii.gz -not -name ?mm_brainmask.nii.gz -not -name ?mm_tpm*.nii.gz -delete

su - root

chown -R neuro /home/neuro/nipype_tutorial

rm -rf /opt/conda/pkgs/*

su - neuro

mkdir -p ~/.jupyter && echo c.NotebookApp.ip = \"0.0.0.0\" > ~/.jupyter/jupyter_notebook_config.py

cd /home/neuro/nipype_tutorial

echo '{
\n  "pkg_manager": "apt",
\n  "instructions": [
\n    [
\n      "base",
\n      "neurodebian:stretch-non-free"
\n    ],
\n    [
\n      "_header",
\n      {
\n        "version": "generic",
\n        "method": "custom"
\n      }
\n    ],
\n    [
\n      "install",
\n      [
\n        "convert3d",
\n        "ants",
\n        "fsl",
\n        "gcc",
\n        "g++",
\n        "graphviz",
\n        "tree",
\n        "git-annex-standalone",
\n        "vim",
\n        "emacs-nox",
\n        "nano",
\n        "less",
\n        "ncdu",
\n        "tig",
\n        "git-annex-remote-rclone",
\n        "octave",
\n        "netbase"
\n      ]
\n    ],
\n    [
\n      "add_to_entrypoint",
\n      "source /etc/fsl/fsl.sh"
\n    ],
\n    [
\n      "env",
\n      {
\n        "LD_LIBRARY_PATH": "/opt/miniconda-latest/envs/neuro/lib:$LD_LIBRARY_PATH"
\n      }
\n    ],
\n    [
\n      "spm12",
\n      {
\n        "version": "r7219"
\n      }
\n    ],
\n    [
\n      "user",
\n      "neuro"
\n    ],
\n    [
\n      "miniconda",
\n      {
\n        "miniconda_version": "4.3.31",
\n        "conda_install": [
\n          "python=3.6",
\n          "pytest",
\n          "jupyter",
\n          "jupyterlab",
\n          "jupyter_contrib_nbextensions",
\n          "traits",
\n          "pandas",
\n          "matplotlib",
\n          "scikit-learn",
\n          "scikit-image",
\n          "seaborn",
\n          "nbformat",
\n          "nb_conda"
\n        ],
\n        "pip_install": [
\n          "https://github.com/nipy/nipype/tarball/master",
\n          "https://github.com/INCF/pybids/tarball/0.6.5",
\n          "nilearn",
\n          "datalad[full]",
\n          "nipy",
\n          "duecredit",
\n          "nbval"
\n        ],
\n        "create_env": "neuro",
\n        "activate": true
\n      }
\n    ],
\n    [
\n      "run_bash",
\n      "source activate neuro && jupyter nbextension enable exercise2/main && jupyter nbextension enable spellchecker/main"
\n    ],
\n    [
\n      "user",
\n      "root"
\n    ],
\n    [
\n      "run",
\n      "mkdir /data && chmod 777 /data && chmod a+s /data"
\n    ],
\n    [
\n      "run",
\n      "mkdir /output && chmod 777 /output && chmod a+s /output"
\n    ],
\n    [
\n      "user",
\n      "neuro"
\n    ],
\n    [
\n      "run",
\n      "printf \"[user]\\\n\\tname = miykael\\\n\\temail = michaelnotter@hotmail.com\\\n\" > ~/.gitconfig"
\n    ],
\n    [
\n      "run_bash",
\n      "source activate neuro && cd /data && datalad install -r ///workshops/nih-2017/ds000114 && cd ds000114 && datalad update -r && datalad get -r sub-01/ses-test/anat sub-01/ses-test/func/*fingerfootlips*"
\n    ],
\n    [
\n      "run",
\n      "curl -L https://files.osf.io/v1/resources/fvuh8/providers/osfstorage/580705089ad5a101f17944a9 -o /data/ds000114/derivatives/fmriprep/mni_icbm152_nlin_asym_09c.tar.gz && tar xf /data/ds000114/derivatives/fmriprep/mni_icbm152_nlin_asym_09c.tar.gz -C /data/ds000114/derivatives/fmriprep/. && rm /data/ds000114/derivatives/fmriprep/mni_icbm152_nlin_asym_09c.tar.gz && find /data/ds000114/derivatives/fmriprep/mni_icbm152_nlin_asym_09c -type f -not -name ?mm_T1.nii.gz -not -name ?mm_brainmask.nii.gz -not -name ?mm_tpm*.nii.gz -delete"
\n    ],
\n    [
\n      "copy",
\n      [
\n        ".",
\n        "/home/neuro/nipype_tutorial"
\n      ]
\n    ],
\n    [
\n      "user",
\n      "root"
\n    ],
\n    [
\n      "run",
\n      "chown -R neuro /home/neuro/nipype_tutorial"
\n    ],
\n    [
\n      "run",
\n      "rm -rf /opt/conda/pkgs/*"
\n    ],
\n    [
\n      "user",
\n      "neuro"
\n    ],
\n    [
\n      "run",
\n      "mkdir -p ~/.jupyter && echo c.NotebookApp.ip = \\\"0.0.0.0\\\" > ~/.jupyter/jupyter_notebook_config.py"
\n    ],
\n    [
\n      "workdir",
\n      "/home/neuro/nipype_tutorial"
\n    ]
\n  ]
\n}' > /neurodocker/neurodocker_specs.json

%environment
export LANG="en_US.UTF-8"
export LC_ALL="en_US.UTF-8"
export ND_ENTRYPOINT="/neurodocker/startup.sh"
export LD_LIBRARY_PATH="/opt/miniconda-latest/envs/neuro/lib:$LD_LIBRARY_PATH"
export FORCE_SPMMCR="1"
export LD_LIBRARY_PATH="$LD_LIBRARY_PATH:/usr/lib/x86_64-linux-gnu:/opt/matlabmcr-2010a/v713/runtime/glnxa64:/opt/matlabmcr-2010a/v713/bin/glnxa64:/opt/matlabmcr-2010a/v713/sys/os/glnxa64:/opt/matlabmcr-2010a/v713/extern/bin/glnxa64"
export MATLABCMD="/opt/matlabmcr-2010a/v713/toolbox/matlab"
export CONDA_DIR="/opt/miniconda-latest"
export PATH="/opt/miniconda-latest/bin:$PATH"

%files
. /home/neuro/nipype_tutorial

%runscript
/neurodocker/startup.sh "$@"
