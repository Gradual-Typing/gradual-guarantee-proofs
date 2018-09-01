FROM base/archlinux

RUN pacman --quiet --noconfirm -Syu
RUN pacman --quiet --noconfirm -S base-devel
RUN pacman --quiet --noconfirm -S git
RUN pacman --quiet --noconfirm -S sudo
RUN pacman --quiet --noconfirm -S npm
RUN pacman --quiet --noconfirm -S time
RUN pacman --quiet --noconfirm -S wget
RUN npm install -g jake

# Swamy's SafeTypeScript
RUN git clone https://github.com/nikswamy/SafeTypeScript.git ~/SafeTypeScript
RUN cd ~/SafeTypeScript && ls && jake local
RUN cd ~/SafeTypeScript && npm install
RUN cd ~/SafeTypeScript && make safe

#' HiggsCheck
RUN pacman --quiet --noconfirm -S curl
RUN mkdir ~/dlang
RUN cd ~/dlang && wget https://github.com/dlang/dmd/archive/v2.077.1.tar.gz
RUN cd ~/dlang && tar -xzf v2.077.1.tar.gz && rm v2.077.1.tar.gz && mv dmd-2.077.1 dmd
RUN cd ~/dlang && wget https://github.com/dlang/phobos/archive/v2.077.1.tar.gz
RUN cd ~/dlang && tar -xzf v2.077.1.tar.gz && rm v2.077.1.tar.gz && mv phobos-2.077.1 phobos
RUN cd ~/dlang && wget https://github.com/dlang/druntime/archive/v2.077.1.tar.gz
RUN cd ~/dlang && tar -xzf v2.077.1.tar.gz && rm v2.077.1.tar.gz && mv druntime-2.077.1 druntime
RUN cd ~/dlang && wget https://github.com/dlang/tools/archive/v2.077.1.tar.gz
RUN cd ~/dlang && tar -xzf v2.077.1.tar.gz && rm v2.077.1.tar.gz && mv tools-2.077.1 tools

RUN cd ~/dlang/dmd && make -f posix.mak -j8 AUTO_BOOTSTRAP=1
RUN cd ~/dlang/phobos && make -f posix.mak -j8

RUN cd ~ && wget https://plg.uwaterloo.ca/~dynjs/higgscheck/src/higgscheck-74f54e3d96ad-2017-07-27.tar.gz
RUN tar -xzf ~/higgscheck-74f54e3d96ad-2017-07-27.tar.gz
RUN pacman --quiet --noconfirm -S python2
RUN mkdir ~/bin
RUN ln -s /usr/bin/python2 ~/bin/python
RUN ln -s /usr/bin/python2-config ~/bin/python-config
RUN ln -s ~/dlang/dmd/generated/linux/release/64/dmd ~/bin/dmd
RUN export PATH=~/bin:$PATH && cd higgscheck-74f54e3d96ad-2017-07-27/source && make release

# CMD ["node", "/app/SafeTypeScript/built/local/tsc.safe.js", "/app/SafeTypeScript/samples/SafeTypeScript/richards-typed.ts"]
