

# Movie Aim

## 1. 목표

- 영화 추천 서비스 구현
- HTML/CSS, Javascript(Vue.js/Vanilla JS), Django, Database 등을 활용한 실제 서비스 설계
- Git을 통한 소스코드 버전 관리 및 협업
- 서비스 배포



## 2. 개발환경

1. Python Web Framework 

   A. Django 2.1.x 

   B. Python 3.7.x 



2. 개발 아키텍처

    Django + VanilaJS + VueJS (CDN, ver 2.5.16)



3. 서비스 배포 환경

   A. 서버 : AWS Elastic Beanstalk

   B. DB : SQLite 



## 3. 데이터 베이스 구조

![image](https://user-images.githubusercontent.com/52685373/69800442-9cf46980-1218-11ea-9f1c-5f35f60cef02.png)





## 4. 분담내용

조규홍 - DB, Vue,  JavaScript,  ERD, 배포

김준호 - django, HTML, CSS



## 5. 프로젝트 소개

> http://movie-aim.yarufnjmpv.ap-northeast-2.elasticbeanstalk.com/
>
> 베포 사이트

### 5.1 Index & nav

![image](https://user-images.githubusercontent.com/52685373/69791317-97425800-1207-11ea-955d-c37020901982.png)

![image](https://user-images.githubusercontent.com/52685373/69791255-78dc5c80-1207-11ea-8628-d79b94a52637.png)

- nav 에서는 영화검색, 로그아웃, 회원정보수정, 홈화면이 가능합니다.

- index에서는 카드가 뒤집히며 정보를 표시합니다.
- 장르별로 영화를 찾는것이 가능합니다.
- 좋아요 한 영화들을 기준으로 영화를 추천합니다.



### 5.1.1 nav - 로그인

![image](https://user-images.githubusercontent.com/52685373/69793220-577d6f80-120b-11ea-83fc-b4235213eae6.png)



- 로그인은 모달로 작동합니다.
- kakao, naver, github 아디로 로그인이 가능합니다.



### 5.1.2 nav - 회원가입

![image](https://user-images.githubusercontent.com/52685373/69793252-67954f00-120b-11ea-9057-d8405bb8aa96.png)



### 5.1.3 nav - 회원정보수정

![image](https://user-images.githubusercontent.com/52685373/69794417-9280a280-120d-11ea-9633-7db246f53c35.png)

- 유저의 회원정보수정의경우 회원탈퇴와 비밀번호 변경이 나타납니다.



### 5.1.4 nav - 영화 정보 생성 

![image](https://user-images.githubusercontent.com/52685373/69794522-c65bc800-120d-11ea-90e9-11a362cecc8f.png)

- 스테프 권한이 있다면 영화 정보를 생성, 수정, 삭제가 가능합니다.



### 5.2 detail

![image](https://user-images.githubusercontent.com/52685373/69791768-89410700-1208-11ea-8a7a-e2158c91a9fe.png)

![image](https://user-images.githubusercontent.com/52685373/69791857-b42b5b00-1208-11ea-869c-3a35d525c5a0.png)

![image](https://user-images.githubusercontent.com/52685373/69791908-d2915680-1208-11ea-9363-c95ef159fbc6.png)



- 모달로서 예고편을 감상할 수 있습니다.
- 좋아요는 비동기로 작동합니다.
- 별갯수를 선택함으로써 별점을 줄 수 있습니다.
- 댓글에서 유저의 프로필, 댓글의 삭제 및 수정이 가능합니다.



### 5.3 profile

![image](https://user-images.githubusercontent.com/52685373/69792353-ce196d80-1209-11ea-8ce7-ee03dee1ec17.png)

- 자신의 프로필이라면 follow버튼은 나타나지않습니다
- 좋아요를 누른 영화를 표시합니다
- 자신이 팔로우한 유저들을 표시합니다



### 5.4 accounts

![image](https://user-images.githubusercontent.com/52685373/69792744-86471600-120a-11ea-8f94-238a3d2a0595.png)

![image](https://user-images.githubusercontent.com/52685373/69792760-8e9f5100-120a-11ea-80a6-4cce23bc74c0.png)

- 유저관리 탭입니다. 스태프 권하이 있는 유저의 경우 nav에서 유저관리를 통해 접근이 가능합니다.
- 유저삭제 및 스태프 권한부여가 가능합니다.



## 6. 목표 서비스 및 실제 구현정도

- 목표로 하는 서비스는 댓글의 생성과 삭제, 수정, 장르의 선택등 redirect하는 페이지를 모두 비동기로 구현하는 것이 목표였습니다.  시간의 부족으로 비동기는 팔로우와, 좋아요 만이 비동기로 작동합니다.



## 7.  에러사항

acotrs와 directors는 CharField로 가져와 json으로 사용되었습니다. 

```
json.loads(movie.actors)
```

를 이용하여 가져왔으나 오류가 생겻습니다.



파이썬은 '를 사용하고 json은 "을 사용하기에 때문에 생긴 오류입니다

또한 내부에 ' 혹은 "이 있을겨우 에러가 생기기에 @혹은 %로 바꾸어주었습니다



```json
movie.actors =  json.loads(movie.actors.replace("'", '"'))
copy_actors = {}
for actor,value in movie.actors.items():
copy_actors[actor.replace('@', "'")] = [value[0].replace('@', "'").replace('%', '"'), value[1]]
movie.actors = copy_actors
```



```vue
<script>
  const app = new Vue({
    el: '#youtube',
    data: {
      movieId: '',
      videoUrl: ''
    },
    computed: {
      getUrl: function () {
        axios.get(
            `https://api.themoviedb.org/3/movie/{{ movie.movie_cd }}/videos?api_key=ae2f91743e07bb1124d4c971051d712e&language=en-US`
            )
          .then(res => {
            this.movieId = res.data.results[0].key
            this.videoUrl = `https://www.youtube.com/embed/${this.movieId}?version=3&enablejsapi=1`
            console.log(this.videoUrl)
          })
        return this.videoUrl
      }
    }
  })
</script>
```

return의 위치가 잘못되어 vue가 작동하지 않았습니다. 





