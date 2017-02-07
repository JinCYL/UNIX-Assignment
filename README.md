# UNIX-Assignment
UNIX-Assignment Workflow
--------------------------------------


Student name: Ching-Yi Liao

Student number: 152455327


# I.	Git repository

## 1.	Create a new repository (UNIX-Assignment) on my GitHub with a README.md file.

## 2.	Clone this repository to my computer.

        $ git clone https://github.com/jincyliao/UNIX-Assignment

## 3.	Copy the three files to this new repository

        $ cd BCB546X-Spring2017/UNIX_Assignment/

        $ cp fang_et_al_genotypes.txt ~/UNIX-Assignment/

        $ cp snp_position.txt ~/UNIX-Assignment/

        $ cp transpose.awk ~/UNIX-Assignment/


# II.	Data inspection

        $ cd /c/Users/user/UNIX-Assignment


## File size

        $ du -h fang_et_al_genotypes.txt snp_position.txt
        11M     fang_et_al_genotypes.txt
        84K     snp_position.txt


## Number of lines, words, and bytes, etc.

        $ wc fang_et_al_genotypes.txt snp_position.txt
              2783  2744038 11054722 fang_et_al_genotypes.txt
               984    13198    83747 snp_position.txt
              3767  2757236 11138469 total

(print *line, word, and byte counts*) or

**wc –c** (print the *byte counts*), **wc –m** (print *character counts*), **wc –l** (print *newline counts*), **wc –w** (print *words counts*),


## Numbers of columns

        $ awk -F "\t" '{print NF; exit}' fang_et_al_genotypes.txt
        986

        $ awk -F "\t" '{print NF; exit}' snp_position.txt
        15

# III.	Data processing

## 1.	Data extraction and cut unnecessary columns

### Genotype data

#### For maize (Group = ZMMIL, ZMMLR, and ZMMMR)

        $ grep -E "(Group|ZMMIL|ZMMLR|ZMMMR)" fang_et_al_genotypes.txt | cut -f 4-986 > maize_genotypes.txt


#### For teosinte (Group = ZMPBA, ZMPIL, and ZMPJA)

        $ grep -E "(Group|ZMPBA|ZMPIL|ZMPJA)" fang_et_al_genotypes.txt | cut -f 4-986 > teosinte_genotypes.txt


### SNP description data

        $ grep -v "^#" snp_position.txt | cut -f 1,3,4 > cut_snp_position.txt

## 2.	Transpose the data

        $ awk -f transpose.awk maize_genotypes.txt > transposed_maize_genotypes.txt

        $ awk -f transpose.awk teosinte_genotypes.txt > transposed_teosinte_genotypes.txt

## 3.	Sort files


        $ sort -k1,1 cut_snp_position.txt > sorted_cut_snp_position.txt

        $ sort -k1,1 transposed_maize_genotypes.txt > sorted_transposed_maize_genotypes.txt

        $ sort -k1,1 transposed_teosinte_genotypes.txt > sorted_transposed_teosinte_genotypes.txt


        $ sort -k1,1 -c sorted_cut_snp_position.txt | echo $?
        0

        $ sort -k1,1 -c sorted_transposed_maize_genotypes.txt | echo $?
        0
        
        $ sort -k1,1 -c sorted_transposed_teosinte_genotypes.txt | echo $?
        0
#### (data are sorted)

## 4.	Join files

        $ join -t $'\t' -1 1 -2 1 sorted_cut_snp_position.txt sorted_transposed_maize_genotypes.txt > joined_maize.txt    

        $ join -t $'\t' -1 1 -2 1 sorted_cut_snp_position.txt sorted_transposed_teosinte_genotypes.txt > joined_teosinte.txt

## 5.	Isolate each chromosome 

        $ awk '$2==1' joined_maize.txt | sort -k3,3g > maize_chr1.txt

(Repeat to get other chromosomes and teosinte data)

        $ awk '$2==1' joined_maize.txt | sort -k3,3gr |sed 's/?/-/g' > maize_chr_1.txt

(Repeat to get other chromosomes and teosinte data)


        $ git commit -m "results"

        $ git push origin master

