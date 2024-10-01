## Traditional Data Fetching


### 1. No Caching (Server-Side Rendering - SSR)
In this example, the time will update on every page refresh because the page is rendered on the server for every request
```ruby
// pages/index.js (using SSR)
export async function getServerSideProps() {
  const currentTime = new Date().toLocaleTimeString();
  
  return {
    props: {
      currentTime,
    },
  };
}

export default function Home({ currentTime }) {
  return <h1>Current Time: {currentTime}</h1>;
}

// Explanation:
// - The time will update whenever the page is refreshed.
// - No caching is applied since we are using Server-Side Rendering (SSR).
```

### 2. Static Site Generation (SSG) - No Revalidation
In this case, the time will not update even if you refresh the page because the page is generated at build time and cached indefinitely.
```ruby
// pages/index.js (using SSG with no revalidation)
export async function getStaticProps() {
  const currentTime = new Date().toLocaleTimeString();
  
  return {
    props: {
      currentTime,
    },
  };
}

export default function Home({ currentTime }) {
  return <h1>Current Time: {currentTime}</h1>;
}

// Explanation:
// - The time will NOT update upon refreshing the page.
// - The page is statically generated at build time, so the time shown will remain the same until a new build is triggered.
```
### 3. Static Site Generation (SSG) with Revalidation (ISR)
Here, the time will update every 10 seconds if the page is refreshed. The page is cached but revalidated periodically in the background.
```ruby
// pages/index.js (using ISR with revalidate set to 10 seconds)
export async function getStaticProps() {
  const currentTime = new Date().toLocaleTimeString();
  
  return {
    props: {
      currentTime,
    },
    revalidate: 10, // Revalidate every 10 seconds
  };
}

export default function Home({ currentTime }) {
  return <h1>Current Time: {currentTime}</h1>;
}

// Explanation:
// - The time will update every 10 seconds if the page is refreshed.
// - The page is statically generated and cached, but Next.js will regenerate it in the background every 10 seconds.

```
### 4. Client-Side Data Fetching with fetch (Cache-Control: No Store)
This setting disables caching, and the clock will always fetch the latest time, even for client-side requests.
```ruby
// app/page.js (Client-side fetching with no caching)
export default async function Home() {
  const res = await fetch('https://worldtimeapi.org/api/timezone/Etc/UTC', {
    cache: 'no-store', // Disable caching
  });
  const data = await res.json();
  const currentTime = new Date(data.datetime).toLocaleTimeString();

  return <h1>Current Time: {currentTime}</h1>;
}

// Explanation:
// - The time will update whenever the page is refreshed, as caching is completely disabled.
// - Each fetch request will always get fresh data because of the `cache: 'no-store'` option.
```
### 5. Client-Side Data Fetching with fetch (Cache-Control: Force Cache)
This forces the request to use cached data, so the time will not update unless the cache expires manually or the page is rebuilt.
```ruby
// app/page.js (Client-side fetching with forced cache)
export default async function Home() {
  const res = await fetch('https://worldtimeapi.org/api/timezone/Etc/UTC', {
    cache: 'force-cache', // Force using cached data
  });
  const data = await res.json();
  const currentTime = new Date(data.datetime).toLocaleTimeString();

  return <h1>Current Time: {currentTime}</h1>;
}

// Explanation:
// - The time will NOT update upon refreshing the page because the `force-cache` setting forces the use of cached data.
// - This is useful for data that doesn't need to be updated frequently.
```
### 6. Client-Side Data Fetching with fetch (Cache-Control with Revalidation)
This setting enables revalidation after 10 seconds. The time will update if you refresh the page after 10 seconds.
```ruby
// app/page.js (Client-side fetching with revalidation every 10 seconds)
export default async function Home() {
  const res = await fetch('https://worldtimeapi.org/api/timezone/Etc/UTC', {
    next: { revalidate: 10 }, // Revalidate cache every 10 seconds
  });
  const data = await res.json();
  const currentTime = new Date(data.datetime).toLocaleTimeString();

  return <h1>Current Time: {currentTime}</h1>;
}

// Explanation:
// - The time will update every 10 seconds if the page is refreshed.
// - After 10 seconds, Next.js will refetch the data in the background and regenerate the cached version.
```
### 7. Client-Side Caching with SWR (Stale-While-Revalidate)
Using SWR, the time will initially display from the cache, and then the latest time will be fetched in the background and updated.
```ruby
// pages/index.js (using SWR for client-side caching)
import useSWR from 'swr'; (external lib/ requires install)

const fetcher = (url) => fetch(url).then((res) => res.json());

export default function Home() {
  const { data } = useSWR('https://worldtimeapi.org/api/timezone/Etc/UTC', fetcher, {
    refreshInterval: 10000, // Revalidate every 10 seconds
  });

  if (!data) return <div>Loading...</div>;
  const currentTime = new Date(data.datetime).toLocaleTimeString();

  return <h1>Current Time: {currentTime}</h1>;
}

// Explanation:
// - The time will update every 10 seconds while the page remains open, without refreshing.
// - SWR caches the data and revalidates it in the background, providing both performance and freshness.
```
---------------------------


