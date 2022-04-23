<template>
  <html lang="ko">
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
          <a id="listA" v-on:click="list(1)">글 목록</a>
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
    <div id="listPage">
      <ul>
        <li v-for="data in pageData" v-bind:key="data">
          <div id="listBoxes">
            <a @click="post(data.id)">&nbsp;&nbsp;id: {{ data.id }} | {{ data.title }}</a>
          </div>
          <div id="between"></div>
        </li>
      </ul>
      <div id="pageSelector">
        <div v-for="page in defMaxPage()" v-bind:key="page" style="display: flex; padding: 0.5em; float: left">
          <button @click="list(page)">{{ page }}</button>
        </div>
      </div>
    </div>
  </div>

  <div class="page" id="writeDiv" v-if="page==='write'">
    <h2>글 쓰기</h2>
    <hr class="listLine">
    <div id="inputs">
      <div id="inputTitle" class="inputsTexts">제목 <input v-model="title" maxlength="100"
                                                         class="beforeWrite"
                                                         placeholder="여기에 제목을 입력해주세요"></div>
      <div id="inputDetail" class="inputsTexts"><h4>내용</h4> <textarea v-model="texts"
                                                                      maxlength="3000"
                                                                      class="mainInput"
                                                                      placeholder="여기에 내용을 작성해주세요"></textarea>
      </div>
      <button v-on:click="submit(title, texts)">게시하기</button>
    </div>
  </div>

  <div class="page" id="post" v-if="page==='post'">
    <div id="showTitle">
      <div id="back">
        <button @click="back()">뒤로가기</button>
      </div>
      <h1>{{ postData.title }}</h1>
    </div>
    <hr class="listLine">
    <div>
      <p v-html="postDataTexts"></p>
    </div>
  </div>
  </body>
  </html>
</template>

<script>
import axios from "axios";
import "./style.css"

export default {
  name: 'App',
  data() {
    return {
      page: 'read',
      title: '',
      texts: '',
      pageList: '1',
      pageData: '',
      postData: '',
      postDataTexts: '',
      readingPage: '1',
      maxPage: '1'
    }
  },
  methods: {
    post(id) {
      this.page = 'post'

      axios({
        url: 'http://222.100.125.234:3000/post/' + id
      }).then(response => this.process(response))
    },
    process(response) {
      this.postData = response.data
      this.postDataTexts = response.data.texts.toString().replace(/\n/gim, '<br>')
    },
    read() {
      this.page = 'read'
    },
    write() {
      this.page = 'write'
    },
    list(page) {
      this.page = 'main'

      axios({
        url: 'http://222.100.125.234:3000/page/'+page,

      }).then(response => this.pageData = response.data)
    },
    submit(title, texts) {
      let url = 'http://222.100.125.234:3000/post';
      let pass = parseInt(prompt("4자리의 비밀번호를 입력해주세요"));
      if (pass.toString().length > 3 && pass.toString().length < 5) {

        const data = {
          "title": title,
          "texts": texts,
          "pass": pass
        }

        return axios.post(url, JSON.stringify(data), {
          headers: {
            "Content-Type": `application/json`,
          },
        })
          .then((res) => {
            alert(res.data);
            this.page = 'read'
          })
          .catch(function (error) {
            alert(error + " 오류가 발생했습니다 다시 시도해주세요 \n반복된다면 관리자에게 연락 바랍니다")
          })
      } else {
        alert("4자리의 숫자만 가능합니다")
      }
    },
    back() {
      this.page = 'main'
    },
    defMaxPage() {
      axios({
        url: 'http://222.100.125.234:3000/maxPage'
      }).then(response => this.maxPage = response.data)

      return this.maxPage
    },
  }
}

</script>
