This is the source for http://learningwitherrors.org/.
This blog uses [Jekyll](http://jekyllrb.com/), a static generator, and is
served by Github pages.

## Dependencies

First, install Ruby, then install bundler:

    gem install bundler

Then, in this repo's directory (containing Gemfile),
install Jekyll/etc by:

    bundle install

## Running Locally

To start a local webserver for the blog, use:

    bundle exec jekyll serve


## Adding a new post

The contents of the \_posts/ directory are served as posts.
Modifying the HTML/Markdown in these files will immediately be reflected on the
website.

1. If using LaTeX, compile to HTML using
[LaTeX3HTML](https://github.com/lwe-blog/latex3html) **with --bodyonly option**.

2. Put the generated html in a new file under <./_posts>, with filename
YYYY-MM-DD-name-of-post.html

3. Add a header block like this:

    ---
    layout: post
    title: "Title of Post"
    authorid: preetum
    ---

4. Add the line "<!--more-->" roughly after the "abstract" of your post (the
remainder will be hidden on the main blog page, with a link to full post).

For example, see this post:
<https://github.com/lwe-blog/lwe-blog.github.io/blob/master/_posts/2016-6-3-small-bias.html>

## Adding a new author
Authors are referenced by authorid.

- Add a section to <_data/authors.yml> , where the key is authorid.
Add the 'name' as you want your name to be displayed, and 'gravatar' as your
[Gravatar](http://en.gravatar.com/) hash.

- Add a new file under <./authors/>, containing whatever you want to be displayed on
  your author page (eg, a blurb/bio). For example,
  <https://raw.githubusercontent.com/lwe-blog/lwe-blog.github.io/master/authors/tselil.md>

## Thanks

This blog was forked from https://github.com/willkoehler/my_blog.
