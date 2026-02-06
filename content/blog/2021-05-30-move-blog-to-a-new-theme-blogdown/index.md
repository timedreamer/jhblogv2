---
title: Move blog to a new theme (blogdown)
author: Ji Huang
date: '2021-05-30'
categories:
  - dry lab
tags:
  - blog
  - web
meta_img: images/image.png
description: Description for the page
---

I was upset that I can't set up Rmarkdown files correctly in my blog using [even theme](https://github.com/olOwOlo/hugo-theme-even). Therefore, I changed it to a new minimalist theme: [hugo-tanka](https://github.com/nanxstats/hugo-tanka) today. There are some tricks, so I wrote down what I did.

1. Start a new blogdown R project in RStudio with the theme I want:

![img](https://i.imgur.com/kJRXfvr.jpg)

2. Move all my posts in the `jhblog\content\post` to the new destination `jhblogv2\content\blog`.

3. Remove **\<\!--more\--\>** from the posts. This sentence has some wield behaviour on the new theme. So I deleted them from all the posts.

4. Set up the Git and the [Github repo](https://github.com/timedreamer/jhblogv2).

5. Delete the old blog site on Netlify, so I can keep the same URL.

6. Set up new Github repo in [Netlify](https://www.netlify.com/).

6. When setting up the Netlify, make sure to add HUGO version to the same as the local. You can do this by add a `New Variable` as `HUGO_VERSION` and set to `0.83.1`

7. **Change site name** in Netlify. The name will become the prefix for the website. For example, I set the site name to `jhuang`, then my blog website becomes `jhuang.netlify.app`.

8. In `config.yaml`, change the `baseurl: https://jhuang.netlify.app/`. I don't have to do this for the Even theme, but necessary for the hugo-tanka theme.

9. Modify `about.md` for my personal introduction page.

10. Choose a good flavicon from *https://favicon.io/favicon-generator/* and replace the `jhblogv2\static\images\favicon1.png`.

11. Now you should be good to go!


