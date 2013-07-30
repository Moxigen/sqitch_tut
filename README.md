Agile development with Sqitch and pgTap
=======================================

[http://sqitch.org](Sqitch)

[http://pgtap.org](pgTAP)

Toolchain - Built on Debian 
---------------------------

[https://github.com/Moxigen/debian-builder](Moxigen Debian Dev Image)


### Debian Packages ###

    apt-get install git git-core gitk git-gui postgresql-9.1 libpq-dev curl


### Perlbrew ###

    echo '# Perlbrew Config' >> ~/.bashrc
    echo 'export PERLBREW_ROOT=/home/perlbrew' >> ~/.bashrc
    source ~/.bashrc
    curl -L http://install.perlbrew.pl | bash
    echo 'export /home/perlbrew/etc/bashrc' >> ~/.bashrc
    source ~/.bashrc
    cd /home/perlbrew
    git init; git add . ; git commit -m "Initialize with Perlbrew"
    perlbrew install --as sqitch-perl stable

    # Grab a coffee

    cd /home/perlbrew
    git add .
    git commit -m "perlbrew install stable"
    perlbrew install-cpanm
    git add .
    git commit -m "perlbrew install-cpanm"
    perlbrew switch sqitch-perl 



### Sqitch & pg\_prove ###

    cd /home/perlbrew
    cpanm App::Sqitch
    git add .; git commit -m "cpanm App::Sqitch"
    cpanm TAP::Parser::SourceHandler::pgTAP
    git add .; git commit -m "cpanm TAP::Parser::SourceHandler::pgTAP"



