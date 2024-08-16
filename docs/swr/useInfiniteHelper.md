---
title: useInfiniteHelper
sidebar_position: 2
---

# useInfiniteHelper

The `useInfiniteHelper` hook is designed to work with SWR's infinite loading capabilities. It provides utilities to handle pagination, manage cache, and optimize data processing. 

## Parameters

`result: SWRInfiniteResponse<Data[], Error>`: The SWR infinite query result.

`pagination: { totalCount: number; page: number; pageSize: number }`: Pagination details.

`idName: keyof Data`: The key used to identify unique items in the data.

`cacheClearKey: string (optional)`: A key to match cache entries for clearing.

## Returns

`data: Data[]`: A unique, flattened list of data items.

`pageData: Data[] | undefined`: Raw data from SWR.

`isLoading: boolean`: Indicates if data is currently loading.

`isReachingEndData: boolean`: Indicates if the end of the data has been reached.

`onMoreRead: () => void`: Function to fetch more data.

`onClearData: () => void`: Function to clear cached data based on the `cacheClearKey`.

## Usage

### Import the hook and necessary utilities:

```jsx
import React from 'react';
import { useInfiniteHelper } from 'ventos-hook-lib';
import useSWRInfinite from 'swr/infinite';
```

### Set up the SWR infinite query:

```jsx
const fetcher = (...args) => fetch(...args).then(res => res.json());

const useData = () => {
  return useSWRInfinite(
    (index) => `/api/data?page=${index}`,
    fetcher
  );
};
```

### Use the useInfiniteHelper hook in your component:

```jsx
const MyComponent = () => {
  const result = useData();
  const pagination = { totalCount: 100, page: 0, pageSize: 10 }; // Adjust as needed
  const idName = 'id'; // The unique key in your data
  const cacheClearKey = '/api/data'; // Optional, for cache clearing

  const {
    data,
    pageData,
    error,
    isLoading,
    isReachingEndData,
    onMoreRead,
    onClearData,
    mutate,
  } = useInfiniteHelper({ result, pagination, idName, cacheClearKey });

  if (isLoading) return <div>Loading...</div>;
  if (isReachingEndData) return <div>No more data</div>;

  return (
    <div>
      {data.map(item => (
        <div key={item[idName]}>{item.name}</div> // Adjust according to your data structure
      ))}
      <button onClick={onMoreRead}>Load More</button>
      <button onClick={onClearData}>Clear Cache</button>
    </div>
  );
};
```