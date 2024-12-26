---
title: "[JS] 스벨트로 영화 검색 "
excerpt: "OMDb API로 svelte 영화 검색"

categories:
  - FE
tags:
  - [tag1, tag2]

permalink: /FE/movieSvelte01/

toc: true
toc_sticky: true

date: 2024-11-10
last_modified_at: 2024-11-10
---

>본 내용은 [HEROPY 강사님 유튜브](https://www.youtube.com/watch?v=yMOSlm667To)  
>위 강의 내용을 내용을 다루고 있습니다.

<br>

OMDB API를 사용해 영화 데이터를 검색하고 관리하는 Svelte Store 기반의 모듈입니다. 이를 통해 여러 컴포넌트에서 상태를 공유하며, 영화 목록, 검색 메시지, 로딩 상태 등을 관리할 수 있습니다.
{: .notice--primary} 

## movie.js
```javascript
// movie.js
import axios from 'axios';
import { writable, get } from 'svelte/store';
import _unionBy from 'lodash/unionBy';

// 스토어 생성 export coonst 객체 이름 = writavle(초기값), 상태관리 여러 컴포넌트에서 공유 가능
// js에서 객체는 참조에 의한 호출(Call by Reference) <-> 기본 데이터 타입(Call by Value): 숫자, 문자열, Boolean
export const movies = writable([]);
export const loading = writable(false);
export const theMovie = writable({});
export const message = writable('Search for the movie title!');

// 스토어 초기화 함수
export function initMovies() {
  movies.set([]); // 리스트를 빈 배열로 초기화
  message.set('Search for the movie title!');
  loading.set(false);
}

// 사용자가 영화 검색했을 때  OMDB API에서 데이터를 가져옴
export async function searchMovies(payload) {
  if (get(loading)) return; // 로딩 중이면 함수 종료
  loading.set(true);
  message.set('');

  let total = 0;

  try { // ...전개 구문(Spread Syntax) 객체 혹은 배열 전개 혹은 분해
    const res = await _fetchMovie({ ...payload, page: 1 }); // payload속성 가져옴
    const { Search, totalResults } = res.data; // 구조 분해 할당 
    movies.set(Search); // 검색된 영화 목록 배열
    total = totalResults; // 검색 결과의 총 개수
  } catch (msg) { 
    movies.set([]);
    message.set(msg);
    loading.set(false);
    return;
  }

  // 페이지 수 계산
  const pageLength = Math.ceil(total / 10);
  if (pageLength > 1) { // 추가 페이지 있는지?있으면 for문 실행
    for (let page = 2; page <= pageLength; page += 1) {
      if (page > payload.number / 10) break;
      const res = await _fetchMovie({ ...payload, page }); // 각 페이지의 데이터를 가져온다.
      const { Search } = res.data;
      movies.update(($movies) => _unionBy($movies, Search, 'imdbID'));
      // _unionBy Lodash의 유틸리티 함수 (중복 데이터 제거)
      // _.unionBy(기준 배열, [병합할 배열], [중복 판단 기준 속성(key)])
      // 기존 영화 리스트와 새로운 영화 리스트를 imdbID 기준으로 중복 없이 병합.
    }
  }
  loading.set(false);
}

export async function searchMovieWithId(id) {
  if (get(loading)) return; // 로딩 상태인지 확인
  loading.set(true);

  // i로 특정 영화 정보 확인
  const res = await _fetchMovie({ id });

  theMovie.set(res.data); // 받은 데이터를 상태 변수에 저장
  loading.set(false);
}

// 영화 데이터 가져오는 함수 _fetchMovie
function _fetchMovie(payload) {

  // payload: 요청 보낼 때 필요한 데이터를 담은 객체
  // res: 응답으로 받아온 데이터를 담은 객체
  const { title, type, year, page, id } = payload;
  const API_KEY = '7035c60c';

  const url = id // id 가 존재할 셩우 : id가 없을 경우
    ? `https://www.omdbapi.com/?apikey=${API_KEY}&i=${id}&plot=full`
    : `https://www.omdbapi.com/?apikey=${API_KEY}&s=${title}&type=${type}&y=${year}&page=${page}`;

    // resolve 비동기 작업 완료일 때 호출, reject 실패 or 에러 발생 했을 때
  return new Promise(async (resolve, reject) => {
    try { // 호출 결과 res에 저장
      const res = await axios.get(url);
      if (res.data.Error) {
        reject(res.data.Error);
      }
      resolve(res);
    } catch (error) { //에러
      console.error(error.response.status);
      reject(error.message);
    }
  });
}
```

### 1. Svelte Store를 이용한 상태 관리
```
writable: 상태 관리를 위해 Svelte의 writable을 사용.  
movies: 영화 리스트를 저장.  
loading: 로딩 상태를 저장.  
theMovie: 특정 영화의 상세 정보를 저장.  
message: 사용자에게 보여질 검색 메시지를 저장. 
```


### 2. 주요 함수
1. initMovies(): 스토어 초기화
영화 리스트(movies), 메시지(message), 로딩 상태(loading)를 초기화.
2. searchMovies(payload): 영화 검색
사용자가 입력한 검색 조건(payload)을 바탕으로 OMDB API에서 영화 데이터를 가져옴.
로직 설명:
payload를 전개(...payload)하여 API 요청을 만듦.
페이지 계산:
검색 결과 총 개수를 바탕으로 페이지 수 계산.
최대 페이지 수만큼 API 호출.
Lodash의 _unionBy 함수로 imdbID 기준 중복 제거.
최종적으로 movies에 검색된 결과를 저장.
3.  searchMovieWithId(id): 특정 영화 검색
영화의 id를 기반으로 상세 정보를 API에서 가져옴.
가져온 데이터를 theMovie에 저장.
4.  _fetchMovie(payload): OMDB API 호출
API 요청을 보내고 데이터를 가져오는 함수.
id가 있으면 영화 상세 정보를 요청, 없으면 검색 요청.
에러 발생 시 Promise의 reject를 통해 에러 메시지 반환.
<br>

### 3. 사용된 기술과 라이브러리
axios: HTTP 요청 라이브러리로, OMDB API 호출에 사용.
Lodash의 _unionBy:
중복 데이터를 제거하면서 배열을 병합.
기준: imdbID.
Svelte의 writable:
컴포넌트 간 상태를 공유.
<br>

### 4. 에러 처리
API 호출 실패 또는 OMDB API의 Error 응답 시, reject로 에러 메시지를 반환.
이를 통해 사용자에게 적절한 메시지를 전달하거나 상태를 초기화.
<br>

### 5. 실제 동작
- 사용자가 영화 제목을 입력 -> searchMovies 함수 호출.
- OMDB API에서 데이터 가져오기 -> 영화 목록 상태(movies) 업데이트.
- 특정 영화를 클릭 -> searchMovieWithId로 영화 상세 정보 가져오기.


>사용자 입력에 따른 영화 데이터 검색.
  중복 제거 및 페이지별 데이터 처리.
  컴포넌트 간 상태 공유.
  에러 처리와 로딩 상태 관리.
  `OMDB API를 통해 영화 검색과 관리에 필요한 모든 로직을 제공하는 모듈`입니다.

## Svelte code

```svelte
// movieCard.svelte
<script>
  import { link } from 'svelte-spa-router';
  import Loader from '~/components/Loader.svelte';

  let imageLoading = true;
  export let movie;

  if (movie.Poster === 'N/A') {
    imageLoading = false;
  } else {
    const img = document.createElement('img');
    img.src = movie.Poster;
    img.addEventListener('load', () => {
      imageLoading = false;
    });
  }
