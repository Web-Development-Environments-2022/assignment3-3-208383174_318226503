<template>
<div>

 <div class="about-section">
  <h1>About Us</h1>
  <p>We are two 3rd year Information Systems Engineer students from BGU.</p>
  <p>We built a recipe site that will give the user a fun and effective browsing experience.</p>
  <p>Our users can: </p>
  <p >• explore more than 100 different recipes</p>
  <p>• search for a recipe that will meet a specific need</p>
  <p>• save favorite recipes</p>
  <p>• add personal recipes</p>
  <p>• experience nostalgia and watch family recipes.</p>
</div>

<h2 style="text-align:center">Our Team</h2>
<div class="row">
  <div class="column">
    <div class="card">
      <img src="../../resources/chen.png" alt="Chen" style="width:100%; height:500px;">
      <div class="container">
        <h2>Chen Mordehai</h2>
        <p class="title">Developer</p>
        <p>mordeche@post.bgu.ac.il</p>
        <p><button class="button">Contact</button></p>
      </div>
    </div>
  </div>

  <div class="column">
    <div class="card">
      <img src="../../resources/shahaf.png" alt="Shahaf" style="width:100%; height: 500px;">
      <div class="container">
        <h2>Shahaf Erez</h2>
        <p class="title">Developer</p>
        <p>shahafi@post.bgu.ac.il</p>
        <p><button class="button">Contact</button></p>
      </div>
    </div>
  </div>
</div> 

<h2 style="text-align:center">Previous Products</h2>
<!-- <div class="row"> -->
<!-- <p>
    Assignment 3.1 : __
</p>
<p>
    Assignment 3.2 : __
</p> -->
<div class="links-section">
  <a href="https://github.com/Web-Development-Environments-2022/assignment3_2-208383174_318226503.git">Assignment 3.2</a>

</div>
</div>
<!-- </div> -->
</template>

<style>
body {
  font-family: Arial, Helvetica, sans-serif;
  margin: 0;
}

html {
  box-sizing: border-box;
}

*, *:before, *:after {
  box-sizing: inherit;
}

.column {
  float: left;
  width: 50%;
  margin-bottom: 16px;
  padding: 0 8px;
}

.card {
  box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2);
  margin: 8px;
}

.about-section {
  padding: 50px;
  text-align: center;
  background-color: #474e5d;
  color: white;
}

.links-section {
  padding: 50px;
  text-align: center;
  color: rgb(0, 0, 0);
  font-size: large;
}

.container {
  padding: 0 16px;
}

.container::after, .row::after {
  content: "";
  clear: both;
  display: table;
}

.title {
  color: grey;
}

.button {
  border: none;
  outline: 0;
  display: inline-block;
  padding: 8px;
  color: white;
  background-color: #000;
  text-align: center;
  cursor: pointer;
  width: 100%;
}

.button:hover {
  background-color: #555;
}

@media screen and (max-width: 650px) {
  .column {
    width: 100%;
    display: block;
  }
}
</style>