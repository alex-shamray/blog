# [4.15 Testing Asynchronous Methods](http://etutorials.org/Programming/Java+extreme+programming/Chapter+4.+JUnit/4.15+Testing+Asynchronous+Methods/)

#### 4.15.1 Problem

You want to test asynchronous methods.

#### 4.15.2 Solution

Use a mock listener to wait for the asynchronous method to complete.

#### 4.15.3 Discussion

An asynchronous method executes in its own thread, notifying some listener when it is complete. Code that calls an asynchronous method does not block, meaning that you cannot write a test like this:

```java
public void testSomething(  ) {
    someAsynchronousMethod(  );
    assertXXX(...);
}
```

The problem with this code lies in the fact that the `assertXXX( )` is almost certainly executed before the thread started by `someAsynchronousMethod( )` has a chance to do its work. We really need to do something like this:

1. Call an asynchronous method.
2. Wait until the method is complete.
3. Get the results.

* If the method times out, fail.
* Otherwise, check the results.

To illustrate, let's look at a simple interface for searching. We assume that searching occurs in its own thread, notifying a `SearchModelListener` whenever the search is complete. Example 4-8 shows the API.

##### Example 4-8. SearchModel interface

```java
public interface SearchModel {
    void search(Object searchCriteria, SearchModelListener listener);
}
```

The `search( )` method is asynchronous, notifying the `SearchModelListener` when it is complete. Example 4-9 shows the code for the `SearchModelListener` interface.

##### Example 4-9. SearchModelListener interface

```java
public interface SearchModelListener extends EventListener {
    void searchFinished(SearchModelEvent evt);
}
```

In order to test the search model, we must write a mock listener that waits for the search to complete. Once the mock listener receives its result, we can verify that the data is correct. Example 4-10 shows the code for a mock listener.

##### Example 4-10. MockSearchModelListener class

```java
class MockSearchModelListener implements SearchModelListener {
    private SearchModelEvent evt;

    public void searchFinished(SearchModelEvent evt) {
        this.evt = evt;
        synchronized (this) {
            notifyAll( );
        }
    }

    public SearchModelEvent getSearchModelEvent(  ) {
        return this.evt;
    }
}
```

The key to our mock listener is the `synchronized` block. This listener assumes that some other thread (our unit test) is waiting for the search to complete. By calling `notifyAll( )`, the mock listener allows the unit test to "wake up" and continue.<sup>[10]</sup> Example 4-11 shows the unit test, which ties everything together.

> <sup>[10]</sup> `notifyAll()` can only be called within synchronized code.


##### Example 4-11. Asynchronous unit test

```java
public void testAsynchronousSearch(  ) throws InterruptedException {
    MockSearchModelListener mockListener = new MockSearchModelListener(  );
    SearchModel sm = new PersonSearchModel(  );
    // 1. Execute the search
    sm.search("eric", mockListener);

    // 2. Wait for the search to complete
    synchronized (mockListener) {
        mockListener.wait(2000);
    }

    // 3. Get the results
    SearchModelEvent evt = mockListener.getSearchModelEvent(  );

    // 3a) if the method times out, fail
    assertNotNull("Search timed out", evt);

    // 3b) otherwise, check the results
    List results = evt.getSearchResult(  );
    assertEquals("Number of results", 1, results.size(  ));
    Person p = (Person) results.get(0);
    assertEquals("Result", "Eric", p.getFirstName(  ));
}
```

The unit test first creates a mock listener, passing that listener to the search model. It then uses a `synchronized` block to wait until the listener calls `notifyAll( )`. Calling `wait(2000)` indicates that the test will wait for at least two seconds before it stops waiting and continues. If this happens, the mock listener's event object is `null` because it was never notified by the search model.

> Having a timeout period is critical; otherwise, your test will wait indefinitely if the asynchronous method fails and never notifies the caller.

Assuming the search completed within two seconds, the test goes on to check the results for correctness.

#### 4.15.4 See Also

Mock objects are described in [Chapter 6](http://etutorials.org/Programming/Java+extreme+programming/Chapter+6.+Mock+Objects/).
