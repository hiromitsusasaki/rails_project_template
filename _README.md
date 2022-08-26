# README

## Rails project template

### Ruby version

3.1.2

### System dependencies

Rails 7.0.3

### Setup

#### create project files

```sh
docker compose run --rm app rails new . --force --no-deps --database=postgresql --skip-bundle --skip-test
```

#### install Gems (with build containers)

```sh
$ docker compose build
```

#### setting database

##### edit configuration


`config/database.yml`

```yml
default: &default
  adapter: postgresql
  encoding: unicode
  host: db
  username: <%= ENV['POSTGRES_USER'] || 'postgres' %>
  password: <%= ENV['POSTGRES_PASSWORD'] || 'password' %>
  pool: 5

development:
  <<: *default
  database: myapp_development


test:
  <<: *default
  database: myapp_test
```

##### create database

```sh
docker compose run --rm app rails db:create
```

#### start containers

```sh
docker compose up
```

then access to http://localhost:3000

### Environment

#### Github Actions

1. add `factory_bot_rails`, `faker`, `rspec-rails`gems
2. generate configuration files for rspec

```sh
docker compose run --rm app rails g rspec:install
```

3. add `rubocop`, `rubocop-performance`, `rubocop-rails` gems
4. add `brakeman` gem

`Gemfile`
```Gemfile

group :development, :test do
  ...
 gem 'factory_bot_rails'
  gem 'faker'
  gem 'rspec-rails', '~> 4.0.0'

  ...
  gem 'rubocop', require: false
  gem 'rubocop-performance', require: false
  gem 'rubocop-rails', require: false

  gem 'brakeman'
  ...
end
```
```sh
docker compose build
```
2. mv `github` directory to `.github`
3. set slack incoming webhook url as `SLACK_WEBHOOK` secret to Github repository
4. push commits then Github Actions CI will run

#### Environment variables

1. add `dotenv-rails` gem

```Gemfile
gem 'dotenv-rails'
```
```sh
docker compose build
```
2. add variables to `.env.local`
```env
HOGE=fuga
```
3. rerun containers

```
docker compose down #if container running
docker compose up
```