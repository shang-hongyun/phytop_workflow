<<<<<<< HEAD
# phytop_workflow
=======
### Introduction ###
This is a complete workflow of phytop (https://github.com/zhangrengang/phytop), including the extraction of orthologous sequences, building single-copy gene trees, infer coalescent‐based species tree for phytop. This example includes several species to build a phylogenetic tree. The gff, cds and protein sequences of all species are in this folder (cds.fa, pep.faa, all_species_gene.gff). The protein sequences of each species were in OrthoFinder/. Users need to install MCScanX_h (https://github.com/wyp1125/MCScanX) and SOI (https://github.com/zhangrengang/SOI).

### Workflow ###
```
# pre-process the example data:
cat ./cds/Carya_illinoinensis.cds.fasta ./OrthoFinder/Juglans_regia.cds.fasta ./OrthoFinder/Juglans_sigillata.cds.fasta ./OrthoFinder/Juglans_mandshurica.cds.fasta ./OrthoFinder/Juglans_nigra.cds.fasta > cds.fa
cat ./OrthoFinder/Carya_illinoinensis.fasta ./OrthoFinder/Juglans_regia.fasta ./OrthoFinder/Juglans_sigillata.fasta ./OrthoFinder/Juglans_mandshurica.fasta ./OrthoFinder/Juglans_nigra.fasta > pep.faa

# Using orthofinder to identify orthologs gene pairs between species
orthofinder -f OrthoFinder/ -M msa -t 60

OFresult=OrthoFinder/OrthoFinder/*/
soi-orth uniqlogs $OFresult
cat orthologs.txt inparalogs2.txt > pairs.homology

# Confirmation of orthologs gene pairs between species identified by orthofinder using MCScanX_h, screening for reliability results in orthologs
ln ../all_species_gene.gff pairs.gff -s
MCScanX_h pairs -a -b 0 -c 0 > MCScanX_h.out 2>&1

# to build single-copy gene trees
soi phylo -og pairs.collinearity -iqtree_opts "-B 1000" -pep pep.faa -cds cds.fa -both -root Carya_illinoinensis -pre sog -mm 0.5 -p 80 -tmp tmp.sc.0.5 -sc -concat -fmt mcscanx

# to infer coalescent‐based species tree
astral-hybrid -u 2 --root Carya_illinoinensis sog.sc.cds.mm0.5.genetrees > sog.sc.cds.mm0.5.genetrees.astral

# run phytop
phytop sog.sc.cds.mm0.5.genetrees.astral
```

### Citation ###
if you use the MCScanX, please cite:
```
Wang Y, Tang H, Debarry JD, et al. MCScanX: a toolkit for detection and evolutionary analysis of gene synteny and collinearity. Nucleic Acids Res. 2012;40(7):e49. https://doi.org/10.1093/nar/gkr1293
```
if you use the SOI, please cite:
```
Zhang RG, Shang HY, Zhou MJ et. al. Robust identification of orthologous synteny with the Orthology Index and its applications in reconstructing the evolutionary history of plant genomes. bioRxiv, 2024 [http://doi.org/10.1101/2024.08.22.609065]
```
>>>>>>> 455281e (first commit)
