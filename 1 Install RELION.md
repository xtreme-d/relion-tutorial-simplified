# Install RELION

* [Prepare the environment](#prepare-the-environment)
* [Prepare the data](#prepare-the-data)

Please follow [the official installation page]. You can also [build RELION with Intel compiler].

Along with RELION, you might need the [UCSF Chimera] application that designed for interactive visualization and analysis of molecular structures and related data in 3D. It will be handy in the second half of the tutorial, so we recommend to [install it] too.


## Prepare the Environment

You can provide fine-tuning for RELION by setting the specific environment variables. The RELION developers provide [the sample RC script] for this purpose. You can override desired settings in your user's `.bashrc` file or manually execute it (`source /path/to/relion_rc_script_file.sh`) before you run RELION.

Also, RELION can run jobs in parallel, but you need an MPI library installed. Also, it’s recommended to use the job scheduler to submit MPI jobs. The recommended combo: [Open MPI 2.1.6+] and [SLURM 18.08.3+].

**NOTE**: RELION works faster if you are using GPU. The original tutorial describes it in details. But in this simplified tutorial, we are going to provide examples based on jobs executed on CPU only.

Let's set up the parallel execution of RELION jobs. First, we have to prepare the job running script.

```bashrc
#!/bin/bash
#SBATCH --ntasks=XXXmpinodesXXX
#SBATCH --ntasks-per-node=XXXdedicatedXXX
#SBATCH --partition=XXXextra1XXX
#SBATCH --cpus-per-task=XXXthreadsXXX
#SBATCH --error=XXXerrfileXXX
#SBATCH --output=XXXoutfileXXX
#SBATCH --job-name=XXXqueueXXX

export OMPI_MCA_btl_tcp_if_include="eno1"
srun --mpi=pmi2 XXXcommandXXX
```

Strings that looks like `XXXsometextXXX` are placeholders for parameters defined in the current user environment. In other words - this is the template of the running script. The actual running script will be generated separately for each job with job-specific parameters.

Now, let’s update user’s `.bashrc` file with the following content

```bash
# Required system environment variables for this RELION job
# submission script template
export RELION_QSUB_TEMPLATE=/home/user/relion/slurm_relion.sh
export RELION_QSUB_EXTRA_COUNT=1
export RELION_QSUB_EXTRA1=Partition
export RELION_QSUB_EXTRA1_DEFAULT=my_partition
export RELION_QSUB_EXTRA1_HELP=Partition

# The mpi runtime ('mpirun' by default)
export RELION_MPI_RUN=srun
# The default for 'Submit to queue?'
export RELION_QUEUE_USE=Yes
# The default for 'Queue submit command'
export RELION_QSUB_COMMAND=sbatch
```
Check [the RELION download page] for detail about running script template keys.


## Prepare the Data

Before we proceed to RELION, we need to download the test microscopy images data.

```bash
wget ftp://ftp.mrc-lmb.cam.ac.uk/pub/scheres/relion30_tutorial_data.tar
tar -xf relion30_tutorial_data.tar
cd relion30_tutorial
```

The `relion30_tutorial` directory will be our working directory from now. Currently, it contains the `Movies` directory with microscopy movie frames in `tiff` containers. That is what we are going to process during the tutorial. Also, please download the following files.

1. [CTFFIND] - utility for finding [contrast transfer functions] (or CTF) of electron micrographs. The fastest way to install it - download the binary executable file (e.g., ctffind-4.1.13-linux64.tar.gz) from [official download page] and unpack it to the working directory. You can also build it if required. *Alternatively, you can use [Gctf] utility.*
2. Parameters of the microscope used to take images we are going to process. You can download it from the following link.
```
wget https://raw.githubusercontent.com/3dem/relion/master/data/mtf_k2_300kV.star
```

Your working directory file tree should be like the following.

```
relion30_tutorial
|- bin                <- ctffind4
|- Movies             <- data images
|- mtf_k2_300kV.star  <- microscope parameters
```

Additionally, you can download the tutorial’s precalculated data to compare it with your jobs data and estimate correctness of your progress.

```
wget ftp://ftp.mrc-lmb.cam.ac.uk/pub/scheres/relion30_tutorial_precalculated_results.tar.gz
tar -zxf relion30_tutorial_precalculated_results.tar.gz
```

Ideally, you need to run two instances of RELION: from your working directory and precalculated data directory, respectively. In that way, you can compare your input data and results.

Everything set up. Lets's move to the next unit.


-----------------------------
[Top Page] | [2 Run RELION] →
---------- | ----------------


[the official installation page]: https://www3.mrc-lmb.cam.ac.uk/relion/index.php/Download_%26_install
[build RELION with Intel compiler]: https://github.com/3dem/relion#building-with-the-intelr-compiler
[UCSF Chimera]: https://en.wikipedia.org/wiki/UCSF_Chimera
[install it]: https://www.cgl.ucsf.edu/chimera/download.html
[the sample RC script]: https://www3.mrc-lmb.cam.ac.uk/relion/index.php?title=Download_%26_install#Edit_the_environment_set-up
[Open MPI 2.1.6+]: https://www.open-mpi.org/
[SLURM 18.08.3+]: https://slurm.schedmd.com/
[the RELION download page]: https://www3.mrc-lmb.cam.ac.uk/relion/index.php?title=Download_%26_install#Set-up_queue_job_submission
[CTFFIND]: http://grigoriefflab.janelia.org/ctffind4
[contrast transfer functions]: https://en.wikipedia.org/wiki/Contrast_transfer_function
[official download page]: http://grigoriefflab.janelia.org/ctf
[Gctf]: https://www.mrc-lmb.cam.ac.uk/kzhang/
[The original tutorial]: ftp://ftp.mrc-lmb.cam.ac.uk/pub/scheres/relion30_tutorial.pdf

[Top Page]: https://github.com/xtreme-d/relion-tutorial-simplified
[2 Run RELION]: ./2%20Run%20RELION.md
