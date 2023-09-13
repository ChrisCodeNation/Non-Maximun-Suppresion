# Non-Maximun-Suppresion

在物件偵測過程中，通常是先框選出物件候選人，再判斷是否真的為物件。但通常一個物件會被很多候選框選到。如下圖所示，下左圖為物件被多個候選框選中的例子。為了消除多餘的物件框，並找出最佳的框，大部分研究都使用Non-Maximum Suppression(NMS)達到這個目的。[Hackmd](https://hackmd.io/@IDT8SB7yQj2L1N5PoZbLEA/HJWNLjaC2)中有更詳細的說明。
![image](https://github.com/ChrisCodeNation/Non-Maximun-Suppresion/assets/90707699/b3e299bb-8f89-4366-a3ca-f55fc61d1571)

## NMS步驟
1. 根據信心程度做排序，接著挑選出信心程度最高的BBox，並加入到「確定是物件的集合」中
2. 其他的BBox與剛挑選出的BBox計算IoU，若IoU>設定閥值，則將那些BBox信心程度設定為0，並刪除這些框
3. Repeat步驟1和2直到所有BBox被處理完（沒有信心程度大於0的框），「確定是物件的集合」中的物件就是最終結果

## 範例一：影像中有兩隻狗，如何使用NMS從候選框中找出兩隻狗
由於第一個例子只看怎麼找出狗，因此這裡只用到五個資訊：

- BBox中心位置(x, y)和BBox長寬(h, w)
- Confidence score

實際流程圖如下：

![image](https://github.com/ChrisCodeNation/Non-Maximun-Suppresion/assets/90707699/ad5743ee-aea2-46d4-90a3-c817ccdb66de)

1. 「確定是物件的集合」 = {空集合}
2.  (Run1)將BBox根據信心程度排序，而信心度最高的BBox（紅色）會加入到「確定是物件的集合」中，接著與其他BBox計算計算IoU。計算完IoU，假設粉色IoU=0.6大於設定閥值，將其信心度設定為0。
 **「確定是物件的集合」 = {紅色BBox}**
 
3. (Run2)不考慮信心度為0及在「確定是物件的集合」內的BBox。從剩下的BBox中挑選出信心度最高的BBox（黃色），並加入到「確定是物件的集合」，接著黃色BBox與其餘的BBox計算IoU。假設其餘BBox的IoU都大於0.5，因此將這些BBox的信心程度設為0。
  **「確定是物件的集合」 = {紅色BBox、黃色BBox}**
  
4. 由於沒有BBox的信心程度>0，結束NMS。
 **「確定是物件的集合」 = {紅色BBox、黃色BBox}**