## Data Fetching with Axios


### 1. Server-Side Rendering (SSR) with Axios
With SSR, the data is fetched on every request, meaning the clock will always update when you refresh the page. Axios can be used here to make the request.
```ruby
// pages/index.js (SSR with Axios)
import axios from 'axios';

export async function getServerSideProps() {
  const response = await axios.get('https://worldtimeapi.org/api/timezone/Etc/UTC');
  const currentTime = new Date(response.data.datetime).toLocaleTimeString();

  return {
    props: {
      currentTime,
    },
  };
}

export default function Home({ currentTime }) {
  return <h1>Current Time: {currentTime}</h1>;
}

// Explanation:
// - The time will update whenever the page is refreshed.
// - Since it's using SSR, the data is fetched fresh for every request.
```

### 2. Static Site Generation (SSG) with Axios (No Revalidation)
With SSG, the page is generated at build time. Axios is used to fetch the time, but since there's no revalidation, the time will not change on refresh until you rebuild the app.
```ruby
// pages/index.js (SSG with Axios and no revalidation)
import axios from 'axios';

export async function getStaticProps() {
  const response = await axios.get('https://worldtimeapi.org/api/timezone/Etc/UTC');
  const currentTime = new Date(response.data.datetime).toLocaleTimeString();

  return {
    props: {
      currentTime,
    },
  };
}

export default function Home({ currentTime }) {
  return <h1>Current Time: {currentTime}</h1>;
}

// Explanation:
// - The time will NOT update upon refreshing the page.
// - The time is fetched at build time, and the page remains static until the app is rebuilt.
```

### 3. Static Site Generation (SSG) with Axios and Revalidation (ISR)
In this example, Axios fetches the time, and Next.js revalidates the page every 10 seconds. If you refresh after 10 seconds, the time will update.
```ruby
// pages/index.js (SSG with Axios and ISR)
import axios from 'axios';

export async function getStaticProps() {
  const response = await axios.get('https://worldtimeapi.org/api/timezone/Etc/UTC');
  const currentTime = new Date(response.data.datetime).toLocaleTimeString();

  return {
    props: {
      currentTime,
    },
    revalidate: 10, // Revalidate every 10 seconds
  };
}

export default function Home({ currentTime }) {
  return <h1>Current Time: {currentTime}</h1>;
}

// Explanation:
// - The time will update every 10 seconds if the page is refreshed.
// - ISR ensures that the static page is re-generated every 10 seconds.
```

