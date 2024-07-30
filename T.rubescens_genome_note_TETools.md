TETools: BuildDatabase, RepeatModeler, RepeatMasker
================
Bailey Carlson

## R Markdown

# Download TETools package

``` bash
# Load module
module load singularity/3.8.3

# Download docker file of TETools with all inset packages and versions
singularity pull dfam-tetools-latest.sif docker://dfam/tetools:latest

# RepeatModeler Version 2.0.5
# RepeatClassifier Version 2.0.5
# RepeatMasker version 4.1.6

# Shows the resources the job used
scontrol show job JOBID
```

## Species

Amphibalanus_amphitrite A.amphitrite
GCA_019059575.1_NRLGWU_Aamphi_draft_genomic.fna.gz Balanus_glandula
B.glandula GCA_030265075.1_ASM3026507v1_genomic.fna.gz Balanus_nubilus
B.nubilus GCA_031763405.1_ASM3176340v1_genomic.fna.gz Chirona_hameri
C.hameri GCA_036785665.1_qxChiHame1_p1.0_genomic.fna.gz Lepas_anatifera
L.anatifera GCA_034440635.1_ASM3444063v1_genomic.fna.gz
Pollicipes_pollicipes P.pollicipes
GCA_011947565.3_Ppol_2.1_genomic.fna.gz Semibalanus_balanoides
S.balanoides GCA_014673585.1_Sbal3.1_genomic.fna.gz Tetraclita_rubescens
T.rubescens soon to uploaded to github

#### Replace in the full selection below organism with whatever species your working on and replace the genomic.fna.gz file with the correct genome file name for your species

## Rename files

``` bash
mv 1.BuildDatabase_T.rubescens.sh 1.BuildDatabase_organism.sh
mv 1.BuildDatabase_T.rubescens.slurm 1.BuildDatabase_organism.slurm 
mv 2.RepeatModeler_T.rubescens.sh 2.RepeatModeler_organism.sh 
mv 2.RepeatModeler_T.rubescens.slurm 2.RepeatModeler_organism.slurm 
mv 3.RepeatMasker_T.rubescens.sh 3.RepeatMasker_organism.sh 
mv 3.RepeatMasker_T.rubescens.slurm 3.RepeatMasker_organism.slurm 
```

## 1.BuildDatabase

- Takes minutes

``` bash
#!/bin/bash

#SBATCH -J 1.BuildDatabase_organism.batch # --job-name= Name for your job.
#SBATCH -N 1 # --nodes= Number of nodes to spread cores across - default is 1.
#SBATCH --mem-per-cpu 240G # the amount of memory per core to request in MB.
#SBATCH -t 0-00:10:00 # --time= Runtime in minutes. Default is 10 minutes.
#SBATCH -p test # --partition Partition to submit to the standard compute node.
#SBATCH -o slurm_1.BuildDatabase_organism_%j.out # --output= Standard out goes to this file.
#SBATCH -e slurm_1.BuildDatabase_organism_%j.err # Standard err goes to this file.
#SBATCH --mail-user bcarlson4@ucmerced.edu # this is the email for notifications.
#SBATCH --mail-type ALL # this specifies what events will be emailed.

# Load module
module load singularity/3.8.3

# Execute your processing script
/home/bcarlson4/data/CCGP/TETools/organism/1.BuildDatabase_organism.sh
```

``` bash
#!/bin/bash

# Assuming your files are in the current directory
dir="/home/bcarlson4/data/CCGP/TETools/organism"
TETools="/home/bcarlson4/local/TETools/dfam-tetools-latest.sif"

# Execute your processing script
      singularity run --home $dir \
        $TETools BuildDatabase \
        -name organism genomic.fna.gz
```

#### sbatch 1.BuildDatabase_organism.slurm

## 2.RepeatModeler

- For barnacles genomes ~550 Mb to 2.5 Gb this takes ~3 days and more
  fragmented and repeated genomes run longer overall.
- I strongly recommend only using 10 cores for this process because if
  you use more RepeatModeler will slow down tremendously.
- If you get a memory error and need to restart step 2 use -recoverDir
  and change run time and partition if need be.
- singularity run –home \$dir \$TETools RepeatModeler  
  –engine ncbi –threads 10 -database organism -recoverDir RM_folder

``` bash
#!/bin/bash

#SBATCH -J 2.RepeatModeler_organism.batch # --job-name= Name for your job.
#SBATCH -N 1 # --nodes= Number of nodes to spread cores across - default is 1.
#SBATCH --ntasks=10 #number of cores per node
#SBATCH --mem-per-cpu 24G # the amount of memory per core to request in MB.
#SBATCH -t 3-00:00:00 # --time= Runtime in minutes. Default is 10 minutes.
#SBATCH -p long # --partition Partition to submit to the standard compute node.
#SBATCH -o slurm_2.RepeatModeler_organism_%j.out # --output= Standard out goes to this file.
#SBATCH -e slurm_2.RepeatModeler_organism_%j.err # Standard err goes to this file.
#SBATCH --mail-user bcarlson4@ucmerced.edu # this is the email for notifications.
#SBATCH --mail-type ALL # this specifies what events will be emailed.

# Load module
module load singularity/3.8.3

# Execute your processing script
/home/bcarlson4/data/CCGP/TETools/organism/2.RepeatModeler_organism.sh
```

``` bash
#!/bin/bash

# Assuming your files are in the current directory
dir="/home/bcarlson4/data/CCGP/TETools/organism"
TETools="/home/bcarlson4/local/TETools/dfam-tetools-latest.sif"

# Execute your processing script
      singularity run --home $dir \
        $TETools RepeatModeler \
        --engine ncbi --threads 10 -database organism
```

#### sbatch 2.RepeatModeler_organism.slurm

## 3.RepeatMasker

- Takes less than a day

``` bash
#!/bin/bash

#SBATCH -J 3.RepeatMasker_organism.batch # --job-name= Name for your job.
#SBATCH -N 1 # --nodes= Number of nodes to spread cores across - default is 1.
#SBATCH --ntasks=10 #number of cores per node
#SBATCH --mem-per-cpu 24G # the amount of memory per core to request in MB.
#SBATCH -t 1-00:00:00 # --time= Runtime in minutes. Default is 10 minutes.
#SBATCH -p medium # --partition Partition to submit to the standard compute node.
#SBATCH -o slurm_3.RepeatMasker_organism_%j.out # --output= Standard out goes to this file.
#SBATCH -e slurm_3.RepeatMasker_organism_%j.err # Standard err goes to this file.
#SBATCH --mail-user bcarlson4@ucmerced.edu # this is the email for notifications.
#SBATCH --mail-type ALL # this specifies what events will be emailed.

# Load module
module load singularity/3.8.3

# Execute your processing script
/home/bcarlson4/data/CCGP/TETools/organism/3.RepeatMasker_organism.sh
```

``` bash
#!/bin/bash

# Assuming your files are in the current directory
dir="/home/bcarlson4/data/CCGP/TETools/organism"
TETools="/home/bcarlson4/local/TETools/dfam-tetools-latest.sif"

# Execute your processing script
      singularity run --home $dir \
        $TETools RepeatMasker \
        -pa 5 --gff -lib RM_path/consensi.fa.classified \
        genomic.fna.gz
```

#### sbatch 3.RepeatMasker_organism.slurm
