# ErrorManagementPattern
Error Management Pattern

# Principal Strategies

- Ignore Error: This unlikely strategy can often be the most effective: do nothing in case of errors
- Isolate Error: ignore the error in the context of the current conversation, but handle all errors afterwards.
- Retry: if you don't succeed at first, try again
- Retry with a delay : if you don't succeed at first, try again

# Show plant uml
![alternative text](http://www.plantuml.com/plantuml/proxy?src=https://raw.github.com/buidiaconseil/ErrorManagementPattern/master/ErrorStateManagement.wsd)
![alternative text](http://www.plantuml.com/plantuml/proxy?src=https://raw.github.com/buidiaconseil/ErrorManagementPattern/master/ExplodePartitionByTime.wsd)
![alternative text](http://www.plantuml.com/plantuml/proxy?src=https://raw.github.com/buidiaconseil/ErrorManagementPattern/master/RetryConsumingStrategy.wsd)
![alternative text](http://www.plantuml.com/plantuml/proxy?src=https://raw.github.com/buidiaconseil/ErrorManagementPattern/master/RetryFlow.wsd)
![alternative text](http://www.plantuml.com/plantuml/proxy?src=https://raw.github.com/buidiaconseil/ErrorManagementPattern/master/errorFlow.wsd)


# Manage by intermediate Flow

# Retries fix or Exponential Backoff Wait time

## Fix Retry

## Exponential Algorithm

```
/*
 * Returns the next wait interval, in milliseconds, using an exponential
 * backoff algorithm.
 */
public static long getWaitTimeExp(int retryCount) {

    long waitTime = (maxTime,(long) Math.pow(2, retryCount) * 100L);

    return waitTime;
}
```

# Error classification

## Causal 

Incomplete / Erroneous Specification / Programming violation  -> DelQueue
Issue on Connection , Time out, temporary defect -> Retry 


## Error Delegation

### Exception 

## Dictionary based

### Annotation Pattern

interface RouteMessage () {
    void route();
}

interface ErrorClassifier (){
    RouteMessage route (String Config , Exception e,Message message);
}


@Retention(RUNTIME)
@Target({ METHOD, TYPE })
public @interface ManageError { 
	String config() default "default";

    // Ordered By Priority
	ErrorClassifier[] classifier() default DefaultErrorClassifier;
}

class DictionnaryBased extend ErrorClassifier
class ExceptionBased extend ErrorClassifier
class CustomBased extend ErrorClassifier

## Retry Delegation

interface RetryStrategy  {
    void retry (RouteMessage message)
}

@Retention(RUNTIME)
@Target({ METHOD, TYPE })
public @interface ManageRecovery {
    String config() default "default"
	long minDelayBeforeRetry() default 3000;
	int nbRetry() default 10;
    // Strategy 
	RetryStrategy retryStrategy default DefaultRetryStrategy;
}

class ExponentialRetry extend RetryStrategy
class FixRetry extend RetryStrategy
class CustomRetry extend RetryStrategy





Redundancy

Interleaving

Throttling

Early Bailout

NeedOnlyOne

Interleaved Operations
