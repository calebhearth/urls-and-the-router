<style>
  code { text-transform: none !important; }
</style>

# URLs & the Router
## Caleb Thompson

* @thompson\_caleb
* thoughtbot

---

## Let's (quickly) go over what URLs are.
* What you type into your browser to get to Google.
* Addresses for things on the Internet

---

## URLs are made up of several parts
Here is the URL for a typical Google search
<span style="white-space: nowrap">`http://www.google.com/search?q=kittens+and+puppies`</span>

---

![](http://1.bp.blogspot.com/-AJvXsfYgMME/T384eUBl8iI/AAAAAAAAAEI/FSrDFoPnm1Q/s1600/Kittens+&+Puppies+11_05_ccnan.jpg)

---

## scheme

<span style="white-space: nowrap"><strong>`https://`</strong>`www.google.com/search?q=kittens+and+puppies`</span>

---

## subdomain

<span style="white-space: nowrap">`https://`<strong>`www.`</strong>`google.com/search?q=kittens+and+puppies`</span>

---

## domain

<span style="white-space: nowrap">`https://www.`<strong>`google.com`</strong>`/search?q=kittens+and+puppies`</span>

---

## path

<span style="white-space: nowrap">`https://www.google.com`<strong>`/search`</strong>`?q=kittens+and+puppies`</span>

---

## query

<span style="white-space: nowrap">`https://www.google.com/search`<strong>`?q=kittens+and+puppies`</strong></span>

---

## How URLs are used

* The __scheme__, __subdomain__, and __domain__ tell your browser how to find the webserver.

<span style="white-space: nowrap"><strong>`https://www.google.com`</strong>`/search?q=kittens+and+puppies`</span>

* The __path__ and __query__ are used by the webserver to know what to do next.

<span style="white-space: nowrap">`https://www.google.com`<strong>`/search?q=kittens+and+puppies`</strong></span>

---

## Rails `params`

* Rails turns the __query__ parameters into a `params` hash.

```ruby
# https://www.google.com/search?q=kittens+and+puppies
params[:q] # => "kittens and puppies"
```

---

## `localhost:3000/topics`

### How does Rails...

1. Know how to handle this URL?
2. Know to send `/topics` to the `index` action on the `TopicsController`?

---

## Enter the Router

* The router is configured in `config/routes.rb`.
* Determines which code to run when a request is received.
* Generates path helper methods that you can use to generate links in your code.

---

## What we already have

```ruby
Suggestotron::Application.routes.draw do
  resources :votes
  resources :topics
end
```

* This code was added when we ran the<br/>`rails generate` commands.

---

## Order matters!

* The router will start at the top of the file and move down.
* It will stop as soon as it find a route to match the request path (`/topics`)
* If none of the routes match, it will try to find a match in the `public` directory of your
  application.
* If there still isn't a match, you will get a routing error.

---

## `resources :topics`

* Tells your application to provide the common routes for doing CRUD\* with your topic model.
<br/><br/>
\* CRUD stands for Create, Read, Update, Destroy

---

## :_(
![](https://www.evernote.com/shard/s9/sh/80b6f29b-c5c2-4064-8d51-446a910fcc9b/e9447574ba7c11179a3e824920394e34/res/a72c093f-bbbe-4d04-92da-b64f7eb291e9/Action_Controller__Exception_caught-20130222-113433.jpg.jpg?resizeSmall&width=832)

Tells you that you need to add something to your application to deal with the `/bananas` path.

---

## Adds the seven RESTful requests

<small><pre><code style="font-size: 24px; line-height: 30px">
HTTP Verb  Path              action   used for
GET        /topics           index    display a list of all topics
GET        /topics/new       new      return an HTML form for creating a new topic
POST       /topics           create   create a new topic
GET        /topics/:id       show     display a specific topic
GET        /topics/:id/edit  edit     return an HTML form for editing a topic
PUT        /topics/:id       update   update a specific topic
DELETE     /topics/:id       destroy  delete a specific topic
</code></pre></small>

---

## Creates link helpers

`resources :topics` tells your application to generate paths for common `Topic` actions.

<small><pre><code>
topics\_path             /topics
topic\_path(topic)       /topics/42
new\_topic\_path          /topics/new
edit\_topic\_path(topic)  /topics/42/edit
</code></pre></small>

---

## Request methods

### Path helpers can do different things depending on the method.

* `GET` is the default method.
* `GET`, `POST`, `PUT`, `DELETE`.
* `topic_path(topic)` goes to the `topics#show` action.
* `topic_path(topic, :method => :delete)` goes to the `topics#destroy` action.

---

## `rake routes`

* Gives information on all of the paths your application knows about.

<pre><code style="font-size: 24px; line-height: 30px">
topics     GET    /topics(.:format)          topics#index
           POST   /topics(.:format)          topics#create
new_topic  GET    /topics/new(.:format)      topics#new
edit_topic GET    /topics/:id/edit(.:format) topics#edit
    topic  GET    /topics/:id(.:format)      topics#show
           PUT    /topics/:id(.:format)      topics#update
           DELETE /topics/:id(.:format)      topics#destroy
</code></pre>

---

## RailsGuides

`guides.rubyonrails.org/routing.html`

![](https://www.evernote.com/shard/s9/sh/56f6a488-6163-47c4-bfbf-6faca0a25121/09f6f788153b83442c20aa5e91fd42be/res/d58905e9-4bae-4bbf-a0ae-1ff32717b53f/Ruby_on_Rails_Guides__Rails_Routing_from_the_Outside_In-20130222-113947.jpg.jpg?resizeSmall&width=832)

---

## Go to this page

`http://localhost:3000/`

![](http://guides.rubyonrails.org/images/rails_welcome.png)
