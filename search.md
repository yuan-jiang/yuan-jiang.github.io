---
layout: page
title: Search
permalink: /search/
sitemap: false
---

<!-- Search page powered by google custom search -->
<div id="home-search" class="home">
<div id='cse-search-form' style='width: 100%;'>Loading</div>
<script src='//www.google.com/jsapi' type='text/javascript'></script>
<script type='text/javascript'>
google.load('search', '1', {language: 'en', style: google.loader.themes.V2_DEFAULT});
google.setOnLoadCallback(function() {
  var customSearchOptions = {};
  var orderByOptions = {};
  orderByOptions['keys'] = [{label: 'Relevance', key: ''} , {label: 'Date', key: 'date'}];
  customSearchOptions['enableOrderBy'] = true;
  customSearchOptions['orderByOptions'] = orderByOptions;
  var customSearchControl =   new google.search.CustomSearchControl('001081081534151912654:nvld-2aryds', customSearchOptions);
  customSearchControl.setResultSetSize(google.search.Search.FILTERED_CSE_RESULTSET);
  var options = new google.search.DrawOptions();
  options.enableSearchboxOnly('https://www.google.com/cse?cx=001081081534151912654:nvld-2aryds');
  options.setAutoComplete(true);
  customSearchControl.draw('cse-search-form', options);
}, true);
</script>
<style type='text/css'>
  input.gsc-input, .gsc-input-box, .gsc-input-box-hover, .gsc-input-box-focus {
    border-color: #D9D9D9;
  }
  input.gsc-search-button, input.gsc-search-button:hover, input.gsc-search-button:focus {
    border-color: #666666;
    background-color: #CECECE;
    background-image: none;
    filter: none;

  }
</style>
</div>
