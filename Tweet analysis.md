
# Make Trump Word-Clouds Again

### Author : Sean D Rosario
##### Version 1.0
##### Last Edit on : 03/02/2016

---

## About this analysis:

The world is taking to twitter to express their feelings about businessman, presidential candidate and entertainer, Donald J. Trump

So I decided to analyze what people are saying by making a word cloud of popular words.

Just for fun, they're shaped and colored to match a very angry Donald Trump (see result towards the end)

The data I used is available at: http://s3.amazonaws.com/bigdataclassecc/twitterdata.txt, from NYU graduate course DS-GA 1004- Big Data

Note: To run this on your local machine, follow the setup instructions at https://github.com/amueller/word_cloud/


## Loading the packages


```python
import pandas as pd
import numpy as np
import re
from os import path
from PIL import Image
import matplotlib.pyplot as plt
from wordcloud import WordCloud, STOPWORDS, ImageColorGenerator
%matplotlib inline
import pylab
from pylab import rcParams
```

## Reading the data


```python
text = ""
with open("twitterdata.txt", "r") as all_tweets:
    count =0
    for tweet in all_tweets:
        tweet_contains_Donald = re.search('Donald', str(tweet), re.IGNORECASE) != None
        tweet_contains_Trump =  re.search('Trump', str(tweet), re.IGNORECASE) != None
        # bing bing bong bong bing bing bing
        if tweet_contains_Donald or tweet_contains_Trump:
            text = text + tweet #[:-1] + " " #Each line ends with a \n so I replace that with a space.
            count+=1
```


```python
print "We are taking "+str(count)+" Trump-related tweets"
```

    We are taking 60471 Trump-related tweets



```python
#Previewing the first 100 characters
print text[0:100]
```

    more I read about the scandals in the Clinton Global Initiative the more concerned I am that Trump w


## Building a wall. jk its word cloud


```python
"""
The below code is based on the code used as an example to 
generate word clouds for Alice in wonderland

The original code can be found at:
https://github.com/amueller/word_cloud/blob/master/examples/colored.py
All credits to Andres Müller and other contributors

link for the image: http://usercontent2.hubimg.com/12839839_f1024.jpg
"""

trump_face =np.array(Image.open("Trump_pic.jpg"))   #Load the image and convert it to a numpy array

wc = WordCloud(background_color="white", max_words=2000, mask=trump_face,
               stopwords=STOPWORDS.add("said"),
               max_font_size=40, random_state=42)

# generate word cloud
wc.generate(text)

# create coloring from image
image_colors = ImageColorGenerator(trump_face)

# show
plt.imshow(wc)
plt.axis("off")
#plt.figure()


# recolor wordcloud and show
plt.imshow(wc.recolor(color_func=image_colors))
plt.axis("off")
F = plt.figure()
DefaultSize = F.get_size_inches()
rcParams['figure.figsize'] =DefaultSize[0]*2,DefaultSize[1]*2
F


"""
# In the following commented out code,
# I tried to upscale the image 5x. Didn't work. 
# If anyone knows how to rescale a pyplot figure, please let me know


F = plt.figure()
DefaultSize = F.get_size_inches()
F.set_size_inches( (DefaultSize[0]*5, DefaultSize[1]*5) )
F
"""

plt.imshow(trump_face, cmap=plt.cm.gray)
plt.axis("off")
plt.show()
```


![png](output_8_0.png)



![png](output_8_1.png)


Note: To make the tiny words are readable, I made the pictures huuuuuuge.


### Understanding the word cloud
The size of the words correspond to their frequency of occurence among tweets

### Let's look at some of the most popular words:

* MakeDonaldDrumfAgain  - John Oliver did a segment where he asked people to tweet with this hashtag
* realDonalTrump - This is his twitter handle
* https - most links starts with this sequence of characters. People were tweeting links (most likely Trump-related articles)

Fun Trump quote: "I know words, I have the best words'

---

