# Web_Scraper_NY_Times_with_MongoDB
<b>Lightweight NLP web scraper of NY Times with MongroDB</b>

In the following illustration, we will be using the NYT API to programmatically retrieve its articles.

<b>Objective:</b>
- Successful gathered article metadata from the NYT API
- Stored said data in MongoDB
- Save URLs for each article that we can now use for scraping

<b>NOTE: Before you begin...</b>
  - Obtain an API key from the NYT for the Article Search API:
    http://developer.nytimes.com/apps/register
  - Install MongoDB on computer
    http://docs.mongodb.org/manual/installation/
    
Now that we have access we can begin to have some fun! Make a request to the article search API endpoint to
retrieve the articles for last week.

Look for what parameters you can set in your request using the API

<b>Use Requests to interact with the API.</b>

<div class="highlight highlight-python"><pre><span class="pl-k">def</span> <span class="pl-en">single_query</span>(<span class="pl-smi">link</span>, <span class="pl-smi">payload</span>):
    response <span class="pl-k">=</span> requests.get(link, <span class="pl-smi">params</span><span class="pl-k">=</span>payload)
<span class="pl-k">    if</span> response.status_code <span class="pl-k">!=</span> <span class="pl-c1">200</span>:
    <span class="pl-k">    print</span> <span class="pl-s"><span class="pl-pds">'</span>WARNING<span class="pl-pds">'</span></span>, response.status_code
<span class="pl-k">    else</span>:
    <span class="pl-k">    return</span> response.json()

link <span class="pl-k">=</span> <span class="pl-s"><span class="pl-pds">'</span>http://api.nytimes.com/svc/search/v2/articlesearch.json<span class="pl-pds">'</span></span>
payload <span class="pl-k">=</span> {<span class="pl-s"><span class="pl-pds">'</span>api-key<span class="pl-pds">'</span></span>: <span class="pl-s"><span class="pl-pds">'</span>74c73309c1052e6aa1785df7cd5cef8c:9:69947183<span class="pl-pds">'</span></span>}
html_str <span class="pl-k">=</span> single_query(link, payload)</pre></div>

Examine one of the articles returned. Look at it's structure and the fields returned. Make sure you can get a single article (and you know what it looks like) before you retrieve ALL of the NYT.

Now that you have some experience with the API and can sucessfully access articles with associated metadata, it is time to start storing them in MongoDB!

Now that you have a sense of what a single article response looks like, we can begin to scale up. Begin
downloading all of the NYT articles starting with the most recent. You will not have time (or effort) to download all of the corpus, so let us start with the most recent articles and download as many as we can! Store these in mongoDB.

<b>NOTE: Inspect how many articles were returned from your request for all of the NYT.</b>

<b>Questions to investigate?</b>

    How many documents are there?

    How many total articles are there in all of the NYT?

Look to the API docs for the Article Search API to learn about why there is a discrepancy between these two numbers (and how to find the second number -- total articles)

The NYT does some things to prevent us from getting it's articles. But we are clever data scientists (cleverer than the NYT API that is)!

<b>Find out how to circumvent the API pagination.</b>

The NYT is rate limited. Deal with it. The API only lets you access a fixed number of pages (from the pagination). 

    What is this number and how can you get around this limit?

Once you have figured how to deal with the NYT quirks, we are ready to start looping. Begin to download the 10,000 most recent NYT articles. You will want to check how your loop is progressing, be sure to print some checkpoint information on how many documents it has downloaded (maybe every 100 articles?).
