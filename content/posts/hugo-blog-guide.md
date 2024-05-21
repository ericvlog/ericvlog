---
title: "Hugo Blog on GitHub guide"
subtitle: "Hugo complete setup guide"
date: 2024-05-18T18:06:04+08:00
draft: false
author: "Eric"
authorLink: ""
authorEmail: "plsharevme@gmail.com"
description: "Step by step setup hugo blog system on GitHub"
keywords: "Hugo,GitHub,blog"
license: ""
comment: true
weight: 0

tags:
- blog
- hugo
- GitHub
categories:
- blog

hiddenFromHomePage: false
hiddenFromSearch: false

summary: ""
resources:
- name: featured-image
  src: featured-image.jpg
- name: featured-image-preview
  src: featured-image-preview.jpg

toc:
  enable: true
math:
  enable: false
lightgallery: false
seo:
  images: []

# See details front matter: /theme-documentation-content/#front-matter
---
# Creating a Blog with Hugo and GitHub Pages on Windows 📝💻

I created this blog using Hugo and GitHub Pages on Windows. Hugo is a static site generator that is very fast and written in Go language. GitHub Pages lets you host the static sites created using Hugo.

## Installing Hugo 🚀

First, install Hugo on Windows. I used the Chocolatey package manager to install Hugo. There are two versions available, and I installed the extended version as it is required if you plan to customize the themes you will be using.

```shell
choco install hugo-extended -y
```

After installation, check the Hugo version to confirm if it was installed successfully.

## Git Repositories and Folder Structure 📂

Now that we have installed Hugo, let's create our blog. Let's discuss the Git repositories and folder structure. I will create two Git repositories:

1. **Development repository** - This will contain all the Hugo files and the posts I create in Markdown.
2. **GitHub Pages repository** - This will contain the output of Hugo, which will be the static sites for my blog.

Since I have two Git repositories, I will have two folders for the same. First, I will create a folder `blog` where the two folders will be. Inside the `blog` folder, use Hugo to create a new site `jsjblog`. The `jsjblog` folder will be linked to the Git development repository. Then the output of Hugo, which will be static sites, will be stored in the `jsjblog_site` folder which I created in later steps.

```shell
mkdir blog
hugo new site jsjblog
```

If you go inside the `jsjblog` folder, you will find all the Hugo setup files.

```plaintext
archetypes
content
data
layouts
resources
static
themes
config.toml
```

## Hugo Theme and `config.toml` File 🎨

The `themes` folder contains all the website design details. There are many custom Hugo themes available. I have decided to use the Even theme for my blog. I will download the theme from Git instead of using `git clone` as I don’t want any other Git links in my development repository. After downloading and unzipping the theme, I pasted the contents inside the `themes/even` folder. As mentioned on the Even theme page, I copied the `config.toml` file from examples to the `jsjblog` folder and replaced the existing file. Now let's check our website using the command below:

```shell
hugo server
```

You can see the site by going to `http://localhost:1313/` as mentioned in the terminal message. Since it's an example website, the title, name, etc., need to be changed. These all can be changed in the `config.toml` file under `jsjblog`. After editing the `config.toml` file, once you run the Hugo server again, you see the updated website with your details.

We can also edit the themes as required. As mentioned on the Even theme page, we can do many customizations to our website. I did a simple color change inside `themes/even/assets/sass/_variable.scss`. Saved the change and now you see the color change on the website.

## Create New Blog Post 📝

Since there are no posts in the blog, let's add a new blog post. As mentioned on the Even theme page, we can create a new post using the command below:

```shell
hugo new post/initial_post.md
```

Note that `post` or `posts` in the command will vary based on the theme you are using.

Open `initial_post` from `jsjblog/content/post` and start writing the post. At the start of the post, there will be some metadata which Hugo populates automatically. Hugo needs this metadata for creating the static sites. `Draft` in the metadata should be set to `false` once we have completed the post and are ready to publish. If it’s true, Hugo will not show the post on the website. You can add other metadata like tags, category, author, etc. These all will depend upon the type of theme you use. After creating our first post, let's run the Hugo server again. We can see our post on the site.

## Git Repositories and Going Online 🌐

Now our development of the site with our first post is completed. Let's create the Git repository for blog development. I created a `jsjblog` Git repository without a README file. Then I linked the `jsjblog` folder to this Git repository using the commands below:

```shell
echo "# jsjblog" >> README.md
git init
git add README.md
git commit -m "first commit"
git branch -M main
git remote add origin https://github.com/jithusjacob/jsjblog.git
git push -u origin main
```

**Attention**: Use `git status` to check, then:

```shell
git add --all
git commit -m "changelog"
git push
```

Above, instead of adding only `README.md`, I actually added all the files in the `jsjblog` folder using the command below:

```shell
git add --all
```

So our development setup is saved in GitHub. As and when a new post is created, you can move the same to this GitHub repository.

Now we will create the Git repository for GitHub Pages. While creating this repository, we need to ensure the repository name has the format `username.github.io` where `username` is your Git username. Here, I created the repository without a README file. Next, we will create the Hugo output folder which will have the static sites and will be linked to this repository. We can use the `hugo` command to build the output files and they will be created inside the `jsjblog` folder under a new folder called `public`. But since I want to separate the `jsjblog` folder and the output folder so that it’s easier to maintain different Git repositories, I will use the `-d` flag to give the destination of the output files.

```shell
hugo -d ../jsjblog_site
```

I gave the output folder as `jsjblog_site` inside the `blog` folder. So now the `blog` folder will have two folders corresponding to the two GitHub repositories.

```plaintext
jsjblog
jsjblog_site
```

Let's link the `jsjblog_site` folder with the GitHub Pages repository so that we can finally see our blog online.

```shell
echo "# jithusjacob.github.io" >> README.md
git init
git add README.md
git commit -m "first commit"
git branch -M main
git remote add origin https://github.com/jithusjacob/jithusjacob.github.io.git
```

**Attention**: Use `git status` to check, then:

```shell
git add --all
git commit -m "changelog"
git push
```

This time I did not do `git add .` as I was getting some errors. I followed the above steps, linked the Git repository to the folder, then added remaining files and moved them to the Git repository.

So now my Hugo blog is ready to be accessed on [https://jithusjacob.github.io/](https://jithusjacob.github.io/)

You can also see the above steps in my YouTube video. 📹
<!--more-->


