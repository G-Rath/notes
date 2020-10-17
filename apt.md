# apt

when you do `apt install <package>` you're _manually_ install the package in
question, which means `apt` will consider it "used" when calculating what packages can be `autoremove`'d.

While typically a good thing, it's not what you want if you're installing packages in batches as part of
doing bulk system updates, as it means the packages you install won't be removed if you ever remove the
original package that required they be installed in the first place.

You can use `apt-mark` to resolve this:

http://manpages.ubuntu.com/manpages/bionic/man8/apt-mark.8.html
