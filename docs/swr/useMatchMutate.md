---
title: useMatchMutate
sidebar_position: 4
---

# useMatchMutate

The `useMatchMutate` hook is a custom hook for managing SWR cache entries. It provides functions for revalidating, adding, updating, and removing cache data based on cache key patterns. This hook is useful for batch operations on cache entries that match a specific regular expression pattern.

## Parameters

`matcher: RegExp`: The regular expression used to match cache keys.

`props: MutateProps`: The options object to determine the operation to perform.

- MutateProps

  `infRevalidate: boolean`: If true, revalidate all infinite pagination cache data.

  `infAdd: InfAddOption`: Options for adding data to infinite pagination cache.

  `infUpdate: InfUpdateOption`: Options for updating data in infinite pagination cache.

  `infRemove: InfRemoveOption`: Options for removing data from infinite pagination cache.

  `updateData: UpdateOption`: Options for updating non-infinite cache data.

## Returns

`matchMutate: Function`: A function that accepts a regular expression and an options object to manage cache entries.

## Types

### InfAddOption

Options for adding data to infinite pagination cache.

`revalidate: boolean`: Whether to revalidate after adding data.

`idName: string`: The property name of the item to identify.

`pageList: number[]`: The list of pages to add data to.

`onAddData: function`: (`revalidate: false`) Function that returns the data to be added.

### InfUpdateOption

Options for updating data in infinite pagination cache.

`revalidate: boolean`: Whether to revalidate after updating data.

`idName: string`: The property name of the item to identify.

`idList: (string | number)[]`: The list of IDs to update.

`onUpdateData: function`: (`revalidate: false`) Function that returns the updated data.

### InfRemoveOption

Options for removing data from infinite pagination cache.

`revalidate: boolean`: Whether to revalidate after removing data.

`idName: string`: The property name of the item to identify.

`idList: (string | number)[]`: The list of IDs to remove.

`isLastPageRevalidate: boolean`: (`revalidate: false`) Whether to revalidate the last page only.

`isDataMove: boolean`: (`revalidate: false`) Whether to move data from the next page if the current page is empty.

### UpdateOption

Options for updating non-infinite cache data.

`revalidate: boolean`: Whether to revalidate after updating data.

`onUpdateData: function`: Function that returns the updated data.

### MutateProps

Parameters for the useMatchMutate hook.

`infRevalidate: boolean`: Whether to revalidate all infinite pagination cache data.

`infAdd: InfAddOption`: Options for adding data to infinite pagination cache.

`infUpdate: InfUpdateOption`: Options for updating data in infinite pagination cache.

`infRemove: InfRemoveOption`: Options for removing data from infinite pagination cache.

`updateData: UpdateOption`: Options for updating non-infinite cache data.

## Usage

### Import the hook and necessary utilities

```jsx
import React from 'react';
import { useMatchMutate } from 'ventos-hook-lib';
```

### Use the useMatchMutate hook in your component (revalidate: true):

```jsx
import React from 'react';
import { useMatchMutate } from 'ventos-hook-lib';

const MyComponent = () => {
  const matchMutate = useMatchMutate();

  // Function to handle revalidating all infinite pagination cache data
  const handleRevalidate = async () => {
    await matchMutate(/api\/data/, { infRevalidate: true });
    console.log('All infinite cache data revalidated');
  };

  // Function to handle adding data to infinite pagination cache
  const handleAdd = async () => {
    await matchMutate(/api\/data/, {
      infAdd: {
        revalidate: true,
        idName: 'id',
        pageList: [1, 2], // Pages where data will be added
      },
    });
    console.log('Data added to infinite cache');
  };

  // Function to handle updating data in infinite pagination cache
  const handleUpdate = async () => {
    await matchMutate(/api\/data/, {
      infUpdate: {
        revalidate: true,
        idName: 'id',
        idList: [data.id], // IDs of items to be updated
      },
    });
    console.log('Data updated in infinite cache');
  };

  // Function to handle removing data from infinite pagination cache
  const handleRemove = async (data) => {
    await matchMutate(/api\/data/, {
      infRemove: {
        revalidate: true,
        idName: 'id',
        idList: [data.id], // IDs of items to be removed
      },
    });
    console.log('Data removed from infinite cache');
  };

  // Function to handle updating non-infinite cache data
  const handleUpdateNonInf = async (data) => {
    await matchMutate(/api\/other-data/, {
      updateData: {
        revalidate: true,
        onUpdateData: (cacheData) => ({
          ...cacheData,
          ...data,
        }),
      },
    });
    console.log('Non-infinite cache data updated');
  };

  return (
    <div>
      <button onClick={handleRevalidate}>Revalidate Infinite Data</button>
      <button onClick={handleAdd()}>Add Data to Infinite Cache</button>
      <button onClick={handleUpdate({ id: 2 })}>Update Infinite Cache Data</button>
      <button onClick={handleRemove({ id: 3 })}>Remove Data from Infinite Cache</button>
      <button onClick={handleUpdateNonInf({ id: 4 })}>Update Non-Infinite Cache Data</button>
    </div>
  );
};

export default MyComponent;
```

### Use the useMatchMutate hook in your component (revalidate: false):

```jsx
import React from 'react';
import { useMatchMutate } from 'ventos-hook-lib';

const MyComponent = () => {
  const matchMutate = useMatchMutate();

  // Function to handle adding data to infinite pagination cache without revalidation
  const handleAddNoRevalidate = async (data) => {
    await matchMutate(/api\/data/, {
      infAdd: {
        revalidate: false,
        idName: 'id',
        pageList: [1, 2], // Pages where data will be added
        onAddData: () => ({ id: Math.random(), ...data }), // Example data to add
      },
    });
    console.log('Data added to infinite cache without revalidation');
  };

  // Function to handle updating data in infinite pagination cache without revalidation
  const handleUpdateNoRevalidate = async (data) => {
    await matchMutate(/api\/data/, {
      infUpdate: {
        revalidate: false,
        idName: 'id',
        idList: [data.id], // IDs of items to be updated
        onUpdateData: (item) => ({
          ...data,
        }),
      },
    });
    console.log('Data updated in infinite cache without revalidation');
  };

  // Function to handle removing data from infinite pagination cache without revalidation
  const handleRemoveNoRevalidate = async (data) => {
    await matchMutate(/api\/data/, {
      infRemove: {
        revalidate: false,
        idName: 'id',
        idList: [data.id], // IDs of items to be removed
        isLastPageRevalidate: true, // revalidate last page
        isDataMove: true, //move data within pages
      },
    });
    console.log('Data removed from infinite cache without revalidation');
  };

  // Function to handle updating non-infinite cache data
  const handleUpdateNonInf = async (data) => {
    await matchMutate(/api\/other-data/, {
      updateData: {
        revalidate: false,
        onUpdateData: (cacheData) => ({
          ...cacheData,
          ...data,
        }),
      },
    });
    console.log('Non-infinite cache data updated');
  };

  return (
    <div>
      <button onClick={handleAddNoRevalidate({ id: 1 })}>Add Data without Revalidation</button>
      <button onClick={handleUpdateNoRevalidate({ id: 2 })}>Update Data without Revalidation</button>
      <button onClick={handleRemoveNoRevalidate({ id: 3 })}>Remove Data without Revalidation</button>
      <button onClick={handleUpdateNonInf({ id: 4 })}>Update Non-Infinite Cache Data</button>
    </div>
  );
};

export default MyComponent;
```
