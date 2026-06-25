Tokenize total precess
===

Raw data filter
---
讀取dataset中的 **raw counts**，只保留 **protein-coding gene** 以及 **miRNA**

Normalization - Cell Level
---
將filter完成後的資料做normalization，**先**將raw count 除以總測序量(cell_counts)，再*10000

$$E_{g,c} = \left( \frac{C_{g,c}}{N_c} \right) \times 10000$$

* $E_{g,c}$：基因 $g$ 在細胞 $c$ 中的標準化值。
* $C_{g,c}$：基因 $g$ 在細胞 $c$ 中的原始計數（Raw count）。
* $N_c$：細胞 $c$ 的總計數（Total read counts）。

Normalization - Gene Level
---
在做完 **cell level** normalization後，接續作gemeformer特定的方式 (**rank value**)

Genecorpus-104M預訓練資料集中，先計算該基因在所有細胞中非零中位數 (Non-zero median value)

注: 非零中位數是經過上述 cell level normalization 再計算的結果

將做完 cell level 的數值 $E_{g,c}$ ，除以該基因的非零中位數 $M_{g}$

$$Norm_{g,c} = \frac{E_{g,c}}{M_g}$$

* $Norm_{g,c}$：基因 $g$ 最終的標準化表現值。
* $M_g$：基因 $g$ 在 Genecorpus-104M 的非零中位數。

Rank & Tokenization
---
將處理好的 $Norm_{g,c}$ 做排序，將相對表現最好的放在序列最前面

如果基因序列長度超過上限 (4,096)，將剩餘表現量不足的基因刪除

再頭尾加入特殊符號 cls & eos，完成最終的 **Rank value Emcoding**

Reference
---
[Geneformer](https://www.nature.com/articles/s41586-023-06139-9)

[Source code](https://huggingface.co/ctheodoris/Geneformer)

[Genecorpus-104M](https://huggingface.co/datasets/theodoris-lab/Genecorpus-104M)
