# After-Install-Ubuntu

## Update Sysmem

```bash
sudo apt update
```

## Install first dependencies

```bash
sudo apt install curl gdebi
```

## Install other dependencies and programs

```bash
curl -sL https://deb.nodesource.com/setup_lts.x | sudo -E bash -
curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | sudo apt-key add -

echo "deb https://dl.yarnpkg.com/debian/ stable main" | sudo tee /etc/apt/sources.list.d/yarn.list

sudo apt-get install git-core zlib1g-dev build-essential libssl-dev libreadline-dev libyaml-dev libsqlite3-dev sqlite3 libxml2-dev libxslt1-dev libcurl4-openssl-dev software-properties-common libffi-dev nodejs yarn gnupg2
```

## Install Rbenv or RVM (my favorite)
### For installing Rbenv:

```bash
cd
git clone https://github.com/rbenv/rbenv.git ~/.rbenv
echo 'export PATH="$HOME/.rbenv/bin:$PATH"' >> ~/.bashrc
echo 'eval "$(rbenv init -)"' >> ~/.bashrc
exec $SHELL

git clone https://github.com/rbenv/ruby-build.git ~/.rbenv/plugins/ruby-build
echo 'export PATH="$HOME/.rbenv/plugins/ruby-build/bin:$PATH"' >> ~/.bashrc
exec $SHELL

rbenv install 3.1.2
rbenv global 3.1.2
```

### For installing RVM:

```bash
gpg2 --keyserver hkp://keyserver.ubuntu.com --recv-keys 409B6B1796C275462A1703113804BB82D39DC0E3 7D2BAF1CF37B13E2069D6956105BD0E739499BDB

\curl -sSL https://get.rvm.io -o rvm.sh

cat rvm.sh | bash -s stable --rails

source ~/.rvm/scripts/rvm

rvm install 3.1.2
```

## Ruby is installed

```bash
ruby -v
```

## Install bundler

```bash
gem install bundler
```

## Configure Git
We'll be using Git for our version control system so we're going to set it up to match our Github account. If you don't already have a Github account, make sure to [register](https://github.com/). It will come in handy for the future.

Replace my name and email address in the following steps with the ones you used for your Github account.

```bash
git config --global color.ui true
git config --global user.name "YOUR NAME"
git config --global user.email "YOUR@EMAIL.com"
ssh-keygen -t rsa -b 4096 -C "YOUR@EMAIL.com"
```

The next step is to take the newly generated SSH key and add it to your Github account. You want to copy and paste the output of the following command and [paste it here](https://github.com/settings/ssh).

```bash
cat ~/.ssh/id_rsa.pub
```
Once you've done this, you can check and see if it worked:

```bash
ssh -T git@github.com
```
You should get a message like this:

```bash
Hi excid3! You've successfully authenticated, but GitHub does not provide shell access.
```

## Installing Rails

```bash
gem install rails -v 7.0.2.4
```

If you're using rbenv, you'll need to run the following command to make the rails executable available:

```bash
rbenv rehash
```

Now that you've installed Rails, you can run the rails -v command to make sure you have everything installed correctly:

```
rails -v
# Rails 7.0.2.4
```

If you get a different result for some reason, it means your environment may not be setup properly.

## Setting Up A Database
Rails ships with sqlite3 as the default database. Chances are you won't want to use it because it's stored as a simple file on disk. You'll probably want something more robust like MySQL or PostgreSQL.

There is a lot of documentation on both, so you can just pick one that seems like you'll be more comfortable with.

If you're new to Ruby on Rails or databases in general, I strongly recommend [setting up PostgreSQL](https://gorails.com/setup/ubuntu/20.04#postgresql).

If you're coming from PHP, you may already be familiar with MySQL.

### Choose MySQL or PostgreSQL

#### Setting Up MySQL

Rails ships with sqlite3 as the default database. Chances are you won't want to use it because it's stored as a simple file on disk. You'll probably want something more robust like MySQL or PostgreSQL.

There is a lot of documentation on both, so you can just pick one that seems like you'll be more comfortable with.

If you're new to Ruby on Rails or databases in general, I strongly recommend [setting up PostgreSQL](https://gorails.com/setup/ubuntu/20.04#postgresql).

If you're coming from PHP, you may already be familiar with MySQL.

You can install MySQL server and client from the packages in the Ubuntu repository. As part of the installation process, you'll set the password for the root user. This information will go into your Rails app's `database.yml` file in the future.

```bash
sudo apt-get install mysql-server mysql-client libmysqlclient-dev
```

Installing the `libmysqlclient-dev` gives you the necessary files to compile the `mysql2` gem which is what Rails will use to connect to MySQL when you setup your Rails app.

hen you're finished, you can [skip to the Final Steps](https://gorails.com/setup/ubuntu/20.04#final-steps).

### Setting Up PostgreSQL

For PostgreSQL, we're going to add a new repository to easily install a recent version of Postgres.

```bash
sudo apt install postgresql-11 libpq-dev
```

The postgres installation doesn't setup a user for you, so you'll need to follow these steps to create a user with permission to create databases. Feel free to replace `chris` with your username.

```bash
sudo -u postgres createuser chris -s

# If you would like to set a password for the user, you can do the following
sudo -u postgres psql
postgres=# \password chris
```

## Final Steps

And now for the moment of truth. Let's create your first Rails application:

```bash
#### If you want to use SQLite (not recommended)
rails new myapp

#### If you want to use MySQL
rails new myapp -d mysql

#### If you want to use Postgres
# Note that this will expect a postgres user with the same username
# as your app, you may need to edit config/database.yml to match the
# user you created earlier
rails new myapp -d postgresql

# Move into the application directory
cd myapp

# If you setup MySQL or Postgres with a username/password, modify the
# config/database.yml file to contain the username/password that you specified

# Create the database
rake db:create

rails server
```

You can now visit http://localhost:3000 to view your new website!

Now that you've got your machine setup, it's time to start building some Rails applications.

If you received an error that said Access denied for user 'root'@'localhost' (using password: NO) then you need to update your config/database.yml file to match the database username and password.
