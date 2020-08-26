---
layout: post
title:      "Params Explained "
date:       2020-08-26 00:24:13 +0000
permalink:  params_explained
---


Params is a hash containing key/value pairs of collected data. All values inside params are strings. 

I like to think of params like a school bus that just carries and drops off data around in your code from the controller to the view and vice versa. This lets us acces the data in different places in your application. 

Params will come from a GET request, the data in the form of a POST request, or from the url. 

Example of a GET request where params get passed to the controller from the url: 
my applications controller looks like this:

def show 
      @user = User.find_by_id(params[:id])
end 

and say the user typed in the url: http://example.com/user/1 

so the controller would pass in {:id => "1"} to show the method, which sets the @user instance variable to the user in the database with the id of 1. 


Example of a POST request where params will get passed to the controller from a form:
my applications controller looks like this

def create 
    @user = User.new(params[:name])
end 

and my form would look like this 

<% form_for(@user) do |f| %>
   <%= f.label :name %>
	 <%= f.text_field :name %>
<% end %>

so when the user submits the form filling out the name of "Nae", the controller will pass the hash :user => {:name => "Nae"} to the create method. So @user = User.new(params[:name]) becomes @user = User.new(name: "Nae"). 







