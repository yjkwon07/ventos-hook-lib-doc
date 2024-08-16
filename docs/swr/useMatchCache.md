---
title: useMatchCache
sidebar_position: 3
---

# useMatchCache

The `useMatchCache` hook provides a way to interact with the SWR cache by matching cache keys with a regular expression. This hook can be useful for debugging or managing cache entries based on specific patterns.

## Parameters

`matcher: RegExp`: The regular expression used to match cache keys.

## Returns

`cache: Map<string, any>`: The SWR cache instance, which is a Map containing cache entries. Only the entries matching the provided `matcher` are considered.

## Throws

`Error`: If the SWR cache is not a `Map` instance, an error is thrown.

## Usage

### Import the hook and necessary utilities:

```jsx
import React from 'react';
import useMatchCache from './useMatchCache'; // Adjust the path as necessary
import { useSWRConfig } from 'swr';
```

### Use the useMatchCache hook in your component:

```jsx
const MyComponent = () => {
  const matcher = /api\/data/; // Regular expression to match cache keys
  const cache = useMatchCache(matcher);

  // Use cache to display, debug, or manage cache entries
  console.log('Matched Cache:', cache);

  return (
    <div>
      {/* Render or manage cache entries as needed */}
    </div>
  );
};
```