</script>

<a use:link href={`/movie/${movie.imdbID}`} class="movie">
  {#if imageLoading}
    <Loader scale="0.5" absolute={true} />
  {/if}
  <div class="poster" style="background-image: url({movie.Poster});">
    {#if movie.Poster === 'N/A'}
      OMDbAPI<br />
      N/A
    {/if}
  </div>
  <div class="info">
    <div class="poster" style="background-image: url({movie.Poster});" />
    <div class="year">{movie.Year}</div>
    <div class="title">{movie.Title}</div>
  </div>
</a>

<style lang="scss">
  .movie {
    display: block;
    width: 200px;
    height: 300px;
    margin: 10px;
    border-radius: 6px;
    text-decoration: none;
    overflow: hidden;
    position: relative;
    cursor: pointer;
    position: relative;
    &:hover {
      &::after {
        content: '';
        position: absolute;
        top: 0;
        left: 0;
        width: 100%;
        height: 100%;
        border: 6px solid $color--primary;
        box-sizing: border-box;
      }
    }
    .poster {
      width: 100%;
      height: 100%;
      background-color: $color--area;
      background-position: center;
      background-size: cover;
      display: flex;
      justify-content: center;
      align-items: center;
      font-family: 'Oswald', sans-serif;
      color: $color--white-5;
      font-size: 20px;
      text-align: center;
    }
    .info {
      width: 100%;
      height: 66px;
      padding: 14px;
      box-sizing: border-box;
      overflow: hidden;
      position: absolute;
      bottom: 0;
      left: 0;
      .poster {
        position: absolute;
        bottom: 0;
        left: 0;
        transform: scale(2);
        filter: blur(5px);
        &::after {
          content: '';
          background-color: $color--black-50;
          position: absolute;
          top: 0;
          left: 0;
          width: 200%;
          height: 200%;
        }
      }
      .year {
        position: relative;
        color: $color--primary;
        font-size: 12px;
      }
      .title {
        position: relative;
        font-size: 15px;
        font-family: 'Oswald', sans-serif;
        color: $color--white;
        overflow: hidden;
        text-overflow: ellipsis;
        white-space: nowrap;
      }
    }
  }
</style>

```

  ```svelte
  //movieList.svetle
<script>
  import { movies, loading, message } from '~/store/movie.js';
  import MovieCard from './MovieCard.svelte';
  import Loader from '~/components/Loader.svelte';
</script>

<div class:no-result={!$movies.length} class="movie-list">
  {#if $loading}
    <Loader />
  {/if}
  <div class="message">
    {$message}
  </div>
  <div class="movies">
    {#each $movies as movie (movie.imdbID)}
      <MovieCard {movie} />
    {/each}
  </div>
</div>

<style lang="scss">
  .movie-list {
    margin-top: 30px;
    padding: 10px;
    background-color: $color--area;
    border-radius: 4px;
    text-align: center;
    &.no-result {
      padding: 70px 0;
    }
    .message {
      color: $color--primary;
      font-size: 20px;
      text-align: center;
    }
    .movies {
      display: flex;
      flex-wrap: wrap;
      justify-content: center;
    }
  }
</style>
```


. . . 