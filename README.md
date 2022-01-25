# KBB_project
Notes and scripts for the Karner blue project. 

## Selection of populations to be included in the project
Working directory for the fastqs for this project on Texas State Anna computer: /Volumes/storage_a1/Parsed_reads_storage/all_the_fastqs

| Group | Location | State | Code | Number |
|---|---|---|---|---|
| Karner Blue | Allegan | MI | ale | 14 |
| Karner Blue | Allegan | MI | all | 13 | 
| Karner Blue | Black River State Forest | WI | brw | 16 |
| Karner Blue | Eau Claire State Forest | WI | ecw	| 5 |
| Karner Blue | Fish Lake |	WI | flw | 16 |
| Karner Blue | Fort McCoy | WI |	fmc	|	23 |
| Karner Blue | Indiana Dunes |	IN | inw | 20 |
| Karner Blue | Saratoga Springs | NY |	ssn	| 19 |
| Total | | | | 126 |
| Anna | Marlette Lake | NV | mar	| 19 |
| Anna | Castle Peak | CA | cpe | 13 |
| Anna | Donner Pass | CA | dop | 14 |
| Anna | Fall Creek | CA | fcr | 15 |
| Anna | Leek Springs | CA | lks | 20 |
| Anna | Yuba Gap | CA | ybg | 19 |
| Anna | Lodge Inn at Mormon Im. Trail | CA | loi | 18 |
| Total | | | | 118 |
| Idas | Cotton Wood Divide | WA | cwd | 25 |
| Idas | White Mt. Fire Overlook | WA | whf | 24 |
| Idas | Bunsen Peak | WY | bnp | 19 |
| Idas | Garnet Peak | MT | gnp | 16 |
| Idas | Hayden Valley | WY | hnv | 20 |
| Idas | Strawberry Mt.s | OR | stb | 17 |
| Total | | | | 121 |
| Ricei | Chinook Pass | WA | chp | 24 |
| Ricei | Big Lake | OR | big | 18 |
| Ricei | Rainy Pass | WA | rap | 20 |
| Ricei | Shovel Creek | CA | shc | 18 |
| Ricei | Soda Mt. | OR | smr	| 18 |
| Ricei | Cave Lake| CA | cav | 24 |
| Ricei | Marble Mts. | CA | mbm | 12 |
| Total | | | | 134 |
| Jackson Hybrids | Big Ice Cave | WY | bic | 18 |
| Jackson Hybrids | Hunt Mt. | WY | hum | 27 |
| Jackson Hybrids | Periodic Spring | WY | psp | 19 |
| Jackson Hybrids | Pinnacles Butte | WY | pin | 19 |
| Jackson Hybrids | Sheffield Creek | WY | sfc | 24 |
| Jackson Hybrids | Riddle Lake | WY | rdl | 23 |
| Total | | | | 130 | 
| Whites/Sierra Hybrids | Carson Pass | CA | csp | 25 |
| Whites/Sierra Hybrids | Lake Emma | CA | lae | 26 |
| Whites/Sierra Hybrids | Sweetwater Mtns. | CA | swm | 21 |
| Whites/Sierra Hybrids | Tioga Crest | CA | tic | 32 |
| Whites/Sierra Hybrids | County Line Hill | CA | clh | 25 |
| Total | | | | 129 |
| Warners Hybrids | Buck Mt.| CA | bkm | 35 |
| Warners Hybrids | Eagle Peak | CA | egp| 35 |
| Warners Hybrids | Steens Mountain | OR | stm | 9 |
| Total | | | | 79 |
| Other Hybrids | Hinkey Summit Site | NV | hss	| 25 |
| Other Hybrids | Jarbidge Mtns | NV | jrb | 30 | 
| Total | | | | 55 |
| Melissa Rockies | Albion Meadow | UT | abm | 42 | 
| Melissa Rockies | Lander | WY | lan | 23 |
| Melissa Rockies | Yellow Pine CG | WY | ywp | 16 |
| Total | | | | 81 |
| Melissa East | Beulah | ND | beu | 10 |
| Melissa East | Brandon | SD | bsd | 18 |
| Melissa East | Cody | WY | cdy | 19 |
| Melissa East | De Beque | CO | dbq | 16 |
| Melissa East | Victor | ID | vic | 20 |
| Melissa East |Abel Creek | NV | abc | 17 |
| Melissa East | Ophir City | NV | ocy | 16 |
| Melissa East | Montague | CA | mtu | 30 |
| Total | | | | 146 | 
| Melissa West | Red Earth Way | NV | rew | 16 |
| Melissa West | Baja qui | NV | bqi | 18 |
| Melissa West | Girl Farm | NV |	gfm | 23 |
| Melissa West | Verdi Crystal Peak Park | NV | vcp | 30 |
| Melissa West | Bishop | CA | bhp | 13 | 
| Melissa West | Silver Lake | NV |	sla | 18 |
| Melissa West | Trout Pond Trailhead | CA | tpt | 12 |
| Total | | | | 130 |
| Idas Alaska | Nolan Road | AK | nol	| 8 |
| Idas Alaska | Spruce Road and Barley Way | AK | sbw | 20 |
| Idas Alaska | Tok | AK | tok | 14 |
| Idas Alaska | Tolovana Creek | AK | tol | 8 |
| Total | | | | 50 |
| Nabokovi | Railroad | MN | rrm	| 23 |
| Nabokovi | Sawbill | MN | sbm	| 10 |
| Total | | | | 33 |
| **Total** | | | | **1,332** |


