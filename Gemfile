source "https://rubygems.org"

# For local previews. GitHub Pages' branch deploy ignores this file and
# builds with its own pinned Jekyll; the templates here work with both.
gem "jekyll", "~> 4.4"

group :jekyll_plugins do
  gem "jekyll-feed"
  gem "jekyll-seo-tag"
  gem "jekyll-sitemap"
end

# Windows helpers
platforms :windows, :jruby do
  gem "tzinfo", ">= 1", "< 3"
  gem "tzinfo-data"
end
gem "wdm", "~> 0.2", platforms: :windows

# Needed on Ruby 3+
gem "webrick", "~> 1.8"
