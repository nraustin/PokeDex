
# Pokedex Thunk

In today's project you will configure an existing application to use thunk
actions.

## Phase 0: Getting started

You'll need the backend for the Pokedex application. Take a moment to clone it
from
<https://github.com/appacademy/practice-for-week-15-pokedex-express-backend>.
Follow the instructions in the backend repo's README to set up the backend
server. Start the backend server by running `npm start`.

The API for the backend is also documented in that repository's README.

Once you have the backend up and running, clone the frontend starter from the
`Download` link at the bottom of this page.

Run `npm start` in the frontend starter repo to start your frontend
development server.

### Explore the reference application

The current application comprises the following components:

* `App`: Does the browser routing
* `PokemonBrowser`: The browser that draws the list on the left and has a route
  to the `PokemonDetail` when the route matches "/pokemon/:pokemonId"
* `PokemonDetail`: Makes a fetch to the API on mount and update to load the
  details of the selected Pokemon
* `Fab`: The "+" button that prompts the `CreatePokemonForm` to show
* `CreatePokemonForm`: Create Pokemon form rendered on `PokemonBrowser`
* `EditPokemonForm`: Edit Pokemon form rendered on the `PokemonDetail` component
  only if the Pokemon is captured
* `PokemonItems`: Renders the list of items on the `PokemonDetail` component
* `ItemForm`: Item form rendered on the `PokemonDetail` component when
  editing an item
* `ErrorMessage`: Displays an error message for a labeled form input

Take time to review the components to see how the component tree is structured
(i.e., the parent-child relationships and where each component is being used).

### Proxy

In this project, you will run two servers using these addresses:

* `http://localhost:3000` for your frontend
* `http://localhost:5000` for your backend

In the `package.json` file on your frontend, notice the
`"proxy": "http://localhost:5000"`. This line tells the development server to
proxy any unknown requests to your backend server port. So **you must always
ensure that the `PORT` variable in your backend `.env` file has the same
port number as the proxy setting in your frontend `package.json`**. Remember:
this approach only works in development using `npm start`.

You will make api calls from your frontend to your backend server. When making
api calls to your backend, don't write out your base URL for every call.
Instead, write your fetch calls like this: `fetch('/api/pokemon')`.

## Phase 1: Dispatch thunk actions in `PokemonBrowser`

As you're connecting your application's components, you'll most likely hit bugs
and break your application. While you're connecting each component, make sure
to test that your connected code is working before moving on to connect the
next component.

There is a thunk action creator made for you already in the
`src/store/pokemon.js` file called `getPokemon`. The thunk action it returns
fetches all the Pokemon as a list from the `GET /api/pokemon` backend API
route. Then it dispatches the action returned from the `load` action creator in
the same file. The reducer normalizes the Pokemon data.

Dispatch the thunk action returned from the `getPokemon` thunk action creator
after the `PokemonBrowser` component first renders.

If done correctly, you should see the list of all the Pokemon in the side of the
browser.

## Phase 2: Create and dispatch a thunk action for `PokemonDetail`

Create a thunk action creator for fetching a single Pokemon's details based on
their `id` by hitting the `GET /api/pokemon/:id` backend API route. Using the
data returned from that, dispatch the return of the `addOnePokemon` action
creator.

Dispatch the thunk action you just created whenever the `pokemonId` in the
`PokemonDetail` component changes.

## Phase 3: `CreatePokemonForm`

Create a thunk action creator for creating a Pokemon in the `CreatePokemonForm`.
The thunk action creator should hit the `POST /api/pokemon` backend API route.
Format the fetch request to have a `Content-Type` header of `application/json`
and the correct request body using the submitted form information.

After the response comes back, add the newly created Pokemon to the Redux store
by dispatching the appropriate regular POJO action.

Dispatch the thunk action you just created on the submission of the
`CreatePokemonForm`.

## Phase 4: `EditPokemonForm`

Create a thunk action creator for editing a Pokemon in the `EditPokemonForm`.
Check out the API docs for which route to hit and how to format the URL path and
the request body in the fetch request. After the response comes back, add the
updated information Pokemon to the Redux store by dispatching the
`addOnePokemon` action.

Dispatch the thunk action you just created on the submission of the
`EditPokemonForm`.

## Phase 5: `PokemonItems`

The thunk actions in phases 5 and 6 should be created in the
`src/store/items.js` file.

