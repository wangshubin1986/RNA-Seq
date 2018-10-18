# 基于RNA-Seq的种间遗传变异挖掘

* GWAS初步定位Hg基因（麸皮绒毛）于chr1A：0-20,000,000区间。拟进一步通过济麦20和农大3432 F2群体对Hg基因进行图为克隆。  
* 本部分，通过RNA-Seq（幼穗）挖掘济麦20和农大3432种间遗传和表达变异。

----

## 文件提取

华大测序已返回序列匹配文件（.bam）， 并且已对bam文件进行了排序， 可以直接用于后续分析。 文件较大，无法在个人电脑上完成分析，所以首先将目标染色体（chr1A）相关匹配提取出来。

* 下载小麦chr1A.fsa
[https://urgi.versailles.inra.fr/download/iwgsc/IWGSC_RefSeq_Assemblies/v1.0/](https://urgi.versailles.inra.fr/download/iwgsc/IWGSC_RefSeq_Assemblies/v1.0/)

* 济麦20和农大3432RNA-Seq数据中chr1A上的匹配

```
samtools view -b JM20.AddRG.Reorder.Sort.bam 'chr1A' > jm20.1A.bam  
samtools view -b ND3432.AddRG.Reorder.Sort.bam 'chr1A' > nd.1A.bam
```

----

## 建立索引

```
samtools index jm20.1A.bam
samtools index nd.1A.bam
samtools faidx chr1A.fst
```

----

## 鉴定遗传变异

```
samtools mpileup -u --region chr1A:1-20000000 -f chr1A.fsa jm20.1A.bam nd.1A.bam > all.bcf
bcftools call -m all.bcf > all.called.bcf
```

----

## 选取可靠的种间变异

```
bcftools view --min-af 0.1 all.called.bcf | \
grep -v '\.\/\.' |less
```

----

## CAPs和dCAPs标记开发

* 举例：

```
chr1A   1159516 .       G       A       180     .       DP=144;VDB=3.51982e-13;SGB=13.0002;RPB=4.72777e-10;MQB=1;MQSB=1;BQB=0.797684;MQ0F=0;ICB=0.5;HOB=0.5;AC=2;AN=4;DP4=56,51,21,0;MQ=60      GT:PL   1/1:219,63,0    0/0:0,255,255
```

chr1A:1,159,516处有一个SNP，中国春为G，济麦20为A，农大3432为G。DP=144，表明reads覆盖度较高。MQ=60表明覆盖该SNP的reads更多的匹配到参考序列的单一位置。因此，该SNP较为可信。

* 提取该区域序列
samtools faidx chr1A.fsa chr1A:chr1A:1159496-1159536

>jm20
GCGATTGTCGGGGTGGTCCAACTGTCTAAGCAGCAGCTTCC
>nd
GCGATTGTCGGGGTGGTCCAGCTGTCTAAGCAGCAGCTTCC

* CAPs/dCAPs鉴定

## 使用已开发的CPAs或dCAPs标记










