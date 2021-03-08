---
layout: post
title:      "Getting Flashy with Sinatra-Flash"
date:       2021-03-08 16:36:13 +0000
permalink:  getting_flashy_with_sinatra-flash
---


Sinatra gives us an easy way to improve the user experience with the gem Sinatra Flash.  Flash messages can be used not only to alert the user of errors in input but also add helpful data when the user is on the right track.  For example, a message that lets the user know their newly updated profile info was successfully saved can be reassuring and can save the user from having to click through pages to verify that the operation was a success.  Let’s take a look at how the gem works.

You can access the github repo here: https://github.com/SFEley/sinatra-flash.
 
To get started:
1.	From the command line $gem install sinatra-flash
2.	In the Gemfile add: gem ‘sinatra-flash’
3.	From the command line run $bundle install
4.	In the application controller add the following;
register Sinatra::Flash
Note: there are 2 different ways to handle this depending on the way your application is set up with Sinatra.  There is a “classic” style and a “modular” style.  We are inheriting from Sinatra::Base so we used the modular style.  Some great info here: http://sinatrarb.com/extensions.html
5.	Make sure sessions is enabled.  Its as simple as putting
enable :sessions
 in your application controller.

In this example we’ll set up messages in the login controller action.   We’ll put the message just before the redirect so that it shows up on the desired view page.  It’s important to use a redirect here rather than rendering an erb file, as the erb file won’t work in the same manner.  What it does is add a key and value pair to a hash labeled flash. The key, such as :error, can be any name you want, but it makes sense to name them something logical for readability’s sake. So we’ll go with :success for a successful login and :error for an invalid login. 

    post '/login' do
        owner=Owner.find_by(:email => params[:email])
        if owner && owner.authenticate(params[:password])
            session[:owner_id]=owner.id
            flash[:success]="Successfully signed in as #{owner.name}."
            redirect to("/owners/#{owner.slug}")

        else
            flash[:error]="Invalid Login" 
            redirect to("/login")
        end
    end

We can then access that saved hash data on a view page. If the login input was invalid the controller redirects back to the login page so on the login view page :

<%if flash[:error]%>
<h3 style="color:red;"><%=flash[:error]%></h3>
<%end%>

In the case of a successful login the controller redirects to the owner’s show page so we can put the following there:

<%if flash[:success]%>
<h3 style="color:blue;"><%=flash[:success]%></h3>
<%end%>

Note: If you have a conditional message that you’d like to be displayed across all the pages you can put the flash message in the layout.erb view.  

And that’s it!  Those messages should now display on the user’s page.  The flash message only appears for one cycle of the controller action so if the user hits refresh the flash message will disappear.

If you want a better understanding of the flash hash, a good way to become more familiar with it is to put a binding.pry in the controller of the ‘get’ just before the flash message.  Putting >flash in the pry line will show you what the flash hash has stored and can be displayed on the view page.  You can test out the hash by putting several conditional statements and various messages.

Finally, the way we’ve set it up here will display a simple text alert to the user.  The only styling we used were simple colors inside the html line.  But you’re not limited to that.  For example, you can make your message stand out more by using CSS styling to have the message appear as a box or even include an image.  What you decide to do it only limited by how creative you can get with css!

