# Short instruction for lecuture
## Gitpod
Access to [gitpod website](https://www.gitpod.io/gitpod-or-classic) and activate a container from gitpod via https://github.com/ymatsumoto/tmp

## Get files for demo
```bash
gitpod:~$ wget http://www.gen-info.osaka-u.ac.jp/~matsumoto/data/mlstverse-singularity.sif
gitpod:~$ wget http://www.gen-info.osaka-u.ac.jp/~matsumoto/data/Loci.fasta.gz
gitpod:~$ wget http://www.gen-info.osaka-u.ac.jp/~matsumoto/data/JCM13569_010x.fastq.gz
gitpod:~$ gunzip Loci.fasta.gz
```

## mlstverse analysis
```bash
gitpod:~$ seqkit stat JCM13569_010x.fastq.gz
gitpod:~$ minimap2 -ax map-ont -t 4 Loci.fasta JCM13569_010x.fastq.gz | samtools sort - -o JCM13569.bam
gitpod:~$ samtools index JCM13569.bam
gitpod:~$ apptainer shell mlstverse-singularity.sif
```

```R
gitpod:~$ R
> library(mlstverse)
> library(mlstverse.Mycobacterium.db)
> result <- mlstverse("JCM13569.bam", th.pvalue=0, threads=4)
> data.frame(result$score$JCM13569.bam)
```

## Genome assembly and AMR detection
```bash
gitpod:~$ flye -t 4 --nano-hq JCM13569_010x.fastq.gz -o JCM13569
#-> See JCM13569/assembly_info.txt, assembly.fasta, assembly_graph.gfa

gitpod:~$ Bandage image JCM13569/assembly_graph.gfa JCM13569/assembly_graph.png
#-> See JCM13569/assembly_graph.png

gitpod:~$ amrfinder -t 4 --nucleotide JCM13569/assembly.fasta --database /opt/conda/bin/data/latest --plus --output JCM13569/amrfinder_result.txt
#-> See JCM13569/amrfinder_result.txt
```

## ANI calculation
```bash
gitpod:~$ ls db_20250331/*.fna.gz > ref.txt
gitpod:~$ fastANI --rl ref.txt -q JCM13569/assembly.fasta -o JCM13569/fastANI.txt -t 4
#-> See JCM13569/fastANI.txt
```
