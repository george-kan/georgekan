---
title: Blogdown and Jupyter/Colab
date: '2021-12-26'
slug: []
categories:
  - general
tags:
  - R
  - Jupyter
  - Python
subtitle: ''
excerpt: 'How to integrate `blogdown` with `Jupyter`'
draft: no
series: ~
layout: single
---

For my blogging purposes, I have been working so far almost exclusively with R. But since I have decided that it is time for me to deep learn❗ and therefore I have to switch to Python.  
Unfortunately though, my whole blog is written with Rmarkdown and Hugo so I could not find a way to easily add Python on top. Furthermore, since deep learning is the goal, using a GPU is essentially a prerequisite so I will start out using Colab. Which begs the question how to showcase a Colab notebook using `blogdown`. Let's have a look:

### Step 1: Create the notebook


```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
```


```python
temp_df = pd.DataFrame({'x':np.array([1,5]), 'y':np.array([2,2])})
temp_df
```





<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>x</th>
      <th>y</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>2</td>
    </tr>
    <tr>
      <th>1</th>
      <td>5</td>
      <td>2</td>
    </tr>
  </tbody>
</table>






```python
np.random.seed(32)

plt.rcdefaults()
fig, ax = plt.subplots()

# Example data
people = ('Tom', 'Dick', 'Harry', 'Slim', 'Jim')
y_pos = np.arange(len(people))
performance = 3 + 10 * np.random.rand(len(people))
error = np.random.rand(len(people))

ax.barh(y_pos, performance, xerr=error, align='center')
ax.invert_yaxis()  # labels read top-to-bottom
ax.set_xlabel('Performance')
ax.set_title('How fast do you want to go today?')

plt.show()
```


<img src="/python_img/2021-12-26-blogdown-and-jupyter-colab/Blogdown_and_Jupyter_Colab_4_0.png" />


### Step 2: Export the notebook as an `.ipynb` file

<img src="/python_img/2021-12-26-blogdown-and-jupyter-colab/download.png" />


### Step 3: Open the command prompt and change the directory to the place where you stored the file. Run the following command:
`jupyter nbconvert --to markdown <Your_file>.ipynb`

This command does the following two things:  
1. Converts the `.ipynb` file to a `.md` Markdown file
2. Creates a folder with all the plots generated from the file

### Step 4: Create a new blogpost via `blogdown` with an .md extension and paste the content of the `.md` file from step 3 in the new `.md` file.
You can do this step in different ways as well, you could create a new folder and place the `.md` file there, whatever works for you.


```python
# In Blogdown run the following
new_post('Time to merge', ext='.md')
```

### Step 5: Store the pictures that were generated in step 3 under a folder in static 
This was a bit tricky for me. Going through the [blogdown manual](https://bookdown.org/yihui/blogdown/static-files.html) I think I understood why this has to be the case. `.md` files do not know the directory they are stored in but they can find files in the `public` folder which is where Hugo moves the `static` folder when the website is rendered.  
⚠ This is my understanding of the logic so be warned that this part might be inaccurate, please let me know if I have written something incorrect here! ⚠

To have the images a bit more tidy I decided to create subfolders for each of my posts. The following `html code` will include the graph/chart in the post:

```python
<img src="/python_img/2021-12-26-blogdown-and-jupyter-colab/Blogdown_and_Jupyter_Colab_4_0.png" />
```

**Notice that there is no `static` in the src and that there is a dash at the beginning of the address**

And that is all! I hope this has been helpful, show me your website if you managed to do something similar! 

### Other useful resources

<a href="https://www.timlrx.com/blog/uploading-jupyter-notebook-files-to-blogdown"> Uploading Jupyter Notebook Files to Blogdown </a>

### Issues encountered so far and their solutions 

1. Could not turn off the interactive display in Colab. → Left only the HTML table without the format
2. Could not render `.jpg` → Changed to `.png` instead
 