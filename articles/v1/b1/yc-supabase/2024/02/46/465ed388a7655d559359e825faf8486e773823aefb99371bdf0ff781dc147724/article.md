---
schema_version: "1.0.0"
document_id: "465ed388a7655d559359e825faf8486e773823aefb99371bdf0ff781dc147724"
company_key: "yc-supabase"
company: "Supabase"
source_id: "yc-supabase-rss-47281c9e7110"
canonical_url: "https://supabase.com/blog/content-recommendation-with-flutter"
published_at: "2024-02-26T07:00:00+00:00"
first_seen_at: "2026-07-20T23:24:12.344578+00:00"
fetched_at: "2026-07-28T22:26:14.489943+00:00"
content_hash: "sha256:9fd8cdb5afd7a28cf6c9a00286bd4d494b860727b7b23150648ad88e45ec026e"
---

# Build a content recommendation app with Flutter and OpenAI

Recommending relevant content to the user is essential to keep the user interested in the app. Although it is a common feature that we would like to have in our apps, building it is not straightforward. This changed as vector databases and Open AI emerged. Today, we can perform semantic searches that are highly aware of the context of the content with just a single query into our vector database. In this article, we will go over how you can create a Flutter movie-viewing app that recommends another movie based on what the user is viewing.


A quick disclaimer, this article provides an overview of what you can build with a vector database, so it will not go into every detail of the implementation. You can find the full code base of the app in this article[here](https://github.com/dshukertjr/examples/tree/main/movie-recommendation) to find more details.


## Why use a vector database for recommending content#


In machine learning, a process of converting a piece of content into a vector representation, called embeddings, is often used, because it allows us to analyze the semantic content mathematically. Assuming we have an engine that can create embeddings that are well aware of the context of the data, we can look at the distance between each embedding to see if the two content are similar or not. Open AI provides a well-trained model for converting text content into an embedding, so using it allows us to create a high-quality recommendation engine.


There are numerous choices for vector databases, but we will use Supabase as our vector database in this article, because we want to also store non-embedding data, and we want to be able to query them easily from our Flutter application.


## What we will build#


We will be building a movie listing app. Think Netflix except the users will not be able to actually view the movie. The purpose of this app is to demonstrate how to surface related content to keep the users engaged.


## Tools/ technologies used#


- [Flutter](https://flutter.dev/) - Used to create the interface of the app
- [Supabase](https://supabase.com/) - Used to store embeddings as well as other movie data in the database
- [Open AI API](https://openai.com/blog/openai-api) - Used to convert movie data into embeddings
- [TMDB API](https://developer.themoviedb.org/docs) - A free API to get movie data


## Creating the app#


We first need to populate the database with some data about movies and its embeddings. For that, we will use the[Supabase edge functions](https://supabase.com/docs/guides/functions) to call the TMDB API and the Open AI API to get the movie data and generate the embeddings. Once we have the data, we will store them in Supabase database, and query them from our Flutter application.


### Step 1: Create the table#


We will have one table for this project, and it is the` films` table.` films` table will store some basic information about each movie like title or release data, as well as embedding of each movie’s overview so that we can perform vector similarity search on each other.


`
_20


-- Enable pgvector extension


_20


create extension vector


_20


with


_20


schema extensions;


_20


_20


-- Create table


_20


create table public.films (


_20


id integer primary key,


_20


title text,


_20


overview text,


_20


release_date date,


_20


backdrop_path text,


_20


embedding vector(1536)


_20


);


_20


_20


-- Enable row level security


_20


alter table public.films enable row level security;


_20


_20


-- Create policy to allow anyone to read the films table


_20


create policy "Fils are public." on public.films for select using (true);


`


### Step 2: Get movie data#


Getting movie data is relatively straightforward. TMDB API provides an easy-to-use[movies endpoint](https://developer.themoviedb.org/reference/discover-movie) for querying information about movies while providing a wide range of filters to narrow down the query results.


We need a backend to securely call the API, and for that, we will use[Supabase Edge Functions](https://supabase.com/docs/guides/functions) . Steps 2 through 4 will be constructing this edge function code, and the full code sample can be found[here](https://github.com/dshukertjr/examples/blob/main/movie-recommendation/supabase/functions/get_film_data/index.ts) .


The following code will give us the top 20 most popular movies in a given year.


`
_32


const searchParams = new URLSearchParams()


_32


searchParams.set('sort_by', 'popularity.desc')


_32


searchParams.set('page', '1')


_32


searchParams.set('language', 'en-US')


_32


searchParams.set('primary_release_year', \`${year}\`)


_32


searchParams.set('include_adult', 'false')


_32


searchParams.set('include_video', 'false')


_32


searchParams.set('region', 'US')


_32


searchParams.set('watch_region', 'US')


_32


searchParams.set('with_original_language', 'en')


_32


_32


const tmdbResponse = await fetch(


_32


\`https://api.themoviedb.org/3/discover/movie?${searchParams.toString()}\`,


_32


{


_32


method: 'GET',


_32


headers: {


_32


'Content-Type': 'application/json',


_32


Authorization: \`Bearer ${tmdbApiKey}\`,


_32


},


_32


}


_32


)


_32


_32


const tmdbJson = await tmdbResponse.json()


_32


_32


const tmdbStatus = tmdbResponse.status


_32


if (!(200 <= tmdbStatus && tmdbStatus <= 299)) {


_32


return returnError({


_32


message: 'Error retrieving data from tmdb API',


_32


})


_32


}


_32


_32


const films = tmdbJson.results


`


### Step 3: Generate embeddings#


We can take the movie data from the previous step and generate embedding for each of them. Here, we are calling the[Open AI Embeddings API](https://platform.openai.com/docs/guides/embeddings) to convert the` overview` of each movie into embeddings.` overview` contains the summary of each movie, and is a good source to create embedding representing each of the movies.


`
_20


const response = await fetch('https://api.openai.com/v1/embeddings', {


_20


method: 'POST',


_20


headers: {


_20


'Content-Type': 'application/json',


_20


Authorization: \`Bearer ${openAiApiKey}\`,


_20


},


_20


body: JSON.stringify({


_20


input: film.overview,


_20


model: 'text-embedding-3-small',


_20


}),


_20


})


_20


_20


const responseData = await response.json()


_20


if (responseData.error) {


_20


return returnError({


_20


message: \`Error obtaining Open API embedding: ${responseData.error.message}\`,


_20


})


_20


}


_20


_20


const embedding = responseData.data\[0\].embedding


`


### Step 4: Store the data in the Supabase database#


Once we have the movie data as well as embedding data, we are left with the task of storing them. We can call the` upsert()` function on the Supabase client to easily store the data.


Again, I omitted a lot of code here for simplicity, but you can find the full edge functions code of step 2 through step 4[here](https://github.com/dshukertjr/examples/blob/main/movie-recommendation/supabase/functions/get_film_data/index.ts) .


`
_20


// Code from Step 2


_20


// Get movie data and store them in \`films\` variable


_20


...


_20


_20


for(const film of films) {


_20


// Code from Step 3


_20


// Get the embedding and store it in \`embeddings\` variable


_20


_20


filmsWithEmbeddings.push({


_20


id: film.id,


_20


title: film.title,


_20


overview: film.overview,


_20


release_date: film.release_date,


_20


backdrop_path: film.backdrop_path,


_20


embedding,


_20


})


_20


}


_20


_20


// Store each movies as well as their embeddings into Supabase database


_20


const { error } = await supabase.from('films').upsert(filmsWithEmbeddings)


`


### Step 5: Create a database function to query similar movies#


In order to perform a vector similarity search using Supabase, we need to create a[database function](https://supabase.com/docs/guides/database/functions) . This database function will take an` embedding` and a` film_id` as its argument. The` embedding` argument will be the embedding to search through the database for similar movies, and the film_id will be used to filter out the same movie that is being queried.


Additionally, we will set an[HSNW index](https://supabase.com/blog/increase-performance-pgvector-hnsw) on the` embedding` column to run the queries efficiently even with large data sets.


`
_14


-- Set index on embedding column


_14


create index on films using hnsw (embedding vector_cosine_ops);


_14


_14


-- Create function to find related films


_14


create or replace function get_related_film(embedding vector(1536), film_id integer)


_14


returns setof films


_14


language sql


_14


as $$


_14


select *


_14


from films


_14


where id != film_id


_14


order by films.embedding <=> get_related_film.embedding


_14


limit 6;


_14


$$ security invoker;


`


### Step 6: Create the Flutter interface#


Now that we have the backend ready, all we need to do is create an interface to display and query the data from. Since the main focus of this article is to demonstrate similarity search using vectors, I will not go into all the details of the Flutter implementations, but you can find the full code base[here](https://github.com/dshukertjr/examples/tree/main/movie-recommendation/flutter) .


Our app will have the following pages:


- **HomePage** : entry point of the app, and displays a list of movies
- **DetailsPage** : displays the details of a movie as well as its related movies


`
_10


lib/


_10


├── components/


_10


│ └── film_cell.dart # Component displaying a single movie.


_10


├── models/


_10


│ └── film.dart # A data model representing a single movie.


_10


├── pages/


_10


│ ├── details_page.dart # A page to display the details of a movie and other recommended movies.


_10


│ └── home_page.dart # A page to display a list of movies.


_10


└── main.dart


`


` components/film_cell.dart` is a shared component to display a tappable cell for the home and details page.` models/film.dart` contains the data model representing a single movie.


The two pages look like the following. The magic is happening at the bottom of the details page in the section labeled` You might also like:` . We are performing a vector similarity search to get a list of similar movies to the selected one using the database function we implemented earlier.


The following is the code for the home page. It’s a simple ListView with a standard` select` query from our` films` table. Nothing special going on here.


`
_48


import 'package:filmsearch/components/film_cell.dart';


_48


import 'package:filmsearch/main.dart';


_48


import 'package:filmsearch/models/film.dart';


_48


_48


import 'package:flutter/material.dart';


_48


_48


class HomePage extends StatefulWidget {


_48


const HomePage({super.key});


_48


_48


@override


_48


State<HomePage> createState() => _HomePageState();


_48


}


_48


_48


class _HomePageState extends State<HomePage> {


_48


final filmsFuture = supabase


_48


.from('films')


_48


.select<List<Map<String, dynamic>>>()


_48


.withConverter<List<Film>>((data) => data.map(Film.fromJson).toList());


_48


_48


@override


_48


Widget build(BuildContext context) {


_48


return Scaffold(


_48


appBar: AppBar(


_48


title: const Text('Films'),


_48


),


_48


body: FutureBuilder(


_48


future: filmsFuture,


_48


builder: (context, snapshot) {


_48


if (snapshot.hasError) {


_48


return Center(


_48


child: Text(snapshot.error.toString()),


_48


);


_48


}


_48


if (!snapshot.hasData) {


_48


return const Center(child: CircularProgressIndicator());


_48


}


_48


final films = snapshot.data!;


_48


return ListView.builder(


_48


itemBuilder: (context, index) {


_48


final film = films\[index\];


_48


return FilmCell(film: film);


_48


},


_48


itemCount: films.length,


_48


);


_48


}),


_48


);


_48


}


_48


}


`


In the details page, we are calling the` get_related_film` database function created in step 5 to get the top 6 most related movies and display them.


`
_106


import 'package:filmsearch/components/film_cell.dart';


_106


import 'package:filmsearch/main.dart';


_106


import 'package:filmsearch/models/film.dart';


_106


import 'package:flutter/material.dart';


_106


import 'package:intl/intl.dart';


_106


_106


class DetailsPage extends StatefulWidget {


_106


const DetailsPage({super.key, required this.film});


_106


_106


final Film film;


_106


_106


@override


_106


State<DetailsPage> createState() => _DetailsPageState();


_106


}


_106


_106


class _DetailsPageState extends State<DetailsPage> {


_106


late final Future<List<Film>> relatedFilmsFuture;


_106


_106


@override


_106


void initState() {


_106


super.initState();


_106


_106


// Create a future that calls the get_related_film function to query


_106


// related movies.


_106


relatedFilmsFuture = supabase.rpc('get_related_film', params: {


_106


'embedding': widget.film.embedding,


_106


'film_id': widget.film.id,


_106


}).withConverter<List<Film>>((data) =>


_106


List<Map<String, dynamic>>.from(data).map(Film.fromJson).toList());


_106


}


_106


_106


@override


_106


Widget build(BuildContext context) {


_106


return Scaffold(


_106


appBar: AppBar(


_106


title: Text(widget.film.title),


_106


),


_106


body: ListView(


_106


children: \[


_106


Hero(


_106


tag: widget.film.imageUrl,


_106


child: Image.network(widget.film.imageUrl),


_106


),


_106


Padding(


_106


padding: const EdgeInsets.all(8.0),


_106


child: Column(


_106


crossAxisAlignment: CrossAxisAlignment.stretch,


_106


children: \[


_106


Text(


_106


DateFormat.yMMMd().format(widget.film.releaseDate),


_106


style: const TextStyle(color: Colors.grey),


_106


),


_106


const SizedBox(height: 8),


_106


Text(


_106


widget.film.overview,


_106


style: const TextStyle(fontSize: 16),


_106


),


_106


const SizedBox(height: 24),


_106


const Text(


_106


'You might also like:',


_106


style: TextStyle(


_106


fontSize: 16,


_106


fontWeight: FontWeight.bold,


_106


),


_106


),


_106


\],


_106


),


_106


),


_106


// Display the list of related movies


_106


FutureBuilder<List<Film>>(


_106


future: relatedFilmsFuture,


_106


builder: (context, snapshot) {


_106


if (snapshot.hasError) {


_106


return Center(


_106


child: Text(snapshot.error.toString()),


_106


);


_106


}


_106


if (!snapshot.hasData) {


_106


return const Center(child: CircularProgressIndicator());


_106


}


_106


final films = snapshot.data!;


_106


return Wrap(


_106


children: films


_106


.map((film) => InkWell(


_106


onTap: () {


_106


Navigator.of(context).push(MaterialPageRoute(


_106


builder: (context) =>


_106


DetailsPage(film: film)));


_106


},


_106


child: FractionallySizedBox(


_106


widthFactor: 0.5,


_106


child: FilmCell(


_106


film: film,


_106


isHeroEnabled: false,


_106


fontSize: 16,


_106


),


_106


),


_106


))


_106


.toList(),


_106


);


_106


}),


_106


\],


_106


),


_106


);


_106


}


_106


}


`


And that is it. We now have a functioning similarity recommendation system powered by Open AI built into our Flutter app. The context used today was movies, but you can easily image that the same concept can be applied to other types of content as well.


## Afterthoughts#


In this article, we looked at how we could take a single movie, and recommend a list of movies that are similar to the selected movie. This works well, but we only have a single sample to get the similarity from. What if we want to recommend a list of movies to watch based on say the past 10 movies that a user watched? There are multiple ways you could go about solving problems like this, and I hope reading through this article got your intellectual curiosity going to solve problems like this.


## Resources#


- [The full code base of the app in this article](https://github.com/dshukertjr/examples/tree/main/movie-recommendation)
- [Matryoshka embeddings: faster OpenAI vector search using Adaptive Retrieval](https://supabase.com/blog/matryoshka-embeddings)
- [Supabase pgvector guide](https://supabase.com/docs/guides/database/extensions/pgvector)
- [Storing OpenAI embeddings in Postgres with pgvector](https://supabase.com/docs/guides/database/extensions/pgvector)
