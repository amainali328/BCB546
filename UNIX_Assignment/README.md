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

awk -f transpose. awk maize_phenotype.txt | sort -k1,1 transposed_maize_phenotype.txt > maize_sorted.txt

cut -f1,3,4 snp_position.txt | sed '1d' | sort -k1,1 > snp_position_sorted.txt

join -t $'\t' -1 1 -2 1 snp_position_sorted.txt maize_sorted.txt > maize_snp_sorted.txt

grep "unknown" maize_snp_sorted.txt | awk -v OFS='\t' 'BEGIN {print "SNP-ID", "Chromosome", "Location", "Genotype_Data"} {print}' > unknown_maize_SNP.txt

grep "multiple" maize_snp_sorted.txt | awk -v OFS='\t' 'BEGIN {print "SNP-ID", "Chromosome", "Location", "Genotype_Data"} {print}' > multiple_maize_SNP.txt

for ((i=1; i<=10; i++)); do awk -v i="$i" '{if ($2==i) print $0}' maize_snp_sorted.txt |awk -v OFS='\t' 'BEGIN {print "SNP_ID", "Chromosome", "Position", "Genotype_data"}{print}'| sort -k3,3n |> chr"$i"_maize_increasing.txt; done

for ((i=1; i<=10; i++)); do awk -v i="$i" '{if ($2==i) print $0}' maize_snp_sorted.txt  awk -v OFS='\t' 'BEGIN{print "SNP_ID", "Chromosome", "Position", "Genotype_data"}{print}' | sort -k3,3nr | sed 's/?/-/g' > chr"$i"_maize_decreasing.txt; done
```


**Here is my brief description of what this code does:**
At first columns 4 to 986  containing SNP IDs and SNP data are extracted for teosinte the groups (ZMMIL, ZMMLR, and ZMMMR). Since the SNP IDs are presented in the first row, the phenotype file is subsequently transposed to put the SNP in the first column followed by sorting of the files based on the first column to get the maize_sorted.txt file, which is now ready for joining. Similarly, the first, third and fourth columns with SNP IDs, chromosome number and position respectively are extracted from the SNP_position.txt file, removing the header and sorted based on the first column.
The sorted files, i.e. maize_Sorted.txt and snp_position.txt are joined based on the first common column.
SNPs characterized by unknown or multiple positions in the genome, identified by their presence in the third column, have their data extracted from the joined file. Preceding this extraction, a header detailing SNP_ID, Chromosome, Position, and Genotype Data is appended. 
A for loop is employed to separate the data based on the chromosomes (second column), subsequently sorting it based on ascending or descending positions in the third column. Additionally, a header is introduced to each column to elucidate the content within. In cases where the positions are arranged in descending order, any missing data indicated by "?" is substituted with "-". 
### Teosinte Data

**Here is my snippet of code used for data processing:**

```sh

(head -n 1 fang_et_al_genotypes.txt && grep 'ZMP' fang_et_al_genotypes.txt) | cut -f 4-986 > teosinte_phenotype.txt

awk -f transpose.awk teosinte_phenotype.txt | sort -k1,1 | teosinte_sorted.txt

cut -f1,3,4 snp_position.txt | sed '1d' | sort -k1,1 > snp_position_sorted.txt

join -t $'\t' -1 1 -2 1 snp_position_sorted.txt teosinte_sorted.txt > teosinte_snp_sorted.txt

grep "unknown" teosinte_snp_sorted.txt | awk -v OFS='\t' 'BEGIN {print "SNP-ID", "Chromosome", "Location", "Genotype_Data"} {print}' > unknown_teosinte_SNP.txt

grep "multiple" teosinte_snp_sorted.txt | awk -v OFS='\t' 'BEGIN {print "SNP-ID", "Chromosome", "Location", "Genotype_Data"} {print}' > multiple_teosinte_SNP.txt

for ((i=1; i<=10; i++)); do awk -v i="$i" '{if ($2==i) print $0}' teosinte_snp_sorted.txt | awk -v OFS='\t' 'BEGIN {print "SNP_ID", "Chromosome", "Position", "Genotype_data"}{print}' | sort -k3,3n> chr"$i"_teosinte_increasing.txt; done

for ((i=1; i<=10; i++)); do awk -v i="$i" '{if ($2==i) print $0}' teosinte_snp_sorted.txt | awk -v OFS='\t' 'BEGIN{print "SNP_ID", "Chromosome", "Position", "Genotype_data"}{print}' | sort -k3,3nr | sed 's/?/-/g'> chr"$i"_teosinte_decreasing.txt; done
```

**Here is my brief description of what this code does:**
At first columns 4 to 986 containing SNP IDs and SNP data are extracted for teosinte the groups (ZMPBA, ZMPIL, and ZMPJA). Since the SNP IDs are presented in the first row, the phenotype file is subsequently transposed to put the SNP in the first column followed by sorting of the files based on the first column to get the teosinte_sorted.txt file, which is now ready for joining. Similarly, the first, third and fourth columns with SNP IDs, chromosome number and position respectively are extracted from the SNP_position.txt file, removing the header and sorted based on the first column.
The sorted files, i.e. teosinte_Sorted.txt and snp_position.txt are joined based on the first common column.
SNPs characterized by unknown or multiple positions in the genome, identified by their presence in the third column, have their data extracted from the joined file. Preceding this extraction, a header detailing SNP_ID, Chromosome, Position, and Genotype Data is appended. 
A for loop is employed to separate the data based on the chromosomes (second column), subsequently sorting it based on ascending or descending positions in the third column. Additionally, a header is introduced to each column to elucidate the content within. In cases where the positions are arranged in descending order, any missing data indicated by "?" is substituted with "-".

