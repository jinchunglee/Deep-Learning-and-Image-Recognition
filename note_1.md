# some functions
1. how to read image?

```pyhton 

cv2.imread()

```

2. how to show the image?

```pyhton
cv2.imshow('image', img)
cv2.waitKey(1000)

```

3. resize

```pyhton

```


4. video 讀取
cap = cv2.VideoCapture("ESP WIFI.mp4")


5. viseo 播放

while True:
    ret, frame = cap.read()
    if ret:
        cv2.imshow('video', frame)
    else:
        break
    cv2.waitKey(1)


6. 圖片生成
import cv2
import numpy as np

#B G R

img = np.empty((400, 300, 3), np.uint8)
for row in range(0, 400):
    for col in range(0, 300):
        img[row, col] = [0, 255, 255]
cv2.imshow('image', img)
cv2.waitKey(0)







## error message

SyntaxError: (unicode error) 'unicodeescape' codec can't decode bytes in position 2-3: truncated \UXXXXXXXX escape

因為你的文件路徑是 "\" 而不是 "/"



## 常用函式

1. 轉換灰階
2. 模糊
3. 邊缘
4. 膨脹
5. 腐蝕
import cv2
import numpy as np

kernal = np.ones((2, 2), np.uint8)
kernal1 = np.ones((2, 2), np.uint8)


img = cv2.imread('HD-wallpaper-2022-mercedes-maybach-s-580-4matic.jpg')
img = cv2.resize(img, (0, 0), fx = 0.5, fy = 0.5)


grey = cv2.cvtColor(img, cv2.COLOR_BGR2GRAY) #灰階

blur = cv2.GaussianBlur(img, (5, 5), 10) #模糊

canny = cv2.Canny(img, 100, 100) #邊缘

dilate = cv2.dilate(canny, kernal, iterations = 1) #膨脹

erode = cv2.erode(dilate, kernal1, iterations = 1) #腐蝕


cv2.imshow('image', img)
cv2.imshow('image_1', grey)
cv2.imshow('blur', blur)
cv2.imshow('canny', canny)
cv2.imshow('dilate', dilate)
cv2.imshow('erode', erode)
cv2.waitKey(0)




## 畫圖、寫字(only English)
import cv2
import numpy as np

img = np.zeros((600, 600, 3), np.uint8)
cv2.line(img, (0, 0), (img.shape[1], img.shape[0]), (255, 255, 0), 5) #img.shape[1]寬度,img.shape[0]高度

cv2.rectangle(img, (0, 0), (250, 350), (0, 255, 255), cv2.FILLED)

cv2.circle(img, (400, 50), 30, (0, 255, 0), 5)

cv2.putText(img, "OPENCV", (400, 200), cv2.FONT_HERSHEY_COMPLEX, 1, (255, 255, 255), 3)

cv2.imshow('img', img)
cv2.waitKey(0)


## 偵測顏色 hsv
import cv2
import numpy as np

def empty(v):
    pass

img = cv2.imread('USA.jpg')
img = cv2.resize(img, (0, 0), fx=0.8, fy=0.8)

cv2.namedWindow('Trackbar')
cv2.resizeWindow('Trackbar', 640, 240)

cv2.createTrackbar('Hue Min', 'Trackbar', 0, 179, empty)
cv2.createTrackbar('Hue Max', 'Trackbar', 179, 179, empty)
cv2.createTrackbar('Sat Min', 'Trackbar', 0, 255, empty)
cv2.createTrackbar('Sat Max', 'Trackbar', 255, 255, empty)
cv2.createTrackbar('Val Min', 'Trackbar', 0, 255, empty)
cv2.createTrackbar('Val Max', 'Trackbar', 255, 255, empty)


hsv = cv2.cvtColor(img, cv2.COLOR_BGR2HSV)


while True:
    h_min = cv2.getTrackbarPos('Hue Min', 'Trackbar')
    h_max = cv2.getTrackbarPos('Hue Max', 'Trackbar')
    s_min = cv2.getTrackbarPos('Sat Min', 'Trackbar')
    s_max = cv2.getTrackbarPos('Sat Max', 'Trackbar')
    v_min = cv2.getTrackbarPos('Val Min', 'Trackbar')
    v_max = cv2.getTrackbarPos('Val Max', 'Trackbar')
    print(h_min, h_max, s_min, s_max, v_min, v_max)

    mask = cv2.inRange(hsv, (h_min, s_min, v_min), (h_max, s_max, v_max))
    result = cv2.bitwise_and(img, img, mask=mask)

    cv2.imshow('image', img)
    cv2.imshow('hsv', hsv)
    cv2.imshow('mask', mask)
    cv2.imshow('result', result)
    cv2.waitKey(1)

## 輪廓檢測
import cv2


img = cv2.imread('image.png')
imgContours = img.copy()

img = cv2.cvtColor(img, cv2.COLOR_BGR2GRAY)

canny = cv2.Canny(img, 150, 200)

contours, hierarchy = cv2.findContours(canny, cv2.RETR_EXTERNAL, cv2.CHAIN_APPROX_NONE) #偵測邊緣

for cnt in contours:
    #print(cnt) #印出邊緣值
    cv2.drawContours(imgContours, cnt, -1, (0, 255, 0), 3) #畫出邊緣
    area = cv2.contourArea(cnt) #計算邊緣面積
    if area > 1000: #把噪點弄掉
        #print(cv2.contourArea(cnt)) #印出邊緣面積
        #print(cv2.arcLength(cnt, True)) #計算邊緣長度 第二個看是不是閉合的
        vertices = cv2.approxPolyDP(cnt, 0.02 * cv2.arcLength(cnt, True), True) #近似邊緣多邊形，並且回傳多邊形頂點座標
        print(len(vertices)) #多邊形的邊緣點數
        x, y, w, h = cv2.boundingRect(vertices) #找出邊界
        cv2.rectangle(imgContours, (x, y), (x + w, y + h), (0, 255, 0), 5)
        if len(vertices) == 4:
            cv2.putText(imgContours, 'Rectangle', (x, y - 5), cv2.FONT_HERSHEY_COMPLEX, 0.5, (0, 0, 255), 1)
        if len(vertices) == 3:
            cv2.putText(imgContours, 'Triangle', (x, y - 5), cv2.FONT_HERSHEY_COMPLEX, 0.5, (0, 0, 255), 1)
        if len(vertices) == 5:
            cv2.putText(imgContours, 'Pentagon', (x, y - 5), cv2.FONT_HERSHEY_COMPLEX, 0.5, (0, 0, 255), 1)
        if len(vertices) == 6:
            cv2.putText(imgContours, 'Star', (x, y - 5), cv2.FONT_HERSHEY_COMPLEX, 0.5, (0, 0, 255), 1)
        if len(vertices) > 6:
            cv2.putText(imgContours, 'Circle', (x, y - 5), cv2.FONT_HERSHEY_COMPLEX, 0.5, (0, 0, 255), 1)


cv2.imshow('image', img)
cv2.imshow('canny', canny)
cv2.imshow('imageContours', imgContours)
cv2.waitKey(0)




## 人臉辨識
import cv2

img = cv2.imread('image/members.png')
grey = cv2.cvtColor(img, cv2.COLOR_BGR2GRAY)
facecascade = cv2.CascadeClassifier('face_detect.xml')
facerect = facecascade.detectMultiScale(grey, 1.1, 4)

print(len(facerect))

for (x, y, w, h) in facerect:
    cv2.rectangle(img, (x, y), (x + w, y + h), (0, 255, 0), 3)

cv2.imshow('image', img)
#cv2.imshow('grey', grey)
cv2.waitKey(0)