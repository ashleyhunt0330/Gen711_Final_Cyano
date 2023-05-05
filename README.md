# Gen711_Final_Cyano
MacNeill Matthews & Ashley Hunt- cyanobacteria

cp /tmp/gen711_project_data/fastp.sh <https://github.com/ashleyhunt0330/Gen711_Final_Cyano.git>/fastp.sh

chmod +x ~/fastp.sh

./fastp.sh 150 /tmp/gen711_project_data/cyano/fastqs  trimmed_fastqs

conda activate qiime2-2022.8

qiime tools import --type "SampleData[PairedEndSequencesWithQuality]" --input-format CasavaOneEightSingleLanePerSampleDirFmt --input-path /home/users/alh1081/trimmed_fastqs --output-path output_trimmed

qiime cutadapt trim-paired --i-demultiplexed-sequences output_trimmed.qza --p-cores 4 --p-front-f GTGYCAGCMGCCGCGGTAA --p-front-r CCGYCAATTYMTTTRAGTTT --p-discard-untrimmed --p-match-adapter-wildcards --verbose --o-trimmed-sequences output_trimmed2

qiime demux summarize --i-data output_trimmed2.qza --o-visualization output_trimmed3

cp denoising-stats.qza /home/users/alh1081/trimmed_fastqs

cp feature_table.qza /home/users/alh1081/trimmed_fastqs

cp rep-seqs.qza /home/users/alh1081/trimmed_fastqs

qiime metadata tabulate --m-input-file denoising-stats.qza --o-visualization denoising-stats.qzv

qiime feature-table tabulate-seqs --i-data rep-seqs.qza --o-visualization rep-seqs.qzv

qiime feature-classifier classify-sklearn --i-classifier /tmp/gen711_project_data/cyano/classifier_16S_V4-V5.qza --i-reads /home/users/alh1081/trimmed_fastqs/rep-seqs.qza --o-classification /home/users/alh1081/trimmed_fastqs/taxonomy.qza

qiime taxa barplot --i-table /home/users/mm1853/trimmed_fastqs/feature_table.qza --i-taxonomy /home/users/mm1853/trimmed_fastqs/taxonomy.qza --o-visualization /home/users/mm1853/trimmed_fastqs/my-barplot.qzv

qiime taxa barplot --i-table feature_table.qza --m-metadata-file metadata.tsv --i-taxonomy taxonomy.qza --o-visualization my-barplot.qzv

qiime feature-table filter-samples --i-table feature_table.qza --m-metadata-file metadata.tsv --o-filtered-table new_samples_table.qza

<img width="2048" alt="Screen Shot 2023-05-05 at 1 04 20 PM" src="https://user-images.githubusercontent.com/130762296/236558675-71ca3f0d-4af4-4dec-b228-e75e7e957ef6.png">

<img width="1344" alt="Screen Shot 2023-05-05 at 4 04 32 PM" src="https://user-images.githubusercontent.com/130781520/236559222-8c010a3d-5d4e-4ca5-a265-58b8d8a60077.png">

