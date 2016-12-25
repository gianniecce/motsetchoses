---
layout: post
title:  "Create a website in 5min on Github using Jekyll"
categories: [jekyll]
tags: [mtus]
---

<span class='newthought'>In this post</span> I will show you how you can easily create a website using Github and Jekyll. 

## 1. Create gh-pages repository
The first step is to create a `new repository`. 
For instance, name you directory `website`. Click on the *Initialize this repository with a README*. Add also a  License if you want. 

<a data-flickr-embed="true"  href="https://www.flickr.com/photos/136480412@N05/31724074152/in/album-72157674402348604/" title="git_1"><img src="https://c1.staticflickr.com/1/289/31724074152_943278753c_z.jpg" width="640" height="447" alt="git_1"></a><script async src="//embedr.flickr.com/assets/client-code.js" charset="utf-8"></script>

Once the repository created click on `Branch: master`. You will need to write `gh-pages` and select it. 


<a data-flickr-embed="true"  href="https://www.flickr.com/photos/136480412@N05/31724070252/in/album-72157674402348604/" title="git_2"><img src="https://c5.staticflickr.com/1/350/31724070252_7a2c60d64d_z.jpg" width="640" height="180" alt="git_2"></a><script async src="//embedr.flickr.com/assets/client-code.js" charset="utf-8"></script>

<a data-flickr-embed="true"  href="https://www.flickr.com/photos/136480412@N05/31062123953/in/album-72157674402348604/" title="git_3"><img src="https://c2.staticflickr.com/1/421/31062123953_e115b116d6_z.jpg" width="640" height="167" alt="git_3"></a><script async src="//embedr.flickr.com/assets/client-code.js" charset="utf-8"></script>

Go then to **Setting** and again to `Branches`. Change `master` to `gh-pages`. Click on *update*. Ignore the warning and update. 

<a data-flickr-embed="true"  href="https://www.flickr.com/photos/136480412@N05/31871653905/in/album-72157674402348604/" title="git_4"><img src="https://c2.staticflickr.com/1/276/31871653905_b4f3f5fac8_z.jpg" width="640" height="206" alt="git_4"></a><script async src="//embedr.flickr.com/assets/client-code.js" charset="utf-8"></script>

<a data-flickr-embed="true"  href="https://www.flickr.com/photos/136480412@N05/31724066292/in/album-72157674402348604/" title="git_5"><img src="https://c5.staticflickr.com/1/557/31724066292_8c5bca0c2c_z.jpg" width="640" height="208" alt="git_5"></a><script async src="//embedr.flickr.com/assets/client-code.js" charset="utf-8"></script>

<a data-flickr-embed="true"  href="https://www.flickr.com/photos/136480412@N05/31724064242/in/album-72157674402348604/" title="git_6"><img src="https://c3.staticflickr.com/1/558/31724064242_0b7985e310_z.jpg" width="300" height="150" alt="git_6"></a><script async src="//embedr.flickr.com/assets/client-code.js" charset="utf-8"></script>


## 2. Clone
The second step consists of "importing" your webiste on your computer. Because I am not very handy with the `Terminal` command line, I am using the excellent github desktop. 


You can download at 

<a href="https://desktop.github.com/">https://desktop.github.com/</a>

Once you downloaded the github desktop, go back to your github page and click on `Clone or download`

<a data-flickr-embed="true"  href="https://www.flickr.com/photos/136480412@N05/31062118413/in/album-72157674402348604/" title="git_7"><img src="https://c6.staticflickr.com/1/682/31062118413_c4e46d53d2_z.jpg" width="400" height="290" alt="git_7"></a><script async src="//embedr.flickr.com/assets/client-code.js" charset="utf-8"></script>


Then click on `Open in Desktop`. This will open a repository on your personal computer with the name of the githubt repository (`clone as Website` in our case). 

<a data-flickr-embed="true"  href="https://www.flickr.com/photos/136480412@N05/31833757686/in/album-72157674402348604/" title="git_8"><img src="https://c7.staticflickr.com/1/555/31833757686_6ab6827e1d_z.jpg" width="400" height="100" alt="git_8"></a><script async src="//embedr.flickr.com/assets/client-code.js" charset="utf-8"></script>

Your page will then be visible on your github desktop. 

Now open the `website` folder that as been *cloned* (copied) on your desktop. It only contains the `Readme` file and the Licence if you added one. 

## 3. Create your website design 

If you are not familiar with web design, you can start by downloading a `theme` (which is a style or design of your webpage). 

Jekyll as a lot of great readymade design you can download on the internet. 

For instance, here 

<a href="http://themes.jekyllrc.org/">http://themes.jekyllrc.org/</a>

Choose a template or theme you like and simply download it. 


<a data-flickr-embed="true"  href="https://www.flickr.com/photos/136480412@N05/31031061374/in/album-72157674402348604/" title="git_9"><img src="https://c7.staticflickr.com/1/298/31031061374_087ddacf43_z.jpg" width="400" height="200" alt="git_9"></a><script async src="//embedr.flickr.com/assets/client-code.js" charset="utf-8"></script>


Unzip the file and copy the file in the github folder on your computer. In doing so, you simply copy the codes of the website. This will enable you to generate the same website on your github page. 

<a data-flickr-embed="true"  href="https://www.flickr.com/photos/136480412@N05/31724076582/in/album-72157674402348604/" title="git_10"><img src="https://c7.staticflickr.com/1/777/31724076582_8cdb7b9cb9_z.jpg" width="400" height="210" alt="git_10"></a><script async src="//embedr.flickr.com/assets/client-code.js" charset="utf-8"></script>

Now go back to the github desktop and you will notice that you can see the files you just copied. When using github you need to `commit` (send) file to your github page. Write down a note and click on `Commit to gh-pages`. This will send the codes on your page. 

<a data-flickr-embed="true"  href="https://www.flickr.com/photos/136480412@N05/31724077862/in/album-72157674402348604/" title="git_11"><img src="https://c7.staticflickr.com/1/398/31724077862_7bc92fd0c5_z.jpg" width="537" height="640" alt="git_11"></a><script async src="//embedr.flickr.com/assets/client-code.js" charset="utf-8"></script>


## 4. The website is ready 

Go back on your github page. Go to `Settings` and scroll down. You will see that it says: `Your site is published at: https://YOURNAME.github.io/website/`. 

Click on the website will appear! 

<a data-flickr-embed="true"  href="https://www.flickr.com/photos/136480412@N05/31724071312/in/album-72157674402348604/" title="git_12"><img src="https://c1.staticflickr.com/1/426/31724071312_61ba38cc3a_z.jpg" width="640" height="169" alt="git_12"></a><script async src="//embedr.flickr.com/assets/client-code.js" charset="utf-8"></script>

## 5. Write posts 

There are many ways to write posts on github. One simple way is to create posts manually in the website folder on your desktop. 

Posts are stored in the folder `_posts`. 

<a data-flickr-embed="true"  href="https://www.flickr.com/photos/136480412@N05/31062907453/in/album-72157674402348604/" title="git_13"><img src="https://c6.staticflickr.com/1/276/31062907453_fa7bbc1e6a_m.jpg" width="340" height="120" alt="git_13"></a><script async src="//embedr.flickr.com/assets/client-code.js" charset="utf-8"></script>

What you need to do is created a Markdown file and simply store it in the `_posts` folder. Posts are generally stored by dates. Make your life easy in copying an existing post (provided by the website imported from the internet) and start tweaking it with a new date and a new name. 