### 4. Client-Side Data Fetching with Axios (No Cache - no-store)
When fetching data on the client side with Axios, you can manually control caching by preventing the browser or server from caching the response. This example shows no cache, so the time will update every time you refresh.
```ruby
// pages/index.js (Client-side Axios with no caching)
import axios from 'axios';
import { useEffect, useState } from 'react';

export default function Home() {
  const [currentTime, setCurrentTime] = useState('');

  useEffect(() => {
    const fetchTime = async () => {
      const response = await axios.get('https://worldtimeapi.org/api/timezone/Etc/UTC', {
        headers: {
          'Cache-Control': 'no-store', // Prevent caching
        },
      });
      setCurrentTime(new Date(response.data.datetime).toLocaleTimeString());
    };

    fetchTime();
  }, []);

  return <h1>Current Time: {currentTime}</h1>;
}

// Explanation:
// - The time will update on every page refresh because caching is disabled.
// - Axios is used to fetch data client-side with `no-store` to ensure no cache is used.
```

### 5. Client-Side Data Fetching with Axios (Force Cache)
Here, we tell Axios to use the cached data by controlling headers, and the time will not update unless the cache is manually cleared or the browser refreshes in a specific way.
```ruby
// pages/index.js (Client-side Axios with forced cache)
import axios from 'axios';
import { useEffect, useState } from 'react';

export default function Home() {
  const [currentTime, setCurrentTime] = useState('');

  useEffect(() => {
    const fetchTime = async () => {
      const response = await axios.get('https://worldtimeapi.org/api/timezone/Etc/UTC', {
        headers: {
          'Cache-Control': 'public, max-age=3600', // Force using cache for 1 hour
        },
      });
      setCurrentTime(new Date(response.data.datetime).toLocaleTimeString());
    };

    fetchTime();
  }, []);

  return <h1>Current Time: {currentTime}</h1>;
}

// Explanation:
// - The time will NOT update unless the cache expires (after 1 hour in this case).
// - Axios uses `Cache-Control` headers to force caching the response.
```

### 6. Client-Side Data Fetching with Axios (Revalidation every 10 seconds)
In this case, Axios will fetch the time, and the browser will cache the response but revalidate it every 10 seconds.
```ruby
// pages/index.js (Client-side Axios with revalidation every 10 seconds)
import axios from 'axios';
import { useEffect, useState } from 'react';

export default function Home() {
  const [currentTime, setCurrentTime] = useState('');

  useEffect(() => {
    const fetchTime = async () => {
      const response = await axios.get('https://worldtimeapi.org/api/timezone/Etc/UTC', {
        headers: {
          'Cache-Control': 'public, max-age=10, must-revalidate', // Cache for 10 seconds
        },
      });
      setCurrentTime(new Date(response.data.datetime).toLocaleTimeString());
    };

    fetchTime();
  }, []);

  return <h1>Current Time: {currentTime}</h1>;
}

// Explanation:
// - The time will update every 10 seconds if the page is refreshed.
// - Axios revalidates the cached data every 10 seconds using `must-revalidate`.
```

### 7. Client-Side Caching with Axios and SWR (Stale-While-Revalidate)
Here, SWR is used with Axios to fetch the time. SWR will serve the cached data and revalidate it in the background, keeping the data fresh without needing to refresh the page manually.
```ruby
// pages/index.js (Axios with SWR for client-side caching)
import useSWR from 'swr';
import axios from 'axios';

const fetcher = (url) => axios.get(url).then((res) => res.data);

export default function Home() {
  const { data, error } = useSWR('https://worldtimeapi.org/api/timezone/Etc/UTC', fetcher, {
    refreshInterval: 10000, // Revalidate every 10 seconds
  });

  if (error) return <div>Failed to load</div>;
  if (!data) return <div>Loading...</div>;

  const currentTime = new Date(data.datetime).toLocaleTimeString();

  return <h1>Current Time: {currentTime}</h1>;
}

// Explanation:
// - The time will update every 10 seconds automatically without refreshing the page.
// - SWR provides client-side caching and revalidates in the background.
```
