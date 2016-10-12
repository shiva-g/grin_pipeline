# grin
GRIN Epilepsy Study

### Snakemake setup
Do this once:
```
wget https://repo.continuum.io/miniconda/Miniconda3-latest-Linux-x86_64.sh
sh Miniconda3-latest-Linux-x86_64.sh
# type "yes"
#replace the your .condarc with the one included here
cp configs/.condarc ~/.condarc
conda create --name grinenv --file requirements.txt python=3.5
source activate grinenv
```

### To run a trio
```
source activate grinenv
snakemake GRCh38/analysis/1KGCEU.trio.phased.com.filtered.ad.de.nm.snpeff.models.html
```

### To generate a VCF of all trios, separately and combined
```
source activate grinenv
snakemake
```


### Setup on Respublica
Set a TMPDIR in your `~/.bash_profile`:
```
export TMPDIR=/mnt/lustre/users/YOURUSERNAME/scratch
```

Use an appropriate config:
```
ln -s configs/config.respublica.yaml configs/config.yaml
cp configs/localconfig.sample.yaml localconfig.yaml
```

- `--drmaa` is not allowed on Respublica yet, use `-c qsub`

### Run on Respublica
```
source activate grinenv
snakemake -j 300 --cluster-config configs/cluster.yaml -c "qsub -V -l h_vmem={cluster.h_vmem} -l mem_free={cluster.mem_free} -l m_mem_free={cluster.m_mem_free} -pe smp {threads}"
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
You might find this will trigger Snakemake to want to remake downstream files, because executables are listed as input (this ensures they actually exist). To remedy this you can postdate any offending executables if you are confident their updates do not affect results.
```
touch -d 20160101 `which novoalign`
```
