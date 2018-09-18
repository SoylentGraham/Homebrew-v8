I needed a more up to date version of v8

Homebrew (as of writing this) is on 5.1.

Found https://github.com/pinepain/homebrew-devtools/blob/master/Formula/v8%406.6.rb but it builds a dynamic lib & external startup ICU data.

I just copied the homebrew official 5.1 and changed the name :)

Useful links
====================================
- Get SHA for new formula
  - https://stackoverflow.com/a/32753064/355753
  - `brew fetch ./v8-6.9.251.rb --build-from-source`


Trying to build v8 from source
=====================================
- Install `depot_tools`...
------------------------------
- https://www.chromium.org/developers/how-tos/install-depot-tools
- leads to http://commondatastorage.googleapis.com/chrome-infra-docs/flat/depot_tools/docs/html/depot_tools_tutorial.html#_setting_up
- scrap that idea
- Install `depot_tools` tap from someones formula: https://github.com/extap/homebrew-chromium
- Fix error https://github.com/extap/homebrew-chromium/issues/1 a'la https://stackoverflow.com/questions/3939651/how-to-modify-a-homebrew-formula
- run `gclient`, permission denied
- `sudo chown -R "$USER":admin /usr/local/Cellar/depot_tools`
- `chmod +x /usr/local/bin/gclient`
- https://github.com/v8/v8/wiki/Building-from-Source
- `gclient`
- `fetch v8`
- `chmod +x /usr/local/bin/fetch`
- `fetch v8`
- Follow rest of the page
- `chmod +x /usr/local/bin/ninja`
- `ninja -C out.gn/x64.release`
- This builds `libv8_libbase.a` and `libv8_libplatform.a`, but missing a few symbols...
- Modified `v8/gni/v8.gni`
- Set `v8_static_library = true`
- This builds a 1.3gb libv8_base.a (!) and all the others, but this fails to link with many missing symbols
- Trying `is_component_build=true` in `out.gn/x64.release/gn.args`


Trying to build for ARM/pi (as of 7.1.0!)
=====================================
- as desktop `tools/dev/v8gen.py arm.release`
- `ninja -C out.gn/arm.release`
- `ninja: error: '../../src/base/bits.cc', needed by 'obj/v8_libbase/bits.o', missing and no known rule to make it` because there's no compiler setup.
- In `out.gn/arm.release/args.gn` change `target_cpu = "arm"`
- https://desertbot.io/blog/how-to-cross-compile-for-raspberry-pi
- Let's try and compile from a vm... `docker run -it -v ~/raspberry/hello:/build mitchallen/pi-cross-compile`
