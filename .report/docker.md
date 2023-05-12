# Building a Rails Alpine Image 
This report documents my findings on building ruby on rails 
on the alpine docker image. The problems and thier resolutions 
are documented accordingly and useful sites for solutions obtained
recorded.

#### cannot load such file - Nokogiri/nokogiri
On first run of the `rails new <project>` command this error is encountered.
However in my case it was a simple case of running the following command which uninstalls then current version of `nokogiri` and installs a new one.
```bash
gem uninstall nokogiri && gem install nokogiri
```
##### Useful links to look at
- [Ryuichiro Suzuki](https://ryuichirosuzuki.com/posts/alpine-ruby-nokogiri-issue/)
- [Github: Installing Nokogiri](https://github.com/sparklemotion/nokogiri.org/blob/main/docs/tutorials/installing_nokogiri.md)
- [Stack-Overflow: Cannot Load such file](https://stackoverflow.com/questions/28999906/require-cannot-load-such-file-nokogiri-nokogiri-loaderror-when-running)
- [Janmeppe.com](https://www.janmeppe.com/blog/how-to-fix-incompatible-library-version-nokogiri/)

#### LoadError: ld-linux-aarch64.so.1
This issue was a bit challenging to fix as it was not clear why it was showing in the first place. 
It is important to note that when using an `alpine` based image a we do not use `glibc` but use alpine's `musl`. Now knowing that looking at the `/lib/` directory for a `lib.musl-*.so.1` file revealed that our file is called `libc.musl-aarch64.so.1`. The solution was to then symbolically link this file to the required `ld-linux-aarch64.so.1`. The following command is the solution to the issue:
```bash
apk add --no-cache libc6-compat &&
ln -s /lib/libc.musl-aarch64.so.1 /lib/ld-linux-aarch64.so.1
```

##### Useful link
- [Ryuichiro Suzuki](https://ryuichirosuzuki.com/posts/alpine-ruby-nokogiri-issue/)
- [Artificial Truth](https://dustri.org/b/error-loading-shared-library-ld-linux-x86-64so2-on-alpine-linux.html)

#### LoadError: cannot load such file -- sqlite3/sqlite3_native
I'm not quite sure why this error showed up but apparently changing the ruby `gem` file to the following line solves the problem. Run this before the following code `gem uninstall sqlite3 --all`
```Gemfile
'sqlite3', git: "https://github.com/larskanis/sqlite3-ruby", branch: "add-gemspec"
```
Then run `bundle install`
