## RNA-Seq 挖掘遗传变异
-----------------------------
### RNA-Seq
* 济麦20，农大3432
* 幼穗RNA-Seq
-----------------------------
### 提取1A染色体序列
  samtools view -b JM20.AddRG.Reorder.Sort.bam 'chr1A' > jm20.1A.bam  
samtools view -b ND3432.AddRG.Reorder.Sort.bam 'chr1A' > nd.1A.bam
