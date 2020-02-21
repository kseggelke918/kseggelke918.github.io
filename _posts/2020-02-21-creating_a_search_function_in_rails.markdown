---
layout: post
title:      "Creating a Search Function in Rails"
date:       2020-02-21 18:47:51 +0000
permalink:  creating_a_search_function_in_rails
---


What's a website without a search function?  No one wants to read down an entire list of items to find the exact one they're looking for.  So let's talk about how to create a search bar in rails. 

In this example, we'll be setting up a search on an index page.  We'll need to work in all parts of the MVC but let's start in the models - where the logic for the search will be held.

Create a class method (I'll call mine "search") that takes in 1 argument - what the user types into the search.  What shows on the index page is conditioned on whether or not the user has actually searched anything.  If the search box is blank, the page should show everything and if something has been entered in the search box, we want the like items to show on the page.  So we'll need to use those handy if statements.  We'll also need to use a SQL statement for the search.  To grab items that are close to what's typed in (for example - so the search isn't case sensitive) we want to use the LIKE SQL operator.  Putting this logic together, we end up with the method below.

```
def self.search(search)
        if search 
            searched_item = Item.where('name LIKE ?', "%#{search}%")
            if searched_item
                self.where(id: searched_item)
            else 
                @items = Item.all 
            end 
        else 
            @items = Item.all 
        end 
    end 
```

Now we have to move onto our controller.  In the index action of the controller, we need to call the search class method and assign it to an instance variable so the view can access it.

```
 def index 
        @items = Item.search(params[:search])
 end
```

Lastly, we need to set up the view.  In index.html.erb, we need to create the search bar that will communicate with the controller which we can do using a form_tag.  We need to identify the path and method in our form_tag.  For a search, we want to stay on the same page (the index page in this instance) so we use a get request to the items_path.  The only thing left is to set up the label_tag and text_field_tag so that they will send the right information to the controller.  

```
<%= form_tag(items_path, method: :get) do %>
        <%= label_tag(:search, "Search for an Item:") %>
        <%= text_field_tag :search, params[:search]%>
        <%= submit_tag ("Search")%>
<% end %>
```

And that's it!  The site now has a functioning search!
