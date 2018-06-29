I needed a more up to date version of v8

Homebrew (as of writing this) is on 5.1.

Found https://github.com/pinepain/homebrew-devtools/blob/master/Formula/v8%406.6.rb but it builds a dynamic lib & external startup ICU data.

I just copied the homebrew official 5.1 and changed the name :)

Useful links
====================================
- Get SHA for new formula
  - https://stackoverflow.com/a/32753064/355753
  - `brew fetch ./v8-6.9.251.rb --build-from-source`
