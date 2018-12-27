## 1. build docker image

Ref book: [Bioinformatics with Python Cookbook 2nd](https://github.com/PacktPublishing/Bioinformatics-with-Python-Cookbook-Second-Edition)

```
# docker build -t bio https://raw.githubusercontent.com/PacktPublishing/Bioinformatics-with-Python-Cookbook-Second-Edition/master/docker/Dockerfile
```
> I **cannot successfully install** by this command, so I take the Dockerfile as a reference and build image manually. 
>

**Manually install: check one-by-one and modify the Dockerfile as below!**

```
// 1. Pull down continuumio/anaconda3:5.2.0
# docker pull continuumio/anaconda3:5.2.0

// 2. Connect into this image:
root@pyfu-System-Product-Name:~# docker run --name bio -v /media/pyfu/DATA/:/media/pyfu/DATA/ -v /home/pyfu/:/home/pyfu/ -it continuumio/anaconda3:5.2.0
(base) root@d1e764951f84:/#

// 3. Set Environment Variables:
(base) root@d1e764951f84:/# export DEBIAN_FRONTEND=noninteractive

// 4. Update & Upgrade:
(base) root@d1e764951f84:/# apt-get update
(base) root@d1e764951f84:/# apt-get upgrade -y

// 5. Install package:
(base) root@d1e764951f84:/# apt-get install -y git wget build-essential unzip
(base) root@d1e764951f84:/# apt-get clean

// 6. Conda:
(base) root@d1e764951f84:/# conda -V
conda 4.5.4
(base) root@d1e764951f84:/# python --version
Python 3.6.5 :: Anaconda, Inc.
(base) root@d1e764951f84:/# conda config --add channels bioconda
(base) root@d1e764951f84:/# conda install --yes biopython=1.70
(base) root@d1e764951f84:/# conda install --yes statsmodels pysam plink gffutils genepop trimal (gffutils failed)
Solving environment: failed

UnsatisfiableError: The following specifications were found to be in conflict:
  - conda[version='>=4.5.12']
  - gffutils
  - python=3
Use "conda info <package>" to see the dependencies for each package.

(base) root@d1e764951f84:/# conda info gffutils
...
gffutils 0.9 py36_0
-------------------
file name   : gffutils-0.9-py36_0.tar.bz2
name        : gffutils
...
url         : https://conda.anaconda.org/bioconda/linux-64/gffutils-0.9-py36_0.tar.bz2
dependencies:
    argcomplete
    argh (無法安裝...)
    pyfaidx
    python 3.6*
    simplejson
    six

(base) root@d1e764951f84:/# conda install --yes statsmodels pysam plink (分開安裝)
(base) root@d1e764951f84:/# conda install --yes argcomplete pyfaidx simplejson six (無法安裝 argh...改用 pip)
(base) root@d1e764951f84:/# pip install gffutils
(base) root@d1e764951f84:/# conda install --yes genepop trimal
(base) root@d1e764951f84:/# conda config --add channels conda-forge (多加一行！)
(base) root@d1e764951f84:/# conda install --yes simuPOP
(base) root@d1e764951f84:/# apt-get install -y r-bioc-biobase （須先安裝 R, 再安裝 rpy2）
(base) root@d1e764951f84:/# pip install rpy2 （須先安裝 R, 再安裝 rpy2）
(base) root@d1e764951f84:/# pip install seaborn
(base) root@d1e764951f84:/# pip install pyvcf
(base) root@d1e764951f84:/# pip install dendropy
(base) root@d1e764951f84:/# pip install pexpect
(base) root@d1e764951f84:/# pip install reportlab
(base) root@d1e764951f84:/# pip install networkx
(base) root@d1e764951f84:/# pip install pygenomics
```

**Dockerfile: (Modified)**

```../Dockerfile``` [link](https://github.com/AmberFu/LearningWith_BioinformaticsWithPythonCookbook_2nd/blob/master/Dockerfile)

**re-Built docker images:**

```
# docker build -t bio:20181227 ../Dockerfile
...
Successfully built 4b5b9b03f0d0
Successfully tagged bio:20181227

// check:
# docker images
REPOSITORY              TAG                 IMAGE ID            CREATED             SIZE
bio                     20181227            e61f63f9c182        16 seconds ago      5.96GB
```

**Push to my docker hub:**

```
// 1. rename image tag: {Username/repository_name}(repository name must be lowercase)
# docker tag bio:20181227 spashleyfu/bioinformatics_python

// 2. Log in Docker:
# docker login
Login with your Docker ID to push and pull images from Docker Hub. If you don't have a Docker ID, head over to https://hub.docker.com to create one.
Username: spashleyfu
Password: ******
WARNING! Your password will be stored unencrypted in /home/pyfu/.docker/config.json.
Configure a credential helper to remove this warning. See
https://docs.docker.com/engine/reference/commandline/login/#credentials-store

Login Succeeded

// 3. Push:
# docker push spashleyfu/bioinformatics_python
```

**Run bio container:**

```
# docker run -ti -p 9875:9875 -v /home/pyfu/:/home/pyfu -v /media/pyfu/DATA/:/media/pyfu/DATA/ spashleyfu/bioinformatics_python
[I 07:54:07.702 NotebookApp] Writing notebook server cookie secret to /root/.local/share/jupyter/runtime/notebook_cookie_secret
[I 07:54:07.915 NotebookApp] JupyterLab beta preview extension loaded from /opt/conda/lib/python3.6/site-packages/jupyterlab
[I 07:54:07.915 NotebookApp] JupyterLab application directory is /opt/conda/share/jupyter/lab
[I 08:13:32.359 NotebookApp] Serving notebooks from local directory: /PacktPublising/notebooks
...
```

**Open browser:**

> Point your browser to `http://localhost:9875` and you should get the Jupyter environment.
>


Ref:
[Bioconda](https://bioconda.github.io/)
