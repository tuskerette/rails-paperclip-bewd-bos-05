![General Assembly Logo](http://i.imgur.com/ke8USTq.png)

# Image Upload with the Paperclip Gem with S3

## Objectives

By the end of this lesson, you should be able to:

- Integrate Paperclip with S3.

### Paperclip with S3

If we want to use paperclip in production, and upload image sto our live apps, hosted on heroku, we need to do some further steps.

One of the more common solutions to store images uploaded via paperclip is Amazon's Simple Storage Service (S3). With only a few modifications, we can configure our app so that it's always storing things to Amazon instead of storing them locally.

1. If you don't have one yet, sign up for an [Amazon Web Services account](https://aws.amazon.com/).
  You do have to enter your credit card info, but it is a free service for the first year.

2. Inside AWS, create a new IAM user, and write down:

  a. the AWS access key ID and

  b. the AWS secret access key.

  You'll need this information later, and if you don't record it now, you'll have to re-generate it.

  Now create a Resource Group with admin privileges, and add the new user to it.

3. Then, create a new 'Bucket', and write down the name of that bucket.

4. Add the following gems to your Gemfile and run `bundle install`.

  ```ruby
  gem 'aws-sdk'
  gem 'dotenv-rails'
  ```
5. Create an empty `.env` file in the root of your Rails app, and add `.env` to your `.gitignore`.

  > If you don't have a `.gitignore`, **you need to make one**. The `.gitignore` file is all that keeps us from commiting sensitive information to your repo for all time.

6. Add the following things to your new `.env` file.

  ```bash
  S3_BUCKET_NAME=...
  AWS_ACCESS_KEY_ID=...
  AWS_SECRET_ACCESS_KEY=...
  ```

  Plug in the values that you got when setting up AWS, and delete them from wherever else you've written them down.

7. Add the following code to just before the final `end` in both the `config/environments/production.rb` and `config/environments/development.rb` files.

  ```ruby
  config.paperclip_defaults = {
    :storage => :s3,
    s3_credentials: {
      bucket: ENV['S3_BUCKET_NAME'],
      access_key_id: ENV['AWS_ACCESS_KEY_ID'],
      secret_access_key: ENV['AWS_SECRET_ACCESS_KEY']
    }
  }
  ```

  If you have set up your `.env` file correctly, and have installed the `dotenv-rails` gem, these references should point to the variables in `.env`, _even as they remain invisible to Git_.

#### Your Turn :: Paperclip with S3

In pairs, go back to your Movie app and switch it over from local storage to using S3.

## Additional References
- [Heroku's Paperclip + S3 Walkthrough](https://devcenter.heroku.com/articles/paperclip-s3)
- [Adding Environmental Variables to Heroku (For When You Deploy)](https://devcenter.heroku.com/articles/config-vars)
- [RubyDocs Page on using S3 with Paperclip](http://www.rubydoc.info/gems/paperclip/Paperclip/Storage/S3)
