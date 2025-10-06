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
   * ```cd alphafold
git checkout 12f16df5e3c36f053b3dd10822e0da165f649a6f

In docker/Dockerfile change
 conda install --quiet --yes --channel nvidia cuda=${CUDA_VERSION} 
To     conda install --quiet --yes nvidia/label/cuda-12.2.2::cuda-toolkit 
podman build --format=docker -t alphafold2-3-2v3 -f docker/Dockerfile .
podman save localhost/alphafold2-3-2v3 -o alphafold232_3_docker.tar
apptainer build /projects/protein_design/alphafold/alphafoldv2/containers/alphafold232_3.sif docker-archive://alphafold232_3_docker.tar
```
