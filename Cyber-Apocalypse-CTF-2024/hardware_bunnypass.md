BunnyPass
> As you discovered in the PDF, the production factory of the game is revealed. This factory manufactures all the hardware devices and custom silicon chips (of common components) that The Fray uses to create sensors, drones, and various other items for the games. Upon arriving at the factory, you scan the networks and come across a RabbitMQ instance. It appears that default credentials will work.

#### Difficulty: very easy

There are no downloadable components for this one, so I spun up the Docker container. The hint says the default credentials for RabbitMQ will work, so I did a quick search and found the said credentials (`guest:guest`).

After logging in, we see the admin GUI of RabbitMQ. Eventually I went to the Queues tab and saw some queues with Ready messages. I got lucky and the first one I tried, `factory_idle` ended up having the flag. I was able to retrieve the flag using the `Get messages` function and inputting 6 as there were 6 ready messages. Scrolling to the 6th message revealed the flag:

`HTB{th3_hunt3d_b3c0m3s_th3_hunt3r}`