# agrag
```
import numpy as np #ライブラリーのインポート
import pandas as pd
import matplotlib.pyplot as plt
from PIL import Image
import seaborn as sns

def pixel(file):    #ピクセル化
    img = Image.open(file)
    img = img.resize((bit,bit), Image.Resampling.LANCZOS) 
    px = np.array(img) 
    px = 1 * (px < 16) #画素値が16以下であればPixel=１(黒)にする
    return px

bit = 30    #30x30を指定
atu =  np.empty((0, bit, bit))
usu =  np.empty((0, bit, bit))

for i in range(1,191):
    h=50+5*(i-1)
    file = "filename"+str(h)+"Hz.jpg" #撮影画像の名前が周波数の為の処理
    atusec = pixel(file)
    atu = np.vstack((atu,[atusec]))
    
for i in range(1,191):
    h=50+5*(i-1)
    file = "filename"+str(h)+"Hz.jpg"
    ususec = pixel(file)
    usu = np.vstack((usu,[ususec]))

#類似度判定
cnt = 0
y=[]

for j in range(120): 
    cnt = 0 #カウント初期化
    for i in range(190): 
        cnt1 = (atu[i,:,:]==usu[j,:,:]).sum()
        if cnt1 >= cnt:
            cnt=cnt1

            atuHz = 50+5*i
            usuHz = 50+5*j
    print(atuHz,usuHz) #類似する組の表示
    y.append(atuHz)

x=list(range(50,550,5))
plt.plot(x,y)
plt.show() #グラフの表示
```
