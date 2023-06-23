---
layout: base
---

# [Tutorials](/tutorials/)

{% for post in site.posts limit:10 %}
{% if post.categories contains 'tutorials' %}
## [{{ post.title }} - {{ post.date | date_to_string }}]({{ post.url }}) 
{{ post.description }}
> {{ post.excerpt }}
{% endif %}
{% endfor %}

# [Projects](/projects/)

{% for post in site.posts limit:10 %}
{% if post.categories contains 'projects' %}
## [{{ post.title }} - {{ post.date | date_to_string }}]({{ post.url }}) 
{{ post.description }}
> {{ post.excerpt }}
{% endif %}
{% endfor %}

# [Recipes](https://recipes.tux2603.com/)

These pages aren't built using the same build system as the rest of the site, so I can't automatically pull out the most recent recipes (yet). Instead, here's a [direct link](https://recipes.tux2603.com) and some of my favorite recipes:

## [Chicken Pot Pie](https://recipes.tux2603.com/main_dishes/chickenPotPie.html)

A fairly simple chicken pot pie recipe. It's a little involved and needs you to make a roux, but if you have chicken and pie crust prepped and ready to go it's not too bad.

## [Football Sandwiches](https://recipes.tux2603.com/main_dishes/footballSandwiches.html)

Also known as funeral sandwiches, these are simple little sandwiches made with Hawaiian rolls, ham, cheese, and a mustard based sauce. These come together really fast, and are great (albeit kinda messy) finger food.

## [Spaghetti Carbonara](https://recipes.tux2603.com/main_dishes/spaghettiCarbonara.html)

Spaghetti with a creamy sauce made from eggs, Parmesan, and bacon. Very rich, very filling, and not too hard to make. You just have to watch the timing so that you don't end up with either raw or scrambled eggs by mistake.