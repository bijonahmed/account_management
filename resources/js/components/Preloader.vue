<template>
  <div class="loader">
  <div class="loader-wheel"></div>
  <div class="loader-text"></div>
</div>
</template>
 <style>
 
.loader {
  width: 60px;
}

.loader-wheel {
  animation: spin 1s infinite linear;
  border: 2px solid rgba(30, 30, 30, 0.5);
  border-left: 4px solid #fff;
  border-radius: 50%;
  height: 50px;
  margin-bottom: 10px;
  width: 50px;
}

.loader-text {
  color: #fff;
  font-family: arial, sans-serif;
}

.loader-text:after {
  content: 'Loading';
  animation: load 2s linear infinite;
}

@keyframes spin {
  0% {
    transform: rotate(0deg);
  }
  100% {
    transform: rotate(360deg);
  }
}

@keyframes load {
  0% {
    content: 'Loading';
  }
  33% {
    content: 'Loading.';
  }
  67% {
    content: 'Loading..';
  }
  100% {
    content: 'Loading...';
  }
}
</style>