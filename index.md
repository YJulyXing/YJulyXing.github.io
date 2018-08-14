# SimulateCNVs

A tool for simulating CNVs for WES or WGS data. It simulates rearranged genomes, short reads (fastq) and bam files as desired by the user. There are several ways and distributions to choose from to generate desired CNVs.

## Installation
Download the package here and unpack it.

## Requirements
* General use: Python 2.7. Required python packages: argparse, random, os, subprocess, math, sys, time
* To generate short reads (fastq) outputs (see requirements for ART_illumina at https://www.niehs.nih.gov/research/resources/software/biostatistics/art/index.cfm): 
 1. GNU g++ 4.0 or above (http://gcc.gnu.org/install) 
 2. GNU gsl library (http://www.gnu.org/s/gsl/)
* To generate bam outputs: 
 1. Samtools
 2. BWA
 3. picard 2.15
 4. GATK

## Usage
SimulateCNVs.py [-h] -Type {g,e} -G GENOME_FILE [-T TARGET_REGION_FILE]
                [-e_cnv EXON_CNV_LIST] [-e_chr EXON_CNV_CHR]
				[-e_tol EXON_CNV_TOL] [-e_cl EXON_CNV_LEN_FILE]
				[-o_cnv OUT_CNV_LIST] [-o_chr OUT_CNV_CHR]
				[-o_tol OUT_CNV_TOL] [-o_cl OUT_CNV_LEN_FILE]
				[-ol OVERLAP_BPS] [-g_cnv GENOME_CNV_LIST]
				[-g_chr GENOME_CNV_CHR] [-g_tol GENOME_CNV_TOL]
				[-g_cl GENOME_CNV_LEN_FILE] [-em]
				[-min_len CNV_MIN_LENGTH] [-max_len CNV_MAX_LENGTH]
				[-min_cn MIN_COPY_NUMBER] [-max_cn MAX_COPY_NUMBER]
				[-p PROPORTION_INS] [-f MIN_FLANKING_LEN]
				[-ms {random,uniform,gauss}]
				[-ml {random,uniform,gauss,user}] [-c COVERAGE]
				[-fs FRAG_SIZE] [-s STDEV] [-l READ_LENGTH]
				[-tf TARGET_REGION_FLANK] [-pr]
				[-q_min MIN_BASE_QUALITY] [-q_max MAX_BASE_QUALITY]
				[-clr CONNECT_LEN_BETWEEN_REGIONS] [-o OUTPUT_DIR]
				[-rn REARRANGED_OUTPUT_NAME] [-n NUM_SAMPLES] [-sc]
				[-ssr] [-sb] [-picard PATH_TO_PICARD]
				[-GATK PATH_TO_GATK]


optional arguments:

|   Parameter                    |     Default value     |    Explanation                             |
| :----------------------------: | :-------------------: | :----------------------------------------- |
| -h, --help                     |                       | show this help message and exit            |

Mandatory inputs:

|   Parameter                    |     Default value     |    Explanation                             |
| :----------------------------: | :-------------------: | :----------------------------------------- |
| -Type {g,e}                     |                     | simulation for WGS or WES            |
| -G GENOME_FILE                     |                     | Reference genome FASTA file           |

Inputs for simulating rearranged genomes for WES data:

|   Parameter                    |     Default value     |    Explanation                             |
| :----------------------------: | :-------------------: | :----------------------------------------- |
| -T TARGET_REGION_FILE                     |                     | Target region file           |
| -e_cnv EXON_CNV_LIST                     |                     | A user-defined list of CNVs overlapping with exons           |
| -e_chr EXON_CNV_CHR                     |                     | Number of CNVs overlapping with exons to be generated on each chromosome           |
| -e_tol EXON_CNV_TOL                     |                     | Total number of CNVs overlapping with exons to be generated across the genome (an estimate)           |
| -e_cl EXON_CNV_LEN_FILE                     |                     | User supplied file of CNV length for CNVs overlapping with exons           |
| -o_cnv OUT_CNV_LIST                     |                     | A user-defined list of CNVs outside of exons           |
| -o_chr OUT_CNV_CHR                     |                     | Number of CNVs outside of exons to be generated on each chromosome           |
| -o_tol OUT_CNV_TOL                     |                     | Total number of CNVs outside of exons to be generated across the genome (an estimate)           |
| -o_cl OUT_CNV_LEN_FILE                     |                     | User supplied file of CNV length for CNVs outside of exons            |
| -ol OVERLAP_BPS                     | 100                    | For each CNV overlapping with exons, number of minimum overlapping bps            |

Inputs for simulating rearranged genomes for WGS data:

|   Parameter                    |     Default value     |    Explanation                             |
| :----------------------------: | :-------------------: | :----------------------------------------- |
| -g_cnv GENOME_CNV_LIST                     |                     | A user-defined list of CNVs outside of exons           |
| -g_chr GENOME_CNV_CHR                     |                     | Number of CNVs overlapping with exons to be generated on each chromosome           |
| -g_tol GENOME_CNV_TOL                     |                     | Total number of CNVs overlapping with exons to be generated across the genome (an estimate)           |
| -g_cl GENOME_CNV_LEN_FILE                     |                     | User supplied file of CNV length           |

General inputs for simulating rearranged genomes with CNVs:

|   Parameter                    |     Default value     |    Explanation                             |
| :----------------------------: | :-------------------: | :----------------------------------------- |
| -em                     |                     | Exclude missing sequences for CNV simulation            |
| -min_len CNV_MIN_LENGTH                    | 1000                    |  Minimum CNV length in bps           |
| -max_len CNV_MAX_LENGTH                     | 100000                    | Maximum CNV length in bps            |
| -min_cn MIN_COPY_NUMBER                     | 2                    | Minimum copy number for insertions           |
| -max_cn MAX_COPY_NUMBER                     | 10                    | Maximum copy number for insertions            |
| -p PROPORTION_INS                     | 0.5                    | Proportion of insertions           |
| -f MIN_FLANKING_LEN                     | 50                    |  Minimum length between each CNV            |
| -ms {random,uniform,gauss}                     | random                    | Distribution of CNVs           |
| -ml {random,uniform,gauss,user}                     | random                    | Distribution of CNV length            |
| -G GENOME_FILE                     |                     | Reference genome FASTA file           |
| -Type {g,e}                     |                     | simulation for WGS or WES            |
| -G GENOME_FILE                     |                     | Reference genome FASTA file           |

Inputs for simulating short reads (fastq):

|   Parameter                    |     Default value     |    Explanation                             |
| :----------------------------: | :-------------------: | :----------------------------------------- |
| -c COVERAGE                     | 20                    | Fold coverage on target regions to be generated for each genome            |
| -fs FRAG_SIZE                     | 100                    | Mean fragment size to be generated            |
| -s STDEV                     | 20                    | Standard deviation of fragment sizes            |
| -l READ_LENGTH                     | 50                    | Read length of each short read            |
| -tf TARGET_REGION_FLANK                     | 0                    | Length of flanking region up and down stream of target regions to be sequenced (this step take place after -clr). Only works with WES simulation.            |
| -pr |  | Select if paired-end sequencing |
| -q_min MIN_BASE_QUALITY | 0 | Minimum base quality for short reads simulation |
| -q_max MAX_BASE_QUALITY | 80 | Maximum base quality for short reads simulation |
| -clr CONNECT_LEN_BETWEEN_REGIONS |  | Maximum length bwtween target regions to connect the target regions. Only works with WES simulation. |

Inputs for other simulation parameters:

|   Parameter                    |     Default value     |    Explanation                             |
| :----------------------------: | :-------------------: | :----------------------------------------- |
| -o OUTPUT_DIR | simulation_output | Output directory |
| -rn REARRANGED_OUTPUT_NAME | test | Prefix of the rearranged outputs (do not include directory name) |
| -n NUM_SAMPLES | 1 | Number of test samples to be generated |
| -sc |  | Simulation for control genome |
| -ssr |  | Simulate short reads (fastq) files |
| -sb |  | Simulate bam files |
| -picard PATH_TO_PICARD |  | Absolute path to picard |
| -GATK PATH_TO_GATK |  | Absolute path to GATK |

