# Sessions FM sample website

![image](https://user-images.githubusercontent.com/887639/90258175-27681200-de16-11ea-991a-850d33e520a7.png)

This document will serve as a sample as to how the Sessions FM app (i.e. the website) is layed out.

## Code

The code is a React application running on Netlify with serverless functions (i.e. lambda). The source is in `/src/client` with the lambda source in `/src/lambda`.

All code is written in TypeScript.

### Client (i.e. front-end)

The client uses Marauder for dynamic data and code loading of a React "data component". Marauder caches data loads so that page re-visits are super fast.

### Sample code

Here is a typical Sessions React component.

In this example, wew are display a deck of cards, where a `Card` describes an artist record. This is what is shown on the initial Sessions FM screen.

```tsx
import React from 'react';

import { Cards } from '../../components/Cards';
import { Markdown } from '../../components/Markdown';
import { DeckExpanded } from '../../../common/types/Deck';
import styles from './style.module.scss';

type DeckProps = {
  deck: DeckExpanded;
};

export const Deck: React.FC<DeckProps> = ({ deck }) => {
  const { cards, title, description } = deck;

  return (
    <div className={styles.deck}>
      <div className={styles.heading}>
        <h2>{title}</h2>
        <Markdown markdown={description} />
      </div>
      <Cards artists={cards} />
    </div>
  );
};
```

As you can see, we use CSS Modules for styling.

Although not shown here, we also use custom React Hooks where applicable.

For peer-to-peer video conferencing, we use AWS Chime.

### Lambda (i.e. back-end)

Most of the Lambda code runs as Netlify functions. It's compiled down and packaged into JavaScript from TypeScript.

We use MongoDB as a service (i.e. Atlas) for data storage. Mongo is a NoSQL database.

Parts of the back-end impliment AWS SQS and use AWS Lambda functions directly.

### Example back-end code

Here is the back-end code that fetches a single record from MongoDB.

```ts
import { FilterQuery } from 'mongodb';

import { getCollection } from './utils/getCollection';

type FetchDocumentProps<T> = {
  collectionName: string;
  filter: FilterQuery<T>;
};

export const fetchDocument = async <T = any>({
  collectionName,
  filter,
}: FetchDocumentProps<T>): Promise<T> => {
  const collection = await getCollection<T>(collectionName);
  return collection.findOne<T>(filter);
};
```

## Interested in joining the team?

We're always looking for good coders to help out. If you're interested, contact [Ross Freeman](mailto:ross@sessions.fm) to find out more.
