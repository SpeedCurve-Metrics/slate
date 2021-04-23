# SpeedCurve API docs

We use the Slate framework for exporting API docs
https://github.com/slatedocs/slate

## Installation
https://github.com/slatedocs/slate/wiki/Using-Slate-Natively

### Mac

On Mac it's best to install Ruby via Homebrew
```bash
brew install ruby
echo 'export PATH="/usr/local/opt/ruby/bin:$PATH"' >> ~/.bash_profile
source ~/.bash_profile
```
Install Middleman
```bash
bundle install
```

## Run Middleman
Only need to do this if you want it to update in the background while you edit
```bash
bundle exec middleman server
```

## Publish updated docs

* Edit any content in `source/index.md`
* Publish static version of docs  
  ```bash
  bundle exec middleman build --clean
  ```
* Copy to API repo  
  Copy any changed files from the "build" dir into the `public_html` dir in the `speedcurve-api` repo
