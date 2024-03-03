---
layout: post
title:  "How to Test Stock Strategies Easily in Python"
date: 2024-02-24
description: A post about how to use RapidAPI's EOD Historical Data API to test stock strategies
image: "/assets/img/stocks-moving.jpg"
display_image: false
---
<p class="intro"><span class="dropcap">H</span>ave you ever tried to test stock strategies manually by looking at charts and indicators, writing down numbers, and then having to redo the entire process to test different inputs? Well, there is an easier way that will allow you to rapidly test tons of strategies in seconds.</p>

### 1 - Connect to the API and Make Your First Request

In order to start getting stock data to work with, you first need to go to the [EOD Historical Data API on Rapid Tables](https://rapidapi.com/eod-historical-data-eod-historical-data-default/api/eod-historical-data) and generate some Python code under "Code Snippets." This will automatically generate an api key for you, which makes things fast!

Go to Google Collaboratory or any other place where you can run your python code (Google Collaboratory makes it easier because you don't have to pip install packages, but if you feel comfortable configuring a virtual environment for your code to run in, any IDE should work).



1. Create a new file in the `_posts` folder called `YYYY-MM-DD-post-name.md`, where YYYY is the year (2024), MM numeric month (01-12), and DD is the numeric day of the month (01-31).  The `post-name` is a short name for the new post with `-` between words.  **You must use this name convention for all new posts.**  

2. Make the YML heading.  All pages in the site need to start with a YML heading.  For posts you should use the following header:
```
---
layout: post
title:  "Post Name"
description: Short yet informative description
image: /assets/img/blog-image.jpg
---
```
*For this theme, the layout should stay as `post`.   All the other fields should be updated with the information for your particular blog post.*

3. If you add an image to the header information, the top banner picture will change for this post.  You can also add an optional field `display_image: true` if you want to display a larger image just below the header image.  If this happens, the banner image will return to the default.
  * The blog image should be a `.jpg` or `.png` file that can be found in `assets/img`.  Don't make it too large or the page will take longer to load (500-800 KB is a good size).  Leave the file path as `/assets/ing/` in the header area.* 
  * Adding a post image will also display the image in the "next" or "previous" post location.

4. Write the body of the blog using markdown.  There are a lot of references for markdown available.  I like the [Markdown Guide](https://www.markdownguide.org) because many of the examples show both the markdown and the html code.  There are separate pages for [basic syntax](https://www.markdownguide.org/basic-syntax/), [extended syntax](https://www.markdownguide.org/extended-syntax/), and a [cheatsheet](https://www.markdownguide.org/cheat-sheet/) for quick reference. 