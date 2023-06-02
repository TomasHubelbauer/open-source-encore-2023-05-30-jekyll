# Open Source Encore: Jekyll documentation improvements for Include/Exclude

While working on this very website, I have come to learn more about Jekyll and
Liquid as a result of setting up GitHub Pages (the mechanism through which the
site is hosted) and using Liquid to generate the landing page with the list of
contents that are available on the site.

I haven't quite learn enough, though, in a sense, because while I did learn that
GitHub Pages ships with an outdated version of Jekyll and Liquid (see the page
<https://pages.github.com/versions> for information - both are one major version
behind), I wasn't sure whether the `exclude` option I had added to `_config.yml`
to tell Jekyll to include `index.html` (and thus "nested" GitHub Pages) in the
submodules that make up the contents of the site would replace or augment the
default exclusion list in Jekyll.

I turned to the Jekyll documentation site for answers and when I couldn't get
any, to the Jekyll repository where I posted this issue:
<https://github.com/jekyll/jekyll/issues/9372>

A helpful maintainer came back and explained that in Jekyll 3, the exclusion
list is replaced whereas in Jekyll 4, it is appended to via this configuration
option.

This way a slightly unfortunate information for me to learn as I was hoping for
the Jekyll 4 behavior (which makes sense to me) to be what is going to happen on
GitHub Pages, but that uses Jekyll 3 so I will need to add the default exclusion
list to my `_config.yml` to make sure I don't inadvertedly include something in
my build I didn't mean to when I added that custom `exclude` line.

In any case, this answer I felt was worth capturing in the Jekyll documentation
so I set out to do just that.

I cloned the Jekyll repository and followed the steps outlined by the maintainer
in his comment on my issue.
Later  on I've learnt they were helping me out by quoting bits of the macOS set
up guide at https://jekyllrb.com/docs/installation/macos as I was coming across
issues this guide explains how to fix:

- `bundle install` would fail with this error message:
  > Your user account isn't allowed to install to the system RubyGems

  I knew something was going to go wrong since I was using the macOS built in
  Ruby install and seeing how well things go with the built in Git or outdated
  shell utilities on macOS, I was sure this wouldn't be much better.

  I found a suggestion on how to work around this on Stack Overflow:
  <https://stackoverflow.com/a/49719722/2715716>

- `bundle install --path vendor/bundle` worked and let me continue to the next
  step as outlined by the maintainer

- `bundle exec jekyll serve -s docs -d docs/_site --livereload` would fail with
  this error message:
  > bundler: failed to load command: jekyll (/Users/tom/Desktop/jekyll/vendor/bundle/ruby/2.6.0/bin/jekyll)
  
  I did more online research and came across another Stack Overflow post at
  <https://stackoverflow.com/a/70916831/2715716> but this did not make any
  difference.
  
- I found a Jekyll repository issue about the problems I was having:
  <https://github.com/jekyll/jekyll/issues/8784>
  
  In this issue, a Jekyll user figured the issues out and captured his solution.
  The same maintainer that was helping him asked him to contribute this to the
  Jekyll docs which he did and that's how the guide I mentioned before was born.
  Also the contributor's blog post on the same topic which predated it can be
  found here:
  <https://www.moncefbelyamani.com/how-to-install-jekyll-on-a-mac-the-easy-way/>
  From this point on I followed
  <https://jekyllrb.com/docs/installation/macos/>
  
- `brew install chruby ruby-install xz`

  `chruby` is a Ruby version manager similar in what it does to something like
  NVM.

- `ruby-install ruby 3.1.3`

  I installed a more recent Ruby version (with a major version bump compared to
  the one that ships with macOS).

- `source $(brew --prefix)/opt/chruby/share/chruby/chruby.sh`

  This brings the `chruby` command to the context of the shell so it can be used
  and called to switch Ruby versions as `ruby-install` installed the more recent
  version of Ruby but it still wasn't the default one.

- `chruby ruby-3.1.3`

  And now it was!

- `ruby -v`

  As scientifically confirmed.
  
- `bundle install`

  We're back at the beginning now but with proper setup so this command passes
  even without the hacky `vendor` switch!

- `bundle exec jekyll serve -s docs -d docs/_site --livereload`

  And off we go serving the site.
  It runs on - http://127.0.0.1:4000/ and the Configuration Options page I was
  interested in editing was at http://127.0.0.1:4000/docs/configuration/options/
  and is live on https://jekyllrb.com/docs/configuration/options.

The backing file for this page is `docs/_docs/configuration/options.md` but it
uses data from `docs/_data/config_options/global.yml` so I opened that file and
pretty much just adjusted the HTML there with my prose.

Saving the files automatically reloads the page in the browser as expected so
testing everything was fine was super easy.

After I was satistied with my changes, I opened a PR to the Jekyll project to
solve the issue I've originally raised:

<https://github.com/jekyll/jekyll/pull/9376>
