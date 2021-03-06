# Cultivate's mini-blog

Place for short posts that may not seem consequential enough to live in our [main blog](https://cultivatehq.com/posts/index.html). It is like our *today_i_learned* Slack channel, except a bit more public and a bit more permanent. It is a little bit inspired by [Hashrocket](https://hashrocket.com)'s Today I Learned [blog](https://til.hashrocket.com).

This is a standard [Jekyll site](https://jekyllrb.com) hosted on Github.

## Suggested content

Things that are interesting, but you don't want to go through a big review to get it onto the main blog. For example:

* You have just learned (or relearned), even if you think you ought to have already known it. If you didn't know, probably lots of other people didn't know it.
* You have just explained to someone; in the first instance you might encourage them to post (if they are a Cultivar)
* A common misconception that you have pointed out in a Pull Request or review.

## Running locally

Install the gems then run:

```
bundle exec jekyll serve
```

Navigate to `localhost:4000` to view the site.

## Contributing

We will generally be only accepting new posts from Cultivars. Corrections are welcome; make a PR.

Cultivars can create a new post in the `_posts` directory. Follow the pattern of existing posts in there. If you want a review, feel free to make a PR and request reviews.

We probably won't be publicising this blog until it is a bit more Cultivate themed. Cultivars are more than welcome to help with that.

The script `bin/new_post.rb` can make the beginnings of a post for ya.

## Deploying

Right now, this is hosted on Github pages. Deploying is just a matter of pushing to master 🙀. It can be found at https://bytesize.cultivatehq.com.
