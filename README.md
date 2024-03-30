RNAseq pepline for DFcKO_hip_nacc_mPC_Momoe
=============================================

Step 1: Format Conversion
------------------------
Tool: [fastq-dump](https://github.com/ncbi/sra-tools)

Converting sra format files to fastq format
```
fastq-dump ${FileName}.sra --gzip &&
mv ${FileName}_R1_001.fastq.gz ./${FileName} &&
mv ${FileName}_R2_001.fastq.gz ./${FileName} &&
```


Step 2: Adapter trimming
-----------------------
Tool: [trim_galore](https://www.bioinformatics.babraham.ac.uk/projects/trim_galore/)

Trim Galore is a wrapper script to automate quality and adapter trimming as well as quality control
```
mkdir ./${FileName}/trim_galore_result
trim_galore --fastqc --fastqc_args ""--nogroup"" --paired ./${FileName}/${FileName}_R1_001.fastq.gz ./${FileName}/${FileName}_R2_001.fastq.gz -o ./${FileName}/trim_galore_result &&
```

Step 3: Reads Mapping
---------------------
Tool: [STAR](https://github.com/alexdobin/STAR) . References：[STAR: ultrafast universal RNA-seq aligner](https://pubmed.ncbi.nlm.nih.gov/23104886/)

Mapping post-trimming reads to reference genomes using STAR, `genomeDir` is the location of the constructed reference genome index. `readFilesIn` is input files (post-trimming reads). `readFilesCommand gunzip -c` indicates that the input file is a gz compressed file. `outSAMtype BAM SortedByCoordinate` output sorted by coordinate Aligned.sortedByCoord.out.bam file. `quantMode TranscriptomeSAM` output SAM/BAM alignments to transcriptome into a separate file

```
STAR --runThreadN 8 --genomeDir /media/ohtanlab/sefan/reference_file/gencode/rsem_index --readFilesIn ./${FileName}/trim_galore_result/${FileName}_R1_001_val_1.fq.gz ./${FileName}/trim_galore_result/${FileName}_R2_001_val_2.fq.gz --readFilesCommand gunzip -c --outSAMtype BAM SortedByCoordinate --outFileNamePrefix ./${FileName}/star_result/${FileName}_ --quantMode TranscriptomeSAM --outSAMattributes All ;
```

Step4: Quantification
----------------------
Tool: [RSEM](https://github.com/deweylab/RSEM). References：[RSEM: accurate transcript quantification from RNA-Seq data with or without a reference genome](https://pubmed.ncbi.nlm.nih.gov/21816040/)

Using the files after STAR mapping we performed expression quantification
```
rsem-calculate-expression --bam --paired-end --no-bam-output -p 8 --strand-specific ./${FileName}/star_result/${FileName}_Aligned.toTranscriptome.out.bam /media/ohtanlab/sefan/reference_file/gencode/rsem_index/gencode_v30 ./${FileName}/rsem_result/${FileName}
```

Step5: Differential gene expression analysis
----------------------
Tool: [DESeq2](https://bioconductor.org/packages/release/bioc/html/DESeq2.html). References:[Moderated estimation of fold change and dispersion for RNA-seq data with DESeq2](https://pubmed.ncbi.nlm.nih.gov/25516281/)

For the quantitative expression files generated by RSEM we performed differential analysis. DESeq2 is an R package used to perform differential analysis of RNA-seq expression data.



