# DRPScore: Evaluating native-like structures of RNA-protein complexes through the deep learning method

## Attention
Please download the compressed zip files separately. The whole DRPScore zip file is too large. There may be an error when downloading the large zip file by clicking the Download ZIP link.

## Requirements
This repo was tested with Ubuntu 18.04.5 LTS, Python 3.8.8, PyTorch (1.12.1+cu113), tensorboardX, biopython and CUDA 11.4. You will need at least 8GB VRAM (e.g. GeForce RTX 3070) for running full experiments in this repo.

This package does not require any installation, can be used directly. DRPScore takes about 8 minutes to evaluate 1000 RNA-protein complex structures.

attention:The input objects of 4DCNN are amino acids and residues at the interaction interface of RNA-protein within 6A, which can be obtained through ./6A_calculation. 

## Using a decoy (PDB ID:3EGZ) as an example, we show the use of three modules.

## 1: get amino acids and residues at the interaction interface of RNA-protein within 6A.
### This step is not necessary if you can directly provide amino acids and residues at the protein-RNA interaction interface within 6A

cd ./6A_calculation   
python rpo.py in out

The commands above will calculate the amino acids and residues at the protein-RNA interaction interface within 6A provided in ./6A_calculation/in and generate the results in ./6A_calculation/out.

### attention: For structures containing multiple chains, you need to manually delete protein-protein, RNA-RNA self-interactions from the output file.

cat complex1.pdb_*.txt > complex1.pdb_6A.txt  
grep -wf complex1.pdb_6A.txt complex1.pdb > Acomplex1.pdb

The commands above will generate a pdb file containing only amino acids and residues at the interaction interface within 6A.
## 2: Generate the scoring preparation file '*.npy'.

cd ./data/predict/  
python Main.py -pl pdblist   (to assess a list of RNA-protein complexes)  
python Main.py -pn pdbname   (to assess an RNA-protein complex)

The commands above will generate a preparation file '*.npy' of RNA-protein complexes in pdblist for DRPScore.

## 3: Predicted by 4DCNN.  

python pred.py

The above commands will predict all *.npy files provided in /data/predict/, and then scoring results are output in the scoring.txt.
The first column is the name of the RNA-protein complex evaluated, and the third column is the probability of a native structure.

### For already generated the pdb file containing only amino acids and residues at the interaction interface within 6A, we provide a simple script (DRPScore.sh) to score.

## Contacts
For any questions, please contact authors.
