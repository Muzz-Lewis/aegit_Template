# Setting up Gitbook for a project and hosting on Github Pages

![node version](https://img.shields.io/badge/node-v6.11.4-blue.svg)
![npm version](https://img.shields.io/badge/npm-v3.10.10-blue.svg)

Credits to [this](https://medium.com/@gpbl/how-to-use-gitbook-to-publish-docs-for-your-open-source-npm-packages-465dd8d5bfba) guide
> This is a documentation of setting up the basic requirements for your gitbook and hosting onto github pages, refer to [gitbook toolchain documentation](https://toolchain.gitbook.com/) for details on how to structure and write your gitbook

> *IMPORTANT* I am using node LTS (node version 6.11.4 and npm version 3.10.10), you may run into erros using current version of node.

1. Create project's README.md if you haven't, which will be the introduction/front page of your gitbook
2. Create book.json in project's root  (Gitbook settings & structure)
```
{ 
    "gitbook": "3.x.x",
    "structure": { 
      "summary": "SUMMARY.md" 
    } ,
    "root" : "./docs"
}
```
3. Create [SUMMARY.md](https://toolchain.gitbook.com/pages.html) in /docs directory
```
  # Table of content 
  * [Getting Started](getting-started.md)
  * [API Guide](api-guide.md)
```

Since in book.json the "root" is specified as the `docs` folder, you don't need to specify that the .md files are in the folder (i.e. no need for /docs/ before the file name unless you store it in another subdirectory)

4. Create and save .md files which are linked in SUMMARY.md in the /docs or its subdirectories (the book's chapters and articles)

Now you have your basic structure of your book that is ready to be published!

## Previewing the book
1. Install gitbook-cli and add to development dependency
```
npm install gitbook-cli --save-dev
```

> you can add node_modules directory to .gitignore

2. create package.json in root and add following scripts:
```
"scripts": {
   "docs:prepare": "gitbook install",
   "docs:watch": "npm run docs:prepare && gitbook serve"
}
```
3. run the watch task (npm run docs:watch)
4. book will be hosted on localhost:4000

## Publishing the book on Github Pages

- In this guide we will create an empty branch called "gh-pages" to push our gitbook which will be displayed on the project github pages (http://[username].github.io/[repo])

1. Create the gh-pages branch and empty all content

2. Go to repository settings on github and under github pages, select the source as gh-pages branch 

2. Switch to master branch and add the following script in package.json:
```
"scripts": {
  "docs:build": "npm run docs:prepare && rm -rf _book && gitbook build"
}
```
> Change rm -rf to del if using windows cmd, or appropriately for your use 

running this npm command will create _book directory which will hold the contents to be published on the github pages (_book/index.html is the static website generated by gitbook) 

> Note: you can add _book to .gitignore

3. Add an npm script to your package.json which will publish the content in _book to the gh_pages branch

This is the command I use:
```
"docs:publish": "npm run docs:build && cd _book && git init && git commit --allow-empty -m \" Update docs\" && git checkout -b gh-pages && git add . && git commit -am \" Update docs\" && git push git@github.com:<username>/<repo> gh-pages --force"
```
Which consits of these commands:
```
$ npm run docs:build
$ cd _book
$ git init
$ git commit --allow-empty -m "Update docs"
$ git checkout -b gh-pages
$ git add .
$ git commit -am "Update docs"
$ git push git@github.com:<username>/<repo> gh-pages --force
```
> IMPORTANT: to use `$ git push git@github.com:<username>/<repo> gh-pages --force` command, you need to [generate](https://help.github.com/articles/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent/)  and [add](https://help.github.com/articles/adding-a-new-ssh-key-to-your-github-account/) your SSH key to your github account

> also remember to change [username] and [repo] to your own

4. Run the docs:publish command

Your page should be live on http://[username].github.io/[repo] !

> you can also use [gitbook plugins](https://plugins.gitbook.com/) or add css by referencing to a .css file in your package.json



