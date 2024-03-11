# [pcap-busterz-1-7](https://pearlctf.in/challenges#pcap-busterz-1-7)

First I opened the sus.pcap using Wireshark

```
$ wireshark sus.pcap
```

In the Wireshark there were bunch of TCP protocols, So I checked the Few TCP Stream and found that all the TCP packects have same data. *x=12,y=45,color=white*

Following the TCP Streams, the first thing that came in my mind is that the x,y are coordinates of pixels and color of the pixel in that coordinate.

So I saved the output on a data.txt file. And formatted it in (12,45,white)

Then I thought of using `csv` python libary to read the data from data.txt file and save it in a 2D List. With `turtle` is used to print pixels in the coordinates given below.

```python
import turtle
import csv

window = turtle.Screen()
pen = turtle.Turtle()
window.bgcolor("red")
turtle.speed("fastest")

def pixels(x,y,colors): 
    pen.up()
    pen.setposition(x*3,y*3)
    pen.down()
    pen.dot(8,colors)

with open("data.txt","r") as file:
    data = csv.reader(file)
    time = 0
    for i in data:
        pixels(int(i[0]),int(i[1]),i[2])
        print("time: ",time)
        time += 1

turtle.done()
```

The Problem with this method/Libary is that Due to it's screen refresh it gets extremely slow (not ideal method), so I left it running in the background. and tried of finding better method to do so. I came across Pillow Libary

```
$ pip install pillow
```

pillow

```python
import csv
from PIL import Image

width = 100
height = 100
def create_image(width, height):
  image = Image.new("RGB", (width, height))
  pixels = image.load()

  with open("data.txt","r") as file:
    data = csv.reader(file)

    for x, y, color in data:
        if color == 'black': color = (0,0,0)
        if color == 'white': color = (255,255,255)
        if 0 <= int(x) < width and 0 <= int(y) < height:
            pixels[int(x), int(y)] = color

  return image

image = create_image(width, height)
image.save("flag.png")
```

This was better and faster method to get the output. And Finally I got a QR-Code png. After Scanning the QR Code, it get the flag.

flag: ' pearl{QR_rev0lution1ses_mod3rn_data_handl1ng} '