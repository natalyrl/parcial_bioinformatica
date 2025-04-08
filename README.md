# parcial_bioinformatica

# Punto 1
```
cat sequence* > secuencias.fasta
scp -i bio.pt.pem -P 53841 bio.pt@loginpub-hpc.urosario.edu.co:/home/familia/Downloads/secuencias.fasta /home/bio.pt/data/Parcial1/parcial1_NatalyRodriguez
```

Este código selecciona el encabezado hasta llegar a la parque que dice ADN, se coge el código y nombre de la especie:

```sed -E 's/(>\w+.\d \w+.\w+)/\1/' secuencias.fasta > secuenciasC.fasta```

Para correr el Blast:
```
salloc
module load blast/2.7.1
makeblastdb -in secuenciasC.fasta -dbtype nucl -parse_seqids -out misticetos -title "genomica_misticetos"
wget https://raw.githubusercontent.com/paula-torres/bioinfo_ur_2025/refs/heads/main/files/query.fasta
blastn -query query.fasta -task megablast -db misticetos -outfmt 7 -word_size 7 -out blast_misticetos -num_threads 1
```
### Resultado del Blast:
# Punto 2
```
cat secuenciasC.fasta query.fasta > alinear.fasta```
cp ejemplo.sh /home/bio.pt/data/Parcial1/parcial1_NatalyRodriguez/`
nano muscle.sh
```

```#!/bin/bash
#SBATCH -p normal #Particion (cola)
#SBATCH -N 1 # Numero de nodos
#SBATCH -n 10 # Numero de nucleos
#SBATCH -t 0-20:00 # limite de tiempo (D-HH:MM)
#SBATCH -o salida.out # Salida STDOUT
#SBATCH -e error.err # Salida STDERR
#SBATCH --mail-user=natalya.rodriguez@urosario.edu.co #Direccion e-mail a donde notificar el estado del trabajo
#SBATCH --mail-type=ALL #Especifica que eventos notificar al correo (ALL, BEGIN, END, REQUEUE, FAIL)
module load muscle/3.8.31
muscle -in alinear.fasta -out misticetos_muscle.fasta```
```
Luego:
```
chmod +x muscle.sh
sbatch muscle.sh
```
Luego, se utiliza FASconCAT:
```./FASconCAT-G_v1.05.1.pl -n -p -s```

Para realizar el **árbol**:
```
module load iqtree/1.6.12
iqtree -s  FcC_supermatrix.fas -m GTR+I+G -bb 1000 -pre misticetosFinal_muscle
```
