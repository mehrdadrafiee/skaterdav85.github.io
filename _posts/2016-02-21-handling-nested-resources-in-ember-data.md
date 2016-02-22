---
layout: post
title: Handling Nested Resources in Ember Data
date: 2016-02-21
description: Many APIs use nested resources. That is, URL paths that contain a hierarchy of resource types. How do you handle that in Ember Data? Let me show you.
keywords: nested resources, ember data
---

Many APIs use nested resources. That is, URL paths that contain a hierarchy of resource types. For example, a nested resource might look something like: `/users/5/pets`, where there is a collection of `pet` resources under a `user` resource. How do you handle that in Ember Data?

Ember Data supports a property called `links` on individual resources, an object which contains URLs that point to related data. For example, let's say we have a `user` model with asynchronous `belongsTo` and `hasMany` relationships:

```js
// app/models/user.js
export default DS.Model.extend({
  first: DS.attr('string'),
  last: DS.attr('string'),
  pets: DS.hasMany('pet', { async: true }),
  company: DS.belongsTo('company', { async: true })
});
```

If we made a request to `/api/v1/users`, each `user` object in the response can have a `links` property:

```js
{
  "users": [
    {
      "id": 1,
      "first": "David",
      "last": "Tang",
      "links": {
        "company": "/api/v1/users/1/company",
        "pets": "/api/v1/users/1/pets"
      }
    }
    // ...
  ]
}
```

When you access `user.pets` or `user.company`, Ember Data will trigger a fetch using these URLs defined in `links`. Note, JSON-API uses `links` too, but the response format is a little different. The example of `links` in this post is applicable if you are using either the `RESTSerializer` or `JSONSerializer`. Learn more about [the differences between the built-in serializers in Ember Data](/2015/12/05/which-ember-data-serializer-should-i-use.html).

As noted in the API documentation:

> The format of your links value will influence the final request URL via the urlPrefix method: Links beginning with //, http://, https://, will be used as is, with no further manipulation. Links beginning with a single / will have the current adapter's host value prepended to it. Links with no beginning / will have a parentURL prepended to it, via the current adapter's buildURL.

What if your API doesn't return a `links` property, and this is how your related data needs to be accessed? I have found this to be a pretty common scenario.

To handle this, you can override one of the normalization methods in the serializer like `normalize`, `normalizeResponse`, `normalizeFindAllResponse`, etc, and create a `links` property for each individual resource:

For example, if you are calling `store.findAll('user')`, you can override `normalizeFindAllResponse`.

```js
// app/serializers/user.js
export default DS.RESTSerializer.extend({
  normalizeFindAllResponse(store, primaryModelClass, payload, id, requestType) {
    payload.users.map((user) => {
      user.links = {
        pets: `/api/v1/users/${user.id}/pets`,
        company: `/api/v1/users/${user.id}/company`
      };
    });

    return this._super(...arguments);
  }
});
```

Here I have created a model-specific serializer to add `links` to each `user` resource. You could probably make this a little more dynamic and use it across the board in an `application` serializer. I'll leave that to you.

Not sure how to use the normalization methods in serializers? Learn more about [the differences between normalize, normalizeResponse, and the other normalization methods here](/2016/01/23/ember-data-and-custom-apis-5-common-serializer-customizations.html).

Have you found another way of handling nested resources? Let me know in the comments!