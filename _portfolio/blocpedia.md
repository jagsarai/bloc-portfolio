---
layout: post
title: Blocpedia
thumbnail-path: img/Blocpedia-Main.png
short-description: Blocpedia is an alternative take on Wikipedia.

---

{:.center}
![]({{site.baseurl}}/img/Blocpedia-Main.png)

## Summary

Blocpedia is and alternative take on Wikipedia. Simple and powerful, it allows users to create and collaborate on engaging and informative topics. 

## Explanation 

This project was done under the guidance of the Bloc curriculum. My role in the project was to create an alternative site that closely models Wikipedia. I was asked to create and implement all of the site given the following requirements: 
1. Log-in/Sign-up using email and password with email confirmation 
2. Ability to create, read, destroy personal wikis for standard users
3. Ability to create private wikis but without collaboration power for standard users
4. Ability to create, read, destroy all wikis for premium users
5. Ability to create private wikis for premium users. 
6. Ability to create, read, update and destroy all wikis for Admin users
7. Premium users pay for extra service
8. Ability to add users to a topic for premium users 
9. All users would be able to create and preview using markdown syntax 


## Problems/Challanges

The major challenge that I want to highlight for this project was the relationship of the user with a given wiki. I had to figure out a way to associate proper permissions for regular vs premium users so that regular users could not get access to private wikis and were not able to edit wikis that they did not own. In addition, users that were added as collaborators to a topic needed to be able to see those topics and edit them, regardless of their user role.   

## Solution

I chose to work on this project in Rails with SQLite as my database. This provided an effective solution in creating the necessary relationships between users and wikis. In addition, Rails provides an abundance of gems for us to use to get things done in short and easy manner. One of which, Pundit, made it possible to write clear and concise policies for each type of user. Therefore, unauthorized access to any kind of material was prevented.

Using Active Record I was able to specify the relationship that both users and wikis have with one another. By using the `has_many` function inside the User class, I was able to specify that a given instance of user can have many wikis. With the addition of the line `:depended => :destroy`, we can now destroy the relationship of the user with a given wiki if it has been deleted.

```Ruby
class User < ActiveRecord::Base
  has_many :wikis, :dependent => :destroy
  has_many :collaborations
  has_many :shared_wikis, through: :collaborations, source: :wikis
```
Inside of the Wiki class, I used the `has_many` and `belongs_to` functions to create the link between the instance of a wiki and the user. The code below states that a given wiki can belong to a single user, hence why the code reads `:user` and not `users`. The `has_many :users, through: collaborations` line clarifies that a given wiki can have many users that are collaborators but not the owners of the wiki. Therefore, they can perform CRUD operations on the wiki. 

```Ruby
class Wiki < ActiveRecord::Base
  belongs_to :user
  belongs_to :edited_by, :class_name => "User"
  has_many :collaborations
  has_many :users, through: :collaborations
```
Once the user and wiki relationship was established, the next hurdle was proper authorization. Pundit allowed us to create application wide policies for user actions. Therefore, users without proper access would not be allowed to perform unauthorized actions on wikis. By adding to the Scope class inside of our policies, we can define which users have access to which parts of the wiki and which actions they are allowed to have.

```Ruby  
  class Scope
    attr_reader :user, :scope

    def initialize(user, scope)
      @user = user
      @scope = scope
    end

    def resolve
      wikis = []
      if user.role == 'admin'
        wikis = scope.all
      elsif user.role == 'premium'
        all_wikis = scope.all
        all_wikis.each do |wiki|
          if !wiki.private? || wiki.user == user || wiki.collaborations.include?(user)
            wikis << wiki
          end
        end
      else
        all_wikis = scope.all
        wikis = []
        all_wikis.each do |wiki|
          if !wiki.private? || wiki.collaborations.include?(user)
            wikis << wiki
          end
        end
      end
      wikis
    end
  end
```
## Results

After running rspec and using local testing, Blocpeida works just as expected. Users are able to perform all of the required actions. With rspec, I was able to run all of the various scenarios that the user might be faced with. Here are some of the screenshots of the user interaction with the site.


![]({{site.baseurl}}/img/Blocpedia-Signin.png) 
![]({{site.baseurl}}/img/Blocpedia-Main.png) 
![]({{site.baseurl}}/img/Blocpedia-All-Wikis.png) 
![]({{site.baseurl}}/img/Blocpedia-New-Wikis.png) 
![]({{site.baseurl}}/img/Blocpedia-Edit-Wikis.png) 


## Conclusion 

The project was really engaging and taught me a lot about Rails. This was my first Rails project, so there was a steep learning curve, but I was surprised with how easy and effective gems can be in simplifying our code and letting us focus in on the core direction of our project. This like user authentication and password recovery are made simple with gems like Devise. Pundit was a great tool in aiding with user permissions and access. Active Record provided easy database relation management. Overall, this was a fun project. 


