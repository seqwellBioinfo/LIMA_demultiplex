---
output:
  word_document: default
  html_document: default
  pdf_document: default
---
# LongPlex (Early Access) Demultiplexing Guidelines for PacBio HiFi Sequencing
### run lima first using i7 barcode, then using i5 barcode on the unbarcoded reads from i7 demultiplex


#### 1. code for run lima using i7 barcode
```

mkdir -p  log_i7 demux_i7

bam=example.hifi_reads.bc001.bam
samp=$(basename $bam .bam)
lima  -j 128   \
 --single-side  \
 --ccs  --min-score 26 \
 --peek-guess  \
 --store-unbarcoded \
 --split-named --log-level INFO \
 --log-file log_i7/$samp.lima.log \
 $bam \
 LongPlex_set3_i7_trimmed_adapters.fa \
 demux_i7/demux_i7.$samp.bam


 ```





#### 2. code for run lima using i5 barcode on the unbarcoded reads after i7 demultiplexing step
##### check the  demux_i7 folder and find the bam file with name ended with unbarcoded.bam. The example here is demux_i7.example.hifi_reads.bc001.unbarcoded.bam
```

mkdir -p  log_i5 demux_i5
mkdir -p demux_i5

bam=demux_i7.example.hifi_reads.bc001.unbarcoded.bam
samp=$(basename $bam .bam)
echo $samp
lima  -j 128   \
 --single-side  \
 --ccs --min-score 26 \
 --peek-guess   \
 --store-unbarcoded \
 --split-named --log-level INFO \
 --log-file log_i5/$samp.lima.log \
 $bam \
 LongPlex_set3_i5_trimmed_adapters.fa \
 demux_i5/demux.$samp.bam
 
 
 ```


#### barcodes example 
barcode is 10 bp seqwell barcode plus illumina adapter, example is as below

##### example from LongPlex_set3_i7_trimmed_adapters.fa
```
>seqwell_UDI3_A01_P7
GGACTAACTTGTCTCGTGGGCTCGGAGATGTGTATAAGAGACAG
>seqwell_UDI3_B01_P7
ATATCGAGGCGTCTCGTGGGCTCGGAGATGTGTATAAGAGACAG
```

##### example from LongPlex_set3_i5_trimmed_adapters.fa
```
>seqwell_UDI3_A01_P5
CACAATCCTATCGTCGGCAGCGTCAGATGTGTATAAGAGACAG
>seqwell_UDI3_B01_P5
TGATTGAAGGTCGTCGGCAGCGTCAGATGTGTATAAGAGACAG
```
