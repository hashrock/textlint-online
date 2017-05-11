<style>
  html, body{
    height: 100%;
    margin: 0;
    padding: 0;
  }
  html{
    font-size: 16px;
  }
  
  main{
    height: 100%;
    display: flex;
  }
  
  textarea{
    flex: 1;
    margin: 0;
    background: #333;
    color: white;
    padding: 10px;
  }
  
  aside{
    flex: 1;
  }
  
  .outputs__item{
    border-bottom: 1px solid gray;
  }
  
  .outputs__item__line{
    font-size: 0.8rem;
    color: gray;
  }
</style>

<template>
  <main>
    <textarea v-model="input" @keyup="lint"></textarea>
    <aside class="outputs__list">
      <div class="outputs__item" v-for="output in outputs">
        <span class="outputs__item__line">
          {{output.line}}:{{output.column}}
        </span>
         {{output.message}}
      </div>
    </aside>

  </main>
</template>

<script>
  var textlint = require("textlint").textlint;
  textlint.setupRules({
    "no-todo": require("textlint-rule-no-todo"),
    "max-ten": require( "textlint-rule-max-ten"),
//    "spellcheck-tech-word":require("textlint-rule-spellcheck-tech-word"),
    "no-mix-dearu-desumasu":require("textlint-rule-no-mix-dearu-desumasu"),
  });

  module.exports = {
    replace: false,
    data: {
      input: "TODO: something\n\nTODO: other",
      msg: "",
      outputs: []
    },
    methods: {
      lint: function(){
        var result = textlint.lintMarkdown(this.input);
        this.outputs = result.messages;
      }
    },
    ready: function(){
      this.lint();
    }
}

</script>