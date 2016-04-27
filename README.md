# grin
GRIN Epilepsy Study

### Snakemake setup
Do this once:
```
wget https://repo.continuum.io/miniconda/Miniconda3-latest-Linux-x86_64.sh
sh Miniconda3-latest-Linux-x86_64.sh
# type "yes"
# allow this to install into your home
conda config --add channels bioconda
conda config --add channels r
conda config --add channels asmeurer
conda create --name grinenv --file requirements.txt
source activate grinenv
```

### Fetch dependencies
```
snakemake Rdeps
````

### To run a trio
```
source activate grinenv
snakemake GRCh37/gemini/PRG_MAE_1.gemini.db
```

### To generate a VCF of all trios, separately and combined
```
source activate grinenv
snakemake
```


## To run on Respublica
- `--drmaa` is not allowed on Respublica yet, use `-c qsub`
```
source activate grinenv
snakemake -j 300 --cluster-config configs/cluster.yaml -c "qsub -V -l h_vmem={cluster.g_vmem} -l mem_free={cluster.mem_free} -l mem_free_l={cluster.m_mem_free} -pe smp {threads}"
```

### Development
To update the requirements file (after installing some new package):
```
conda list -e > requirements.txt
```

To update your conda environment with a new requirements file:
```
conda install --name grinenv --file  requirements.txt
```
