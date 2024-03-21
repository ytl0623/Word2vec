# Word2Vec
Word2vec 是基於非監督式學習，訓練集建議越大越好，語料涵蓋的越全面，訓練出來的結果相對比較好，當然也有可能 garbage input 進而得到 garbage output ，由於檔案所使用的資料集較大，所以每個過程中都請耐心等候。(ps: word2vec 如果把每種字當成一個維度，假設總共有 4000 個總字，那麼向量就會有 4000 維度。故可透過它來降低維度)</br></br>

![demo](https://github.com/Alex-CHUN-YU/Word2vec/blob/master/image/demo.png)</br></br>
WiKi簡介:</br>
Word2vec is a group of related models that are used to produce word embeddings. These models are shallow, two-layer neural networks that are trained to reconstruct linguistic contexts of words. Word2vec takes as its input a large corpus of text and produces a vector space, typically of several hundred dimensions, with each unique word in the corpus being assigned a corresponding vector in the space. Word vectors are positioned in the vector space such that words that share common contexts in the corpus are located in close proximity to one another in the space.</br></br>

By the way :</br>
Skip-gram: works well with small amount of the training data, represents well even rare words or phrases.</br>
CBOW: several times faster to train than the skip-gram, slightly better accuracy for the frequent words.

## 使用方式
Input:</br>
```
1.download wiki data(請參考資料集)
2.進入 Word2Vec 資料夾
3.執行 python wiki_to_txt.py zhwiki-latest-pages-articles.xml.bz2(wiki xml 轉換成 wiki text)
4.執行 python segmentation.py(簡體轉繁體，在進行斷詞並同步過濾停用詞，由於檔案較大故斷詞較久)
5.執行 python train.py(訓練並產生 model ，時間上也會比較久)
5.執行 python main.py(使用 Model，輸入詞彙)
註:如果在 Windows cmd 下執行 python 時有編碼問題請下以下指令:chcp 65001(使用utf-8)
```
Output:</br>
```
1.輸入一個詞彙會找出前5名相似
2.輸入兩個詞彙會算出兩者之間相似度
3.輸入三個詞彙爸爸之於老公,如媽媽之於老婆

輸入格式( Ex: 爸爸,媽媽,....註:最多三個詞彙)
老師
詞彙相似詞前 5 排序
班導,0.6360481977462769
班導師,0.6360464096069336
代課,0.6358826160430908
級任,0.6271134614944458
班主任,0.6270170211791992

輸入格式( Ex: 爸爸,媽媽,....註:最多三個詞彙)
爸爸,媽媽
計算兩個詞彙間 Cosine 相似度
0.780765200371

輸入格式( Ex: 爸爸,媽媽,....註:最多三個詞彙)
爸爸,老公,媽媽
爸爸之於老公，如媽媽之於
老婆,0.5401346683502197
蠢萌,0.5245970487594604
夠秤,0.5059393048286438
駁命,0.4888317286968231
孔爵,0.4857243597507477
```

## 資料集(wiki data)
```
主要以 pages-articles.xml.bz2 結尾之檔案類型，這邊使用 zhwiki-latest-pages-articles.xml.bz2。
```
* 維基資料集:</br>
https://zh.wikipedia.org/wiki/Wikipedia:%E6%95%B0%E6%8D%AE%E5%BA%93%E4%B8%8B%E8%BD%BD</br>
* zhwiki-latest-pages-articles.xml.bz2 下載網址:</br>
https://drive.google.com/file/d/0B4rlWa2S_JMBUmlMSG5IRVRMbnc/view?usp=sharing </br>
* 程式參考網址:</br>
https://radimrehurek.com/gensim/corpora/wikicorpus.html</br>
https://radimrehurek.com/gensim/models/word2vec.html</br>

## 開發環境
Python 3.5.2</br>
pip install gensim</br>
pip install jieba</br>
pip install hanziconv</br>
[note: if your gensim can't install, please check your os then install correct gensim version.](https://blog.csdn.net/dalangzhonghangxing/article/details/78191593)</br>

## Steps
```
git clone https://github.com/Alex-CHUN-YU/Word2vec.git
cd Word2vec

# Dataset
wget https://dumps.wikimedia.org/zhwiki/latest/zhwiki-latest-pages-articles.xml.bz2

# Anaconda
conda create -n Word2vec Python==3.5
conda activate Word2vec
pip install --upgrade pip
pip install gensim // Gensim 是一個用於處理自然語言的 Python 庫
pip install jieba // Jieba 是一個用於中文分詞的 Python 庫
pip install hanziconv // HanziConv 是一個用於繁簡體中文轉換的 Python 庫

# wiki xml -> wiki text
python wiki_to_txt.py zhwiki-latest-pages-articles.xml.bz2 // for 17 mins
"""
2024-03-21 15:36:12,548 : INFO : 目前已處理 10000 篇文章
...
2024-03-21 15:53:33,693 : INFO : 目前已處理 470000 篇文章
2024-03-21 15:53:40,378 : INFO : finished iterating over Wikipedia corpus of 472345 documents with 109828203 positions (total 4436651 articles, 129538612 positions before pruning articles shorter than 50 words)
轉檔完畢!
"""

# zh-CN -> zh-TW
python segmentation.py // for one hour
"""
(Word2vec) u7088883@yu5k6bctr1711003858184-b7sv2:~/Word2vec$ python segmentation.py
StopWord Set 已儲存!
2024-03-21 15:54:07,537 : INFO : 等待中..(簡 to 繁)
成功簡體轉繁體!
2024-03-21 15:59:41,018 : INFO : 等待中..(jieba 斷詞，並過濾停用詞)
Building prefix dict from the default dictionary ...
2024-03-21 15:59:41,083 : DEBUG : Building prefix dict from the default dictionary ...
Dumping model to file cache /tmp/jieba.cache
2024-03-21 15:59:41,714 : DEBUG : Dumping model to file cache /tmp/jieba.cache
Loading model cost 0.756 seconds.
2024-03-21 15:59:41,840 : DEBUG : Loading model cost 0.756 seconds.
Prefix dict has been built successfully.
2024-03-21 15:59:41,840 : DEBUG : Prefix dict has been built successfully.
jieba 斷詞完畢，並已完成過濾停用詞!
"""

# Training
python train.py
"""
(Word2vec) u7088883@yu5k6bctr1711003858184-b7sv2:~/Word2vec$ python train.py
訓練中...(喝個咖啡吧^0^)
model 已儲存完畢
"""

# Inference
python main.py
```
![image](https://github.com/ytl0623/Word2vec/assets/55120101/66aed8cd-7d30-4990-a4a9-dfa5a5dc3b51)

