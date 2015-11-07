![General Assembly Logo](http://i.imgur.com/ke8USTq.png)

# Image Upload with the Paperclip Gem

## Objectives

By the end of this lesson, you should be able to:

- Use Paperclip to implement image upload on localhost.

## Paperclip

Paperclip is a gem that was created by a dev shop called [thoughtbot](https://thoughtbot.com/).
It is a tool to simplify the process of integrating image upload (a common feature for web applications) into Rails. Although Paperclip can be used in conjunction with 3rd party storage systems like Amazon S3, it can also be used simply to upload images to an app on localhost.

Together, we're going to walk through the steps of how you take an ordinary Rails application and incorporate image upload.

### Paperclip with Local Storage

Like many things in Rails, incorporating Paperclip into a project involves following a recipe.
In this example, we'll be building a simple CRUD app for records, and adding image upload to add a cover picture to our `Records` collection.

1. First, install ImageMagick from HomeBrew - Paperclip uses ImageMagick to process the images it receives.

  ```bash
    brew install ImageMagick
  ```

2. Make your new Rails app. We are going to use the scaffold functionality to make some of the first steps easier and focus on how to implement the Paperclip gem.

  a. `rails new records_app`

  b. `cd records_app`

  c. `rails g scaffold record artist title`

  d. `bundle install`

  e. copy the content of the seed file from this repo to your `seed.rb` file and run `rake db:seed` to populate your db.

  f. `rake db:migrate`

  g. test the app with `rails s`

3. Once you've made your Rails app and have confirmed that it's working correctly, you can move onto the next step - adding attachments to models.

  a. Add the `paperclip` gem to your Gemfile. Afterwards, run `bundle install`. Instead of using a specific version, we want to make surewe get the latest, so we get master from the main paperclip repository:

    ```ruby
    gem "paperclip", :git => "git://github.com/thoughtbot/paperclip.git"
    ```

  b. Edit the model that you want to add the attachment to, in our case `app/models/record.rb` as follows:

    ```ruby
    has_attached_file :cover,
                :styles => { :medium => "300x300>", :thumb => "100x100>" },
                :default_url => "/images/:style/missing.png"
    validates_attachment_content_type :cover, :content_type => /\Aimage\/.*\Z/
    ```

  c. Create a new migration `rails g paperclip record cover`.
  Run `rake db:migrate` to run your migration.

  d. Go to your `views` folder, and open the partial `_form.html.erb` that renders in the `edit.html.erb` and `new.html.erb` files.
  Temporairily comment out the current form and replace it with this code, to add the image uploader. Make sure you have `:html => { :multipart => true }`:

    ```erb
    <%= form_for @record, :html => { :multipart => true } do |form| %>
      <%= form.label :title%>
      <%= form.text_field :title%>
      <%= form.label :artist%>
      <%= form.text_field :artist%>
      <%= form.label 'Upload Cover'%>
      <%= form.file_field :cover%>
      <%= form.submit %>
      <%= link_to 'Back', records_path, class: 'button' %>
    <% end %>```

  e. Edit your controller: go to the `new` and `edit` methods and delete from these actions the render JSON lines. Then, adjust your strong params function to permit the new property (in this case, `:cover`).

  f. Once all of this is done, restart Rails and try visiting your routes.
  Try to upload a cover. You can find the JPGs of the covers in the `/images` folder of this repo.

  Once you upload an image, check it out, you will find it (by default) in a nested directory within the `public` directory in your Rails app. To access these images, you can create a Rails view to show them.

  g. Go to your `show.html.erb` and add:

  ```erb
  <p>MEDIUM SIZE: <%= image_tag @record.picture(:medium) %></p>
  ```

  h. Now add a thumbnail to your `index.html.erb` view of all the records:

  ```erb
  <td><%= image_tag record.picture(:thumb) %></td>
  ```

#### Your Turn :: Paperclip

In pairs, add Paperclip to the Movie app we have been working on these past weeks - make it so that you can attach Posters to Movies.

## Additional References
- [Paperclip's GitHub Page](https://github.com/thoughtbot/paperclip)
- [RubyDocs Page on Paperclip](http://www.rubydoc.info/gems/paperclip/Paperclip)
