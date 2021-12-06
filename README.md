# PopPipe
 A comprehensive pipeline for population genetic analysis containing Read mapping, Variant calling, and Population genetic analysis

![fig1.png](Readme%20%E1%84%8C%E1%85%A1%E1%86%A8%E1%84%89%E1%85%A5%E1%86%BC%208472678d2d5f4e37add2713b08d1af86/fig1.png)

### Install PopPipe
--

```
1. Download source 
	$ git clone https://github.com/jkimlab/PopPipe.git
```

### Install PopPipe using Docker



```
1. Install docker (https://docs.docker.com/install/linux/docker-ce/ubuntu)
    curl -fsSL https://get.docker.com/ | sudo sh
    sudo usermod -aG docker $USER 	# adding user to the “docker” group

2. Download source
  git clone https://github.com/jkimlab/PopPipe.git
  
3. Build image using Dockerfile 
  - Change to the directory where Dockerfile is located (PopPipe/docker/)
    docker build -t [docker image name] ./ &> log_image_build

4. Run by docker
  - Run image and create container
    docker run -v [Local directory containing data]:[Path of connecting directory on container] -it [docker image name]
```

### Prepare parameter files



Check out the directory `./bin/params/` containing example parameter files

1. Input file
    1. Input for read mapping 
        
        ```
        #===================================================================#
        #                  Input file for ReadMapping step                  #
        #===================================================================#
        
        #### ReadMapping ####
        ### DNA-seq data path(input file of ReadMapping) ###
        ## Paired-end read pairs
        ## <Hanwoo_Hanwoo1> => RGSM name, format:(BreedName)_(BreedName)(Number)
        ## [lib1]
        ## Path of forward read
        ## path of reverse read
        <Abreed_Abreed1>
        [lib1]
        [path to sequencing read]/[sequencing read]-1.fq.gz
        [path to sequencing read]/[sequencing read]-2.fq.gz
        [lib2]
        [path to sequencing read]/[sequencing read]-1.fq.gz
        [path to sequencing read]/[sequencing read]-2.fq.gz
        <Abreed_Abreed2>
        [lib1]
        [path to sequencing read]/[sequencing read]-1.fq.gz
        [path to sequencing read]/[sequencing read]-2.fq.gz
        [lib2]
        [path to sequencing read]/[sequencing read]-1.fq.gz
        [path to sequencing read]/[sequencing read]-2.fq.gz
        ```
        
    2. Input for variant calling 
        
        ```
        #==================================================================#
        #                Input file for VariantCalling step                #
        #==================================================================#
        
        #### VariantCalling ####
        ### Bam file path(input file of ReadMapping or Varaint Calling) ###
        ## <Hanwoo_Hanwoo1> => RGSM name (Before read grouping in ReadMapping step, format:(BreedName)_(BreedName)(Number), example : Hanwoo_Hanwoo1)
        # Path of bam file
        <Abreed_Abreed1>
        [path to bam file]/[Abreed_Abreed1].recal.addRG.marked.srot.bam
        <Abreed_Abreed2>
        [path to bam file]/[Abreed_Abreed2].recal.addRG.marked.srot.bam
        <Bbreed_Breed1>
        [path to bam file]/[Bbreed_Breed1].recal.addRG.marked.srot.bam
        <Bbreed_Breed2>
        [path to bam file]/[Bbreed_Breed2].recal.addRG.marked.srot.bam
        ```
        
    3. Input for postprocessing (Format converting or data filtering step)
        
        ```
        #==================================================================#
        #                Input file for Postprocessing step                #
        #==================================================================#
        
        #### Postprocessing ####
        ### Vcf file path(input file of Postprocessing) ###
        ## Path of vcf file
        [path to variant call VCF]/[variant call].vcf.gz
        
        #### If you take the Effective size step in Population analysis, write the BAM files path ####
        #### Population ####
        ### Bam files path ###
        ## <Hanwoo_Hanwoo1> => RGSM name (Before read grouping in ReadMapping step, format:(BreedName)_(BreedName)(Number), example : Hanwoo_Hanwoo1)
        ## Path of BAM file
        <Abreed_Abreed1>
        [path to bam file]/[Abreed_Abreed1].recal.addRG.marked.srot.bam
        <Abreed_Abreed2>
        [path to bam file]/[Abreed_Abreed2].recal.addRG.marked.srot.bam
        <Bbreed_Breed1>
        [path to bam file]/[Bbreed_Breed1].recal.addRG.marked.srot.bam
        <Bbreed_Breed2>
        [path to bam file]/[Bbreed_Breed2].recal.addRG.marked.srot.bam
        ```
        
    4. Input for postprocessing (Format converting or data filtering step)
        
        ```
        #==================================================================#
        #                  Input file for Population step                  #
        #==================================================================#
        
        #### If you take the Effective size step in Population analysis, write the BAM files path ####
        #### Population ####
        ### BAM ###
        ## <Hanwoo_Hanwoo1> => RGSM name (Before read grouping in ReadMapping step, format:(BreedName)_(BreedName)(Number), example : Hanwoo_Hanwoo1)
        ## Path of bam file
        <Abreed_Abreed1>
        [path to bam file]/[Abreed_Abreed1].recal.addRG.marked.srot.bam
        <Abreed_Abreed2>
        [path to bam file]/[Abreed_Abreed2].recal.addRG.marked.srot.bam
        <Bbreed_Breed1>
        [path to bam file]/[Bbreed_Breed1].recal.addRG.marked.srot.bam
        <Bbreed_Breed2>
        [path to bam file]/[Bbreed_Breed2].recal.addRG.marked.srot.bam
        
        ### Vcf ###
        ## Path of vcf file
        
        ### Plink ###
        ## Input file of Population analysis ##
        
        ### Hapmap ###
        ## Input file of Population analysis ##
        
        ```
        
