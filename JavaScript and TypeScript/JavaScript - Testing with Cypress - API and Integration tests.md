#javascript 

Cypress can be used to test the expected behaviour of API endpoints and their integration with the wider webapp. Below is a simple API example API test that  makes a request to a GET endpoint and then makes assertions about the HTTP status code and the response body:
```typescript
// cypress/tests/api/api-users.spec.ts

context("GET /users", () => {
  it("gets a list of users", () => {
    cy.request("GET", "/users").then((response) => {
      expect(response.status).to.eq(200)
      expect(response.body.results).length.to.be.greaterThan(1)
    })
  })
})
```

And here is another example that uses syntax more familiar to the way I personally have been writing my e2e tests"
```typescript
describe('TODO api testing', () => {
   let todoItem;
   it('fetches Todo items - GET', () => {
       cy.request('/todos/').as('todoRequest');
       cy.get('@todoRequest').then(todos => {
           expect(todos.status).to.eq(200);
           assert.isArray(todos.body, 'Todos Response is an array')
       });
   });
});
```
 There isn't much more to it than this. The main things to remember are the request command syntax. I will add more here as I come across it.