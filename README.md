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


### Postgresql User ###

    sudo su - postgres
    psql postgres
    # User your username at [username]
    CREATE ROLE [username] WITH SUPERUSER LOGIN PASSWORD 'password';

    # Add to /etc/postgresql/9.1/main/pg_hba.conf
    local   all [username] peer


### Setup git repositories ###

    mkdir flipr-remote
    cd flipr-remote
    git init --bare
    cd ..

    mkdir flipr-db
    cd flipr-db
    git init
    echo 'Flipr Database Project' > README.md
    git add .; git commit -m "Initialize with README.md"

    git remote add origin ../flipr-remote
    git push -u origin master    

    git remote add upstream git@github.com:theory/agile-flipr.git
    git fetch upstream

Chapt 1 - Init, change, deploy, verify, revert and commit
=========================================================

    sqitch config --user user.name "Wes Cravens"
    sqitch config --user user.email wcravens@moxigen.com
    sqitch config --user core.pg.client `which psql`

    cd flipr-db
    # Generate sqitch.[plan,conf] files and empty deploy, refert and verify dirs
    sqitch --engine pg init flipr --uri file://../flipr-remote
    
    sqitch add appschema -n "Adds flipr app schema"
    # Modify deploy/appschema.sql,  revert/appschema.sql and verify/appschema.sql

    createdb flipr_test
    sqitch --db-name flipr_test deploy
    sqitch --db-name flipr_test verify
    sqitch --db-name flipr_test revert 
    sqitch --db-name flipr_test status
    git add .
    git commit -m "Add flipr schema"
    git push
    sqitch --db-name flipr_test deploy --verify

    sqitch config core.pg.db_name flipr_test
    sqitch config --bool deploy.verify true
    sqitch config --bool rebase.verify true 
   
    git add .; git commit -m "Add helpful defaults to sqitch config" 
    git push


Chapt 2 - Sqitch Overview
=========================

### Sqitch Philosopy ###

 * No Opinions
 * Native Scripting ( psql, sqlite3, SQL\*Plus )
 * Cross-Project dependency resolution
 * Distribution Bundling
 * Integrated verification testing
 * No numbering
 * Reliable sequential deployment ordering

### Objects ###

Stolen from git all objects get a SHA1 hash.

 * change
 * tag
 * deploy
 * revert
 * verify

### Features ###

 * Reduced duplication
 * Built-in configuration
 * Iterative development
 * Deployment planning
 * Git-style interface
 * Deployment tagging


