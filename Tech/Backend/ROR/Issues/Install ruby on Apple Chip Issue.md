```bash
brew reinstall libyaml
export RUBY_CONFIGURE_OPTS=--with-libyaml-dir=$(brew --prefix libyaml)
export LDFLAGS="-L/opt/homebrew/opt/libffi/lib"
export CPPFLAGS="-I/opt/homebrew/opt/libffi/include"
export PKG_CONFIG_PATH="/opt/homebrew/opt/libffi/lib/pkgconfig"
```

https://github.com/rbenv/ruby-build/discussions/2237