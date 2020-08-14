---
layout: post
title:      "POST '/login' explained "
date:       2020-08-14 16:35:19 +0000
permalink:  post_login_explained
---


I'm going to break down this code I have here line by line to better understand what the heck I am even doing.

#receive the login form, find user, log user in
 post '/login' do 
      @user = User.find_by(email: params[:email])
 		  if @user && @user.authenticate(params[:password])
 		      session[:user_id] = @user.id
 		      redirect "/users/#{@user.id}"
 		  else 
 		      flash[:error] = "Invalid, please try again."
 					redirect '/login'
 		  end 
 end

So this post method will load up right when the user clicks on the submit button to login. 

**@user = User.find_by(email: params[:email])**
This line of code tells the computer to find this specific User in the databse by the email entered. I am setting the user variable (@user) to that user object that was pulled from the database. Params is a hash that is passed in by the post request.  The params look like this: (email: "me@gmail.com", password: "password") but this line is only looking at the email key/value pair. 


**@user && @user.authenticate(params[:password])**
Once the user email is found, we then have to check the password. That is what this line of code is doing. We get the authenticate method from our Bcrypt gem and our has_secure_pasword macro in our Users model. This method takes in an argunment of the password params(shown above what it looks like). The method is calling on the @user object to bycrpt the password entered and insures that it matches the one in the database. So, if the user exists and(&&) the password matches, the next line of code happens...

**session[:user_id] = @user.id**
... the ID of the user instance is stored in a session. And then they are logged in. Here, what we do is setting a key( [:user_id] ) in the session hash to equal the id of that user. This allows the user to change to a different page on the site and not have to login again. The user id is stored in a cookie in the browser so when you send another http request, the browser sends the user id to the server, saying 'hey, this is the same person' so they dont have to login again. 
For example, if you set session[:user_id] = 1 and a sessoin does not already exist, a new record will be created in the sessions table with a random session ID( something like: 09596d46878bf6e32265fcfb5cc51266). It will be stored in a hash like this: {user_id: 1}. So the next time the user requests a page, the browser sends the same cookie to the server. And when you call session[:user_id], you will get the ID from the sessions table. 

**redirect "/users/#{@user.id}"**
This line redirects to that specific users show page. 

**flash[:error] = "Invalid, please try again."**
IF the user types in the wrong email and/or the wrong password, this line of code runs and just shows the message you type in the string. Flash is a hash, so you can set it to any key, with the value of any string. In this case I set it to the key of :error with the value of "Invalid, please try again."

**redirect '/login'**
Here the page redirects to the original login form page to login. 


