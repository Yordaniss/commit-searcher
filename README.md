

# Commit-Searcher

Small project to search commits from your repository.

# Example

 ![alt text](https://github.com/Yordaniss/commit-searcher/blob/master/example.png?raw=true)

## How it works

### How the data is stored:

After server was loaded data will be stored to Redis database from api endpoint.

There are several steps to:

* create Schema like this:

```
const commitSchema = new Schema(Commit, {

	message: { type: 'text' },

	author: { type: 'string' },

	url: { type: 'string' }

})
```

* get data from your repository:
```
const response = octokit.request('GET https://api.github.com/repos/{owner}/{repo}/commits', {

	owner: process.env.GITHUB_OWNER,

	repo: process.env.GITHUB_REPO

});
```
* save data to Redis

```
response.then(function(result) {

	result.data.map((commit) => {

		saveDataToRedis(

			commit.commit.message,

			commit.commit.author.name,

			commit.html_url,

		)

	})

})
```
* create index in Redis:
```
await  commitRepository.createIndex()
```

### How the data is accessed:

Data will be loaded from server via endpoints to frontend. There you can see all commits and search for data.

 1. Get all data (used **search()** function from Redis repository)
```
app.get('/commits', async (req, res) => {

	const commits = await commitRepository.search().return.all();

	res.json(commits)

})
```
2. Search messages (used **search()** function from Redis repository with parameters)
```
app.post('/search', async (req, res) => {
	const commits = await commitRepository.search().where('message').matches(req.body.message).return.all()

	res.json(commits)
})
```

## How to run it locally?

* Server (Node.js) -> node server.js
* Frontend (React) -> npm start

### Prerequisites

1. For development was used Docker to run Redis. You can self host Redis or create an account at official website  [Redis Cloud](https://redis.info/try-free-dev-to). 
2. Generate token from your GitHub account 
3. Install Node.js and NPM
4. Create .env file with variables:
 * [ ] REDIS_URL
 * [ ] GITHUB_TOKEN
 * [ ] GITHUB_OWNER
 * [ ] GITHUB_REPO

### Local installation

Pull data from repository [Github](https://github.com/Yordaniss/commit-searcher)

## More Information about Redis Stack

Here some resources to help you quickly get started using Redis Stack. If you still have questions, feel free to ask them in the [Redis Discord](https://discord.gg/redis) or on [Twitter](https://twitter.com/redisinc).

### Getting Started

1. Sign up for a [free Redis Cloud account using this link](https://redis.info/try-free-dev-to) and use the [Redis Stack database in the cloud](https://developer.redis.com/create/rediscloud).
1. Based on the language/framework you want to use, you will find the following client libraries:
    - [Redis OM .NET (C#)](https://github.com/redis/redis-om-dotnet)
        - Watch this [getting started video](https://www.youtube.com/watch?v=ZHPXKrJCYNA)
        - Follow this [getting started guide](https://redis.io/docs/stack/get-started/tutorials/stack-dotnet/)
    - [Redis OM Node (JS)](https://github.com/redis/redis-om-node)
        - Watch this [getting started video](https://www.youtube.com/watch?v=KUfufrwpBkM)
        - Follow this [getting started guide](https://redis.io/docs/stack/get-started/tutorials/stack-node/)
    - [Redis OM Python](https://github.com/redis/redis-om-python)
        - Watch this [getting started video](https://www.youtube.com/watch?v=PPT1FElAS84)
        - Follow this [getting started guide](https://redis.io/docs/stack/get-started/tutorials/stack-python/)
    - [Redis OM Spring (Java)](https://github.com/redis/redis-om-spring)
        - Watch this [getting started video](https://www.youtube.com/watch?v=YhQX8pHy3hk)
        - Follow this [getting started guide](https://redis.io/docs/stack/get-started/tutorials/stack-spring/)

The above videos and guides should be enough to get you started in your desired language/framework. From there you can expand and develop your app. Use the resources below to help guide you further:

1. [Developer Hub](https://redis.info/devhub) - The main developer page for Redis, where you can find information on building using Redis with sample projects, guides, and tutorials.
1. [Redis Stack getting started page](https://redis.io/docs/stack/) - Lists all the Redis Stack features. From there you can find relevant docs and tutorials for all the capabilities of Redis Stack.
1. [Redis Rediscover](https://redis.com/rediscover/) - Provides use-cases for Redis as well as real-world examples and educational material
1. [RedisInsight - Desktop GUI tool](https://redis.info/redisinsight) - Use this to connect to Redis to visually see the data. It also has a CLI inside it that lets you send Redis CLI commands. It also has a profiler so you can see commands that are run on your Redis instance in real-time
1. Youtube Videos
    - [Official Redis Youtube channel](https://redis.info/youtube)
    - [Redis Stack videos](https://www.youtube.com/watch?v=LaiQFZ5bXaM&list=PL83Wfqi-zYZFIQyTMUU6X7rPW2kVV-Ppb) - Help you get started modeling data, using Redis OM, and exploring Redis Stack
    - [Redis Stack Real-Time Stock App](https://www.youtube.com/watch?v=mUNFvyrsl8Q) from Ahmad Bazzi
    - [Build a Fullstack Next.js app](https://www.youtube.com/watch?v=DOIWQddRD5M) with Fireship.io
    - [Microservices with Redis Course](https://www.youtube.com/watch?v=Cy9fAvsXGZA) by Scalable Scripts on freeCodeCamp
