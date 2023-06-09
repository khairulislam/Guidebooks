# [SLURM](https://www.cs.virginia.edu/wiki/doku.php?id=compute_slurm)

* https://researchcomputing.princeton.edu/support/knowledge-base/python
* https://slurm.schedmd.com/sbatch.html
* [Scheduling a Job using the SLURM job scheduler](https://www.cs.virginia.edu/wiki/doku.php?id=compute_slurm)

```bash
# To view information about compute nodes in the SLURM system
sinfo 

# To view jobs running on the queue
squeue

# lets say you have the following script

# Since it is a shell script it must begin with a “shebang”
#!/bin/bash

# This is followed by a preamble describing the resource requests the job is making. 
# Each request begins with #SBATCH followed by an option.
# --- this job will be run on any available node
# and simply output the node's hostname to
# my_job.output
#SBATCH --job-name="Slurm Simple Test Job"
#SBATCH --error="my_job.err"
#SBATCH --output="my_job.output"
echo "$HOSTNAME"

# We run the script with sbatch
sbatch slurm.test

# To cancel a running job, use the scancel [jobid] command
scancel --signal=KILL 467039

# Say we want to use 4 GPUs on a system, we would use the following sbatch option:
#SBATCH --gres=gpu:4

To specify a partition with sbatch file:
#SBATCH --partition=gpu
```


## [Options](https://www.rc.virginia.edu/userinfo/rivanna/slurm/)
Note that most Slurm options have two forms, a short (single-letter) form that is preceded by a single hyphen and followed by a space, and a longer form preceded by a double hyphen and followed by an equals sign. In a job script these options are preceded by a pseudocomment #SBATCH. They may also be used as command-line options on their own.

Option	Usage
Number of nodes	-N <n> or --nodes=<n>
Number of tasks	-n <n> or --ntasks=<n>
Number of processes (tasks) per node	--ntasks-per-node=<n>
Total memory per node in megabytes	--mem=<M>
Memory per core in megabytes	--mem-per-cpu=<M>
Wallclock time	-t d-hh:mm:ss or --time=d-hh:mm:ss
Partition (queue) requested	-p <part> or --partition <part>
Account to be charged	-A <account> or --account=<allocation>
The --mem and --mem-per-cpu options are mutually exclusive. Job scripts should specify one or the other but not both.

## Submitting a Job
Job scripts are submitted with the sbatch command, e.g.:

> $ sbatch hello.slurm
The job identification number is returned when you submit the job, e.g.:

> $ sbatch hello.slurm
Submitted batch job 18341

## Displaying Job Status

The `squeue` command is used to obtain status information about all jobs submitted to all queues. The fields of the display are clearly labeled, and most are self-explanatory. The TIME field indicates the elapsed walltime (hrs:min:secs) that the job has been running. Note that JOBID 12346 has the name bash, which indicates it is an interactive job. In that case, the TIME field provides the amount of walltime during which the interactive session has be open (and resources have been allocated). The ST field lists a code which indicates the state of the job. Commonly listed states include:

* PD PENDING: Job is waiting for resources;
* R RUNNING: Job has the allocated resources and is running;
* S SUSPENDED: Job has the allocated resources, but execution has been suspended.

A complete list of job state codes is available here.

To check on your job’s status

> $ sstat <jobid>
For detailed information

> $ scontrol show job <jobid>
To request an estimate of when your pending job will run

> $ squeue --start -j <jobid>

## Canceling a Job

```bash
# Slurm provides the scancel command for deleting jobs from the system using the job identification number:

$ scancel 18341

# If you did not note the job identification number (JOBID) when it was submitted, you can use squeue to retrieve it.

$ squeue -u mst3k

# To cancel all of your jobs

$ scancel -u mst3k
# To cancel all of your pending jobs

$ scancel -t PENDING -u mst3k
# To cancel all jobs with a specified name

$ scancel --name myjob
# To restart (cancel and rerun)

$ scontrol requeue <jobid>
```
For further information about the squeue command, type man squeue on the cluster front-end machine or see the Slurm Documentation.

## CPU and Memory Usage
### Running job

Use sstat to get CPU and memory usage for a running job.
> sstat -j <jobid> --format=AveCPU,MaxRSS

The CPU efficiency can be calculated by dividing the total core time in AveCPU by the number of requested cores and run time. The above example was obtained for a job that had been running for about 4 days on 20 cores. 

### Completed job
The seff command reports the CPU and memory usage of a completed job.

Please use this for completed jobs only - efficiency statistics may be misleading for running jobs.
> $ seff <jobid>

## [Modules in Slurm](https://www.cs.virginia.edu/wiki/doku.php?id=linux_environment_modules)
Due to the way sbatch spawns a bash session (non-login session), some init files are not loaded from /etc/profile.d. This prevents the initialization of the Environment Modules system and will prevent you from loading software modules.

To fix this, simply include the following line in your sbatch scripts:
> source /etc/profile.d/modules.sh

### Virtual Environments
For most Python related packages (i.e. pip install …), it is often more applicable to utilize a virtual environment. This can be done with Anaconda or venv . The former requires loading an anaconda module, and the latter requires the python3 module.

## Others

In case of the following error try `dos2unix script_name`.

```
sbatch: error: Batch script contains DOS line breaks (\r\n)
sbatch: error: instead of expected UNIX line breaks (\n).
```