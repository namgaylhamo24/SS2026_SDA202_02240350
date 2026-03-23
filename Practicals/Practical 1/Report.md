# Design Report: URL Shortener System
In this guide we are building a url shortener design like TinyURL, in which it takes time, the unclean and long url were turned into a short one and when the client clicks on the short url it redirects to the original long url.

## 1. Problem Analysis

The primary challenge of this project is to build a system that can handle an amount of data quickly. We get 100 million URLs every day. * Building a system needs to be able to handle a lot of data without slowing down.

* We need to ensure that our system can handle a lot of requests at the time without getting slow.

* The system wilpredominantly  asked to redirect often than create new URLs because most people will read the URLs more than create new ones.

* The ratio of reads to writes is 10 to 1 so we must guarantee that the system can handle a lot of reads quickly.

The URLs need to be short, with a maximum length of 62 characters. This implies that we need to find a way to create IDs that are short but also don't get confused with each other.We need to find a balance between simplicity and making it able to handle a lot of data.Fortunately, we don't need to worry about deleting or updating records.This makes things simpler.Our system only needs to ensure that new records are correct.We must find ways to give IDs to all the records across all the servers.

* Creating unique URLs for a vast amount of data. 365 Billion records.

We also need to handle a lot of requests. 11,600 Reads per second.

The main problems we need to solve are:

* Creating URLs for a huge amount of data
* Developing a data model that can handle hundreds of billions of records
* Building a system that can handle a lot of traffic without getting slow.

## 2. Author's Solution Analysis
### API Design

The author keeps things simple busing just two API endpoints.

1. The first endpoint accepts a POST request, which takes a URL and provides a shortened link.

2. The second endpoint operates as a GET request, which accepts a URL and delivers the full original URL.

This method allows the system to determine the appropriate user redirection path.The system presents a simple design that enables users to perform two fundamental tasks without experiencing any complications.

### URL Redirecting Strategy

A key decision is whether to use 301 or 302 redirects.A 301 redirect makes the browser treat the mapping as permanent.The browser saves it in its cache.After the user clicks the link, all following requests will go directly to the initial destination.

This method decreases the amount of work that servers must perform.The ability to track link clicks will be lost to you.

The shortener service never sees those requests.A 302 redirect makes the browser check the shortener service first.This process requires additional time to complete.

The system provides total tracking capabilities for each individual user interaction.The system allows you to monitor user interactions while tracking their sources together with analytic data.

The author shows that both methods perform equally well.The choice depends on which factors you consider more important. 

### Hash Function Approaches

The author presents two ways to generate URLs.

#### Method 1: Hash Plus Collision Resolution

The first method uses base 62 conversion.
* CRC32 and MD5 and SHA-1 are common examples of hash functions.
* The long URL is run through the hash function.
* The first seven characters are taken to keep things short.
* Truncating a hash raises the possibility of multiple hash values.

Two different long URLs might end up with the same short URL.

The system adds extra content to the hash until it reaches a new short URL.

The process continues until the system discovers a distinct short URL.

Bloom filters enable efficient URL checking through their ability to track short URLs and existing short URLs. 

Bloom filters provide a fast method to determine whether a short URL has been claimed.


### Method 2: Base 62 Conversion

The second method uses base 62 conversion.

Websites receive unique numeric identifiers, which function as their URLs.

The ID is converted into a string using base 62.

Base 62 uses numbers, including lowercase letters and uppercase letters.

*An ID like 2009215674938 becomes "zn9edcu".*

The underlying ID is guaranteed to be unique.
Collisions cannot happen.

The author compares the two methods.

* Hash plus collision gives a fixed seven-character URL.
* The system does not depend on an ID generator for its operation.
* Collisions are possible.
* Handling collisions adds complexity.
* Base 62 conversion makes collisions impossible.
* It flows naturally from an ID generator.

However, the short URL length can grow over time.
There is a security tradeoff.With base 62, if someone knows how the system works,  they could guess the short URL.This could be a problem with private links.

## My Understanding and Reflections

### What Makes Sense to Me

**The scale numbers feel real.** The author presents his writing speed at 1,160 words per second and his reading speed at 11,600 words per second which provides me with a definite measurement to evaluate. The seven-character URL design chose by them gets explained through their decision because 62^7 creates 3.5 trillion URL combinations which exceeds their planned 365 billion URL storage capacity. The math checks out.