Create a thunk action creator for fetching the items for a single Pokemon based
on the `id` of the Pokemon in the `PokemonItems` component. Check out the API
docs for which route to hit and how you should format the URL path for this
information. After the response comes back, use the data to dispatch the return
of the `load` action creator for items.

Dispatch the thunk action you just created when the `id` of the Pokemon changes
in the `PokemonItems` component.

## Phase 6: `ItemForm`

Create a thunk action creator for editing an item in the `ItemForm`. Check
out the API docs for which route to hit and how to format the URL path and the
request body in the fetch request. After the response comes back, use the data
to dispatch the return of the `update` action creator for items.

Dispatch the thunk action you just created on the submission of the `ItemForm`.

## Bonus Phase 1: Create and dispatch a thunk action to delete an item

Create a thunk action creator for deleting an item. As before, check out the API
docs for the route, URL path, and request body format. Upon receiving a
successful response, dispatch the return of the appropriate action creator.

Dispatch the thunk action you just created when a user clicks the `DELETE`
button next to an item in the Pokemon Detail view.

## Bonus Phase 2: Create and dispatch a thunk action to create an item

Create a thunk action creator for creating an item. Once again, check out the
API docs for the route, URL path, and request body format. Upon receiving a
successful response, dispatch the return of the appropriate action creator.

Dispatch the thunk action you just created whenever a user submits an Add Item
form. Of course, there is no "Add Item" form at the moment, but you do have a
form that already has almost everything you need to add an item: the `ItemForm`
that the app currently uses to edit an item.

Modify the `ItemForm` so that it can display either an Add Form or an Edit Form.
As you think about how to do this, consider the following:

1. How your `ItemForm` will determine whether it should display an Add Form or
   an Edit Form
2. What the form will need to display/do differently in the two cases

Attach your Add Form version to the `+` button next to the `Items` header in the
Pokemon Detail view so that it displays whenever a user clicks the `+`. You
should enable the feature only if the Pokemon in view is captured.

(The Edit Form version is already attached to the `EDIT` button that appears
next to an item of a captured Pokemon.)

## Bonus Phase 3: Provide error-checking feedback on `CreatePokemonForm`

Your app should be able to handle the spectrum of errors that it might receive:
failed validations, DB constraint violations, and internet issues--Uh oh! Did
you forget to start your backend server before sending your request?!?--to name
a few of the more common you are likely to encounter.

If a user's attempt to create a new Pokemon fails, let them know what went wrong
so they can fix it!

Examine the response from the API when the creation of a new Pokemon fails
(e.g., when you submit the form with invalid inputs like a non-unique `name`).
You will probably also want to use your browser's DevTools to inspect the
response that the frontend receives when you submit the form with invalid
inputs.

Remember, if the API endpoint returns an unsuccessful response, then the `ok`
key on the response object resolved from a `fetch` Promise will be `false`.
Here's an example of how to determine if a response from a `fetch` call is
unsuccessful:

```js
const response = await fetch('/api/pokemon', {
  method: 'post',
  headers: {
    'Content-Type': 'application/json'
  },
  body: JSON.stringify(data)
});

if (!response.ok) {
  // fetch call was unsuccessful
} else {
  // fetch call was successful
}
```

If the request to create a new Pokemon returns a `422` response, then the form
was submitted with invalid inputs.

How do you relay to the `CreatePokemonForm` that there was an error in the
`fetch` call from the thunk action that was dispatched when the form was
submitted? One way is to throw the response as an error in the thunk action when
the `fetch` call is unsuccessful.

Use the `try...catch` pattern described in [this MDN article][try-catch] to
catch the response thrown from the thunk action in the `CreatePokemonForm`
component.

Create a component state variable in the `CreatePokemonForm` to hold the errors
thrown as an error from the thunk action. The state variable should be an object
which has a key for each input field that returned an error message.

If there is an error message for an input field, render the message right under
the input field using the `ErrorMessage` component. You should make sure that
each specific error message is associated with the appropriate field. For
instance, if the `Name` field has an error message of "cannot be empty", your
form should report "Name: cannot be empty" underneath the `Name` input.

## What you've learned

In this project, you have learned how to create and dispatch multiple thunk
actions. Well done!

[try-catch]: https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Asynchronous/Async_await#adding_error_handling