2. Parameter file
    - There is a fixed parameter file for runDocker environment
    
    ```python
    #=======================================================================#
    #                   parameter file for Population pipeline              #
    #=======================================================================#
    
    #==================================================#
    ####                 ReadMapping                ####
    #==================================================#
    ### Program path ###
    ## Write 'OPTION = 1' if you want to use the BWA tool in Mapping step
    ## Write 'OPTION = 2' if you want to use the Bowtie2 tool in Mapping step
    
    OPTION = 1
    BWA = [program path]/bwa
    BOWTIE2 = [program path]/bowtie2
    SAMTOOLS = [program path]/samtools
    PICARD = [program path]/picard.jar
    ```
    
3. Sample file
    
    ```python
    #sample sex[F, M, U, FemaleMale/U for none] population
    ABreed1 U        Abreed
    ABreed2 U        Abreed
    BBreed1 U        Bbreed
    BBreed2 U        Bbreed
    ```
    

### Run PopPipe



Parameters for run `main.py`

```python
#run all steps
./main.py -D 0 -P ./main_param.txt -I ./main_input_pre.txt -A ./main_sample.txt -V 1 -J 8 &> logs

#from variant.vcf to population analysis results
./main.py -D 0 -P ./main_param.txt -I ./main_input_pre.txt -A ./main_sample.txt -V 1 -J 8 &> 02.logs

#
./main.py -P ./main_param.txt -I ./main_input_pre.txt -A ./main_sample.txt -V 1 -J 8 &> 03.logs

#
./main.py -P ./main_param.txt -I ./main_input_pre.txt -A ./main_sample.txt -V 1 -J 8 &> 04.logs
```

### PopPipe Results



Results from all steps

1. Read mapping 
    
    Read mapping files for all samples
    
    `01_readMapping/04ReadRegrouping/[population]_[sample].addRG.marked.sort.bam`
    
2. Variant Calling 
    
    Variant call generated using all population sequencing data 
    
    `02_VariantCalling/VariantCalling/All.variant.combined.g.vcf.gz`
    
3. Postprocessing
    
    Various formatted variant call data 
    
    `03_Postprocessing/Hapmap/variant.combined.GT.SNP.flt.hapmap`
    
    `03_Postprocessing/plink/[prefix].bed`
    
    `03_Postprocessing/plink/[prefix].bim`
    
    `03_Postprocessing/plink/[prefix].fam`
    
    `03_Postprocessing/plink/[prefix].map`
    
    `03_Postprocessing/plink/[prefix].nosex`
    
    `03_Postprocessing/plink/[prefix].ped`
    
4. Population analysis
    
    7 population analysis results
    
    1. Population structure analysis
        
        `04_Population/STRUCTURE/CLUMPAK/K=[n].MajorCluster.png`
        
        `04_Population/STRUCTURE/CLUMPAK/pipeline_summary.pdf`
        
    2. Fixation index analysis
        
        `04_Population/Fst/round[n]/Fst_result.pdf`
        
    3. LD decay analysis
        
        `04_Population/LD/[maximum distance parameter]/Plot/Rplots.pdf`
        
        `04_Population/LD/[maximum distance parameter]/Plot/out.[population name]`
        
    4. Population admixture analysis 
        
        `04_Population/ADMIXTOOLS/admixtools_3pop/result.out`
        
        `04_Population/ADMIXTOOLS/admixtools_4diff/result.out`
        
        `04_Population/ADMIXTOOLS/admixtools_f4stat/result.out`
        
        `04_Population/ADMIXTOOLS/admixtools_Dstat/result.out`
        
    5. PCA
        
        `04_Population/PCA/all.PCA.pdf`
        
        `04_Population/PCA/PCs.info`
        
    6. Phylogenetic analysis
        
        `04_Population/SNPhylo/snphylo_tree.pdf`
        
        `04_Population/SNPhylo/Rplots.pdf`
        
        `04_Population/SNPhylo/snphylo.ml.txt`
        

### Run PopPipe for making the results



- DATA
    - Four cattle population (Jersey, Simmental, Angus, Holstein from NCBI SRA, PRJNA238491, DOI: [10.1038/ng.3034](https://doi.org/10.1038/ng.3034))
    - Additional single cattle breed data (Hanwoo from NCBI SRA, PRJNA210523, DOI: [10.1093/gbe/evu102](https://dx.doi.org/10.1093%2Fgbe%2Fevu102))
- Quality Control
    - IlluQC ([https://doi.org/10.1371/journal.pone.0030619](https://doi.org/10.1371/journal.pone.0030619))
    - TrimGalore ([https://www.bioinformatics.babraham.ac.uk/projects/trim_galore/](https://www.bioinformatics.babraham.ac.uk/projects/trim_galore/))
- Run PopPipe
    - Command
    
    ```python
    ./main.py -D 0 -P ./main_param.txt -I ./main_input_pre.txt -A ./main_sample.txt -V 1 -J 8 &> logs
    ```
    
    - see `example/main_input_pre.txt`
    - see `example/main_sample.txt`
    - see `example/main_param.txt`

### **Included third party tools**



See `Requirements/thirdPartyTools.txt`

### Contact


[bioinfolabkr@gmail.com](mailto:bioinfolabkr@gmail.com)
