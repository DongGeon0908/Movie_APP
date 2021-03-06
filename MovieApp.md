 MOVIE APP 만들기 with react.js
 ==============================
react를 사용해서 영화APP 만들기 기초!
-----------------------------------

### 개발환경
```
react.js
node.js
npm
npx
yts -> movie list를 가져오는 api 받아오기!
```

<hr/>

#### 영화리스트 가지고 오는 방법!
- axios를 통해서 api로 웹의 리스트를 가지고 온다! 이때 axios의 로딩시간이 오래 걸리므로 async()await을 통해 비동기 처리를 한다.
```
getMovies = async () => {
    const {
      data: {
        data: { movies }
      }
    } = await axios.get(
      "https://yts-proxy.now.sh/list_movies.json?sort_by=rating"
    );
    this.setState({ movies, isLoading: false });
  };
```

#### 영화리스트를 prop로 변환!
1. 가장 먼저 영화리스트를 받을 객체를 만들어준다.
```
Movie.propTypes = {
  id: PropTypes.number.isRequired,
  year: PropTypes.number.isRequired,
  title: PropTypes.string.isRequired,
  summary: PropTypes.string.isRequired,
  poster: PropTypes.string.isRequired
};

export default Movie;
```
2. 받아온 영화 정보를 객체에 주입시킨다!
```
render() {
    const { isLoading, movies } = this.state;
    return (
      <div>
        {isLoading
          ? "Loading..."
          : movies.map(movie => (
              <Movie
                key={movie.id}
                id={movie.id}
                year={movie.year}
                title={movie.title}
                summary={movie.summary}
                poster={movie.medium_cover_image}
              />
            ))}
      </div>
    );
  }
```
3. 주입시킨 데이터를 다시 꺼내서 사용자에게 보여준다!
```
function Movie({ id, year, title, summary, poster }) {
  return <h4>{title}</h4>;
}
```

#### props에 배열 추가하기!
```
genres: PropTypes.arrayOf(PropTypes.string).isRequired 
```

밑의 코드를 주의깊게 보자!
```
function Movie({ year, title, summary, poster, genres }) {
  return (
    <div className="movie">
      <img src={poster} alt={title} title={title} />
      <div className="movie__data">
        <h3 className="movie__title">{title}</h3>
        <h5 className="movie__year">{year}</h5>
        <ul className="genres">
          {genres.map((genre, index) => (
            <li key={index} className="genres__genre">
              {genre}
            </li>
          ))}
        </ul>
        <p className="movie__summary">{summary}</p>
      </div>
    </div>
  );
}
```
- 배열의 값을 뽑아오려면 map()을 사용해야 한다.!

### 네비게이션을 만들자!
- yarn add react-router-dom 설치 
