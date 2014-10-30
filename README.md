A127 Webstie
============

This website is using [Jekyll](http://jekyllrb.com) to generate a static website
from Markdown documents.

## Development

1. Install Jekyll via [RubyGems](https://rubygems.org/)
  ```shell
  gem install jekyll
  ```

2. Clone the repository
  ```shell
  git clone git@github.com:apigee-127/website.git
  ```

3. Start the Jekyll server from within the repository folder
  ```shell
  jekyll serve
  ```

4. Edit or add files in `_pages` folder or edit SASS files for styling in `_sass`
folder

## Publishing
GitHub will render Jekyll automatically. Pushing to `gh-pages` branch is all you
need to do for publishing.


If you are interested to see what generated website looks like build the project
first
```shell
jekyll build
```
Now generated website is living in `_site` folder.

## License
MIT
