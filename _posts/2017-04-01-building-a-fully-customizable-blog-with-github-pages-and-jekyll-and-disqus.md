---
layout: blogpost
title: Building a personal blog with github pages and jekyll and disqus
comments: true
---

A personal blog is one of the easiest and affordable medium of sharing your thoughts with others. As a developer, you need a way to
- Showcase you capabilities
- Document your learning and experiments so you can return to them in the future
- Share your thoughts with others as well as provide a guide to other developers who may be struggling with a problem you have encountered and solved in the past.

While there are several options available for setting up a personal blog, working with Github pages affords you the convenience of working from your Github profile while sharing your projects with other, also, you have . Github pages is also a great too for project documentation. 
In this post I am going to share with you how this blog was set up straight from my [github page](https://github.com/EzechukwuJI/ezechukwuji.github.io) and customized using [Jekyll](https://jekyllrb.com/). I will also share how to add automated commenting, enable discussions and increase engagement on your blog using [disqus](https://disqus.com/). 

Enough introduction already. Let's get started.
### Setting up Github Page
To set up a github page you need an [github](https://github.com/) account(Surprised?).
Log into your github account, click on the Plus sign on the top right hand corner to pull down the menu as shown in the diagram below

![create new repository](/assets/static/post_images/create_new_repo.png)

create a new repository as below, your repository name should be of the form *username.github.io*. This is where your website files reside. 

![create site repository](/assets/static/post_images/create_page_repo.png)

Next, click on the newly created repository, on the next page click on settings on the top right hand corner or the page, this opens the settings page for the website you are creating.

![go to settings](/assets/static/post_images/open_settings.png)

On the settings page, scroll to the github pages section. Everything is set for you, you only need to set the theme for your website.
by clicking on the **choose aa theme** button.

![select theme page](/assets/static/post_images/pages section.png)

Clicking the button takes you to the theme chooser. Click on any of the thumbnails to preview theme. Click the **select theme** button to automatically add theme to your website.

![select theme](/assets/static/post_images/select theme.png)

After this, your blog is ready to fly.
Next Navigate to your github pages repository and make your first post to your blog.

### Blogging with Jekyll

While I was setting up this website, I found [this](https://jekyllrb.com/docs/pages/) article on the Jekyll website very helpful. This link provides a guide on how to add pages to your site, create layouts for different pages of your site, how to create a structured post, liquid template syntax, creating navigations and many more.

#### Customizing Layouts

You can style your layout(s) to any design of your choice. For this website I a few inline CSS as shown in the image below, to achieve a little aesthetics. You can also write custom style sheet and include it in the header section of your layout file.
![code snippet](/assets/static/post_images/snippet.png)







