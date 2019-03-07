<template>
  <div class="container">
    <h2 class="welcome">Formulario vinculado a <span style="color: #35495e;">{{email}}</span></h2>
    <div>
      <label for="create-post">Entre com seu e-mail: </label>
      <input type="text" id="create-post" v-model="email" placeholder="E-mail">
    </div><br>
    <div class="create-post">
      <h2>Crie seu formulário!</h2>

      <label for="create-post">Create Question Name: </label>
      <input type="text" id="create-post" v-model="text" placeholder="Create a Question">
      <div>
        <label for="create-post">First Option: </label>
        <input type="text" id="create-post" v-model="questionOne" placeholder="First Option">
        <input type="radio" name="input-value" value="1" v-model="pickedTrue">
        
        <br>
        <label for="create-post">Second Option: </label>
        <input type="text" id="create-post" v-model="questionTwo" placeholder="Second Option">
        <input type="radio" name="input-value" value="2" v-model="pickedTrue">
        
      </div>
      <span id="create-post">Resposta verdadeira: {{ pickedTrue }}</span>
      <br>
      <button v-on:click="createPost">Create Question!</button>
    </div>
    <h1>Posts</h1>

    <hr>
    <p class="error" v-if="error"> {{ error }} </p>
    <div class="posts-container">
      <div class="post main-container-question"
        v-for="(post, index) in even(posts, email)"
        v-bind:item="post"
        v-bind:index="index"
        v-bind:key="post._id"
      >
        {{ `${post.createdAt.getDate()}/${post.createdAt.getMonth()}/${post.createdAt.getFullYear()}`}}
        <p class="text">{{ post.text }}</p>
        <div>
          <input type="radio" name="choise-area" class="text" value="1">{{ post.questionOne }}<br>
          <input type="radio" name="choise-area" class="text" value="2">{{ post.questionTwo }}
        </div>

        <p class="text true-question">Resposta verdadeira: <span>{{ post.pickedTrue }}</span></p>
        <p style="font-size: 10px;">created by: {{post.email}}</p>
        <br><button class="btn-submit" >Submit!</button>
        <button v-on:click="deletePost(post._id)">Delete!</button>
  
      </div>
    </div>

    <button class="btn-submit" >Veja sua pontuação</button>
  </div>
</template>

<script>
import PostService from '../PostService';

export default {
  name: 'PostComponent',
  data(){
    return {
      posts: [],
      error: '',
      text: '',
      email: '',
      questionOne: '',
      questionTwo: '',
      pickedTrue: ''
    }
  },
  async created(){
    try{
        this.posts = await PostService.getPosts();
    } catch(err) {
      this.error = err.message;
    }
  },
  methods: {
    even: function (posts, email) {
      return posts.filter(function (post) {
        return post.email === email
      })
    },
    async createPost(){
      await PostService.insertPost(this.text, this.questionOne, this.questionTwo, this.pickedTrue, this.email);
      this.posts = await PostService.getPosts();
    },
    async deletePost(id){
      await PostService.deletePost(id);
      this.posts = await PostService.getPosts();
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
label {
    font-size: 17px;
    font-weight: 700;
    color: #ff335c;
}
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
.post {
    background-color: #f3f2f5;
    width: 280px;
    border-radius: 5px;
    margin: 15px auto;
    font-size: 19px;
    font-family: sans-serif;
    font-weight: 700;
    color: #35495e;
    -webkit-box-shadow: 0px 2px 8px 2px rgb(208, 208, 208);
    box-shadow: 0px 2px 8px 2px rgb(179, 190, 195);
    padding: 10px;
}
button {
    padding: 15px 110px;
    background-color: #ff2b5d;
    color: white;
    font-size: 18px;
    font-weight: 700;
    border-radius: 5px;
    border-style: none;
    margin-top: 15px;
    cursor: pointer;
    transition: 1s;
}
button:hover {
    opacity: 0.7;
}
input#create-post {
    padding: 5px 10px;
    border-radius: 5px;
    color: #35495e;
    border-style: double;
}
button.btn-submit {
    padding: 15px 25px;
    background: #101911;
    border-radius: 5px;
}
.create-post {
    max-width: 790px;
    margin: auto;
    background-color: #f3f2f5;
    padding: 10px;
    border-radius: 5px;
    box-shadow: 0px 2px 8px 2px rgb(179, 190, 195);
}
</style>
