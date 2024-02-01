This is just a note for my **personal** usage.

# Dev setup

- clone the repo
- install [hugo](https://gohugo.io/getting-started/quick-start)
- make sure themes are [installed](https://github.com/adityatelange/hugo-PaperMod/wiki/Installation)
- run hugo dev server(`hugo server -D`)
- navigate to the site

## Flow for adding a new post

`hugo new content posts/new-post.md`

## Deployment

Although site appears to be hosted on `bkjha.com`, behind the scenes, it's being hosted by the super awesome Github Pages.

It's just that Github allows you to add a custom domain for Github Pages hosted static site.


Github workflow is being used to build the final static files(html/css/js) and deploy the site.

As you make a commit on the master branch, the workflow(./github/workflow/gh-pages.yml) gets trigerred.


## Update the underlying theme -- papermod

```sh
git submodule update --remote --merge
```
