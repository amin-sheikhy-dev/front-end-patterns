نمونه هایی از فچ کردن API

```ts
const url = 'https://newsapi.org/v2';

async function fetchNews(endpoint: string) {
  try {
    const res = await fetch(`${url}${endpoint}${endpoint.includes('?') ? '&' : '?'}apiKey=YOUR_API_KEY`);

    if (!res.ok) throw new Error(`HTTP Error: ${res.status}`);

    const data = await res.json();

    if (data.status !== 'ok') throw new Error(`Error! ${data.message}`);

    if (data.totalResults === 0) {
      return { articles: [], totalResults: 0 };
    }

    return data;
  } catch (err) {
    throw new Error(`${err}`);
  }
}

type SearchNewsParams = {
  query?: string;
  domains?: string;
  fromDate?: string;
  toDate?: string;
  searchInTitle?: string;
  page?: string;
};

export function searchNews({ query, domains, fromDate, toDate, searchInTitle, page }: SearchNewsParams) {
  let str: string = '/everything?';

  if (!query && !domains) return;

  if (query) {
    const keyWords: string[] = query.trim().split(/\s+/).filter(Boolean);
    if (keyWords.length === 0) return;

    let q: string = 'q=';

    if (keyWords.length === 1) {
      q = q + keyWords[0];
    } else if (keyWords.length === 2) {
      q = q + keyWords.join(' AND ');
    } else {
      q = q + keyWords.join(' OR ');
    }

    str = str + q;
  }

  if (domains) {
    const d: string = `${str.includes('q=') ? '&' : ''}domains=${domains}`;

    str = str + d;
  }

  if (fromDate && toDate) {
    const t = `&from=${fromDate}&to=${toDate}`;
    str = str + t;
  }

  if (searchInTitle) {
    str = str + '&searchIn=title';
  }

  if (page) {
    const p = `&page=${page}&pageSize=7`;
    str = str + p;
  }

  return fetchNews(str);
}

export function getLatestNews() {
  return fetchNews('/top-headlines?country=us&page=1&pageSize=10');
}

export function getAllSources() {
  return fetchNews('/top-headlines/sources');
}

export function categorySources(category: string) {
  if (!category.trim()) return;
  return fetchNews(`/top-headlines/sources?category=${category}`);
}

export function categoryNews(category: string) {
  if (!category.trim()) return;
  return fetchNews(`/top-headlines?category=${category}`);
}
```

```ts
export async function translate(text: string) {
  try {
    const res = await fetch(`https://api.mymemory.translated.net/get?q=${encodeURIComponent(text)}&langpair=en|fa&de=YOUR_EMAIL@gmail.com`);

    if (!res.ok) throw new Error(`HTTP Error: ${res.status}`);

    const data = await res.json();

    if (data.responseStatus !== 200) {
      throw new Error(data.responseDetails || 'Translation failed');
    }

    return data;
  } catch (error) {
    console.error('Translate error:', error);
    throw error;
  }
}
```
