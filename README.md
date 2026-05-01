# 🧬 NGS Variant Calling & Annotation Pipeline (E. coli K-12)

## 📌 Project Overview

This project demonstrates a complete **NGS data analysis workflow** starting from raw sequencing data to **biological interpretation of a variant**.

The analysis was performed on *Escherichia coli K-12 MG1655* dataset obtained from **NCBI SRA**, and includes all major steps used in real bioinformatics pipelines.

---

## 🔬 Dataset Details

* **Database:** NCBI SRA
* **Accession:** SRR37624945
* **Organism:** *E. coli* K-12 MG1655
* **Sequencing:** Whole Genome Sequencing (WGS)
* **Platform:** Illumina
* **Reads:** ~750,000
* **File:** FASTQ

---

## 🧪 Complete Workflow

```
FASTQ → FastQC → Trimming → Alignment → BAM Processing → Variant Calling → Filtering → Annotation
```

---

## ⚙️ Tools Used

* FastQC
* Trimmomatic
* BWA
* Samtools
* Bcftools
* IGV
* SnpEff

---

## 🚀 Step-by-Step Analysis

### 1. Quality Check (FastQC)

```bash
fastqc SRR37624945.fastq
```

✔ Data quality was generally good
✔ No major poor-quality reads detected (0 flagged reads)

<img width="872" height="850" alt="image" src="https://github.com/user-attachments/assets/95948b33-479e-4de0-a6b5-79718f00d696" />

---

### 2. Trimming (Trimmomatic)

```bash
java -jar /usr/share/java/trimmomatic.jar SE -phred33 \
SRR37624945.fastq \
SRR37624945_trimmed.fastq \
LEADING:20 SLIDINGWINDOW:4:20 MINLEN:36
```

✔ Removed low-quality bases from beginning and across reads
✔ Cleaned dataset for reliable downstream analysis

<img width="1083" height="847" alt="image" src="https://github.com/user-attachments/assets/15d75a57-e7b7-4f84-b89a-fb6762735bed" />

---

### 3. Alignment (BWA)

```bash
bwa index GCF_000005845.2_ASM584v2_genomic.fna

bwa mem GCF_000005845.2_ASM584v2_genomic.fna \
SRR37624945_trimmed.fastq > aligned.sam
```

✔ Reads mapped to reference genome (*E. coli K-12 MG1655*)

---

### 4. Convert & Process Alignment

```bash
samtools view -S -b aligned.sam > aligned.bam

samtools sort aligned.bam -o aligned_sorted.bam

samtools index aligned_sorted.bam
```

✔ Generated sorted and indexed BAM file for variant analysis

---

### 5. Variant Calling

```bash
bcftools mpileup -f GCF_000005845.2_ASM584v2_genomic.fna aligned_sorted.bam | \
bcftools call -mv -Ov > variants.vcf
```

✔ Identified SNPs and INDELs

---

### 6. Variant Filtering

```bash
bcftools filter -i 'DP>=5 && QUAL>=20' variants.vcf > filtered_variants.vcf
```

✔ Removed low-confidence variants
✔ Retained biologically relevant variants

---

## 🧬 Key Variant Identified

* **Position:** 4,616,669
* **Reference Base:** G
* **Alternate Base:** T
* **Type:** SNP
* **Quality (QUAL):** 153
* **Depth (DP):** 5

---

## 🔍 IGV Validation

* All aligned reads show **T (ALT allele)**
* No mixed signal observed
* Strong visual confirmation of variant

<img width="1911" height="1130" alt="image" src="https://github.com/user-attachments/assets/fa4841d2-ad40-4bd8-b195-7c1cf857a33d" />

---

## 🧬 Variant Annotation (SnpEff)

```bash
snpEff ecoli_k12 filtered_variants.vcf > annotated_variants.vcf
```

### ANN Field:

```
ANN=T|missense_variant|MODERATE|yjjI|...
```

---

## 🧠 Biological Interpretation

| Feature        | Value       |
| -------------- | ----------- |
| Gene           | yjjI        |
| Variant Type   | Missense    |
| Coding Change  | c.397C>A    |
| Protein Change | p.Leu133Ile |
| Impact         | MODERATE    |

👉 **Final Meaning:**

This SNP causes an amino acid change:

**Leucine → Isoleucine at position 133**

---

## 🔬 Biological Insight

* Variant is located inside a coding gene (**yjjI**)
* Causes a **non-synonymous mutation**
* May alter protein structure or function

---

## ⚠️ Important Limitation

* Read depth (DP = 5) is low
* Variant signal is strong but requires higher coverage for confirmation

---

## 🧾 Key Learning

* Variant calling identifies differences from reference
* Annotation adds biological meaning
* IGV provides visual validation
* **Variant ≠ Mutation (needs validation)**

---

## 🎯 Conclusion

This project successfully demonstrates a **real-world bioinformatics pipeline**, transforming raw sequencing data into biologically meaningful insights.

---

## 👩‍🔬 Author

**Muskan Kashyap**
MSC Biotechnology

---

## 💼 Interview Explanation

> “In this project, I performed end-to-end NGS data analysis including quality control, trimming, alignment, variant calling, and annotation. I identified a SNP (G→T) in the yjjI gene, which resulted in a missense mutation (Leu133Ile), potentially affecting protein function.”
