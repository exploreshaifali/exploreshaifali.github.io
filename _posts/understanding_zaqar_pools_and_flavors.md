#Understanding Zaqar Pools and Flavor

Here I am answering very common questions that are being asked on zaqar IRC channel(#openstack-zaqar @ freenode): 
	- What are pools and flavor?  
    - Okay, how a pool is connected to queues?  
	- What are pools group?  
	- And pools weight?  

###What are pools?

Pools are containers(that mean storage nodes) which is independent, isolated and contain the messages to transfer. Their main purpose is to provide an easy way to scale Zaqar storage because you can add as many pool as you want. Though as like a node, it is recommended to scale each independent pool as much as possible before adding new ones.

Each pool have 4 main properties:  

	1. Name  
	2. URI  
	3. Group  
	4. Weight  

**Name** is the name that you want to give to that pool.  
**URI** is the endpoint that your pool will have.  
**Group**, think of it like a tag that you want to given to more than one pool so that it keeps you remind the purpose/capabilities of all pools that falls under that group. Basically very few times a single pool will satisfy you, in most of the cases you will need a group of pools, and to keep track of all those pools you can assign them into a group. You can consider a pool as a node and a group of pool as clusters of pools that serve some common purpose.  
**Weight** is the property that allow you set priority of a pool for accepting coming queues. More the weight, more likely it is for a pool to accept the coming queue. It cab be any integer more -1. That mean 0 is the lowest priority of any pool. So if you don't want a pool to get registered with any coming queue, you can set its weight to 0.	

###Lets visualize things:

    1. You have created a queue
    2. Post a message in that queue    

After posting message to a queue, it get stored on a node(pool) on which you have installed your data plane, till the time message does not get expire or is not being destroyed.

    3. If you have attached a queue to a pool, message will go to that pool else it will go to default pool.

Now question arise how to attach to a pool, let say you have a queue with name q1 and a pool with name p1. How will you register queue q1 to pool p1 so that all messages that you post to q1 goes to the receiver through pool p1.

####Registering queues to pools:

This is where **flavors** comes in. A flavor in terms of Zaqar, specify a pool's capability(can be any combination of FIFO, claims, durability, AOD(at least once delivery), hight_throughput). So let say we have following pools: 
  
    p1: {name: p1, uri: "xxx/xxx/xx", group: g1, weight: 3}  
    p2: {name: p2, uri: "xxx/xxx/aa", group: g1, weight: 4}  
    p3: {name: p1, uri: "yyy/yyy/yy", group: g2, weight: 3}  
    p4: {name: p1, uri: "yyy/yyy/bb", group: g2, weight: 4}      

and flavors:

    f1 : {name": f1", capabilities: ['durability'], group: [g1]}  
    f2 : {name: "f2", capabilities: ['FIFO'], group: [g1, g2]}  
    f3 : {name: "f3", capabilities: ['high_throughput'], group: [g2]}

and queues:  

    q1: {name: q1, flavors: [f1, f2]}  
    q2: {name; q2, flavors: [f3, f2]}  

Thus while creating a queue you can add existing flavor names to queue that can be a part of some existing group from pools. This is how linking of queues to pool works. A queue will get linked to the group via flavor and then will get registered to more likelyhood pool(the one with max priority and is free to accept more queues) of that group, as per weith of the pools in that group.



  


