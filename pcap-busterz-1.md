## Challenge: [pcap-busterz-1-7](https://pearlctf.in/challenges#pcap-busterz-1-7)

This document outlines the steps I took to solve the "pcap-busterz-1-7" challenge from [PearlCTF](https://pearlctf.in/).

**Initial Analysis**

1. **Examining the Capture File:**
   I started by opening the `sus.pcap` file using Wireshark, a network protocol analyzer. Inside, I observed a significant number of TCP packets.
2. **Data Pattern Recognition:**
   Upon inspecting a few TCP streams, I discovered that all packets contained identical data in the format `x=12,y=45,color=white`. This pattern suggested a potential correlation between `x` and `y` values representing pixel coordinates and `color` indicating the pixel's shade.

**Data Processing and Visualization**

1. **Extracting Data:**
   I extracted the data from the capture file and saved it in a text file named `data.txt`. Each line in `data.txt` followed the format `(12,45,white)`, representing a pixel's coordinates and color.
2. **Initial Attempt with `csv` and `turtle` (Slow Method):**
   I initially used the `csv` library in Python to read the data from `data.txt` and convert it into a two-dimensional list. The `turtle` library was then employed to draw pixels on the screen based on the provided coordinates.


   ```python
   import turtle
   import csv

   window = turtle.Screen()
   pen = turtle.Turtle()
   window.bgcolor("red")
   turtle.speed("fastest")

   def pixels(x, y, colors):
       pen.up()
       pen.setposition(x * 3, y * 3)
       pen.down()
       pen.dot(8, colors)

   with open("data.txt", "r") as file:
       data = csv.reader(file)
       time = 0
       for i in data:
           pixels(int(i[0]), int(i[1]), i[2])
           print("time: ", time)
           time += 1

   turtle.done()
   ```

   While functional, this method suffered from slow performance due to screen refresh limitations.
3. **Optimization with Pillow Library:**
   To address the performance issue, I opted for the Pillow library, which is more efficient for image manipulation.


   ```python
   import csv
   from PIL import Image

   width = 100
   height = 100

   def create_image(width, height):
       image = Image.new("RGB", (width, height))
       pixels = image.load()

       with open("data.txt", "r") as file:
           data = csv.reader(file)

           for x, y, color in data:
               if color == 'black':
                   color = (0, 0, 0)
               elif color == 'white':
                   color = (255, 255, 255)
               else:
                   continue  # Skip invalid colors

               if 0 <= int(x) < width and 0 <= int(y) < height:
                   pixels[int(x), int(y)] = color

       return image

   image = create_image(width, height)
   image.save("flag.png")
   ```

   This implementation significantly improved the processing speed.

**Result and Flag**

* By processing the data, I obtained a PNG image that, upon scanning with a QR code reader, revealed the flag:

```
pearl{QR_rev0lution1ses_mod3rn_data_handl1ng}
```

I hope this explanation is helpful!