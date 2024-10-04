FROM ruby:3.0

WORKDIR /usr/src/app

# Copy Gemfile and Gemfile.lock to the container
COPY Gemfile Gemfile.lock ./

# Install Bundler and dependencies
RUN gem install bundler -v 2.5.9 && bundle install

# Copy the rest of the application files
COPY . .

CMD ["bundle", "exec", "jekyll", "serve", "--host", "0.0.0.0", "--trace"]
