# To publish

Make changes in the `master` branch (default), commit and push. Wait for a couple of minutes for the deployment action to finish. Please see [Making Changes](#making-changes) section below. For having a local environment see [Local setup using Docker](#local-setup-using-docker) section below.



# al-folio
This site is based on **al-folio**, a simple and clean [Jekyll](https://jekyllrb.com/) theme for academics.

[![demo](https://img.shields.io/badge/theme-demo-brightgreen.svg)](https://alshedivat.github.io/al-folio/)
[![license](https://img.shields.io/github/license/mashape/apistatus.svg?maxAge=2592000)](https://github.com/alshedivat/al-folio/blob/master/LICENSE)



[![Screenshot](assets/img/full-screenshot.png)](https://alshedivat.github.io/al-folio/)


# Getting started

For more about how to use Jekyll, check out [this tutorial](https://www.taniarascia.com/make-a-static-website-with-jekyll/).
Why Jekyll? Read this [blog post](https://karpathy.github.io/2014/07/01/switching-to-jekyll/)!

## Installation - New Site

If for some reason you want to create a new site based on this site instead of **al-folio**, please follow the instructions in the [Install.md](https://github.com/alshedivat/al-folio/blob/master/INSTALL.md) at **al-folio**. 

In summary:
1. Create a new repository forking this repository.
2. In this new repository, go to `Settings -> Actions -> General -> Workflow permissions` and give `Read and write permissions` to GitHub Actions.
3. Open file `_config.yml`, set `url` to `https://<your-github-username>.github.io` and leave `baseurl` **empty**.
4. Finally, in the repository page go to `Settings -> Pages -> Build and deployment`, make sure that `Source` is set to `Deploy from a branch` and set the branch to `gh-pages` (**NOT** to master). For more details, see [Configuring a publishing source for your GitHub Pages site](https://docs.github.com/en/pages/getting-started-with-github-pages/configuring-a-publishing-source-for-your-github-pages-site#choosing-a-publishing-source).
5. Wait until the GitHub actions finish, then simply navigate to `https://<your-github-username>.github.io` in your browser. At this point you should see a copy of this site.

## Making Changes
This section assumes you have enabled automatic deployment by following step 4 in the section above. If needed, please see [INSTALL](https://github.com/alshedivat/al-folio/blob/master/INSTALL.md#enabling-automatic-deployment) at **al-folio**.

> [!NOTE]  
> Work with the `master` branch (should be default). The actions configured to automatically run on deployment create a `gh-pages` branch. Github serves the webpage based on `gh-pages` but you don't work with it directly.

1. Make any changes to your webpage, commit, and push. This will automatically trigger the **Deploy** action.
2. Wait for a few minutes and let the action complete. You can see the progress in the **Actions** tab. If completed successfully, your repository should now have an updated `gh-pages` branch.
3. You can now see the updated website.

> [!NOTE]
> If you use `vs-code`, you should install [GitHub Actions](https://marketplace.visualstudio.com/items?itemName=GitHub.vscode-github-actions) extension for `vs-code`. Then you can see the status of the **Deploy** action from within `vs-code`.

## Local setup using Docker
Install [docker](https://docs.docker.com/get-docker/) and [docker-compose](https://docs.docker.com/compose/install/). Easiest option is to install [Docker Desktop](https://www.docker.com/products/docker-desktop/).

- Download the Docker Desktop image
- Open the downloaded .dmg file. It will ask to move Docker to applications
- Find Docker Desktop in applications (possibly using Spotlight Search - CMD + SPACE) and run it. Now Docker and Docker Compose are installed and available

Then 
```bash
$ docker compose pull
$ docker compose up
```

Note that when you run it for the first time, it will download a docker image of size 400MB or so. To see the template running, open your browser and go to [http://localhost:8888](http://localhost:8888). You should see a copy of the theme's demo website.

> To change port number, you can edit `docker-compose.yml` file.

# Features

#### Ergonomic Publications

Your publications page is generated automatically from your BibTex bibliography.
Simply edit `_bibliography/papers.bib`.
You can also add new `*.bib` files and customize the look of your publications however you like by editing `_pages/publications.md`.

Keep meta-information about your co-authors in `_data/coauthors.yml` and Jekyll will insert links to their webpages automatically.

#### Collections
This Jekyll theme implements collections to let you break up your work into categories.
The example is divided into news and projects, but easily revamp this into apps, short stories, courses, or whatever your creative work is.

> To do this, edit the collections in the `_config.yml` file, create a corresponding folder, and create a landing page for your collection, similar to `_pages/projects.md`.

Two different layouts are included: the blog layout, for a list of detailed descriptive list of entries, and the projects layout.
The projects layout overlays a descriptive hoverover on a background image.
If no image is provided, the square is auto-filled with the chosen theme color.
Thumbnail sizing is not necessary, as the grid crops images perfectly.

#### Theming
Six beautiful theme colors have been selected to choose from.
The default is purple, but quickly change it by editing `$theme-color` variable in the `_sass/variables.scss` file (line 72).
Other color variables are listed there, as well.

#### Photos
Photo formatting is made simple using rows of a 3-column system.
Make photos 1/3, 2/3, or full width.
Easily create beautiful grids within your blog posts and projects pages:

<!-- <p align="center">
  <a href="https://alshedivat.github.io/al-folio/projects/1_project/">
    <img src="assets/img/photos-screenshot.png" width="75%">
  </a>
</p> -->

#### Code Highlighting
This theme implements Jekyll's built in code syntax highlighting with Pygments.
Just use the liquid tags `{% highlight python %}` and `{% endhighlight %}` to delineate your code:

<!-- <p align="center">
  <a href="https://alshedivat.github.io/al-folio/blog/2015/code/">
    <img src="assets/img/code-screenshot.png" width="75%">
  </a>
</p> -->

## Contributing

Feel free to contribute new features and theme improvements by sending a pull request.
Style improvements and bug fixes are especially welcome.

## License

MIT
