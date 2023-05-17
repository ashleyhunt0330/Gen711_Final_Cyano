# Gen711 Final - Cyanobacteria in New England Lakes
MacNeill Matthews & Ashley Hunt

# Background

Picocyanobacteria are photoautotrophs that are commonly found in olgiotrophic and mesotrophic waters, contributing significantly to oxygen productivity and nutrient cycling (Schallenberg et al, 2021). While much study of picocyanobacteria has focused on marine species, freshwater species have recently been studied for their variable community structure and differing horizontal-spacial niches based on trophic levels (Schallenberg et al, 2021).

The production of cyanobacteria in aquatic ecosystems causes harm through the production of toxins secreted by the cyanobacteria (MacKeigan et al, 2022). The degradation of these blooms also causes a depletion in oxygen, leading to further ecosystem damage (MacKeigan et al, 2022). To properly study these blooms, it is crucial to identify the species involved for accurate bioassesment (MacKeigan et al, 2022).

In this study, it was being examined whether large cyanobacteria or picocyanobacteria were responsible for causing poisonous toxins in algae blooms. This study targeted the eDNA of the bacteria to compare target bacteria, large cyanobacteria, and bloom forming cyanobacteria on the surface during a cyanobacteria bloom. This data was collected in fastq files.

# Methods
This study is comparing the 16s metabarcoding locus of the bacteria of a lake in bloom, as metabarcoding with eDNA is currently one of the foremost ways to assess aquatic biodiversity (MacKeigan et al, 2022). Unpacking and arranging of the data was performed on VScode. Here is our code for opening and arranging the data 

### FASTQ sample QA/QC-

cp /tmp/gen711_project_data/fastp.sh <https://github.com/ashleyhunt0330/Gen711_Final_Cyano.git>/fastp.sh

chmod +x ~/fastp.sh

./fastp.sh 150 /tmp/gen711_project_data/cyano/fastqs  trimmed_fastqs

conda activate qiime2-2022.8

Note: We initially had a problem activating the qiime environment, which delayed this project.

qiime tools import --type "SampleData[PairedEndSequencesWithQuality]" --input-format CasavaOneEightSingleLanePerSampleDirFmt --input-path /home/users/alh1081/trimmed_fastqs --output-path output_trimmed

qiime cutadapt trim-paired --i-demultiplexed-sequences output_trimmed.qza --p-cores 4 --p-front-f GTGYCAGCMGCCGCGGTAA --p-front-r CCGYCAATTYMTTTRAGTTT --p-discard-untrimmed --p-match-adapter-wildcards --verbose --o-trimmed-sequences output_trimmed2

qiime demux summarize --i-data output_trimmed2.qza --o-visualization output_trimmed3

### Denoising-
Note: Denoising step for cyanobacteria took over 2 hours, an thus we were unable to complete it during given time. Files were denoised by Hannah Pare, and copied to continue further steps. 

cp denoising-stats.qza /home/users/alh1081/trimmed_fastqs

cp feature_table.qza /home/users/alh1081/trimmed_fastqs

cp rep-seqs.qza /home/users/alh1081/trimmed_fastqs

qiime metadata tabulate --m-input-file denoising-stats.qza --o-visualization denoising-stats.qzv

### Taxonomy assignment-

qiime feature-table tabulate-seqs --i-data rep-seqs.qza --o-visualization rep-seqs.qzv

### Classify rep seqs-

qiime feature-classifier classify-sklearn --i-classifier /tmp/gen711_project_data/cyano/classifier_16S_V4-V5.qza --i-reads /home/users/alh1081/trimmed_fastqs/rep-seqs.qza --o-classification /home/users/alh1081/trimmed_fastqs/taxonomy.qza

qiime taxa barplot --i-table /home/users/mm1853/trimmed_fastqs/feature_table.qza --i-taxonomy /home/users/mm1853/trimmed_fastqs/taxonomy.qza --o-visualization /home/users/mm1853/trimmed_fastqs/my-barplot.qzv

qiime taxa barplot --i-table feature_table.qza --m-metadata-file metadata.tsv --i-taxonomy taxonomy.qza --o-visualization my-barplot.qzv

### Metadata and background info-

qiime feature-table filter-samples --i-table feature_table.qza --m-metadata-file metadata.tsv --o-filtered-table new_samples_table.qza

# Findings

<img width="1344" alt="Screen Shot 2023-05-05 at 4 04 32 PM" src="https://user-images.githubusercontent.com/130781520/236559222-8c010a3d-5d4e-4ca5-a265-58b8d8a60077.png">

<img width="1068" alt="Screen Shot 2023-05-05 at 1 12 00 PM" src="https://user-images.githubusercontent.com/130762296/236559976-1e16b219-c2aa-45ba-83f1-dff894df57bf.png">

<img width="330" alt="Screen Shot 2023-05-05 at 4 25 59 PM" src="https://user-images.githubusercontent.com/130781520/236562204-dc1a28dc-4106-4d0f-8912-dfa8a91fe44c.png">

# Analysis

Each different colored bar on the chart shows a different species of bacteria. When looking at the data, one can see that during a cyanobacteria bloom many other species are no longer found in the water samples. A cyanobacteria bloom severely limits biodiversity in a body of water. In the first chart the data is organized by species found in each sample. It shows the process of the environment going from being a healthy and diverse ecosystem to a cyanobacteria bloom with little diversity. The second chart is organized by the dates of sample collection. This shows a cyclical pattern of more cyanobacteria growing in an area and the environment later recovering from the bloom. 

# Sources

Paul W. MacKeigan, Rebecca E. Garner, Marie-Ève Monchamp, David A. Walsh, Vera E. Onana, Susanne A. Kraemer, Frances R. Pick, Beatrix E. Beisner, Michael D. Agbeti, Naíla Barbosa da Costa, B. Jesse Shapiro, Irene Gregory-Eaves, Comparing microscopy and DNA metabarcoding techniques for identifying cyanobacteria assemblages across hundreds of lakes, Harmful Algae, Volume 113, 2022, 102187, ISSN 1568-9883, https://doi.org/10.1016/j.hal.2022.102187. (https://www.sciencedirect.com/science/article/pii/S1568988322000166)
Lena A Schallenberg, John K Pearman, Carolyn W Burns, Susanna A Wood, Spatial abundance and distribution of picocyanobacterial communities in two contrasting lakes revealed using environmental DNA metabarcoding, FEMS Microbiology Ecology, Volume 97, Issue 7, July 2021, fiab075, https://doi.org/10.1093/femsec/fiab075
