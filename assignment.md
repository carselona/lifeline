## Introduction

- Your job is to integrate LifeLine, a straightforward single-user blogging system, into Node. js
- The purpose here is not only to check if you can do an app, but how you do it, so you don't have to finish every subtask. Just do what you have the time and ability to do. As a result, it is important that you create the code as though it were a sizable production programme, complete with code structure, validations, git usage, documentation, tests, linting, etc.
- We are aware that some of the smaller tasks can take a lot of time. If you simply don't have the time, you can submit a brief written explanation of the solution you would propose.
- On the other hand, if you have the time, you can improve on a lot of things. For instance, you are allowed to implement user registration even though we don't require you to.
- Please feel free to use this exercise and its instructions for review by other businesses.

## Technologies

### Node.js

- we prefer Node.JS, Sequelize, Jest and of course TypeScript

## The Exercise

- design the API yourself and document it
  - ideally, create REST API
  - document the API - REST in Swagger, GraphQL with documentation comments in schema
  - write Swagger yourself or generate it, you can expose it as an endpoint (Not mandatory)
- implement login
  - the user should just need a password to login
  - seed the database with user data
- easy CRUD creation for blog posts (articles)
  - Each piece of writing needs a title, perex, and content.
  - A distinct generated id should be assigned to each article.
  - Additionally, each article needs to be timestamped.
- add the ability for readers to leave comments on articles
  - A comment must contain both content and an author (simply a string).
  - each comment should also have a timestamp
  - comments are brief and have no bearing on the content
    - Bonus points for nested comments
- the ability to vote on comments (using the + and - buttons from Reddit)
  - votes should identified by IP address and unique
- present your ability to test the code
  - You don't have to test everything, add at least some unit tests
  - Optionally also include some integration and e2e tests

# Frontend Assignment

### Common

- TypeScript is preferred
- Jest is preferred for unit tests, TestCafe or Cypress for e2e tests
- Sass for styling
- Don't worry too much about styling; you can use libraries like bootstrap or material.

### React

- React Router for routing
- Redux, hooks or Apollo Link for state management
- Axios for HTTP requests


## The Exercise

- check out the API [here](api.yml)
- check out the WS API [here](ws.json)
- implement the [wireframes](https://www.figma.com/file/9rRjxEwNmTF9DAm6WIrkQe/LifeLine---FullStack-Developer?node-id=2%3A3&t=YluyrzCrTFZWFfGf-1 "[clickable](https://www.figma.com/file/9rRjxEwNmTF9DAm6WIrkQe/LifeLine---FullStack-Developer?node-id=2%3A3&t=YluyrzCrTFZWFfGf-1")

### User Perspective

- Article List [Article List Wireframe](https://www.figma.com/file/9rRjxEwNmTF9DAm6WIrkQe/LifeLine---FullStack-Developer?node-id=2%3A3&t=YluyrzCrTFZWFfGf-4)

  - display a list of all articles, ordered by date descending
  - each article should show title, perex and publication date
  - each article should have a link to the full text

- Article View [Article View Wireframe](https://www.figma.com/file/9rRjxEwNmTF9DAm6WIrkQe/LifeLine---FullStack-Developer?node-id=2%3A75&t=YluyrzCrTFZWFfGf-4)

  - display an article
  - article should be in markdown, take care of proper rendering

- New Article View [New Article View Wireframe](https://www.figma.com/file/9rRjxEwNmTF9DAm6WIrkQe/LifeLine---FullStack-Developer?node-id=14%3A2255&t=YluyrzCrTFZWFfGf-4)

  - display a page with form to add new article
  - the form should take title, perex and content
  - the content should be in markdown, you can use some existing markdown editor
  - add necessary validations
  - this page should be protected by password

- Add Comment functionality
- display comments on Article View page
- each comment should have content, timestamp and author
- add comment form to Article View page
- comment form should take author and content

- Add Comment voting functionality
  - add the ability to vote on comments (+/-)
  - display score on each comment

### Admin Perspective

- Login Screen [Login Screen Wireframe](https://www.figma.com/file/9rRjxEwNmTF9DAm6WIrkQe/LifeLine---FullStack-Developer?node-id=14%3A2693&t=YluyrzCrTFZWFfGf-4)

  - implement login
  - after successful login redirect to next screen
  - on unsuccesful login display error message

- My Article List [My Article List Wireframe](https://www.figma.com/file/9rRjxEwNmTF9DAm6WIrkQe/LifeLine---FullStack-Developer?node-id=9%3A0&t=YluyrzCrTFZWFfGf-4)

  - display table of all articles
  - display a button to create new article
  - implement edit and delete buttons
  - optionally implement article ordering

- Article Detail View [Article Detail Wireframe](https://www.figma.com/file/9rRjxEwNmTF9DAm6WIrkQe/LifeLine---FullStack-Developer?node-id=14%3A2622&t=YluyrzCrTFZWFfGf-4)

  - display editable sections of article
  - implement publish button
  - use some existing Markdown editor, unless you really want to implement your own

## Multitenancy and accessing the API

The LifeLine Blog Engine has **multitenancy API** - multiple users can use the same application and access their own blog data separately from everyone else. In other words, your content (articles, comments, images) will be available only to you.

In order to access your own “space” in the app, you need to create a tenant first. 

**Creating a tenant**

To do so, send the following request to the API:

`POST https://localhost:3000/tenants`

The request should contain a JSON body with the following fields:

```json
{
	"name": "your-new-tenant-name",
	"password": "your-new-tenant-password"
}
```

The response will contain `apiKey` field. The API key is used to identify your tenant when using any other API endpoint - make sure to write it down!

**NOTE**: The key serves purely as an identifier and as such is not considered to be a secret. That means you can store it in your repository without worrying about security.

**Logging in**

You can now log in with your new tenant. Send the following request:

`POST https://localhost:3000/login`

With this body:

```json
{
	"username": "your-new-tenant-name",
	"password": "your-new-tenant-password"
}
```

You will also need to identify your tenant - this is done by adding a new header to the request:

```
X-API-KEY: "your-tenant-API-key"
```

If successful, you will receive a response with `access_token` field. This token expires in an hour and is used to access protected API capabilities (e.g. uploading images).

**Accessing protected resource**

Whenever you want to access a protected endpoint, you will need to supply:

- `apiKey` to identify your tenant
- `access_token` to authorize yourself

For example, if you want to upload a new image:

`POST https://localhost:3000/images`

you will need to provide the following headers:

```
X-API-KEY: "your-tenant-API-key"
Authorization: "your-access-key"
```