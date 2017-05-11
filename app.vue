<style>
  html,
  body {
    height: 100%;
    margin: 0;
    padding: 0;
  }
  
  html {
    font-size: 16px;
  }
  
  main {
    height: 100%;
    display: flex;
  }
  
  textarea {
    flex: 1;
    margin: 0;
    background: #333;
    color: white;
    padding: 10px;
  }
  
  aside {
    flex: 1;
  }
  
  .outputs__item {
    border-bottom: 1px solid gray;
  }
  
  .outputs__item__line {
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
        </span> {{output.message}}
      </div>
    </aside>

  </main>
</template>

<script>
  var textlint = require("textlint").textlint;
  textlint.setupRules({
    "no-todo": require("textlint-rule-no-todo"),
    "max-ten": require("textlint-rule-max-ten"),
//    "no-doubled-conjunctive-particle-ga": require("textlint-rule-no-doubled-conjunctive-particle-ga"),
    "no-mix-dearu-desumasu": require("textlint-rule-no-mix-dearu-desumasu"),
    "no-nfd": require("textlint-rule-no-nfd"),
//    "no-double-negative-ja": require("textlint-rule-no-double-negative-ja"),
//    "no-doubled-joshi": require("textlint-rule-no-doubled-joshi"),
    "sentence-length": require("textlint-rule-sentence-length"),
    "spellcheck-tech-word": require("textlint-rule-spellcheck-tech-word"),
    "date-weekday-mismatch": require("textlint-rule-date-weekday-mismatch"),
//    "ja-no-weak-phrase": require("textlint-rule-ja-no-weak-phrase"),
//    "ja-no-redundant-expression": require("textlint-rule-ja-no-redundant-expression"),
    "no-mixed-zenkaku-and-hankaku-alphabet": require("textlint-rule-no-mixed-zenkaku-and-hankaku-alphabet"),
//    "no-dropping-the-ra": require("textlint-rule-no-dropping-the-ra"),
//    "no-doubled-conjunction": require("textlint-rule-no-doubled-conjunction"),
    "ja-no-mixed-period": require("textlint-rule-ja-no-mixed-period"),
    "ja-unnatural-alphabet": require("textlint-rule-ja-unnatural-alphabet"),
  });

  module.exports = {
    replace: false,
    data: {
      input: "TODO: something\n\nTODO: other",
      msg: "",
      outputs: []
    },
    methods: {
      lint: function () {
        textlint.lintMarkdown(this.input).then((result) => {
          this.outputs = result.messages;
        })
      }
    },
    ready: function () {
      this.lint();
    }
  }

</script>