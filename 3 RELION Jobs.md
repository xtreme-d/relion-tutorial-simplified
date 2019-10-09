# RELION Jobs

* [Job types](#job-types)
* [Job files](#job-files)
* [Job actions](#job-actions)
* [Similar job parameters](#similar-job-parameters)

In this unit, you can find a description of each job type, what kinds of actions you can apply to any job, and job files structure.

To run a RELION job:

* select desired job type in the job types selector;
* fill job parameters;
* press "Run" button to start job execution immediately or press the "Schedule" button if you want to start job later.

Note that the output of some of RELION GUI components depends on the currently selected job (job parameters input section, stdout block, stderr block). For example, if you just started a job, or clicked on the existing job in any jobs list (running, scheduled, finished, "input to this job," and "output from this job") you will see the information in mentioned components related to the selected job.


## Job Types

RELION provides a variety of job types required for model building. Some job types are running instantly; others need time for running and could be executed in parallel (See "Relion Tutorial / MPI" section for details).

* **Import.** Builds a pipeline STAR file from micrograph images.
* **Motion correction.** Fix position of particles distorted due to motion during the photography process.
* **CTF Estimation.** Calculate the best contrast for images.
* **Manual picking.** Manually set coordinates of particles on micrograph images.
* **Auto-picking.** Automatic search of coordinates of particles on micrograph images using a specific algorithm.
* **Particle extraction.** Cut particle images using coordinates from manual or auto picking.
* **Particle sorting.** Sort extracted images by a specific value of each particle. This job type needed to separate “good” and “bad” particle images.
* **Subset selection.** Select a subset of micrographs or particles from an input set.
* **2D classification.** Process extracted particle images and create 2D classes from similar images.
* **3D initial model.** Build an initial 3D model reference from a set of 2D classes.
* **3D classification.** Build a 3D model using 3D model reference and a set of 2D classes.
* **3D auto-refine.** Refine a 3D model by Fourier shell correlation method.
* **3D multi-body.** *[Not used in the tutorial]* Refine a 3D model by the iterative alignment of independently moving rigid bodies of a particle.
* **CTF refinement.** Estimate the defocus and beam tilt values for input particles.
* **Bayesian polishing.** Polish particle with Bayesian beam-induced motion correction.
* **Mask creation.** Create a mask of a particle, a “cover” that covers a 3D model of particle and represents a “border” of it.
* **Join star files.** *[Not used in the tutorial]* Join STAR files of particles, micrographs or micrograph movies.
* **Particle subtraction.** *[Not used in the tutorial]* Subtract projections of an input model from the input particles and apply a mask to them.
* **Post-processing.** Apply B-factor sharpening and calculate masked FSC curves for a 3D model.
* **Local resolution.** Estimate a resolution of a 3D model in the local scale.


## Job Files

RELION jobs related files stored in the filesystem as a two levels folder structure:

* First level - a job type folder,
* Second level - a particular job folder

```
workdir/
    Import/
        job001/
        movies/  <-- symlink to job001
    MotionCorr/
        job002/
        job003/
        own/     <-- symlink to job003
```

By default, each job has a numerical identifier, but you can specify an alias. Once you set an alias, you can find a symlink to this job folder with selected name in the corresponding job type folder.

Each job contains the following common files:

* `run.job` - Job parameters from the GUI
* `run.log` - stdout of the job
* `run.err` - stderr of the job
* `note.txt` - automatically generated logs of RELION commands of the current job. You can edit it via GUI using "Job actions" → "Edit note" action.
* `logfile.pdf` - reports, charts, and plots related to the current task.


## Job Actions

You can apply any action from `Job actions` tab to a currently selected task. You can set a job as current by clicking on job’s alias in any job list (Regions 9, 10, 11, and 12 in the "Run RELION / RELION GUI" section’s picture).

* `Edit Note` - edit job notes.
* `Alias` - edit job alias.
* `Mark as Finished` - mark running job as completed.
* `Make flowchart` - create a PDF file with a flowchart from the first task in the list until selected one.
* `Gentle clean` - remove all job-related files except those that could be used as input for new jobs.
* `Harsh clean` - remove all job-related files.
* `Delete`- move the current job to the trash folder. Note that **RELION does not erase the job data from the disk**. It just moves the job folder to the `Trash` folder in the root of the working directory.


## Similar Job Parameters

Almost each job type contains input parameters tabs that have similar values across all jobs.

* **Helix** tab that related to [helical processing]. it's not required for the tutorial so that you can ignore it.
* **Computing** tab. Here you can define computing options related to disk usage, GPU usage, particles RAM caching, and other optimizations.
* **Running** tab - contains parameters for the parallel running of jobs via MPI.


Now we have enough information about RELION to proceed to the single-particle processing tutorial.


-----------------------------------------------------
← [2 Run RELION] | [Top Page] | [4 RELION Tutorial] →
---------------- | ---------- | ---------------------


[helical processing]: https://www3.mrc-lmb.cam.ac.uk/relion/index.php?title=Helical_processing
[2 Run RELION]: ./2%20Run%20RELION.md
[Top Page]: https://github.com/xtreme-d/relion-tutorial-simplified
[4 RELION Tutorial]: ./4%20RELION%20Tutorial.md
