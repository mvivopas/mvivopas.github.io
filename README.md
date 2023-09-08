
# Setting Up Your Personal Website with GitHub Pages and Hyde 🛠️📇

In this documentation, I will guide you through the process of creating your personal blog using GitHub Pages as your hosting platform and Hyde as a static web generator. Please note that the instructions are based on using a Mac M2, and there may be slight variations when using other devices.

## 1. Install Jekyll and Ruby

To ensure a smooth installation of Jekyll and its dependencies, it's recommended to use a version of Ruby managed by a package manager like Homebrew or rbenv. This approach also helps prevent permission issues. Follow these steps:

### Install rbenv and Ruby (Using HomeBrew)

```bash
brew install rbenv ruby-build
```

### Add rbenv to your shell profile (e.g., .zshrc or .bashrc)

```bash
echo 'if which rbenv > /dev/null; then eval "$(rbenv init -)"; fi' >> ~/.zshrc  # For zsh
echo 'if which rbenv > /dev/null; then eval "$(rbenv init -)"; fi' >> ~/.bashrc  # For bash
```

Close and reopen your Terminal window or run `source ~/.zshrc` (or `~/.bashrc`) to apply the changes.

### Install Ruby (Latest stable version is 3.2.2)

```bash
rbenv install 3.2.2
rbenv global 3.2.2
```

Verify that Ruby is correctly installed by running `ruby -v`.

### Install Jekyll and its dependencies

```bash
gem install jekyll bundler
gem install jekyll-gist jekyll-sitemap jekyll-seo-tag
```

Verify the installation by running `jekyll -v`.

## 2. Fork the Hyde Repository and Rename

Fork the [Hyde](https://github.com/poole/hyde) repository on GitHub and rename it to `<git-username>.github.io`. This naming convention informs GitHub that it's your website.

## 3. Start a Jekyll Server

Refer to this [source](https://docs.github.com/es/pages/setting-up-a-github-pages-site-with-jekyll/creating-a-github-pages-site-with-jekyll#creating-your-site) for detailed instructions.

- Clone your repository locally, navigate to its directory, and open a terminal.

```bash
# Create a Jekyll site in the current directory
jekyll new --skip-bundle .
```

- Open the generated `Gemfile` and make the following changes:
  - Comment out the line starting with `gem "jekyll"`.
  - Replace the line starting with `gem "github-pages"` with `gem "github-pages", "~> GITHUB-PAGES-VERSION", group: :jekyll_plugins`, where you replace `GITHUB-PAGES-VERSION` with the latest version (see [dependency versions](https://pages.github.com/versions/)).

- Run `bundle install` to install the necessary dependencies.

## 4. Set Up a Local Server to Preview Changes

Refer to this [source](https://docs.github.com/es/pages/setting-up-a-github-pages-site-with-jekyll/testing-your-github-pages-site-locally-with-jekyll) for detailed instructions.

Simply execute the following command to start a local server:

```bash
bundle exec jekyll serve
```

You can preview your website changes by visiting `http://localhost:4000` in your web browser.

## Usage

Once your Jekyll server is set up and the repository is generated, the website customization process involves trial and error. You can modify templates and tweak the `_config.yml` file, instantly seeing the results at `http://localhost:4000`. After making changes, push them, and your website at `<git-username>.github.io` will be updated.

### Important Note

If you encounter an issue where pages appear fine locally but lose their style elements when published to the web, it might be related to the use of `url` and `baseurl`. To address this problem, consider the following:

Change all instances of `{{ site.baseurl }}` in `head.html` and `sidebar.html` to `{{ '/' | relative_url }}` to ensure that the correct files can be located.

At this point, it's advisable to learn some basics of Jekyll, such as understanding front matter, creating layouts, and working with pages. You can start by watching this informative [YouTube series](https://www.youtube.com/watch?v=wCOInE7-E0I&list=PLr5uaPu5L7xIg2GqO7HE0Tf-BCCkAf7-I).

## License

This project is open-sourced under the [MIT license](LICENSE.md).

<3
