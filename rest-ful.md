REST
====

Relax. You can do it.

---

# What is REST and why do I care?

If you are building anything on HTTP, you're (probably) doing it wrong.

---

# But That's OK...

Because we all are, and we can all do better.

---

### What are we gonna cover?

  - What is REST
    + Definition
    + Clarification
  - Examples
    + A simple object collection
    + Less-obvious event modeling

---

## What is REST?

  - **REST** - REpresentation State Transfer
  - An architectural style
  - REST is not HTTP
  - HTTP exhibits REST

  > "The World Wide Web represents the largest implementation of a system conforming to the REST architectural style." ~ [wikipedia](http://en.wikipedia.org/wiki/REST)

---

## What is REST is not?

  - URIs without file extensions
    + http://example.com/index vs. http://example.com/index.html
  - SEO-friendly URIs
    + http://example.com/user-settings vs. http://example.com/User/Settings
  - Task-based URIs
    + http://example.com/GetUserData
    + http://example.com/SaveUserData
    + http://example.com/UpdateUserData
  - Much more...

---

## Maturity Model

The Richardson Maturity Model describes the three steps towards the glory of REST.

````
RESTful enlightenment
(Level 3) Hypermedia Controls
(Level 2) HTTP Verbs & Status Codes
(Level 1) Resources
The swamp of POX - Plain 'ole XML
````

---

# REST === HyperMedia

REST is supposed to mean HyperMedia.

---

# But...

... we - the developers of the internet at large - have not used the word that way.

---

# HyperMedia is Hard.

Think application state; not resources.

---

# Lets focus on RRREST

*Realized Resource* REpresentation State Transfer

---

![](http://www.qasinternational.com.au/wp-content/uploads/2013/09/pirate2.jpg)

---

# Ummm.

I made that up.

---

## What do you mean by *Realized*?

  - Realized
  - Realistic
  - Reasonably-simple
  - Rational
  - Radical
  - Ratified

By 'realized' I mean what most people acually do, or work towards as an ideal.

---

# A Rose by Another Name

## Pragmatic REST

---

## Sample

### Request

````
GET /people/kalisjoshua HTTP/1.1
<headers>
````

### Response

````
HTTP/1.1 200 OK
<headers>

{
  "name": "Joshua T Kalis",
  "socks": "colorful"
}
````

---

## Some Basics - URIs

````
<scheme>://<host>:<port>/<path>?<query>#<fragment>
````

When building RESTful HTTP APIs most work will be done within the `path` and `query` portions and don't need to be concerned with the other parts of the URL.

  - The `path` should identify a resource
    + A collection of objects
    + A single object with a unique identity
  - The `query` should filter or refine what is addressed by the `path`
    + Filtering a list of object based on a criteria
    + Returning only a subset of properties on a resource

---

## HTTP Verbs

Verb    | Action | Idempotent | Side-effects
------- | ------ | ---------- | ------------
POST    | Create | no         | yes
GET     | Read   | yes        | no
PUT     | Update | yes        | yes
DELETE  | Delete | yes        | yes
OPTION* |||
HEAD*   |||
PATCH*  |||

  - **Idempotent** - repeating a request will not continually change the system
  - **Side-effects** - changes will occur as a result of the request

\* *Left as an exercise for the brave and over-achieving.*

---

# Examples Time

---

## BAD Examples - Don't Do These

  1. `/v3/json/GetUser/1234` or `/v3/GetUser/1234.json`
    + Don't put the media type in the URI; put it in the request (accept) headers
    + Don't add verbs to the resource; use the proper HTTP verb
    + Pluralize the resource since it is a collection; 'Users' not 'User'
  2. `/MyCollectionOfThings/789/InstanceOfObject/432/NameOfGrouping/Property`
    + Don't filter properties in the URI; use a query to filter
    + Don't nest the URIs to match the data modeling; move resources closest to root as possible

---

# Do You Want To Design A Good API?

---

## Do These Things First

  1. Evaluate business process*
  2. Identify objects (Resource) that need be represented in the API
  3. Define Verbs for each Resource

\* *We are not going to cover this here.*

---

## Plan

Resource   | POST   | GET    | PUT    | DELETE
---------- | ------ | ------ | ------ | ------
/cats      | Create a new cat resource | Retrieve a list of all Cats | Update the list of all Cats | Empty the list of all Cats
/cats/{id} | error | Retrieve Cat information | Update info about a specific Cat | Remove the Cat from the list of all Cats
/dogs      | Create a new dog resource | Retrieve a list of all Dogs | Update the list of all Dogs | Empty the list of all Dogs
/dogs/{id} | error | Retrieve Dog information | Update info about a specific Dog | Remove the Dog from the list of all Dogs
/pets      | error | Retrieve a list of all Pets | error | Empty the list of all Pets
/pets/{id} | error | Retrieve Pet information| error | Remove the Pet from the list of all Pets

---

## Best Practices

  - Use pluralized URIs
  - Model Resources as close to root of API as possible (makes sense for the domain)
  - Be consistent across all Resources

---

# Why Follow Best Practices?

---

## Library (Helper) - [AngularJS](https://docs.angularjs.org/api/ngResource/service/$resource)

````javascript
angular.module('app')
  .service('API', function ($resource) {
    
    return {
        Dogs: $resoure('/dogs')
      };
  });

angular.module('app')
  .controller('MainCtrl', function (API) {
    API.Dogs
      .get(function (data) {
        // Do something with data
      });

    API.Dogs
      .save({
        // New Dog representation saved to collection
      });
  });
````

---

## Library - [Fermata](https://github.com/natevw/fermata)

````javascript
var api = fermata.json('http://example.com'),
    Dogs = api.dogs;

Dogs.get(function (err, data) {
  // data is a JavaScript object
});

Dogs.post({/* data */}, function (err, result) {
  // report success or failure
});
````

---

## Other Good Reasons

  - Be part of a community
  - Help others; outside your immediate team
  - Get help from others; outside your immediate team
  - Benefit from the mistakes of other people; stand on the shoulders of giants

---

# Look like a genius

---

## Thank You

### Joshua T Kalis

  - [twitter](//twitter.com/kalisjoshua)
  - [github](//github.com/kalisjoshua)

### Resources

  - http://en.wikipedia.org/wiki/Representational_state_transfer
  - http://blog.steveklabnik.com/posts/2011-07-03-nobody-understands-rest-or-http
