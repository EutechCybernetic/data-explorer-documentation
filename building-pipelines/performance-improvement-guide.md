# Performance Improvement Guide

The first step in performance optimization is knowing whether you need it or not.

Some queries that return large volumes of data can be surprisingly fast. Other queries that return just a few calculated rows could be expensive.

There is no single rule to determine what queries need performance improvements. You need to run them and measure them.

This guide provides some general guidelines on performance improvements you can apply and keep in mind while building your query pipeline.

## Avoid calling queries in a loop

The first and most important step to avoid is having your external code (or Lucy model) repeatedly calling a query in a loop.

It's usually better to issue one query and return a lot of data than to run a loop with a small amount of data returned in each iteration.

For example, if you need to get a count of equipment for each category, rather than looping through each category, get all of them at once with an aggregation that groups by the category.

## Understand the source of your data

Data Explorer can seamlessly work with data from multiple sources — the iviva database (SQL Server), Lucy collections (MongoDB), and web service sources (via Lucy actions).

Even though you can construct pipelines without having to care about the type of data source, knowing this will help improve your query performance.

## Database operations and Data Explorer operations (DB-OPS and DE-OPS)

When a stage in a pipeline executes, it will either execute within the database engine (SQL Server, MongoDB, etc.) or be explicitly executed by Data Explorer within the Data Explorer service memory.

Even though Data Explorer is fast, to optimize performance, you want, as much as possible, to push operations on data into the database where that data resides. This reduces the amount of data transfer and improves performance.

All operations (like joins, sorts, summarize, filter) can be either a DE-OP or a DB-OP.

Which one gets used depends on where in the pipeline the operation exists.

## Keep data from the same type of data source near each other

Data Explorer does special optimizations when it knows that the next step in the pipeline is operating on data from the same type of data source.

It can combine all the operations into one step.

For example, if your query pipeline does the following:

```
[Pull Locations] -> [Pull Assets] -> [Join Location and Assets] -> [Filter] -> [Summarize]
```

Data Explorer will optimize it down to a single SQL query. The whole pipeline becomes one operation. Fast.

However, if your assets (for example) are in a Lucy collection and locations are in the iviva SQL Server database, all the operations are issued separately. So you have 2 data queries (pull locations, pull assets), and 3 explicit operations (joining, filtering, aggregation).

You now have 5 operations instead of just 1. Slower.

The operations can still be very fast and you may not notice. However, if you are looking to squeeze more performance, this is something to be aware of.

## Move custom transformations as late as possible in the pipeline

Custom transformations are a really powerful way of manipulating data.

The downside is that they are always DE-OPS — executed by Data Explorer.

Once an operation is executed in Data Explorer, all subsequent operations on that data set are also done within Data Explorer and are no longer executed in the source's database. This means all future operations in the pipeline on that data cannot be optimized into a smaller number of steps.

By moving custom transformations as late as possible in the pipeline, you can have more steps get combined by the optimizer and reduce execution time.

Example: extracting the list of domain names of users who belong to a specific set of sites and getting a count of users by their email address domain in each site.

The slower way to do it:

```
[Pull Users] -> [Custom Transform to extract domain names] -> [Pull Sites] -> [Filter site list down to specific sites] -> [Join sites with Users] -> [Summarize Data]
```

This will result in 5 operations:

1. Pull Users
2. Apply Custom Transform
3. Pull and filter sites (the optimizer will combine them into a single step since the data source is the same type)
4. Join transformed users with filtered sites
5. Summarize data

If you re-arrange the pipeline:

```
[Pull Users] -> [Pull Sites] -> [Filter site list down to specific sites] -> [Join sites with users] -> [Custom transform to extract domain names] -> [Summarize Data]
```

This will result in 3 operations:

1. Pull users, pull sites, filter sites, join with users (the optimizer will combine all of them into a single step as they are the same source type)
2. Apply custom transform
3. Summarize data

Only steps 2 and 3 are DE-OPS — executed by Data Explorer. The rest are pushed down to the database.

Furthermore, by the time Data Explorer has to run its operations, the data set has already been filtered down so the volume is less.

The second option will be faster than the first.

## Avoid joins that mix data from different sources

Joins that do not involve SQL Server will likely be a DE-OP and will be slower than joins that are purely on SQL data.

## Select as few fields as possible

The more fields you return, the larger the volume of data that has to be processed.

## Collapse multiple custom transforms into one

If you have multiple custom transformation steps in your pipeline, consider collapsing them into a single transform that does multiple things.

Each custom transform executes a JavaScript function for every record in the result. Having a single complex transformation is generally faster than multiple simple transforms.

## Apply filtering as early as possible in the pipeline

Filtering reduces the amount of data subsequent steps in the pipeline have to process. Try to filter as early as possible to make future steps faster.
