<template>
<div class="wrapper">
    <div class="box">
    <h1>404</h1>
    <p>Не найдено</p>
    <p>То что вы ищите вероятно не создано или удалено</p>
    <p>&#58;&#40;</p>
    <p><a href="/">Вернуться на главную</a></p>
    </div>
</div>
</template>

<style>html { box-sizing: border-box; }

    *,
    *::before,
    *::after { box-sizing: inherit; }
    
    body * {
      margin: 0;
      padding: 0;
    }
    
    body {
      font: normal 100%/1.15 "Merriweather", serif;
      background-color: #F7FFFF;
      color: #3516e6;
    }
    
    .wrapper {
      position: relative;
      max-width: 1298px;
      height: auto;
      margin: 2em auto 0 auto;
    }
    
    /* https://www.flaticon.com/authors/vectors-market */
    /* https://www.flaticon.com/authors/icomoon */
    .box {
      max-width: 70%;
      min-height: auto;
      margin: 0 auto;
      padding: 1em 1em;
      text-align: center;
      background-color: #F7FFFF;
    }
    
    h1, p:not(:last-of-type) { text-shadow: 0 0 6px hsl(187, 78%, 73%); }
    
    h1 {
      margin: 0 0 1rem 0;
      font-size: 8em;
    }
    
    p {
      margin-bottom: 0.5em;
      font-size: 3em;
    }
    
    p:first-of-type { margin-top: 4em; }
    
    p > a {
      border-bottom: 1px dashed #216f79;
      font-style: italic;
      text-decoration: none;
      color: #216f79;
    }
    
    p > a:hover { text-shadow: 0 0 6px #216f79; }
    
    p img { vertical-align: bottom; }
    </style>