---
layout: post
title:      "Find What Your Looking For"
date:       2021-03-08 16:37:14 +0000
permalink:  find_what_your_looking_for
---


There are a number of ways to locate a record in your ActiveRecord database.  Let’s look at a few and discuss times when they might be helpful:

Find. This is an easy way to find an object by ID. Pet.find(1) would return the first pet in the database if it exists.  If the object is not found then an ActiveRecord::RecordNotFound error will be raised, which is an important distinction from the find_by method. You can also look up multiple records by using Pet.find(1,2,3).

Find_by. Returns the first record found.  So, this is the best option to use if you’re looking for a unique record instead of a group of records with a similar attribute.  If you’re looking to return multiple records then the where method would be a better option.  If you want to find an object by the attribute’s contents use find_by.  You can use with but Pet.find_by(name: “Fido”) is a newer version of Pet.find_by_name(“Fido”).  Find_by returns nil if no record is found so it won’t interrupt your application the same way the find method would.  

Where is a useful search term that allows you to combine multiple conditions so that your search can be very specific.  If you wanted to query your database for all dogs named Fido you could do Pet.where(type: “Dog”, name: “Fido”). Further increasingly its flexibility .where can be combined with a scope method as well. Let’s say you had a scope method called big_dog that filters dogs that weigh more than 50 lbs. You could do Pet.big_dog.where(name: “Fido”) to only find big dogs named Fido.  What to return every pet NOT name Fido? You can used .not for that. Pet.where.not(type: “dog”). Or use or to expand the search for multiple options.   Pet.where(name: “Fido”).or(name: “Rex”) 

If you want to query the relationship between two models you must use a join statement.  To join two models. If pets belong to owners and you want to find all the dog owners, Owner.joins(:pets).where(type: “dog”) would return owners that have a dog or dogs. 

There are a few others in the Rails documentation but the methods we’ve discussed will go a long way in making queries of your database

