---
title: useInfiniteScroll
sidebar_position: 1
---

# useInfiniteScroll

The `useInfiniteScroll` hook provides a way to load more content when the user scrolls to the bottom of a container or viewport. It uses the Intersection Observer API to observe when a target element (e.g., a "load more" trigger) becomes visible in the viewport.

## Parameters

`hasMore (boolean)`: Indicates if there is more content to load. If false, the observer will not trigger the loadMore function.

`loadMore (function)`: A callback function that gets triggered when the target element is in view and hasMore is true.

`rootMargin (string, optional)`: The margins around the root element to use for the intersection calculation (e.g., "0px 0px 100px 0px"). Defaults to "0px".

`threshold (number or number[], optional)`: The percentage of the target element that must be visible before the loadMore function is triggered. Defaults to 0.

## Returns

`setObserveTarget (function)`: Sets the element that should be observed for intersections. This function should be called with the target element (e.g., the "load more" trigger element).

`setObserveRoot (function)`: Sets the root element for the intersection observer. This function should be called with the root element (e.g., the scroll container).

## Example Code

Here's how you might use the useInfiniteScroll hook in a React component:

```jsx
import React, { useState, useRef, useEffect } from 'react';
import useInfiniteScroll from './useInfiniteScroll'; // Adjust the path as necessary

const InfiniteScrollComponent = () => {
  const [items, setItems] = useState<string[]>([]);
  const [hasMore, setHasMore] = useState(true); // Indicates if there's more data to load
  const [loading, setLoading] = useState(false); // Indicates if data is currently loading

  // Load more data function
  const loadMore = async () => {
    if (loading) return; // Prevent multiple loads
    setLoading(true);
    
    // Simulate an API call to load more data
    const newItems = await new Promise<string[]>((resolve) =>
      setTimeout(() => resolve(Array.from({ length: 10 }, (_, i) => `Item ${i + items.length + 1}`)), 1000)
    );
    
    setItems((prevItems) => [...prevItems, ...newItems]);
    setLoading(false);
    
    // If no more items to load, set `hasMore` to false
    if (newItems.length === 0) {
      setHasMore(false);
    }
  };

  // Initialize the hook with the appropriate parameters
  const { setObserveTarget, setObserveRoot } = useInfiniteScroll({
    hasMore,
    loadMore,
    rootMargin: '0px',
    threshold: 1.0,
  });

  const targetRef = useRef<HTMLDivElement | null>(null);
  const rootRef = useRef<HTMLDivElement | null>(null);

  useEffect(() => {
    // Set the target and root elements for observation
    setObserveTarget(targetRef.current);
    setObserveRoot(rootRef.current);
  }, [setObserveTarget, setObserveRoot]);

  return (
    <div ref={rootRef} style={{ height: '400px', overflowY: 'auto' }}>
      <div>
        {items.map((item, index) => (
          <div key={index}>{item}</div>
        ))}
        {/* The target element to observe */}
        {hasMore && !loading && <div ref={targetRef}>Loading more...</div>}
        {loading && <div>Loading...</div>}
        {!hasMore && <div>No more items</div>}
      </div>
    </div>
  );
};

export default InfiniteScrollComponent;
```