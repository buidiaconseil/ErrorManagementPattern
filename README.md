# ErrorManagementPattern
Error Management Pattern

# Principal Strategies

- Ignore Error: This unlikely strategy can often be the most effective: do nothing in case of errors
- Isolate Error: ignore the error in the context of the current conversation, but handle all errors afterwards.
- Retry: if you don't succeed at first, try again
- Retry with a delay : if you don't succeed at first, try again

# Show plant uml


# Manage by intermediate Flow

# Retries fix or Exponential Backoff Wait time

## Exponential Algorithm

```
/*
 * Returns the next wait interval, in milliseconds, using an exponential
 * backoff algorithm.
 */
public static long getWaitTimeExp(int retryCount,long maxTime) {

    long waitTime = Math.min(maxTime,(long) Math.pow(2, retryCount) * 100L);

    return waitTime;
}
```

# Error classification

## Causal 

Incomplete / Erroneous Specification / Programming violation  -> DelQueue
Issue on Connection , Time out, temporary defect -> Retry 


## Delegation
### Strategy pattern

IErrorStrategieManagement

### Exception 


## Dictionary based



