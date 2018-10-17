## RNA-Seq 挖掘遗传变异
-----------------------------
### RNA-Seq
* 材料：济麦20，农大3432 幼穗RNA-Seq
* 序列比对文件和基因组参考序列
-----------------------------
## 麸皮绒毛初步定位
chr1A:1-20,000,000
-----------------------------
### 提取1A染色体序列
```
samtools view -b JM20.AddRG.Reorder.Sort.bam 'chr1A' > jm20.1A.bam  
samtools view -b ND3432.AddRG.Reorder.Sort.bam 'chr1A' > nd.1A.bam
```
-----------------------------
### 建立索引文件
```
samtools index jm20.1A.bam
samtools index nd.1A.bam
samtools fdx chr1A.fst
```
-----------------------------
### SNP分析
```
samtools mpileup -u --region chr1A:1-20000000 -f chr1A.fsa jm20.1A.bam nd.1A.bam > all.bcf
bcftools call -m all.bcf > all.called.bcf

```
-----------------------------