## DNA sequence filtering, alignment, and variant calling

- DNA sequences for this project include several individuals from past projects, and some new sequences that are as of yet unpublished
- CCN filtered for PhiX, split into individual fastqs and demultiplexed using custom perl scripts
- Reads were aligned to the *L. melissa* pacbio genome using `bwa` version 0.7.17-r1188 with `bwa mem`
- Split the genome into autosomal chromosomes and the z chromosome, to allow us to carry out separate analyses for the z
``` 
bwa mem /Volumes/data_a1/melissaGenomes/pacbio_dovetail_reference/autosomes_unassigned_Lmel_dovetailPacBio_genome.fasta ID.fastq > KBB_alnID.sam
```
- Compress, sort, and index using `samtools` version 1.9 (using htslib 1.9)
```
samtools view -b -S -o KBB_alnID.bam KBB_alnID.sam
samtools sort KBB_alnID.bam -o KBB_alnID.sorted.bam
samtools index KBB_alnID.sorted.bam
```
- Moved bam files to /Volumes/data_a2/KBB_Dec2021/variant_calling/bamfiles_keep/ on Texas State Anna computer
- Call variants using `bcftools` version 1.9 (using htslib 1.9)
```
bcftools mpileup -d 8000 -o KBB_autosomes.bcf -O b -I -f /Volumes/data_a1/melissaGenomes/pacbio_dovetail_reference/autosomes_unassigned_Lmel_dovetailPacBio_genome.fasta KBB_aln*sorted.bam
bcftools call -c -V indels -v -p 0.05 -P 0.001 -o KBB_autosomes_variants.vcf KBB_autosomes.bcf 
```
- This resulted in 23,236 variable loci (SNPs)
- Used custom perl script written by CCN to filter variants
- Remove SNPs where more than 134 individuals are missing data
- Remove SNPs where mean sequence depth is more than 2 SD the mean for all sequences
- Remove SNPs within 5 base pairs of each other

| VCF flag | Values | Explanation |  
|---|---|---|
| DP | &lt; 2664 | removed sequences with sequence depth less than 2,664 - 2 x number of individuals (1,332)|
| AF | 1 | removed sequences where allele frequency of the alternate allele was 1 |
| BQB | 3 |  maximum absolute value of the base quality rank sum test; BaseQRankSum BQB as z-score normal approximation |
| MQB | 2.5 | maximum absolute value of the mapping quality rank sum test | 
| RPB | 2 | maximum absolute value of the read position rank sum test |
| AF | &lt; 0.05 and &gt; 0.95 | remove sequences with minor allele frequencies that meet these conditions |
| MQ | 30 | minimum mapping quality |

- Started `entropy` version 1 runs for scaffolds 11, 1628, 1646. Running for 100k steps, thin 10, burnin 5k. Running on Texas State leap cluster

































