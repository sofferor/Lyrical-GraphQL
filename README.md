# Client Side React Tutorial with a GraphQL Server and a MongoDB Database

This project is a tutorial for building a Client Side React Application with a GraphQL Server.
It was cloned from https://github.com/StephenGrider/Lyrical-GraphQL with all the Server side, 
and in the Tutorial we built the Client side.
The Tutorial is from the Udemy course: "GraphQL with React: The Complete Developers Guide"



### The components

1. The server folder - I didn't change it, except configuring the MONGO_URI in server.js.
2. The client folder which there we did all the Tutorial and it contains:
    1. index.js - the root of the application, which contains the ApolloClient that is wrapped by the ApolloProvider.
    2. The style folder which contains some css definitions.
    3. The queries folder which contains a GraphQL queries that is used throughout the application.
    4. The components folder which contains the main logic. All the React Components with the following pages:
        1. The root page ("/") which display the SongList component and enable navigation to each song page and to create a new song page.
        2. The ("/songs/new") page which is used to create a new song, and it displays the SongCreate component.
        3. The ("songs/:id") page which contains all the details on a specific song including his name, lyrics and an ability to add & like a lyric.
           The main component of this page is the SongDetail which contains the LyricList & LyricCreate components.

           
### Notes
1. The `{ graphql } from "react-apollo"` enables to connect between GraphQL Query/Mutation and the React Component.
   In the code it looks like `graphql(likeLyric)(LyricList)` 
   When `likeLyric` is the query/mutation, and `LyricList` is the React Component, and the `graphql` connect between them.
2. The `gql from "graphql-tag"` enables to write GraphQL Query/Mutation directly in a Javascript file.
3. Pass params to mutation - `this.props.mutate({ variables: {title: this.state.title}}`.
4. Pass params to query - `graphql(fetchSong, {
   options: props => {return {variables: {id: props.params.id}}}
   })(SongDetail);`.
5. Apollo Client doesn't refresh the queries by default.
   If a content of a query was changed (after a mutation), 
   e.g. if a song was added to the song list, the content of the query to fetch all the songs is now changed (because there is another song in the list now).
   There are two ways to tell Apollo to update the query:
   1. Less recommended way - to tell Apollo which specific queries to run after a specific Mutation - `refetchQueries: [{query: fetchSongs}]`.
      There are two disadvantages in this approach: You need to specify the specific queries you want to run for each specific mutation and you run extra queries.
      This approach is good only if the content of the query you specify to run again is not returned anyway from the mutation (and that's why you can't use the second method).
   2. Better way to tell Apollo to save IDs for his Data.
      In this way Apollo knows to update the content of query to fetch a Song (with ID) after running mutation on this Song and a Data with the Song ID was returned as the mutation response.
      So in order to do that we need to define Apollo Client to save ID for each Object, and the ID he can take is the same ID in the Server - `ApolloClient({ dataIdFromObject: o => o.id })`.
6. Apollo Client (also called Apollo Store) is what connect the client to the server and also store the Data he received from the Server.
   Apollo Provider is the middleware between our React Application and Apollo Client.
   The Apollo Client doesn't know and doesn't care that our Application is written in React.
   So most of our configuration will be in the Apollo Provider.


### Those are the schema definitions

![root_query_definitions](/images/root_query_definitions.png?raw=true)

![mutation_definitions](/images/mutation_definitions.png?raw=true)


### This is an example for a function with variables in GraphQL

![function_with_variables](/images/function_with_variables.png?raw=true)


### Those are the pages of the Client side of the application

![song_list_page](/images/song_list_page.png?raw=true)

![create_song_page](/images/create_song_page.png?raw=true)

![song_detail_page](/images/song_detail_page.png?raw=true)

