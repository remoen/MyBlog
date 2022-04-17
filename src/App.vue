<template>
  <body>
    <header id="header">
      <h1 id="title">
        <a v-on:click="read()">MyBlog</a>
      </h1>
      <nav class="links">
        <ul>
          <li>
            <a id="write" v-on:click="write()">글 쓰기</a>
          </li>
          <li>
            <a id="listA" v-on:click="list()">글 목록</a>
          </li>
        </ul>
      </nav>
    </header>

    <div class="page" id="read" v-if="page==='read'">
      <h2>반가워요!</h2>
      <hr class="listLine">
      <h3>MyBlog</h3>
      <ul>
        <li>
          개발자 공부용
        </li>
        <li>
          로그인 없이 글, 댓글 작성
        </li>
        <li>
          오픈 소스 (<a href="https://github.com/remoen">클릭</a>)
        </li>
      </ul>
    </div>

    <div class="page" id="list" v-if="page==='main'">
      <h2>글 목록</h2>
      <hr class="listLine">
    </div>

    <div class="page" id="writeDiv" v-if="page==='write'">
      <h2>글 쓰기</h2>
      <hr class="listLine">
      <div id="inputs">
        <div id="inputTitle" class="inputsTexts">제목 <input v-model="title" maxlength="100" class="beforeWrite" placeholder="여기에 제목을 입력해주세요"></div>
        <div id="inputDetail" class="inputsTexts"><h4>내용</h4> <textarea v-model="texts" maxlength="3000" class="mainInput" placeholder="여기에 내용을 작성해주세요"></textarea></div>
        <button v-on:click="submit(title, texts)">게시하기</button>
      </div>
    </div>
  </body>
</template>

<script>
import axios from "axios";

export default {
  name: 'App',
  data(){
    return{
      page: 'read',
      title: '',
      texts: ''
    }
  },
  methods : {
    read() {
      this.page = 'read'
    },
    write() {
      this.page = 'write'
    },
    list() {
      this.page = 'main'
    },
    submit(title, texts) {
      let url = 'http://localhost:3000/post';
      let pass = parseInt(prompt("4자리의 비밀번호를 입력해주세요"));
      if (pass < 10000 && pass > 999) {
        alert("게시되었습니다");
        window.location.reload()
        return axios.post(url, {
          title: title,
          texts: texts,
          pass: pass
        })
      } else {
        alert("4자리의 숫자만 가능합니다")
      }
    }
  },
}

</script>
import axios form 'axios'


<style>

html,
body {
  width: 99.5%;
  height: 99.5%;
}

#header {
  display: -moz-flex;
  display: -webkit-flex;
  display: -ms-flex;
  display: flex;
  -webkit-justify-content: space-between;
  justify-content: space-between;
  background-color: #ffffff;
  border-bottom: solid 1px rgba(160, 160, 160, 0.3);
  height: 3.5em;
  left: 0;
  line-height: 3.5em;
  position: fixed;
  top: 0;
  width: 100%;
  z-index: 10000;
}

#header h1 {
  height: inherit;
  line-height: inherit;
  padding: 0 0 0 1em;
}

#header h1 a {
  font-size: 0.7em;
  text-decoration: none;
  color: black;
  text-align: center;
}

h1 {
  color: #3c3b3b;
  font-family: "Raleway", Helvetica, sans-serif;
  font-weight: 800;
  letter-spacing: 0.25em;
  line-height: 1.5;
  margin: 0 0 1em 0;
  text-transform: uppercase;
}

#header .links {
  -webkit-flex: 1;
  -ms-flex: 1;
  flex: 1;
  border-left: solid 1px rgba(160, 160, 160, 0.3);
  height: inherit;
  line-height: inherit;
  margin-left: 1.5em;
  overflow: hidden;
  padding-left: 1.5em;
}

#header ul {
  list-style: none;
  margin: 0;
  padding-left: 0;
}

#header .links ul li:first-child {
  border-left: 0;
  margin-left: 0;
  padding-left: 0;
}

#header .links ul li {
  border-left: solid 1px rgba(160, 160, 160, 0.3);
  line-height: 1;
  margin-left: 1em;
  padding-left: 1em;
}

#header ul li {
  display: inline-block;
  padding-left: 0;
}

.page {
  display: block;
  padding: 3em 1em;
}

.page h2 {
  display: flex;
}

.listLine {
  width: 100%;
  border-bottom: 0;
  text-align: left;
  margin-left: 0;
}

#read li {
  font-size: 16px;
  line-height: 36px;
}

#read li a {
  text-decoration: none;
  color: #3c3b3b;
}

#inputs {
  margin: 1em;
}

.inputsTexts {
  font-size: 20px;
  font-weight: bold;
}

.beforeWrite {
  width: 90%;
  height: 20px;
  text-align: left;
  align-self: center;
}

.mainInput {
  width: 93%;
  height: 500px;
  vertical-align: top;
  text-align: left;
  align-self: center;
}

</style>
