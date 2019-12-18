# Reliable, scalable and maintainable applications

-  Many data-intensive applications include the following functionalities:
    -  Store data so that they, or another application, can find it again later (*databases*)
    -  Remember the result of an expensive operation, to speed up reads (*caches*)
    -  Allow users to search data by keyword or filter it in various ways (*search indexes*)
    -  Send a message to another process, to be handled asynchronously (*stream processing*)
    -  Periodically crunch a large amount of accumulated data (*batch processing*)

## Reliability

-  Types of faults include hardware faults, software errors and human errors.
-  There are situations where we may choose to sacrifice reliability in order to reduce development cost or operational cost, but we should be very conscious of when we are cutting corners.

## Scalability

-  First, we need to succinctly describe the current load on the system, which can be described with a few numbers which we call load parameters.
-  Twitter's example: Twitter’s scaling challenge is not primarily due to tweet volume, but due to *fan-out*—each user follows many people, and each user is followed by many people. There are broadly two ways of implementing these two operations:
    -  Posting a tweet simply inserts the new tweet into a global collection of tweets.
    -  Maintain a cache for each user’s home timeline—like a mailbox of tweets for each recipient user.
-  There are several ways to describe performance:
    -  In a batch processing system like Hadoop, people usually use *throughout*-the number of records we can process per second.
    -  In online systems, what's more important is the service's *response time*-the time between a client sending a request and receiving a response.
    -  Usually it's better to use *percentile* to describe *response time*: For example, Amazon describes response time requirements for internal services in terms of the 99.9th percentile (those are their most valuable customers).

## Maintainability

-  We will pay attention to three design principles for software systems:
    -  *Operability*. Make it easy for operations teams to keep the system running smoothly.
    -  *Simplicity*. Make it easy for new engineers to understand the system.
    -  *Evolvability*. Make it easy for engineers to make changes to the system in the future.
