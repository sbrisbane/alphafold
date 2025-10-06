 * Install podman 
    * yum install podman
 * Install apptainer
    * on BCM clusters
       * `yum install cm-apptainer`
    * Otherwise
       * todo
 * Install cuda >=12.2.2
   * On BCM clusters
     * `yum install cuda-dcgm-libs cuda12.8 cuda12.8-toolkit cuda-driver cuda-dcgm cuda-dcgm-nvvs`
   

 * Create a conda environment for run_singularity_alphafoldv2.py
   * `module load conda` 
     * See elsewhere if you need to install miniconda first
   * `conda env create -f environment.yml  -p $CONDA_PREFIX/envs/alpharun`
  
* Build container
* You may need to do this as root if you have not set up podman to allow rootless for users (subuid/subgid)
  
```
#Load conda into your environment
module load conda
conda activate alpharun
module load apptainer
module load cuda12.8/blas/12.8.0 cuda12.8/fft/12.8.0 cuda12.8/toolkit/12.8.0 
cd alphafold
git clone https://github.com/sbrisbane/alphafold
cd alphafold

podman build --format=docker -t alphafold2-3-2v3 -f docker/Dockerfile .
podman save localhost/alphafold2-3-2v3 -o alphafold232_3_docker.tar
apptainer build /path/to/output/container/alphafold232_3.sif docker-archive://alphafold232_3_docker.tar
```
* get an example fasta file
 ```
 mkdir -p $HOME/alphafold_Files
    wget https://github.com/sbrisbane/alphafold/alphafold_Files/rcsb_pdb_7UNJ.fa -O $HOME/alphafold_Files/rcsb_pdb_7UNJ.fa
```
  
* Set up alphafoldv2_apptainer.sh for your environment

Change at least 

`OUTPUTDIR="CHANGEME"`

*  Run container
  *  `sbatch -p gpuq --gres=gpu:1 alphafoldv2_apptainer.sh`
