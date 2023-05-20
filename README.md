# Book Search Engine _(GraphQL API)_

This is a refactor project that took a fully functioning Google Books API search engine built with a RESTful API, and refactored it to be a GraphQL API built with Apollo Server. The app was built using the MERN stack, with a React front end, MongoDB database, and Node.js/Express.js server and API. Functionality includes users to save book searches to the back end.

![book-search-website-demo-gif](img/book-search-demo-01.gif)

##### Refactor Steps

1. Set up an Apollo Server to use GraphQL queries and mutations to fetch and modify data, replacing the existing RESTful API.

2. Modify the existing authentication middleware so that it works in the context of a GraphQL API.

3. Create an Apollo Provider so that requests can communicate with an Apollo Server.

4. Deploy the application to Heroku.

### Criteria

- [x] Load the search engine, that presents a menu with the options Search for Books, Login/Signup, input field to search for books, and a submit button.
- [x] The Search for Books menu option holds an input field to search for books and a submit button.
- [x] Not logged in users will enter a search term in the input field and click the submit button will be presented with several search results, each featuring a book’s title, author, description, image, and a link to that book on the Google Books site.
- [x] The Login/Signup menu option is a modal with a toggle between the option to log in or sign up.
- [x] The Signup toggle is a form with three inputs: username, email address, password, and a signup button.
- [x] The Signup toggle will login users after sigining up with a valid email address and password.
- [x] The Login toggle a form with two inputs: email address, password, and login button.
- [x] Once logged in, the menu options change to Search for Books, an option to see saved books, and Logout.
- [x] When logged in, enter a search term in the input field and click the submit button to be presented with several search results, each featuring a book’s title, author, description, image, and a link to that book on the Google Books site and a button to save a book to the account.
- [x] The Save button on a book will be saved to the user's account.
- [x] Clicking the option to see saved books will display all of the books saved to the account, each featuring the book’s title, author, description, image, and a link to that book on the Google Books site and a button to remove a book from the account.
- [x] The Remove button on a book will delete the book from the saved books list.
- [x] The Logout button will return display menu with the options Search for Books and Login/Signup and an input field to search for books and a submit button.

### Back-end "Specs"

- auth.js: Update the auth middleware function to work with the GraphQL API.
- server.js: Implement the Apollo Server and apply it to the Express server as middleware.

###### Schemas directory:

- **index.js**: Export your typeDefs and resolvers.
- **resolvers.js**: Define the query and mutation functionality to work with the Mongoose models. the functionality in the user-controller.js as a guide.
- **typeDefs.js**: Define the necessary Query and Mutation types: -** Query type**:
  - **me**: Which returns a User type.
- **Mutation type**:
  - **login**: Accepts an email and password as parameters; returns an Auth type.
  - **addUser**: Accepts a username, email, and password as parameters; returns an Auth type.
  - **saveBook**: Accepts a book author's array, description, title, bookId, image, and link as parameters; returns a User type. (Look into creating what's known as an input type to handle all of these parameters!)
- **removeBook**: Accepts a book's bookId as a parameter; returns a User type.
- **User type**: \_id, username, email, bookCount, savedBooks (This will be an array of the Book type.)
- **Book type**: bookId (Not the \_id, but the book's id value returned from Google's Book API), authors (An array of strings, as there may be more than one author), description, title, image, link
- **Auth type**: token, user (References the User type.)

###### Front-end "Specs"

- **queries.js**: Holds query GET_ME, which will execute the me query set up using Apollo Server.
- **mutations.js**:
  - _LOGIN_USER_ will execute the loginUser mutation set up using Apollo Server.
  - _ADD_USER_ will execute the addUser mutation.
  - _SAVE_BOOK_ will execute the saveBook mutation.
  - _REMOVE_BOOK_ will execute the removeBook mutation.
- **App.js**: Create an Apollo Provider to make every request work with the Apollo server.
- **SearchBooks.js**:
  - Apollo **useMutation()** Hook to execute the _SAVE_BOOK_ mutation in the handleSaveBook() function instead of the saveBook() function imported from the API file.
  - The logic for saving the book's ID to state in the try/catch block!
- **SavedBooks.js**:
  - Removed the useEffect() Hook that sets the state for UserData, used the useQuery() Hook to execute the GET_ME query on load and save it to a variable named userData.
  - The useMutation() Hook to execute the REMOVE_BOOK mutation in the handleDeleteBook() function instead of the deleteBook() function that's imported from API file. Keeping the removeBookId() function in place.
- **SignupForm.js**: Replaced the addUser() functionality imported from the API file with the ADD_USER mutation functionality.
- **LoginForm.js**: Replaced the loginUser() functionality imported from the API file with the LOGIN_USER mutation functionality.

### _dev note_

Apollo Server recently migrated to Apollo Server 3. This major-version release impacts how Apollo Server interacts in an Express environment. To implement Apollo Server 2 as demonstrated in the activities, you MUST use the following script npm install apollo-server-express@2.15.0 to install Apollo Server 2. Alternately, to migrate to the latest version of Apollo Server, please refer to the Apollo Server Docs on Migrating to Apollo Server 3Links to an external site. and Apollo Server Docs on Implementing Apollo Server Express with v3Links to an external site.. Note that if you are using Apollo Server 3 you are required use await server.start() before calling server.applyMiddleware.
