# BCB546-assignment1
repository containing code and solutions for unix bcb546 course assignment

Initial Repository set up steps:
1. Created a new repository for Assignment on Github and cloned into HPC cluster
2. Copied required file for analysis from the Assignment folder, created new READ.ME and pushshed files to GitHub via add, commit and push commands

## Data Inspection Steps (snippets of the code):
* Fang_et_al_genotype file
1. Full Size: $ **ls -lh fang_et_al_genotypes.txt**: 11 MB
2. Line count: $ **wc fang_et_al_genotypes.txt** :2783  2744038 1105193
3. File content: $ **cat fang_et_al_genotypes.txt**
4. Information: $ **head fang_et_al_genotypes.txt** : get information - SAMPLE ID, Group and glimpse of information in the file
5. Information: $ **tail fang_et_al_genotypes.txt**:
6. Brief Overview: $ **less fang_et_al_genotypes.txt**: 
7. Total Column count: $ **awk -F "\t" '{print NF; exit}' fang_et_al_genotypes.txt** : 986

* snp_position.txt file
1. Full size: $ **ls -lh snp_position.txt** : 81 K
2. Line count: $ **wc snp_position.txt** : 984 13198 82763 snp_position.txt
3. File contents: $ **cat snp_position.txt** : returns contents of file- columns-SNP_ID, cdv_marker_id, Chromosome, Position, gene etc
4. File information: $ **head snp_position.txt** :
5. File information: $ **tail snp_position.txt**: 
6. Brief overview: $ **less snp_position.txt**
7. Total Column count: $ **awk -F "2t" '{print NF; exit}'snp_position.txt**:215 columns
8. 

## Information obtained using data inspection tools- Fang et al.txt:
This is huge file of approximately 11MB and 2783 lines of information. It is clearly oberved when you run $ cat, $ head and $ less command. Upon running these commands we get the information - SAMPLE ID, Group 

## Information obtained using data inspection tools- snp_position.txt:
THe file contains information about SNP ID, marker id, chromosome number, gene etc.It is quite small file compared to fang_et_al file. 

# Data processing

1. cut -f3 fang_et_al_genotypes.txt |sort|uniq -c
2. grep 'ZMM' fang_et_al_genotypes.txt > maize_genotype
3. grep 'ZMP' fang_et_al_genotypes.txt > teosinate_genotype
4. cat header_file.txt maize_geotype.txt > header_miaze
5. cat header_file.txt teosinate_geotype.txt > header_teosinate
6. awk -f transpose.awk header_maize > transposed_maize
7. awk -f transpose.awk header_teosinate > transposed_teosinate
8. tail -n +2 snp_position.txt > sorted_snp_position (could have used sort -k1,1)
9. head -n 1 snp_position.text > snp_header (probably not needed)
10. tail -n +2 transposed_maize > sorted_transposed_maize
11. tail -n +2 transposed_teosinate > sorted_transposed_teosinate
12. join -1 1 2 1 -t $'\t' sorted snp_position sorted_transposed_maize > joined_maized_file
13. join -1 1 2 1 -t $'\t' sorted snp_position sorted_transoised_teosinate > joined_teosinate_file
14. head -n 1 joined_maized_file| wc
15. head -n 1 joined_teosinate_file | wc
16. cut -f1,3,4,16-1586 joined_maize_file > genotype_maize_file
17. cut -f1,3,4,16-988 joined_ teosinate_file > genotype_teosinate_file

## To generate individual file for chromosome 1-10 in ascending order with ? for missing values
  (need to move or copy the transpose.awk in the local repository to run this command)
  
  1. awk '$2 == 1' genotype_maize_file | sort -k3,3n > ch1_SNPA_maize
  2. awk '$2 == 2' genotype_maize_file | sort -k3,3n > ch1_SNPA_teosinate 

## To generate individual file for chromosome 1-10 in descending order

  1. awk '$2 == 1' genotype_maize_file | sort -k3,3nr > ch1_SNPA_maize
  2. awk '$2 == 2' genotype_maize_file | sort -k3,3nr > ch1_SNPA_teosinate
 
 Similarly, run awk command for remaining chromosome number 2-10 for maize and teosinate. Also ran $ cat to check the contents of the file and make sure they were not empty
 
 ## NOTE: should have run the sed command before generating individual chromosome files in descensing order
 
 1. sed -e 's/?/-/g' ch1_SNPD_maize > ch1_SNPD_maize.txt
 2. sed -e 's/?/-/g' ch1_SNPD_teosinate > ch1_SNPD_teosinate.txt
 
 ### Run the same command for different chromosome 2-10 for maize and teosinate. So the files ch1_SNPD_maize.txt has SNP values based on decending order of positions for chromosome 1 with missing values replaced as '-' and ch1_SNPD_maize has SNP values in decreasing order only
 
 ## To generate file with unknown value
 1. awk '$2 == "unknown"' genotype_maize_file > unknown_maize_file
 2. awk '$2 == "unknown"' genotype_teosinate_file > unknown_teosinate_file
 
 ## To generate file with multiple value
 1. awk '$2 == "multiple"' genotype_maize_file > multiple_maize_file
 2. awk '$2 == "multple"' genotype_teosinate_file > multiple_teosinate_file
 
