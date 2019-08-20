## 使用貝葉斯定理來做後處理

### 可以在+-10%自由平移

使用這個後處理的關鍵在於題目告訴我們在+-10%以內都算一分，所以反過來說，假設今天你有一個預測值落在+-10%以內，此時你不管調上調下10%，你的這一分永遠不會掉，因為它永遠落在算分的區間裡面，縱使可能讓預測值離真實值更遠，所以在+-10%自由平移是不會讓主要分數變差的。

但是平移造成了潛在的好處，假設今天我們移動的很好，還會有可能把原本不在算分範圍內的變成在範圍內，那要怎麼移動才可以讓我們趨近一個好的成果，我們可以用過去資料來作移動的準則，這就是接下來要介紹的貝葉斯定理。

### 使用貝葉斯定理來移動

列一下貝葉斯定理：  

<img src="http://latex.codecogs.com/gif.latex?P(real=x|predict=y)*P(predict=y)"/>

<img src="http://latex.codecogs.com/gif.latex?=P(predict=y|real=x)*P(real=x)"/>

上述式子當中，$P(real=x)$是顯而易見的，可以由過去數據來的到這個機率分布，而我們想要得到的是$P(real=x|predict=y)$，它代表我給定一個預測值 $y$ 它實際是 $x$ 的機率，有了這個機率分布我們就有了準則去平移，讓平移後的結果可以覆蓋最大的可能性，那既然是考慮給定預測值的情形，那這一項 $P(predict=y)$ 就是定值了，所以也不需要知道它實際的數值，因此我們只剩下這項 $P(predict=y|real=x)$ 需要計算。

此時作一個假設，假設我的預測值會呈正態分布偏離實際值，那們  $P(predict=y|real=x)$ 就變成可以計算的了，所以：  

<img src="http://latex.codecogs.com/gif.latex?P(real=x|predict=y_0)\propto \mathcal{N}(\mu=y_0,\sigma)|_x*P(real=x)"/>

使用上述的式子去移動區間，讓區間內所囊括的機率最大，也就是下面式子：  

<img src="http://latex.codecogs.com/gif.latex?x^*=argmax_x \sum_{s=x*0.9}^{s=x*1.1} \mathcal{N}(\mu=y_0,\sigma)|_s*P(real=s)"/>

我們回頭來看正態分布的這個假設，其實也是相當的合理的，因為我們使用RMSE來當作Loss，所以不準度的分布的確應該是呈現正態分布的。



