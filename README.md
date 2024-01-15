# Blog

This blog is a place for me to rant about topics that interest me.

I'm using this site as a place to refine my thoughts and maybe share some interesting ideas. 
However, one shouldn't expect polished content from it, as I will be more or less posting rough drafts without much editing.

## Technical information

Built using [Hugo](https://gohugo.io/) and the [Archie](https://github.com/athul/archie) theme.

This site uses [MathJax](https://www.mathjax.org/) to render Latex in the browser.

Reader comments are currently not supported, but I have plans to do this at some point.

## Common Hugo Commands

### Building / Running Site
To build the static site `$ hugo`

To run a live-reloading server `$ hugo server`

To run a live-reloading server and build drafts `$ hugo server --buildDrafts`

### Posts
Add a new post `$ hugo new content posts/<name of article>.md`

Edit file `$ $EDITOR /content/posts/<name of article>.md`

## A few additional remarks about post creation
If you want to include a site-wide image, throw the image in `/images/` and put
`![alt](/blog/images/<name of new image file>.<ext>)`

If the image is just for the article, create a directory in `/content/posts` 
with the same as the markdown file generated with the post-adding command, 
rename the markdown file to `index.md`, move it into the created directory, 
within the new directory create a directory `images/` on the same level as `index.md` 
and put your article-specific images in there. 
Finally, put the markdown `![alt][images/<name of new image file>.<ext>]` in the article
and everything should just work.
