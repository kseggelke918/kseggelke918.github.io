---
layout: post
title:      "First Venture Into Creating an Application With Sinatra"
date:       2019-12-12 22:04:15 +0000
permalink:  first_venture_into_creating_an_application_with_sinatra
---


In what seemed like no time time at all, we came to a close on our second Flatiron section and began the second portfolio project - an MVC Sinatra Application.   I decided to do a basic Online Banking application where the user can sign up, open accounts, close accounts and make edits to their own profiles.  

Some of the more interesting pieces of code and/or thought processes I encountered:

*** Editing a User's Information**

I wanted to allow a user to edit their own login information much like you can on a real online banking website.  Let's say a user changed their name or needed to change a password, I wanted them to be able to easily update that information (even if a bank may not allow that in the real world).  

I started off by creating the get route to display my edit form: 

```
  get '/users/:id/edit' do 
    if logged_in?
      @user = User.find_by_id(params[:id])
      erb :'users/edit'
    else 
      redirect to "/login"
    end 
  end
```

I then created the edit form making sure to make the action route specific to the user:

```
<form action="/users/<%=@user.id%>" method="post">
  <input id="hidden" type="hidden" name="_method" value="patch">
	</form>
```

I also knew I wanted to be able to display the current information (for the username and customer name).   So I came up with a simple format for both:

```
    <h4><label for="change_username">Change Username:</label></h4>
    <br>
    <h5><label for="current_username">Current Username: </label><%=@user.username %> </h5>
    <h5><label for="new_username">New Username: </label><input type="text" name="new_username"></h5>
```

The password change would be a bit different, because I knew I wanted to be able to validate the current password in my controller much like what would be required for real world password changes:

```
    <h4><label for="change_password">Change Password:</label> </h4>
    <br>
    <h5><label for="current_password">Enter Current Password: </label><input type="password" name="current_password"> </h5>
    <h5><label for="new_password">Enter New Password: </label><input type="password" name="new_password"> </h5>
    <h5><label for="confirm_password">Confirm Password: </label><input type="password" name="confirm_password"> </h5>
```

So after adding a submit button, I moved onto creating the route in the users controller.  There were a lot of conditions that would need to be met in order for everything to work correctly and securely.  Like making sure that we only made changes if the fields had information entered into them:

```
@user.username = params[:new_username] if params[:new_username] != ""
```

And making sure that passwords were only changed if:
1. All the password fields were completed
2. The current password was authenticated
3. The new password fields both matched

```
if params[:current_password] != "" && params[:new_password] != "" && params[:confirm_password] != "" 
      if @user.authenticate(params[:current_password]) && params[:new_password] == params[:confirm_password]
        @user.password = params[:new_password]
      else 
        @errors = ["The current password entered is incorrect or the new passwords do not match"]
      end 
end 
```

* **Validating Username Uniqueness **

This seemed a lot more daunting than it need to.  After doing some research, I found that ActiveRecord macro `validates_uniqueness_of` so in my User model I could simply do:

```
validates_uniqueness_of :username, { message: "already taken" }
```

If this validation failed, the new user could not be saved (or existing user could not save edits) so I added a conditional statement in the user controller route.

```
if @user.save 
     session[:user_id] = @user.id 
     redirect to "/#{@user.id}/accounts"
else 
     @errors = @user.errors.full_messages
     erb :failure 
end 
```

* **System Generated Unique Account Numbers**

At a real bank, the customer doesn't get to choose their account numbers.  I wanted to include that logic in my application.  When I created accounts, I did so without account numbers.

```
@user = current_user
@account = Account.new(account_type: params[:account_type], balance: params[:balance], user_id: @user.id)
@account.save 
```

In my Account model, I created an account number using rand at initialization.  I wanted to inherit the initialize method from ActiveRecord, but I also wanted to use my own logic as well, so I needed to use the "super" keyword. 

```
def initialize(args = {}) 
    args[:account_number] ||= account_number_generator
    super 
end 

def account_number_generator
    rand(111111..999999)
end 
```

But I also wanted to ensure that all account numbers were unique so I updated the account_number_generator method to check the database for an existing account number before assigning it.

```
  def account_number_generator
    acct_num = rand(111111..999999)
    
    while Account.exists?(account_number: acct_num)
      acct_num = rand(111111..999999) 
    end 
    
    acct_num
  end 
```

I got to do some really interesting stuff with this project that really pushed the boundaries of my knowledge and even surprised me a little!  I can't wait for Rails to push it even more!








