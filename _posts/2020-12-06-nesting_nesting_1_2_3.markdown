---
layout: post
title:      "Nesting, nesting, 1 2 3..."
date:       2020-12-06 23:50:54 -0500
permalink:  nesting_nesting_1_2_3
---


*Nested Forms vs. Nested Routes vs. Nested Resources: A Quick Simple Cheat Sheet*

I'm writing the basic breakdown reminders of the differences, similarities, and connections between nested routes, forms, and resources that I wish I had a week ago when I started my third project for the Flatiron Software Engineering program. It's easy to get confused by terms when starting out, especially when you're dealing with similar terms for similar/related concepts, and I went halfway through my project thinking I'd fulfilled the requirement for a nested route when in fact what I'd delivered was a nested form.

(If a time traveler finds this, please deliver the following message to Cailin Kless on November 29th, 2020:)


**1.) NESTED FORMS**

**In simplest possible terms,** a nested form is one that creates (or updates) not just one object but also a second object with a built in relationship to the first.

**Example:** In my project, my nested form created a theater production that also needed to `belongs_to` a production company— either selected from a list of existing theater companies, or a new one created on the spot. The fact that I would sometimes need to create a new company in the process of creating a production necessitated a nested form.

**How to make it work:** A simple way to do it is using ActiveRecord’s `accepts_nested_attributes_for` and Ruby on Rails’ form_for and fields_for. 

In my case, I was nesting a form for a company into the form for a theater production, so in my model for theater productions I included:

`accepts_nested_attributes_for :companies`

Then, in my productions _form, I included the following: 

```
<%= f.fields_for :company, @production.build_company do |cf| %>
		<%= cf.text_field :name %>
<% end %>
```

The `@production.build_company` establishes a relationship between the new company and this form’s `@production`, right from the very start.

I also made sure to account for the nested attributes in my ProductionsController’s production_params, like so:

```
def production_params
		params.require(:production).permit(
				:title,
				:opening, 
				:closing,
				:company_id,
				company_attributes: [
						:name
				]
		)
end
```

Back in my production model, I added this bit of code so that if someone puts an already-existing company into my nested new company form, I won’t end up with a duplicate in the database— the production will just be assigned to the previously existing company:

```
def company_attributes=(company_attributes)
				@company = Company.find_or_create_by(company_attributes)
# this condition guards against a blank company name
				unless @company.name == ""
					self.company = @company 
				end
end
```

**Relation to Nested Routes / Resources:** A nested form does not require nested routes or resources. My original draft of my project utilized a nested form but no nested routes or resources. 

**2.) NESTED ROUTES**

**In simplest possible terms,** a nested route is for when you want to be able to track a relationship between objects in the path you’re using to access them.

**Example:** My theater production app has a feature where users can browse all published productions and all theater companies that have been added to the database. I can look at each project on its own terms (i.e. “productions/1”), but sometimes, like if a user is looking at a production specifically because they found it when looking at a specific company’s production listing page, I want to be able to keep that relationship between a company and its production explicit (i.e. “companies/5/productions/1”). I also want to be able to create a new production that knows its relationship to a specific company right from the beginning (i.e. “companies/5/productions/new” will bring me to a production form that is already set to belong_to company 5 automatically).

**How to make it work:** The simplest way to create a nested route is via a nested resource. In my config/routes.rb, I used the following nested resource:

```
  resources :companies do 
    resources :productions
  end
```

This gave me access to the types of paths mentioned above. *Note: I still needed a separate* `resources :productions` *for when I was dealing with productions on their own and not focusing on their relationship to a company.*

**Relation to Nested Forms / Resources:** Just as a nested form does not require a nested route as mentioned above, a nested route does not require a nested form. However, if the relationship between your models is important enough that you want a nested route for them, it will likely come in handy to have a form that deals with both at the same time— a nested form. 

The relation to nested resources should be clear from the example above— nested resources are a tool to create nested routes.

**3.) NESTED RESOURCES**

**In simplest possible terms,** nested resources are a simple way of creating nested routes for CRUD functions.

**Example:** See above.

**How to make it work:** See above.

**Relation to Nested Routes / Forms:** The nested resource is a method for achieving the nested route. As mentioned above, a nested route / resource may imply that a nested form would be desirable somewhere in your program, but they are NOT the same and they do NOT require one another.




