### Stability 
system keeps processing transactions, event when impulses, stresses, or component failure disrupts the normal processing

### Stability Antipatterns

#### Integration Point : Every single one presetns a stability risk
* Socket based protocols
    * connection failures
        * it may take a long time to discover connection failures(firewall, etc.)
    * http protocols: adds own set of issues    
        * connection, content type, status code, payload
        * client library 
            * how **blocking** is handled
* countering the problem
    * Circuit Breaker
    * Timeouts
    * Decoupling Middleware
    * Tests

#### Chain Reactions

* defect revealed by load related conditions
    * memoery leak
    * dead lock
* one server down, the others pick up the traffic
* countering the problem
    * auto scaling
    * bulkheads

#### Cascading Failures

* crack in one layer triggers crack in a calling layer
    * often results from a resource pool
* high fan-in crucial services worth extra scrutiny
* countering the problem
    * Circuit Breaker
    * Timeouts

#### Users

* traffic/excessive demand
    * heap memory
    * sockets
* expensive users
* unwanted users
* malicious users
* countering the problem
    * [code] weak reference
    * test aggressively

#### Blocked Threads

* normally with integration point, around resource pools
* library
    * may need warpper to do timeouts
* countering the problem
    * scrutinize resource pools
    * use proven primitives
    * timeouts
    * be aware of the library
    * command query responsibility separation
    * external loggings
    * counters

#### Self-Denail Attacks

* system, including extended system and human, conspire agianst itself
* countering the problem
    * keep the lines of communication open
    * protect shared resources 

#### Scaling Effects
#### Unbalanced Capacities

* mismatched ratios between different layers
    * dealing with rate limit APIs

#### Dogpile

* servers impose load at once
* steady-state load != startup or periodic load
* coutering the problem
    * avoid pulsing

#### Force Multiplier

* automation vs manual belief of the state
* control plane - logging, monitoring, schedulers, scalers, load balancers, configuration management
    * sense the current state, and compare the desired state
* coutering the problem
    * build limiters and safeguard

#### Slow Response

* ties up resources in the calling/called system
* memory leaks, resource connection
* coutering the problem
    * fail fast

#### Unbounded Result Sets

* coutering the problem
    * caller indicate limit


### Stability Patterns

#### Timeouts

* fault isolation
* organize long-running opeartions into shared component and reuse       
* retry
    * less retry if upper stream clients wait
    * slow retry if queuing the work is ok

#### Circuit Breaker

* allow one subsystem to fail without destroying the whole system
* detect & fail fast
* state
    * **closed** - usual 
    * **open** - fail immediately 
    * **half open** - trial
* detect failure
    * fault density, instead of total count
* handle failure
    * specific exception
    * fall back result(cache/default)
    * call another service
* scope of single thread