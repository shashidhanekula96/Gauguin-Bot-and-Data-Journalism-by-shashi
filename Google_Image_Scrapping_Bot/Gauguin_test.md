# Gauguin Google Image Scraping Bot 

Summary: In order to train the CNN model we need to form an image dataset with different types of graphs. I have created a scrapping bot which uses selenium and extracts images from google images. The images are dirctly downloaded into specific folders in the local system.

 Required packages


```python
from selenium import webdriver
import time
import urllib.request
import os
from selenium.webdriver.common.keys import Keys

```

Python bot using Chromedriver and selenium.
Chromedriver is the seperate executable that selenium webdriver utlizes to control google chrome.


```python

# Mention local location of the chromedriver executable
browser = webdriver.Chrome("C:\\Users\shash\Downloads\chromedriver_win32 (1)\\chromedriver.exe") 

#This code opens the Google chrome, accesses the mentioned website and searches the images.
browser.get("https://www.google.com/")
search = browser.find_element_by_name('q')
search.send_keys("Heatmap",Keys.ENTER)    #The type of images required are to be mentioned in the quotes
ele = browser.find_element_by_link_text('Images')
ele.get_attribute('href')
ele.click()

# As the bot only scrapes the loaded images therefore we need this code to automatically scroll and load the maximium Images in the webpage.
num = 0
for i in range(40):
   browser.execute_script("scrollBy("+ str(num) +",+2000);")
   num += 2000
   time.sleep(5)

  
ele1 = browser.find_element_by_id('islmp') #Here we are using a unique id which google uses to identify images.

image_tag = ele1.find_elements_by_tag_name("img")

# Here we are creating a new folder in the name of image and download the images into the folder.
try:
    os.mkdir('C:\\Users\shash\Desktop\Gauguin\Charts\Heatmap')
except FileExistsError:
    pass
count = 0
for i in image_tag:
    src = i.get_attribute('src')
    try:
        if src != None:
            src  = str(src)
            print(src)
            count+=1
            urllib.request.urlretrieve(src, os.path.join('C:\\Users\shash\Desktop\Gauguin\Charts\Heatmap','image'+str(count)+'.jpg')) #Change image name in the quotes as required
        else:
            raise TypeError
    except TypeError:
        print('fail')
```
