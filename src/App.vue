<template>
    <TicketTeller />
</template>

<script>

import TicketTeller from './components/TicketTeller.vue'

export default {
  name: 'App',
  components: {
    TicketTeller
  }
}
</script>

<style>
  html {
    height: 100%;
  }

  html, body {
    width: 100%;
    padding: 0;
    margin: 0;
  }

  body {
    min-height: auto;
    box-sizing: border-box;
  }

  header {
    padding: 1rem 0 1rem 0;
    background-color: #014ecb;
    color: white;
  }

  main, header > h1 {
    padding: 0 16px;
    max-width: 760px;
    box-sizing: border-box;
    margin: 0 auto;
  }

  main {
    margin: 24px auto;
    box-sizing: border-box;
  }

  pre {
    white-space: pre-wrap;
    background-color: #EEEEEE;
    padding: 16px;
  }

  pre code {
    word-break: break-word;
  }

  @media (max-width: 600px) {
    header > h1 {
      font-size: 36px;
    }
  }


</style>
