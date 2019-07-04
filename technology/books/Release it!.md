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