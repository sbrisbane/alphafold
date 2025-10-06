 * Install podman 
    * yum install podman
 * Install apptainer
    * on BCM clusters
       * yum install cm-apptainer
    * Otherwise
       * todo
   

 * Create a conda environment for run_singularity_alphafoldv2.py
   * module load conda (
     * See elsewhere if you need to install miniconda first)
   * `conda env create -f environment.yml  -p $CONDA_PREFIX/envs/o`
  
* Build container
```
cd alphafold
git clone https://github.com/sbrisbane/alphafold
cd alphafold

podman build --format=docker -t alphafold2-3-2v3 -f docker/Dockerfile .
podman save localhost/alphafold2-3-2v3 -o alphafold232_3_docker.tar
apptainer build /path/to/output/container/alphafold232_3.sif docker-archive://alphafold232_3_docker.tar
```
