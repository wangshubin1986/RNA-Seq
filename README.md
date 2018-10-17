# 基于RNA-Seq的种间遗传变异挖掘
* GWAS初步定位Hg基因（麸皮绒毛）于chr1A：0-20,000,000区间。拟进一步通过济麦20和农大3432 F2群体对Hg基因进行图为克隆。  
* 本部分，通过RNA-Seq（幼穗）挖掘济麦20和农大3432种间遗传和表达变异。

----

## 内容
*""






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
[sam文件格式]()

-----------------------------
### SNP分析
```
samtools mpileup -u --region chr1A:1-20000000 -f chr1A.fsa jm20.1A.bam nd.1A.bam > all.bcf
bcftools call -m all.bcf > all.called.bcf

```
[samtools参数](http://www.htslib.org/doc/samtools-1.2.html)

-----------------------------

