In order to run vagrant-vbguest must be installed this is due to the virtualbox guests on the ubuntu cloud images being incompatible
vagrant plugin install vagrant-vbguest --plugin-version 0.10.0.pre1 --plugin-prerelease --plugin-source https://rubygems.org

latest virtualbox is currently broken download this test version:
http://www.virtualbox.org/download/testcase/VirtualBox-4.3.3-90468-Win.exe
https://www.virtualbox.org/ticket/12182#comment:9
