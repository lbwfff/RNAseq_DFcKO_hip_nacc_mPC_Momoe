RNAseq pepline for DFcKO_hip_nacc_mPC_Momoe
=============================================


##Trim galore
```mkdir ./${FileName}/trim_galore_result ;
```trim_galore --fastqc --fastqc_args ""--nogroup"" --paired ./${FileName}/${FileName}_R1_001.fastq.gz ./${FileName}/${FileName}_R2_001.fastq.gz -o ./${FileName}/trim_galore_result &&

