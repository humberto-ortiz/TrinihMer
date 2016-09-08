#!/bin/bash

clear
# Run the program in Mutual-program directory
# If you do the make for each of the software before this you could eliminate the make lines.
# Remember to change the paths, and also the output directory.
# Strongly Recomend if you are going to use the same output directory, delete the previous data, in case it's not rewritten


echo "Start" >> time.txt
date >> time.txt
#change to oases to create graph
cd oases/velvet


# VELVET
#create the running scripts
make clean
make

#First ./velveth [output directory] [size of kmer] [format of file] [Read-type] [file input]
./velveth /home/kevin/data/OrganismA/ 25 -fastq -short /home/kevin/data/0Hour_ATCACG_L002_R1_001.fastq 
./velveth /home/kevin/data/OrganismB/ 25 -fastq -short /home/kevin/data/0Hour_ATCACG_L002_R1_001.fastq  

#Then ./velvetg [path of velveth output] -read_trkg yes
./velvetg /home/kevin/data/OrganismA -read_trkg yes
./velvetg /home/kevin/data/OrganismB -read_trkg yes

#OASES
cd ..
make clean 
make

#Run oases for both organisms 
#./oases [path to output of velvet]
./oases /home/kevin/data/OrganismA
./oases /home/kevin/data/OrganismB

date >> time.txt

#MUTUAL
cd .. 
cd mutual
make clean
make

#Now we compare both outputs:

./mutual prefixa=/home/kevin/data/OrganismA/ prefixb=/home/kevin/data/OrganismB kmera=25 kmerb=25 blast_path=/home/kevin/Mutual-program/blast

date >> time.txt

