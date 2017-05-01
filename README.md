Carrierwave 1.1.0
Rails 5.1

Create .railsrc in your home folder.

--skip-spring

rails new cwave

gem 'carrierwave', '~> 1.0'

bundle

rails generate uploader Image

class ImageUploader < CarrierWave::Uploader::Base

  # Include RMagick or MiniMagick support:
  # include CarrierWave::RMagick
  # include CarrierWave::MiniMagick

  # Choose what kind of storage to use for this uploader:
  storage :file

  # Override the directory where uploaded files will be stored.
  # This is a sensible default for uploaders that are meant to be mounted:
  def store_dir
    "uploads/#{model.class.to_s.underscore}/#{mounted_as}/#{model.id}"
  end
end


rails g model product name image


rails db:migrate

class Product < ApplicationRecord
  mount_uploader :image, ImageUploader
end

_form.html.erb:
<%= form_with(model: product, local: true, :html => {multipart: true}) do |form| %>
  <% if product.errors.any? %>
    <div id="error_explanation">
      <h2><%= pluralize(product.errors.count, "error") %> prohibited this product from being saved:</h2>

      <ul>
      <% product.errors.full_messages.each do |message| %>
        <li><%= message %></li>
      <% end %>
      </ul>
    </div>
  <% end %>

  <div class="field">
    <%= form.label :name %>
    <%= form.text_field :name, id: :product_name %>
  </div>
  
  <div class='field'>
    <%= form.file_field :image %>
  </div>
  
  <div class="actions">
    <%= form.submit %>
  </div>
<% end %>

new

<h1>New Product</h1>

<%= render 'form', product: @product %>

<%= link_to 'Back', products_path %>

show

<p id="notice"><%= notice %></p>

<p>
  <strong>name:</strong>
  <%= @product.name %>
</p>

<%= image_tag @product.image_url %>


index

<p id="notice"><%= notice %></p>

<h1>Products</h1>

<table>
  <thead>
    <tr>
      <th>name</th>
      <th colspan="3"></th>
    </tr>
  </thead>

  <tbody>
    <% @products.each do |product| %>
      <tr>
        <td><%= product.name %></td>
      </tr>
    <% end %>
  </tbody>
</table>


<br>

<%= link_to 'New Product', new_product_path %>






