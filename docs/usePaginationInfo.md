---
title: usePaginationInfo
sidebar_position: 2
---

# usePaginationInfo

The usePaginationInfo hook calculates pagination information based on total items, current page, rows per page, and the number of pagination links to display. It is useful for managing pagination in lists or tables where you need to navigate between pages.

## Parameters

`totalCount (number)`: Total number of items.

`curPage (number)`: Current page number.

`rowsPerPage (number)`: Number of items per page.

`numOfPagination (number)`: Number of pagination links to display at a time.

## Returns

`firstPage (number)`: The first page number (usually 1).

`lastPage (number)`: The last page number based on totalCount and rowsPerPage.

`previousNumOfPage (number)`: The starting page number for the previous set of pagination links.

`nextNumOfPage (number)`: The starting page number for the next set of pagination links. Adjusts if it exceeds the lastPage.

`rangePage (number[])`: An array of page numbers to display in the pagination controls.

## Example

Here’s how you might use the usePaginationInfo hook in a React component:

```tsx
import React, { useState } from 'react';
import usePaginationInfo from './usePaginationInfo'; // Adjust the path as necessary

const PaginationComponent = ({ totalCount, rowsPerPage, numOfPagination }) => {
  const [curPage, setCurPage] = useState(1);

  const { firstPage, lastPage, previousNumOfPage, nextNumOfPage, rangePage } = usePaginationInfo({
    totalCount,
    curPage,
    rowsPerPage,
    numOfPagination,
  });

  return (
    <div>
      <button disabled={curPage === firstPage} onClick={() => setCurPage(firstPage)}>
        First
      </button>
      <button
        disabled={curPage === firstPage}
        onClick={() => setCurPage(Math.max(curPage - 1, firstPage))}
      >
        Previous
      </button>
      {rangePage.map((page) => (
        <button
          key={page}
          onClick={() => setCurPage(page)}
          style={{ fontWeight: curPage === page ? 'bold' : 'normal' }}
        >
          {page}
        </button>
      ))}
      <button
        disabled={curPage === lastPage}
        onClick={() => setCurPage(Math.min(curPage + 1, lastPage))}
      >
        Next
      </button>
      <button disabled={curPage === lastPage} onClick={() => setCurPage(lastPage)}>
        Last
      </button>
    </div>
  );
};

export default PaginationComponent;
```