# Camelyon Slide to Tiles Experiments

This repository contains a reproducible RED experiment. This experiment can be reproduced with CC-FAICE or CC-Agency version 6 of the [Curious Containers](https://www.curious-containers.cc/) project.

The experiments read image slides from the CAMELYON16 database via OpenSlide to create image tiles for tumor and normal tissue. Tiles are saved in one HDF5 file per slide. It is used as data preparation step of machine learning tasks.

## External resources used for this Experiment

* CAMELYON16 DataBase from `ftp://parrot.genomics.cn/gigadb/pub/10.5524/100001_101000/100439/CAMELYON16/`
* [camelyon-utils](https://github.com/deep-projects/camelyon-utils) release 0.2
* camelyon-utils appliance (Docker image)
    * [docker.io/deepprojects/camelyon-utils:0.2](https://cloud.docker.com/u/deepprojects/repository/docker/deepprojects/camelyon-utils)
    * [Dockerfile](https://github.com/deep-projects/appliances/tree/master/camelyon-utils/0.2)


## Reproduce at CBMI - HTW Berlin

Running the requires access to compute resources of CBMI - HTW Berlin. If you had access, you could run the original experiment as follows.

```bash
pip3 install --user --upgrade cc-faice==6.*
faice exec slide-tiles.red.yml
```


## Reproduce without access to CBMI resources

To reproduce this on your own hardware, you should perform the following steps:

* Download CAMELYON16 to your own storage server:

```bash
wget -r -nH --cut-dirs=5 -nc ftp://parrot.genomics.cn/gigadb/pub/10.5524/100001_101000/100439/CAMELYON16/
```

* Change the RED connectors in `slide-tiles.red.yml` to access the files from your storage server. For more information about possible connectors refer to the [Curious Containers](https://www.curious-containers.cc/) documentation.
* Run RED experiment via `faice agent red --outputs slide-tiles-custom.red.yml` commandline tool or your own instance of CC-Agency server software.
* The experiments use the `camelyon-utils slide-tiles` commandline tool to create one HDF5 file per slide. Use `camelyon-utils merge-hdf5` to merge them into a single HDF5 file.


## Merge files

The experiment produces one HDF5 file per slide. You can merge them to a CNN training data set using `camelyon-utils merge-hd5` as follows:

```bash
camelyon-utils merge-hdf5 --indir /data/ldap/camelyon/camelyon_new_downloaded_2019_02_06/CAMELYON16/tiles/training/ --outfile /data/ldap/camelyon/camelyon_new_downloaded_2019_02_06/CAMELYON16/merged_tiles/merged.hdf5 --mode single --numtiles 750
```
