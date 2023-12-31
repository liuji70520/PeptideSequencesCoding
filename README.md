# 以下为主要步骤代码解析,知识点请移步[here](https://github.com/ZhijunBioinf/Pattern-Recognition-and-Prediction/blob/master/Lab5_PeptideSequencesCoding/sequence_coding2.md),具体源码与jupyter notebook实现请查看[AA531coding.py](https://github.com/liuji70520/PeptideSequencesCoding/blob/main/AA531coding.py)和[AA531coding.ipynb](https://github.com/liuji70520/PeptideSequencesCoding/blob/main/AA531coding.ipynb)

# 编程实现肽序列的AA531(531 properties of Amino Acids)特征表征/数值化
最终目的是将每一个三肽相关的特征转换为数值矩阵，所以得先将每一个氨基酸对应的生理生化指标提取出来。其中用数字标签来表示对应的多肽
## 一、将AA531properties.txt做成字典  
这里每一个氨基酸有着531生理生化指标，即AA531properties.txt第一行
```python3
df = pd.read_csv("../data/AA531properties.txt", delimiter='\t', header=None, skiprows=1)
AA531Dict = df.set_index(0).T.to_dict('list')
```
## 二、肽序列表征           
### 1、得到行数即三肽数目150行，一个三肽有着三个氨基酸，一个氨基酸有着531个生理生化，故而一个三肽有着3*531个特征数值 
```python3
df = pd.read_csv(AASeqFileName,sep="\t",header=None)
numberOFlines = len(df)
seqlength = seqlength
returnMat = np.zeros((numberOFlines,seqlength*531))
Y = np.zeros((numberOFlines,1))
```
### 遍历每个三肽，与字典比对，添加到一个新的列表中
```python3
for lineNum in range(numberOFlines):
    Y[lineNum] = float(df.iloc[lineNum,1])
    feaVec = []
    for AA in df.iloc[lineNum,0]:
        if AA in AA531Dict.keys():
            feaVec.extend(AA531Dict[AA])
        else:
            print('Warning: nonregular amino acid found! Coding "%s" in "%s" (seqId: %d) with 531 zeros.' % (AA, AAseq, lineNum))
            feaVec.extend([0.0]*531)
            Y[lineNum] = -1
    returnMat[lineNum,:] = np.array(feaVec)
    lineNum += 1
```
# 运行程序AA531Coding.py实现以AA531表征序列。在命令行指定ACE抑制剂三肽序列文件名、AA531属性文件名、序列长度、输出文件名。
```
python3 AA531Coding.py ACEtriPeptidesSequencesActivities.txt AA531properties.txt 3 result.txt
```
