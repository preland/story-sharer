<template>
<div id="app">
  <div class="header">
    <router-link to="/">
      <div class="logo">
        <img class = "logos" src="/note.png">
      </div>
    </router-link>
    <div class="title">
      <h1>Story Sharer!</h1>
    </div>
  </div>
  <div class="content">
    <router-view />
  </div>
  <div class="footer">
    <router-link to="/admin">Share Your Story!</router-link>
    <br>
    <a href="https://github.com/preland/story-sharer.git">Github</a>
  </div>
</div>
</template>

<style>
html {
  box-sizing: border-box;
}

body {
  font-family: 'Montserrat', sans-serif;
  font-size: 16px;
  background: ##c2b8a1;
  padding: 0px;
  margin: 0px;
}

/* Header */
.header {
  display: flex;
  padding: 10px 100px 10px 10px;
  background-color: #a88732;
  color: #d9c79a;
  justify-content: left;
}
.logo{
  align-self: left;
}
.title {
  margin-top: 5px;
}

.title h1 {
  font-size: 30px;
}

.content {
  padding: 20px 100px;
  min-height: 500px;
}

/* Footer */
.footer {
  height: 50px;
  padding: 20px 100px 0px 100px;
  background: #e3e3e3;
  font-size: 12px;
}

.footer a {
  color: #000;
}

h1 {
  font-size: 20px;
}

h2 {
  font-size: 14px;
}
</style>
