
# 🧬 BlobTools2 Visualization Workflow for Metagenomic Binning

This guide documents how to generate taxonomic and coverage-based visualizations from binned metagenomic data using **BlobTools2**, and transfer the results to a local machine for inspection.

---

## 📁 Project Structure

The following shows your working directories on the GACRC cluster are structured like so:

```
/scratch/bas42855/blobtools_output/
/scratch/bas42855/nematode-microbiome/results/10-read-map-short-reads/epacanthion.1/
```

---

## 🧾 Required Input Files

BlobTools2 needs the following inputs:

| Input Type              | Description                                    | Example Path |
|-------------------------|------------------------------------------------|--------------|
| **FASTA file**          | Assembled contigs or bins                      | `/scratch/bas42855/nematode-microbiome/results/10-read-map-short-reads/epacanthion.1-bin.1.fa` |
| **BAM file**            | Mapped reads (short reads mapped to assembly) | `/scratch/bas42855/nematode-microbiome/results/10-read-map-short-reads/epacanthion.1-bin.1.sorted.bam` |
| **Taxonomy file**       | Output of `blast` or `diamond` + MEGAN format | `epacanthion.1-bin.1.blast.out` (converted with MEGAN or compatible format) |
| **FAI index (optional)**| FASTA index file (e.g., created via `samtools faidx`) | `epacanthion.1-bin.1.fa.fai` |

---

## 🛠️ Step 1: Generate BlobDB

```bash
blobtools add \
  --fasta /scratch/bas42855/nematode-microbiome/results/10-read-map-short-reads/epacanthion.1-bin.1.fa \
  --bam /scratch/bas42855/nematode-microbiome/results/10-read-map-short-reads/epacanthion.1-bin.1.sorted.bam \
  --taxrule bestsum \
  --taxfiles /scratch/bas42855/nematode-microbiome/results/10-read-map-short-reads/epacanthion.1-bin.1.blast.out \
  -o /scratch/bas42855/blobtools_output/epacanthion.1-bin.1
```

This generates a `.blobDB.json` file.

---

## 📊 Step 2: Generate Plots

```bash
blobtools plot \
  -i /scratch/bas42855/blobtools_output/epacanthion.1-bin.1.blobDB.json \
  -o /scratch/bas42855/blobtools_output/epacanthion.1-bin.1-plots
```

This command produces several `.png` files (e.g., taxonomic blobplots, coverage plots) and a summary `.stats.txt` file.

---

## 💻 Step 3: Transfer Results to Local Machine

Use `scp` from your **local computer terminal** (make sure you're not on the cluster):

```bash
scp bas42855@txfer.gacrc.uga.edu:/scratch/bas42855/blobtools_output/epacanthion.1-bin.1-plots* \
/c/Users/UNKNOWN/Desktop/ddt-project/ddt-project/results/
```

To copy the summary stats file as well:

```bash
scp bas42855@txfer.gacrc.uga.edu:/scratch/bas42855/blobtools_output/epacanthion.1-bin.1-plots*.stats.txt \
/c/Users/UNKNOWN/Desktop/ddt-project/ddt-project/results/
```

---

## 👀 Step 4: View Visualizations

Simply open the transferred `.png` files from:

```
C:/Users/UNKNOWN/Desktop/ddt-project/ddt-project/results/
```
![Image](https://github.com/user-attachments/assets/4caac32f-d431-4309-8f89-1bcf60dac686)

![Image](https://github.com/user-attachments/assets/ba6155f3-feda-42f9-8423-049acb62f40d)

![Image](https://github.com/user-attachments/assets/678d9c5d-4357-4d46-96dc-4b9f14d568a1)

![Image](https://github.com/user-attachments/assets/09c06742-072f-4a79-9f47-f0c784676309)

![Image](https://github.com/user-attachments/assets/6283b173-1085-4fe9-a9ea-10266e336fd6)

using any image viewer or browser.

---

## ✅ Optional: Clean-Up (on cluster)

```bash
rm /scratch/bas42855/blobtools_output/*.png
rm /scratch/bas42855/blobtools_output/*.json
```




