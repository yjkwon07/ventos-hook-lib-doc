---
title: useClearSWRCache
sidebar_position: 1
---

# useClearSWRCache

The `useClearSWRCache` hook provides a way to clear items from the SWR cache based on matching patterns. This is useful for invalidating or removing cached data that matches specific criteria.

## Usage

### Import the hook:

```jsx
import useClearSWRCache from './useClearSWRCache'; // Adjust the path as necessary
```

### Using the hook in a component:

```jsx
import React from 'react';
import { useClearSWRCache } from 'ventos-hook-lib';

const CacheClearer = () => {
  const clearCache = useClearSWRCache();

  const handleClearCache = () => {
    // Example: Clear all cache entries that start with 'api/data/'
    clearCache(/^api\/data\//);

    // Example: Clear all cache entries that start with 'api/data/' but exclude those ending with 'user'
    clearCache(/^api\/data\//, /user$/);
  };

  return (
    <div>
      <button onClick={handleClearCache}>Clear Cache</button>
    </div>
  );
};

export default CacheClearer;
```