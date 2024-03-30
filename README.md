RNAseq pepline for DFcKO_hip_nacc_mPC_Momoe
=============================================

Step 1: Format Conversion
------------------------
Converting sra format files to fastq format

Tool: [fastq-dump](https://github.com/ncbi/sra-tools)

```
fastq-dump ${FileName}.sra --gzip &&
mv ${FileName}_R1_001.fastq.gz ./${FileName} &&
mv ${FileName}_R2_001.fastq.gz ./${FileName} &&
```


Step 2: Adapter trimming
-----------------------

Tool: [trim_galore](https://www.bioinformatics.babraham.ac.uk/projects/trim_galore/)

```
mkdir ./${FileName}/trim_galore_result
trim_galore --fastqc --fastqc_args ""--nogroup"" --paired ./${FileName}/${FileName}_R1_001.fastq.gz ./${FileName}/${FileName}_R2_001.fastq.gz -o ./${FileName}/trim_galore_result &&
```

Step 3: Reads Mapping

Tool: [STAR](https://github.com/alexdobin/STAR) 

References：[STAR: ultrafast universal RNA-seq aligner](https://pubmed.ncbi.nlm.nih.gov/23104886/)

```
STAR --runThreadN 8 --genomeDir /media/ohtanlab/3ec63b8d-e640-41c8-96d2-6de634743b30/sefan/reference_file/gencode/rsem_index --readFilesIn ./${FileName}/trim_galore_result/${FileName}_R1_001_val_1.fq.gz ./${FileName}/trim_galore_result/${FileName}_R2_001_val_2.fq.gz --readFilesCommand gunzip -c --outSAMtype BAM SortedByCoordinate --outFileNamePrefix ./${FileName}/star_result/${FileName}_ --quantMode TranscriptomeSAM --outSAMattributes All ;
```

Step4: Quantification

Tool: [RSEM](https://github.com/deweylab/RSEM)

References：[RSEM: accurate transcript quantification from RNA-Seq data with or without a reference genome](https://pubmed.ncbi.nlm.nih.gov/21816040/)

```
rsem-calculate-expression --bam --paired-end --no-bam-output -p 8 --strand-specific ./${FileName}/star_result/${FileName}_Aligned.toTranscriptome.out.bam /media/ohtanlab/3ec63b8d-e640-41c8-96d2-6de634743b30/sefan/reference_file/gencode/rsem_index/gencode_v30 ./${FileName}/rsem_result/${FileName}
```


