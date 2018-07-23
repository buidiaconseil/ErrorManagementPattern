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

![alternative text](http://www.plantuml.com/plantuml/proxy?src=https://raw.github.com/buidiaconseil/ErrorManagementPattern/master/errorFlow2.wsd)
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

## Retry Delegation

```
@Retention(RUNTIME)
@Target({ METHOD, TYPE })
public @interface RetryManager {

	enum TimeDelay {
		FIVE_SECONDS,TEN_SECONDS,THIRTY_SECONDS,ONE_MINUTE,FIVE_MINUTES,TEN_MINUTES,FIFTEEN_MINUTES,THIRTY_MINUTES,ONE_HOUR,TWO_HOURS,FIVE_HOURS
	}
	
	@Nonbinding
	TimeDelay [] delaysBeforeRetry() default {TimeDelay.FIVE_SECONDS, TimeDelay.TEN_SECONDS,TimeDelay.THIRTY_SECONDS,TimeDelay.ONE_MINUTE,TimeDelay.FIVE_MINUTES,TimeDelay.TEN_MINUTES,TimeDelay.FIFTEEN_MINUTES};

	@Nonbinding
	int nbRetry() default 10;

}

```






Redundancy

Interleaving

Throttling

Early Bailout

NeedOnlyOne

Interleaved Operations

 error-correcting
codes, checkpoint followed by rollback and replay [8, 9, 16]
and redundant computation.

acceptability-oriented  computing  [17],  task  skipping  [18],
failure-oblivious computing [19], transactional function ter-
mination [21, 22], exiting infinite loops [3], automatic patch
Copyright is held by the author/owner(s).
AOSD ’12,
March 25-30, 2012, Potsdam, Germany.
ACM 978-1-4503-1222-6/12/03.
1
generation [15, 25], software rejuventation [24], recovery-
oriented  computing  [2],  heap  overprovisioning  [1],  input
rectification [11], and data structure repair [5–7, 12, 20, 26].

generation [15, 25], software rejuventation [24], recovery-
oriented  computing  [2],  heap  overprovisioning  [1],  input
rectification [11], and data structure repair [5–7, 12, 20, 26].
Because such techniques may have effects that easily propa-
gate across module boundaries, understanding their accept-
ability often requires the adoption of an end-to-end perspec-
tive that takes the larger goals of the complete system into
account.
Finally,  we  consider  the  emerging  field  of  approximate
computing. Inspired by the surprising ability that many soft-
ware systems exhibit to tolerate perturbations or even large-
scale  changes  to  their  execution  (for  example,  loop  per-
foration  [13,  23],  which  simply  skips  iterations  of  time-
consuming  loops),  researchers  are  now  exploring  a  vari-
ety  of  techniques  that  purposefully  adapt  the  functional-
ity  that  a  system  provides  to  obtain  benefits  such  as  im-
proved  performance,  reduced  energy  consumption,  or  ro-
bustness [4, 10, 14, 27]. The initial success of these tech-
niques suggests that it may now be possible to move toward
a world in which the dominant software systems exhibit un-
precedented flexibility, malleability, and adaptability with-
out requiring engineers to understand precisely how or why
they are able to do so
