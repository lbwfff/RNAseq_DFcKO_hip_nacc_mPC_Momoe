RNAseq pepline for DFcKO_hip_nacc_mPC_Momoe
=============================================

Step 1: Format Conversion, converting sra format files to fastq format

Tool: [fastq-dump](https://github.com/ncbi/sra-tools)

```
fastq-dump ${FileName}.sra --gzip &&
mv ${FileName}_R1_001.fastq.gz ./${FileName} &&
mv ${FileName}_R2_001.fastq.gz ./${FileName} &&
```


Step 2: Adapter trimming, Trim Galore is a wrapper script to automate quality and adapter trimming

Tool: [trim_galore](https://www.bioinformatics.babraham.ac.uk/projects/trim_galore/)

```
mkdir ./${FileName}/trim_galore_result
trim_galore --fastqc --fastqc_args ""--nogroup"" --paired ./${FileName}/${FileName}_R1_001.fastq.gz ./${FileName}/${FileName}_R2_001.fastq.gz -o ./${FileName}/trim_galore_result &&
```

Step 3: Reads Mapping

Tool: [STAR](https://github.com/alexdobin/STAR) 

References：[STAR: ultrafast universal RNA-seq aligner](https://pubmed.ncbi.nlm.nih.gov/23104886/)



