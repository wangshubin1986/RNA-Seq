# RNA-Seq挖掘遗传变异
济麦20和农大3432幼穗RNA-Seq（华大），鉴定1A染色体SNP/InDel，用于Hg基因（麸皮绒毛，chr1A:0-20,000,000）图位克隆。

----

## 获得chr1A参考基因组和匹配
* 下载参考基因组序列chr1A.fsa  
[https://urgi.versailles.inra.fr/download/iwgsc/IWGSC_RefSeq_Assemblies/v1.0/](https://urgi.versailles.inra.fr/download/iwgsc/IWGSC_RefSeq_Assemblies/v1.0/)

* 提取bam文件chr1A匹配
```
samtools view -b JM20.AddRG.Reorder.Sort.bam 'chr1A' > jm20.1A.bam  
samtools view -b ND3432.AddRG.Reorder.Sort.bam 'chr1A' > nd.1A.bam
```
公司返回的bam文件已排序和去未匹配的reads，直接用于分析。

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

## 选取可靠种间变异
```
bcftools view --min-af 0.1 all.called.bcf | \
grep -v '\.\/\.' |less
```
----

## 酶切标记开发

* 举例：

```
chr1A   1159516 .       G       A       180     .       DP=144;VDB=3.51982e-13;SGB=13.0002;RPB=4.72777e-10;MQB=1;MQSB=1;BQB=0.797684;MQ0F=0;ICB=0.5;HOB=0.5;AC=2;AN=4;DP4=56,51,21,0;MQ=60      GT:PL   1/1:219,63,0    0/0:0,255,255
```

chr1A:1,159,516处有一个SNP，中国春为G，济麦20为A，农大3432为G。  
DP=144，表明reads覆盖度较高。MQ=60表明覆盖该SNP的reads更多的匹配到参考序列的单一位置。  
该SNP比较可信，并且易得到特异性扩增。

* 提取该区域序列（SNP左右各20bp）
samtools faidx chr1A.fsa chr1A:chr1A:1159496-1159536

>jm20
GCGATTGTCGGGGTGGTCCAACTGTCTAAGCAGCAGCTTCC
>nd
GCGATTGTCGGGGTGGTCCAGCTGTCTAAGCAGCAGCTTCC

* CAPs/dCAPs鉴定
[http://helix.wustl.edu/dcaps/](http://helix.wustl.edu/dcaps/)  
[http://223.65.208.206:8018/](http://223.65.208.206:8018/)  
网站2直接给出引物，不建议直接使用，该引物未考虑扩增的特异性。

* 标记开发
** 有CAPS先用CAPS
**






## 使用已开发的CPAs或dCAPs标记