**The 301 versus 302 choice taught me something new.** I never realized redirect types mattered beyond just sending people to the right place. The 301 redirect reduces server requirements because it lets browsers store redirects while the 302 redirect improves website analytics through direct user interaction. The 302 method lets me observe clicks until I switch to the 301 method which I will use if necessary.

**Base 62 conversion is clever.** The process requires you to convert a distinct number into a brief alphanumeric sequence instead of creating a URL hash and relying on collision-free operation. The example showing 11157 turning into "2TX" made it easy to understand. The short URL system uses the number which already exists in its unique form as its base for generating short URLs without requiring duplicate existence checks.

**Cache makes perfect sense.** The system requires 11,600 reading operations per second which makes direct database access too inefficient. The system achieves quick performance through its cache which intercepts most incoming requests. The diagram in Figure 8-8 shows this clearly—check cache first, go to database only if needed, then store the result in cache for next time.

## My Understanding and Reflections

### What Makes Sense to Me

**The scale numbers feel real to me.** The author says he can write at 1,160 words per second and read at 11,600 words per second. This gives me an idea of what to expect. The reason they chose a seven-character URL is because it creates a lot of combinations. 3.5 Trillion to be exact. This is more than the 365 billion URLs they plan to store. The math works out.

**I learned something about the 301 and 302 redirects.** I did not know that the type of redirect mattered. The 301 redirect is good because it helps the server by letting browsers remember the redirects. The 302 redirect is good for tracking website traffic because it shows me what people are clicking on. I can use the 302 redirect to see what people are doing. Then switch to the 301 redirect if I need to.

**The base 62 conversion is really clever.** It takes a number and turns it into a short code with letters and numbers. This is better than using a URL hash because it does not require checks to make sure the code is unique. For example the number 11157 becomes "2TX". The short URL system uses the existing number to make an URL so it does not need to check for duplicates.

**The cache system makes sense to me.** The system needs to read 11,600 things per second which's too much for the database to handle. So it uses a cache to make things faster. The cache checks if it already has what the user is asking for and if not it goes to the database and then stores the answer in the cache for time. The diagram, in Figure 8-8 shows how this works. It checks the cache first then the database if needed and then stores the result in the cache.

### The Confusion:
I am trying to understand how the author uses Bloom filters to deal with collisions. The author says Bloom filters help him check if a short URL already exists. I know Bloom filters are a way to save space and they can probably tell us if something is in a list but I need information about how to use them in this project. Do we need to use a Bloom filter to keep track of all the URLs that are available right now? What happens when we get positives, which are when the filter says something is in the list when it is not? This is something we should look into more.



The way we use hash and collision resolution is also not very clear. Figure 8-5 shows that we add a string to resolve collisions. It does not say what that string is. Do we add the number 1 the number 2 and so on?. Do we use a random string? We need to know how times we can try to add a string before we reach the maximum limit.

I am also thinking about what happens when our data grows. The author says we will need 365 terabytes of storage over 10 years.. This is just the basic storage we need and we have to think about other things like indexes, backups and replication. In the world we will need two or three times more storage than that. The author talks about having a system that's always available but does not say how to make that happen.

### Things I want to further Explore:

There are a things I want to learn more about. I want to research systems that can generate IDs across many servers. The author mentions an ID generator in Chapter 7 but does not summarize it here. We need a system that can generate IDs that work across many servers and can do it very fast at a rate of 1,160 per second. Twitters Snowflake UUIDs and database sequences are two ways to do this. They have their own advantages and disadvantages.

When we have a lot of data 365 billion records we need to decide how to split it up. We need to choose a way to divide our data, which is called sharding. The way we shard our data affects how fast our system can answer questions and how well it can handle a lot of users.

We also need to decide which short URLs to store in our cache. Do we use the recently used or LRU method?. Do we use the least frequently used or LFU method? We need to come up with ways to stop cache stampedes, which happen when many users try to access an URL at the same time and it has just expired.

The author talks a bit about rate limiting, which is a way to control how many requests our system can handle.. The author does not explain how to do rate limiting in a system that has many servers without using a central server. The author talks about rate limiting. Does not say how to apply it to a system, with many servers without using a central server.