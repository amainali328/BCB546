# UNIX Assignment

## Data Inspection

### Attributes of fang_et_al_genotypes.txt

**Here is my snippet of code used for data inspection:**

```sh
wc fang_et_al_genotypes.txt

awk -F "\t" '{print NF; exit}' fang_et_al_genotypes.txt

du -h fang_et_al_genotypes.txt
```

**By inspecting this file I learned that:**

1. The .txt file contains 2744038 words, 2783 lines, and 11051939 characters
2. Number of columns = 986 
3. File size = 6.7M

### Attributes of snp_position.txt

**Here is my snippet of code used for data inspection:**

```sh
wc snp_position.txt

awk -F "\t" '{print NF; exit}' snp_position.txt

du -h snp_position.txt
```
**By inspecting this file I learned that:**

1. The .txt file contains 13198 words, 984 lines, and 82763 characters
2. Number of columns = 15 
3. File size = 49K

# Data Processing

### Maize Data

**Here is my snippet of code used for data processing:**
```sh

(head -n 1 fang_et_al_genotypes.txt && grep 'ZMM' fang_et_al_genotypes.txt) | cut -f 4-986 > maize_phenotype.txt

awk -f transpose. awk maize_penotype.txt > transposed_maize_phenotype.txt

sort -k1,1 transposed_maize_phenotype.txt > maize_sorted.txt

cut -f1,3,4 snp_position.txt | sed '1d' | sort -k1,1 > snp_position_sorted.txt

join -t $'\t' -1 1 -2 1 snp_position_sorted.txt maize_sorted.txt > maize_snp_sorted.txt

awk -v OFS='\t' 'BEGIN {print "SNP-ID", "Chromosome", "Location", "Genotype_Data"} /unknown/ {print}' maize_snp_sorted.txt > unknown_maize_SNP.txt

awk -v OFS='\t' 'BEGIN {print "SNP-ID", "Chromosome", "Location", "Genotype_Data"} /multiple/ {print}' maize_snp_sorted.txt > multiple_maize_SNP.txt

for ((i=1; i<=10; i++)); do awk -v i="$i" '{if ($2==i) print $0}' maize_snp_sorted.txt | sort -k3,3n | awk -v OFS='\t' 'BEGIN {print "SNP_ID", "Chromosome", "Position", "Genotype_data"}{print}'> chr"$i"_maize_increasing.txt; done

for ((i=1; i<=10; i++)); do awk -v i="$i" '{if ($2==i) print $0}' maize_snp_sorted.txt | sort -k3,3nr | sed 's/?/-/g' | awk -v OFS='\t' 'BEGIN{print "SNP_ID", "Chromosome", "Position", "Genotype_data"}{print}'> chr"$i"_maize_decreasing.txt; done
```


Here is my brief description of what this code does:

SNP_ID and SNP data for maize (group = ZMMIL, ZMMLR, and ZMMMR) are extracted from fang_et_al_genotypes.txt with the header, transposed and sorted based on SNP_ID. The header and columns other than SNP_ID, Chromosome and Position are removed from snp_position.txt and sorted based on SNP_ID. The resulting two sorted files created are subsequently joined.
For all SNPs with unknown or multiple positions in the genome (third column), their data is extracted from the joined file after a header indicating SNP_ID, Chromosome, Position and Genotype Data is added.
A for loop is utilized to separate the data based on chromosome, sorted based on increasing or decreasing positions on the third column and a header is added to indicate SNP_ID, Chromosome, Position and Genotype Data within the file. For files with decreasing positions, missing data encoded by ? is replaced by - within the for loop.

### Teosinte Data

**Here is my snippet of code used for data processing:**

```sh

(head -n 1 fang_et_al_genotypes.txt && grep 'ZMP' fang_et_al_genotypes.txt) | cut -f 4-986 > teosinte_phenotype.txt

awk -f transpose.awk teosinte_penotype.txt > transposed_teosinte_phenotype.txt

sort -k1,1 transposed_teosinte_phenotype.txt > teosinte_sorted.txt

cut -f1,3,4 snp_position.txt | sed '1d;$d' | sort -k1,1 > snp_position_sorted.txt

join -t $'\t' -1 1 -2 1 snp_position_sorted.txt teosinte_sorted.txt > teosinte_snp_sorted.txt

awk -v OFS='\t' 'BEGIN {print "SNP-ID", "Chromosome", "Location", "Genotype_Data"} /unknown/ {print}' teosinte_snp_sorted.txt > unknown_teosinte_SNP.txt

awk -v OFS='\t' 'BEGIN {print "SNP-ID", "Chromosome", "Location", "Genotype_Data"} /multiple/ {print}' teosinte_snp_sorted.txt > multiple_teosinte_SNP.txt

for ((i=1; i<=10; i++)); do awk -v i="$i" '{if ($2==i) print $0}' teosinte_snp_sorted.txt | sort -k3,3n | awk -v OFS='\t' 'BEGIN {print "SNP_ID", "Chromosome", "Position", "Genotype_data"}{print}'> chr"$i"_teosinte_increasing.txt; done

for ((i=1; i<=10; i++)); do awk -v i="$i" '{if ($2==i) print $0}' teosinte_snp_sorted.txt | sort -k3,3nr | sed 's/?/-/g' | awk -v OFS='\t' 'BEGIN{print "SNP_ID", "Chromosome", "Position", "Genotype_data"}{print}'> chr"$i"_teosinte_decreasing.txt; done
```

Here is my brief description of what this code does:

SNP_ID and SNP data for teosinte (group = ZMPBA, ZMPIL, and ZMPJA) is extracted from fang_et_al_genotypes.txt with the header, transposed and sorted based on SNP_ID. This sorted file, alongside the snp_position.txt sorted file mentioned in "Maize Data", is subsequently joined.
For all SNPs with unknown or multiple positions in the genome (third column), their data is extracted from the joined file after a header indicating SNP_ID, Chromosome, Position and Genotype Data is added.
A for loop is utilized to separate the data based on the chromosome, sorted based on increasing or decreasing positions on the third column and a header is added to each column to describe the data within that column. For files with decreasing positions, missing data encoded by ? is replaced by - within the for loop.


